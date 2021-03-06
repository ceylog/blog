---
title:      redis 整理（一）简介及安装
category: blog
description: redis 相关整理（一）简介及安装
date: 2014-08-06
---

## NoSQL 简介
NoSQL更注重的是对海量数据存取的性能、分布式、扩展性支持上，并不需要传统关系数据库的一些特征，例如：Schema、事务、完整SQL查询支持等等，因此在分布式环境下的性能相对与传统的关系数据库有较大的提升。
Redis就是NoSQL这个大家族中的一份子，它是一个开源的使用ANSI C语言编写、支持网络、可基于内存也可持久化的日志型、Key-Value数据库，并提供多种语言的API。



## Redis 简介

[Redis][1] is an open source, BSD licensed, advanced key-value cache and store. It is often referred to as a data structure server since keys can contain strings, hashes, lists, sets, sorted sets, bitmaps and hyperloglogs.

Redis是一个开源的，高级的键值式存储数据库。它通常被称为一个数据结构服务器可以包含字符串，哈希表，链表，集合和有序集合。


为了实现其出色的表现，Redis是工作在内存中的数据集。您还可以根据您的具体的使用情况，每隔一段时间或执行命令时将数据集写入到磁盘，并且添加到日志中。Redis还支持主从复制，并且配置起来很简单，第一次同步就可以无阻塞的达到极快的速度，在网络断开的时候可以自动重连等等。另外它的简单的检查与设置机制，发布(Publish)与订阅(Subscribe)和配置设置的特性使它看起来像是一个缓存。

## Redis 适用场景
### 1、取最新N 个数据的操作
比如典型的取你网站的最新文章，通过下面方式，我们可以将最新的5000 条评论的ID 放在Redis 的List 集合中，
并将超出集合部分从数据库获取。


### 2、排行榜应用，取TOP N 操作
这个需求与上面需求的不同之处在于，前面操作以时间为权重，这个是以某个条件为权重，比如按顶的次数排序，
这时候就需要我们的sorted set 出马了，将你要排序的值设置成sorted set 的score，将具体的数据设置成相应的value，
每次只需要执行一条ZADD 命令即可。


### 3、需要精准设定过期时间的应用
比如你可以把上面说到的sorted set 的score 值设置成过期时间的时间戳，那么就可以简单地通过过期时间排序，
定时清除过期数据了，不仅是清除Redis 中的过期数据，你完全可以把Redis 里这个过期时间当成是对数据库中数据的索引，
用Redis 来找出哪些数据需要过期删除，然后再精准地从数据库中删除相应的记录。


### 4、计数器应用
Redis 的命令都是原子性的，你可以轻松地利用INCR，DECR 命令来构建计数器系统。


### 5、Uniq 操作，获取某段时间所有数据排重值
这个使用Redis 的set 数据结构最合适了，只需要不断地将数据往set 中扔就行了，set 意为集合，所以会自动排重。


### 6、实时系统，反垃圾系统
通过上面说到的set 功能，你可以知道一个终端用户是否进行了某个操作，可以找到其操作的集合并进行分析统计对比等。
没有做不到，只有想不到。


### 7、Pub/Sub 构建实时消息系统
Redis 的Pub/Sub 系统可以构建实时的消息系统，比如很多用Pub/Sub 构建的实时聊天系统的例子。


### 8、构建队列系统
使用list 可以构建队列系统，使用sorted set 甚至可以构建有优先级的队列系统。


### 9、缓存
这个不必说了，性能优于Memcached，数据结构更多样化。

## Redis 安装

### 第一步，下载Redis，可以通过官网[下载][2]最新版本的源代码包

### 第二步，将下载好的源代码包上传到我们的Linux主机中，解压，编译安装

    $ tar zxf redis-2.8.13.tar.gz
    $ cd redis-2.8.13
    $ make
    $ cd src && make all

### 第三步，启动redis服务

    $ src/redis-server

### 第四步，测试redis

    $ redis－cli
    redis 127.0.0.1:6379> set foo 123
    redis 127.0.0.1:6379> get foo  
    "123"
    redis 127.0.0.1:6379> exit


Redis服务器的端口默认是6379，但是你会发现Redis服务会一直占用我们当前登录Linux的SESSION，那能否像Mysql或者是MongoDB一样在后台执行Redis进程呢，当然可以，我们只需要更改Redis的配置文件，并且启动的时候指定配置文件即可！

