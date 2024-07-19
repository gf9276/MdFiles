# ElasticSearch & Kibana

# docker 安装 ElasticSearch

小心：`http.host: 0.0.0.0` 冒号后面要跟一个空格

```shell
mkdir -p /data1/guof/DockerVolume/gulimall/elasticsearch/config
mkdir -p /data1/guof/DockerVolume/gulimall/elasticsearch/data
mkdir -p /data1/guof/DockerVolume/gulimall/elasticsearch/plugins
echo "http.host: 0.0.0.0" >> /data1/guof/DockerVolume/gulimall/elasticsearch/config/elasticsearch.yml
```

docker 的用户权限管理真是混乱

```shell
chmod -R 777 /data1/guof/DockerVolume/gulimall/elasticsearch
```


```shell
docker run --name elasticsearch \
--restart=always \
-p 20102:9200 \
-p 20103:9300 \
-e "discovery.type=single-node" \
-e "ES_JAVA_OPTS=-Xms512m -Xmx512m" \
-v /data1/guof/DockerVolume/gulimall/elasticsearch/config/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml \
-v /data1/guof/DockerVolume/gulimall/elasticsearch/data:/usr/share/elasticsearch/data \
-v /data1/guof/DockerVolume/gulimall/elasticsearch/plugins:/usr/share/elasticsearch/plugins \
-d elasticsearch:7.4.2
```

# docker 安装 Kibana

```shell
docker run --name kibana \
-e ELASTICSEARCH_HOSTS=http://10.68.107.228:20102 \
-p 20104:5601 \
-d kibana:7.4.2
```

# Java REST Client API

已经被抛弃了

(官方文档，已经被抛弃了)[https://www.elastic.co/guide/en/elasticsearch/client/java-rest/7.4/java-rest-high-search.html#_source_filtering]