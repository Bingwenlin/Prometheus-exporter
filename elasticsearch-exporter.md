### elasticsearch-exporter
```
# 下载
wget https://github.com/justwatchcom/elasticsearch_exporter/releases/download/v1.1.0/elasticsearch_exporter-1.1.0.linux-amd64.tar.gz
# 解压
tar -xvf elasticsearch_exporter-1.1.0.linux-amd64.tar.gz
cd elasticsearch_exporter-1.1.0.linux-amd64/
```
- 1、常用参数
```
## 参数说明：
--es.uri         　　　　默认http://localhost:9200，连接到的Elasticsearch节点的地址（主机和端口）。 这可以是本地节点（例如localhost：9200），也可以是远程Elasticsearch服务器的地址
--es.all                默认flase，如果为true，则查询群集中所有节点的统计信息，而不仅仅是查询我们连接到的节点。
--es.cluster_settings   默认flase，如果为true，请在统计信息中查询集群设置
--es.indices            默认flase，如果为true，则查询统计信息以获取集群中的所有索引。
--es.indices_settings   默认flase，如果为true，则查询集群中所有索引的设置统计信息。
--es.shards             默认flase，如果为true，则查询集群中所有索引的统计信息，包括分片级统计信息（意味着es.indices = true）。
--es.snapshots          默认flase，如果为true，则查询集群快照的统计信息。
```

- 2、只要设置不同的-web.listen-address监听端口，可启动多个实例，分别监控不同的ES集群：
```
# es集群1：10.xxx.xxx.10:9200
nohup ./elasticsearch_exporter --es.all --es.indices --es.cluster_settings --es.indices_settings --es.shards --es.snapshots --es.timeout=10s --web.listen-address=":9114" --web.telemetry-path="/metrics" --es.uri http://10.xxx.xxx.10:9200 &
 
# es集群2：10.xxx.xxx.11:9200
nohup ./elasticsearch_exporter --es.all --es.indices --es.cluster_settings --es.indices_settings --es.shards --es.snapshots --es.timeout=10s --web.listen-address=":9115" --web.telemetry-path="/metrics" --es.uri http://10.xxx.xxx.11:9200 &
```

- 3、如果es集群开启了x-pack验证则可以使用如下：
```
nohup ./elasticsearch_exporter --es.all --es.indices --es.cluster_settings --es.indices_settings --es.shards --es.snapshots  --es.uri http://用户名:密码@111.xxx.xxx.6:9200 &
```
