title: 'Cassandra client 总结'
date: 2015-09-05 21:30:59
categories:
tags: 分布式
---

#### 1. 起因

本来我们的监控系统是自己做文件系统来做存储,但是最后由于:"可扩展性太差，之后数据越多，绝对是单点，肯定扛不住..."；而且他很不相信hbase，或者应该是我们公司没有靠谱的hbase维护团队，最后他选择了cassandra这款存储系统；cassandra是facebook开发的，查了一下关于它的新闻，当年twitter想使用这款产品，但是最后放弃了。。。但是那是2010年的事情，我想过了那么多年应该是稳定了不少；所以提议使用这款产品还是比较靠谱的，除了有facebook优良的血统，还有以下优点：

* 无中心，无单点
* 可扩展，这个就表示我们可以通过堆机器来实现扩展
* 支持cql查询，这是一种类似于mysql协议的东西，个人认为就是实现了一层类似于mysql的协议，用户可以使用类似于sql语言来查询和插入;

而我主要负责编写client与cassandra通信;

#### 2. Cassandra 驱动

主要使用的是Cassandra官方的[C++驱动](https://github.com/datastax/cpp-driver);说真的我很想吐槽这个"C++"驱动，里面完全是C的模式写代码的，这就算了，关键是资源管理这块实在是太恶心了，要new/free成对，而且有的对象是从它的内部申请内存，却要我在外部释放....我已经无比的烦躁了。。写完第一次的时候，就发现了内存泄露了...

#### 3. CRUD

我基本上也是造这他的基本文档来做的，但是说到这个文档我又一次想吐槽facebook这个文档真的好简单，看着这个文档基本上都不能做什么具体的事情，包括它的folly库也是...在这里我还要吐槽czmq这个文档真心简单到爆了，demo完全都不知道在干什么...最起码你要有文档把基本的功能说一些哇...

文档不行，基本上还要看代码来知道如何使用...

* [github 首页的代码](https://github.com/datastax/cpp-driver)
* [get start](http://datastax.github.io/cpp-driver/topics/#futures)
* [examples](https://github.com/datastax/cpp-driver/tree/1.0/examples)，这个比较好，因为很多上面没有的这边基本都有..我发现很多开源文档很简单，但是test用例基本上会涵盖基本用户...google 的flatbuffer也是这样的..

下面我以我在项目中写的代码来看这个client要如何使用;

##### 插入

项目中的插入包含两种，一种是简单的现成的数据类型插入， 第二种插入自己定义的类型；

###### 简单类型

* 创建查询语句CassStatement
* 为CassStatement绑定具体参数
* 运行函数cass_session_execute
* 得打future，由于插入我不关心结果，所以不用对future进行处理
* 最后释放各种对象;


```
/**
 *有一张表key_index,里面有两个字段:metric,tags,类型都是string
 */
 
bool insertKeyIndex(const string &metric, const string &tagStr) {
    CassError rc = CASS_OK;
    CassStatement* statement = nullptr;
	
	# 1. cql语句，与sql一样
    const char* sql = "INSERT INTO key_index(metric, tags) VALUES(?, ?)";
    
    # future是用来取得结果的
    CassFuture* future = nullptr;
	
	# 创建一个查询语句的对象，参数:查询语句，参数个数;这个对象之后要自己释放;
    statement = cass_statement_new(sql, 2);
    # 绑定参数，cass_statement_bind_xxx，这个库提供了各种绑定参数的函数;
    cass_statement_bind_string(statement, 0, metric.c_str());
    cass_statement_bind_string(statement, 1, tagStr.c_str());

	# 执行命令,这是一个异步的过程，函数运行不会阻塞
    future = cass_session_execute(m_session, statement);
    #释放查询对象
    cass_statement_free(statement);
    # 释放future对象
    cass_future_free(future);

    return true;
}
```

###### 自定义类型

自定义类型其实有[例子](https://github.com/datastax/cpp-driver/tree/master/examples/udt)

```
/**
 * datapoints表中有三个字段key,time,data,其中key是string，time是timestamp类型，data是我们自己定义的SCMM类型
 */

struct SCMM {
    double sum;
    int count;
    double min;
    double max;
};

bool insertDataPoints(const string &key, const vector<timeValue> &values) {
    CassError rc = CASS_OK;
    CassStatement* statement = nullptr;

    const char* sql = "INSERT INTO datapoints (key, time, data) VALUES(?, ?, ?)";
    CassFuture* future = nullptr;
    CassBatch* batch = cass_batch_new(CASS_BATCH_TYPE_LOGGED);

    for(auto val : values){
        statement = cass_statement_new(sql, 3);
        cass_statement_bind_string(statement, 0, key.c_str());
        cass_statement_bind_int64(statement, 1, val.timestamp * 1000);
		
		# 创建用户定义的类型对象，这里的m_scmm
		# cass_schema_get_udt:m_schema是这张表的schema信息，sentry1类似于mysql的库的概念，scmm是你自定的类型的名字
		# m_schema = cass_session_get_schema(m_session);
		# m_scmm = cass_schema_get_udt(m_schema, "sentry1", "scmm");
        CassUserType* scmm = cass_user_type_new_from_data_type(this->m_scmm);
        cass_user_type_set_double_by_name(scmm, "sum", val.value.sum);
        cass_user_type_set_int32_by_name(scmm, "count", val.value.count);
        cass_user_type_set_double_by_name(scmm, "min", val.value.min);
        cass_user_type_set_double_by_name(scmm, "max", val.value.max);

		# 经过上面的绑定，然后再把自定义类型绑定到statement就ok了;
        cass_statement_bind_user_type(statement, 2, scmm);

        cass_batch_add_statement(batch, statement);
        cass_statement_free(statement);
        cass_user_type_free(scmm);
    }

    future = cass_session_execute_batch(m_session, batch);
    cass_future_free(future);
    cass_batch_free(batch);

    return true;
```

#### 4. 总结

上面只有插入，因为其他的我还没有写；之后会慢慢的补齐...关于这个client，后期准备使用C++做个封装，尤其是资源的释放还是很需要的....