如果是一个专业的DBA，那么实例启动时会加很多参数以便是系统运行的非常稳定，这样就可能在启动时在Redis后面加一个参数，以指定配置文件的路径，就像mysql一样的读取启动配置文件的方式来启动数据库。
 
Redis的配置文件redis.conf里面都有什么：

    #是否作为守护进程运行  
    daemonize yes  
    #配置 pid 的存放路径及文件名,默认为当前路径下  
    pidfile redis.pid  
    #Redis 默认监听端口  
    port 6379  
    #客户端闲置多少秒后,断开连接  
    timeout 300  
    #日志显示级别  
    loglevel verbose  
    #指定日志输出的文件名,也可指定到标准输出端口  
    logfile stdout  
    #设置数据库的数量,默认连接的数据库是 0,可以通过 select N 来连接不同的数据库  
    databases 16  
    #保存数据到 disk 的策略  
    #当有一条 Keys 数据被改变是,900 秒刷新到 disk 一次  
    save 900 1  
    #当有 10 条 Keys 数据被改变时,300 秒刷新到 disk 一次  
    save 300 10  
    #当有 1w 条 keys 数据被改变时,60 秒刷新到 disk 一次  
    save 60 10000  
    #当 dump .rdb 数据库的时候是否压缩数据对象  
    rdbcompression yes  
    #dump 数据库的数据保存的文件名  
    dbfilename dump.rdb  
    #Redis 的工作目录  
    dir /home/falcon/redis-2.0.0/  
    ########### Replication #####################  
    #Redis 的复制配置  
    # slaveof <masterip> <masterport>  
    # masterauth <master-password>  
    ############## SECURITY ###########  
    # requirepass foobared  
    ############### LIMITS ##############  
    #最大客户端连接数  
    # maxclients 128  
    #最大内存使用率  
    # maxmemory <bytes>  
    ########## APPEND ONLY MODE #########  
    #是否开启日志功能  
    appendonly no  
    # 刷新日志到 disk 的规则  
    # appendfsync always  
    appendfsync everysec  
    # appendfsync no  
    ################ VIRTUAL MEMORY ###########  
    #是否开启 VM 功能  
    vm-enabled no  
    # vm-enabled yes  
    vm-swap-file logs/redis.swap  
    vm-max-memory 0  
    vm-page-size 32  
    vm-pages 134217728  
    vm-max-threads 4  
    ############# ADVANCED CONFIG ###############  
    glueoutputbuf yes  
    hash-max-zipmap-entries 64  
    hash-max-zipmap-value 512  
    #是否重置 Hash 表  
    activerehashing yes

可以看到第一条就是启动后台进程启动Redis服务，将其设置为yes，就会在后台运行Redis服务！

启动的时候来指定redis的配置文件

    $ /usr/local/redis/bin/redis-server /usr/local/redis/redis.conf

## 开机启动

### 1.配置
将以下代码存为redis,放到/etc/init.d/下面,注意修改相应的路径

    #
    # chkconfig: - 90 10
    # description: Redis is an open source, advanced key-value store. 
    #
    # processname: redis-server
    # config: /etc/redis.conf
    # pidfile: /var/run/redis.pid
     
    PATH=/usr/local/bin:/sbin:/usr/bin:/bin
     
    REDISPORT=6379
    EXEC=/usr/local/bin/redis-server
    REDIS_CLI=/usr/local/bin/redis-cli
     
    PIDFILE=/var/run/redis.pid
    CONF="/etc/redis.conf"
     
    case "$1" in
        start)
            if [ -f $PIDFILE ]
            then
                    echo -n "$PIDFILE exists, process is already running or crashed\n"
            else
                    echo -n "Starting Redis server...\n"
                    $EXEC $CONF
            fi
            ;;
        stop)
            if [ ! -f $PIDFILE ]
            then
                    echo -n "$PIDFILE does not exist, process is not running\n"
            else
            PID=$(cat $PIDFILE)
                    echo -n "Stopping ...\n"
                    $REDIS_CLI -p $REDISPORT SHUTDOWN
                    while [ -x ${PIDFILE} ]
                    do
                        echo "Waiting for Redis to shutdown ..."
                        sleep 1
                    done
                    echo "Redis stopped"
            fi
            ;;
    esac

### 2.修改配置文件权限

    $ chmod a+x /etc/init.d/redis

### 3.设定开机启动

    $ chkconfig redis on

### 4.启动，停止redis服务

    $ service redis start   #或者 /etc/init.d/redis start  
    $ service redis stop    #或者 /etc/init.d/redis stop 


[1]:    http://redis.io "redis"
[2]:    http://redis.io/download "redis download"