<!-- TOC -->

- [1. redis](#1-redis)

<!-- /TOC -->

# 1. redis

这个是 gulimall 里的教程，有点问题啊

```
docker run -p 6379:6379 --name nlogging_redis \
--restart=always \
-v /data1/guof/DockerVolume/nLogging/redis/data:/data \
-v /data1/guof/DockerVolume/nLogging/redis/conf/redis.conf:/etc/redis/redis.conf \
-d redis redis-server /etc/redis/redis.conf
```

```
docker run -p 6379:6379 --name redis \
--restart=always \
-v /data/redis/data:/data \
-v /data/redis/conf/redis.conf:/etc/redis/redis.conf \
-d redis redis-server /etc/redis/redis.conf
```

下面这个没有问题

```
docker run -p 20100:6379 --name gulimall_redis \
--restart=always \
-v /data1/guof/DockerVolume/gulimall/redis/data:/data \
-v /data1/guof/DockerVolume/gulimall/redis/conf:/usr/local/etc/redis \
-d redis redis-server /usr/local/etc/redis/redis.conf
```

```
docker run -p 6379:6379 --name redis \
--restart=always \
-v /data/redis/data:/data \
-v /data/redis/conf:/usr/local/etc/redis \
-d redis redis-server /usr/local/etc/redis/redis.conf
```

```
cd /data1/guof/DockerVolume/nLogging/redis/conf
wget https://raw.githubusercontent.com/antirez/redis/4.0/redis.conf
```

[自描述文件](https://raw.githubusercontent.com/antirez/redis/4.0/redis.conf)

[可视化软件](https://redis.tinycraft.cc/zh/)

连不上怎么办？

改两个地方

```
# bind 127.0.0.1
protected-mode no
```
