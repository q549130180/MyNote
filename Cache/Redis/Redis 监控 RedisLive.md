# Redis 监控 RedisLive

**目录：**

[TOC]



## 一、简介

Redis Live是一个用来监控redis实例，分析查询语句并且有web界面的监控工具，使用python编写。


## 二、环境准备


- OS:Cent OS 7.0
- Python:2.7.10



## 三、安装依赖

安装pip
easy_install pip

- pip install tornado
- pip install redis
- pip install python-dateutil
- pip install argparse

Redis Python Client
git clone https://github.com/andymccurdy/redis-py.git

To install redis-py, simply:

$ sudo pip install redis
or alternatively (you really should be using pip though):

$ sudo easy_install redis
or from source:

$ sudo python setup.py install
Getting Started

```python
>>> import redis
>>> r = redis.StrictRedis(host='localhost', port=6379, db=0)
>>> r.set('foo', 'bar')
True
>>> r.get('foo')
'bar'
```

## 四、安装部署

### 下载
使用git clone 或在Github下载RedisLive-master.zip
git clone https://github.com/nkrode/RedisLive.git


### 修改配置文件

cd src
mv redis-live.conf.example redis-live.conf
vim redis-live.conf


可以看出这个配置文件是json格式的，注意不要产生格式错误，对着原始的配置文件来
原始的配置文件如下：
```json
{
        "RedisServers":
        [
                {
                        "server": "154.17.59.99",
                        "port" : 6379
                },

                {
                        "server": "localhost",
                        "port" : 6380,
                        "password" : "some-password"
                }
        ],

        "DataStoreType" : "redis",

        "RedisStatsServer":
        {
                "server" : "ec2-184-72-166-144.compute-1.amazonaws.com",
                "port" : 6385
        },

        "SqliteStatsStore" :
        {
                "path":  "to your sql lite file"
        }
}
```

解析一下：
`RedisServers`: 就是所要监控的redis集群的所有主机，可以配置host, port, password，注意最后一个元素后面没有逗号。
`DataStoreType`: 就是类似元数据存储的类型，默认是redis，也可以是sqlite；
`RedisStatsServer`: 如果存储类型选择了reids，就需要配置此项，即另外拿出一个redis来存储其他redis的状态信息，也就是上面说的元数据。
`SqliteStatisStore`: 如果存储类型选择了sqlite，就配置此项，指定一个路径保存sqlite文件。
没仔细研究，估计是已经把sqlite文件包含在安装文件里头了。

**参考资料：**

[安装redis-live监控redis集群](http://blog.csdn.net/cxz_hijacker/article/details/16862389)
