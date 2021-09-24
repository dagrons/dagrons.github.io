<!--
.. title: redis初探
.. slug: redischu-tan
.. date: 2021-09-23 22:36:52 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

## redis简介

- redis是单进程, 单线程, 所以具有很好的同步能力
- redis所有操作都是原子性的, 单个操作是原子性的, 多个操作也支持事务(通过MULTI和EXEC指令), 乍一看redis支持的数据类型也不特殊, 而其灵活性就来源于redis的操作原子性, 借助这个特性我们可以轻易实现分布式锁
- redis是分布式通信的绝佳手段

## redis支持的数据类型

- string: 最基础的数据类型, 二进制安全字符串, 存储图片等二进制格式需要存储base64编码
- hash: 哈希
- list: 列表
- set: 集合
- zset(sorted set): 有序集合


## 悲观锁实现

```python
from redis import StrictRedis 

redis_client = StrictRedis(decode_response=True) # StrictRedis 接口和命令行接口兼容

while True:
    order_lock = redis_client.setnx('lock:order', 1)
    redis_client.expire('lock:order', 2) # 设置过期时间防止死锁
    if order_lock:
        reserve_count = redis_client.get('reserve_count')
        if int(reserve_count) > 0:
            redis_client.decr('reserve_count')
            print ('已下单')
        else:
            print ('已告罄')
        redis_client.delete('lock:order')
        break
```

## 乐观锁实现
```python
from redis import StrictRedis, WatchError

redis_client = StrictRedis(host='127.0.0.1',port=6379,db=0,)
pipe = redis_client.pipeline() # 管道用于打包多条无关命令批量执行, 用来提升性能

while True:
    try:        
        pipe.watch('reserve_count')        
        count = pipe.get('reserve_count')
        if int(count) > 0: # 有库存, 让库存 -1            
            pipe.multi()            
            pipe.decr('reserve_count')            
            pipe.execute() # 会执行之前的所有命令
            print('已下单')
        else: 
            print('已售罄')            
            pipe.reset()
        break
    except WatchError as e:  # 出现该异常, 说明监听的数据被修改了, 重试/取消
        print('重试')
```


## redis cheathsheet

[cheatsheet](https://www.runoob.com/w3cnote/python-redis-intro.html)

