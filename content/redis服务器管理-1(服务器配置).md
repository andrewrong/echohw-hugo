title: redis服务器管理-1(服务器配置) 
date: 2014-10-25 16:11:08
categories: redis
tags: redis
---

### 关于redis服务器的配置方式

#### 1. 在启动redis服务器的时候，通过参数进行设置;

```
    redis-server --port 6380 //指定端口号，让redis服务器只是监听端口6380
```

关于redis-server这个命令的支持的其他的参数可以通过`redis-server --help`来进行查询;

#### 2. 在redis服务器启动的时候，指定配置文件;

```
    redis-server redis.conf //其中redis.conf就是配置文件，默认启动的时候会加载默认的配置文件，在就是redis目录下面的redis.conf
```
在redis的目录下面，有一个redis.conf的文件，里面有各种配置，port、创建的数据库的个数、slave、timeout等一切可以设置的配置;只要模仿这个文件就可以配置自己的配置文件，然后在启动redis的时候把这个配置文件指定到你要指定的目录就可以了;**参数的优先级会比配置文件的要高**;

#### 3. 通过config get 和 config set 修改运行中的redis服务器的配置

上面的两种配置的局限性在于必须在redis服务器启动的时候来指定的，但是保不定可能在服务器运行期间来修改一些配置，所以redis通过config get 和 config set两个命令来支持动态修改配置的功能;

```
    config get <option> // option是就是在redis.conf中配置的参数，比如port、save等
    config set <option> <value> //与get对应，set是用来设置参数的
```
对config命令需要注意2点:

1. 不是所有的命令都支持动态配置;比如`databases` 和 `port`就只能在服务器启动的时候才能被设置，运行的过程中是不能被配置的;
2. 使用config修改的配置不会记录在配置文件中的，这也就表示当服务器重启的时候，config修改的配置就会丢失;可以通过`config rewrite`这个命令把config修改的配置写入到配置文件中去;

关于redis服务化的其他的配置都在redis默认的redis.conf中，如果有兴趣你可以把这里面的配置看看，可以定制化的东西比较多~~


