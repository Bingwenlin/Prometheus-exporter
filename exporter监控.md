## linux node监控安装
下载链接：https://github.com/Bingwenlin/Bingwenlin/blob/master/percona.tar.gz

```
wget https://github.com/Bingwenlin/Bingwenlin/blob/master/percona.tar.gz

tar -zxvf percona.tar.gz && cd percona/pmm-client/

cp percona/pmm-client/pmm-linux-metrics-42000.service /system.slice/

systemctl start pmm-linux-metrics-42000.service
```
## 阿里云mysql、redis指标监控
####  aliyun-exporter 的配置文件
```
# mkdir -p /data/aliyun-exporter/
# cd /data/aliyun-exporter/
// 编辑 aliyun-exporter 的配置文件，更改 access_key_id、access_key_secret、region_id 的值
# vim rds.yml
credential:
  access_key_id: 更改为你的 Access Key ID
  access_key_secret: 更改为你的 Access Key Secret
  region_id: 更改为你的 region_id

metrics:
  rds_performance:
  - name: MySQL_QPSTPS
  - name: MySQL_NetworkTraffic
  - name: MySQL_Sessions
  - name: MySQL_InnoDBBufferRatio
  - name: MySQL_InnoDBDataReadWriten
  - name: MySQL_InnoDBLogRequests
  - name: MySQL_InnoDBLogWrites
  - name: MySQL_TempDiskTableCreates
  - name: MySQL_MyISAMKeyBufferRatio
  - name: MySQL_MyISAMKeyReadWrites
  - name: MySQL_COMDML
  - name: MySQL_RowDML
  - name: MySQL_MemCpuUsage
  - name: MySQL_IOPS
  - name: MySQL_DetailedSpaceUsage
  - name: MySQL_CPS
  
```

## 通过 docker 方式启动 aliyun-exporter ，端口定义为 9525
```
	 docker run -d -p 9525:9525 -v /data/aliyun-exporter/rds.yml:/usr/src/app/aliyun-exporter.yml aylei/aliyun-exporter:0.3.1 -c /usr/src/app/aliyun-exporter.yml
```

## 通过 curl 请求 localhost:9525/metrics 可以看到监控数据
```
	curl -s localhost:9525/metrics | grep QPSTPS | head -n 3

# HELP aliyun_rds_performance_MySQL_QPSTPS_QPS 
# TYPE aliyun_rds_performance_MySQL_QPSTPS_QPS gauge
aliyun_rds_performance_MySQL_QPSTPS_QPS{instanceId="rm-wz9h42ku9o21xt1m2"} 14.75
```

## stackdriver_exporter
https://github.com/prometheus-community/stackdriver_exporter#google-stackdriver-prometheus-exporter
```
#下载二进制包 
wget https://github.com/prometheus-community/stackdriver_exporter/releases/download/v0.12.0/stackdriver_exporter-0.12.0.linux-amd64.tar.gz 

tar -xf stackdriver_exporter-0.12.0.linux-amd64.tar.gz 

cd stackdriver_exporter-0.12.0.linux-amd64
```
## 启动方式
 - 前期添加服务账号认证权限
```
vim /etc/environment 
GOOGLE_APPLICATION_CREDENTIALS=/opt/gcp/stackdriver_exporter-0.12.0.linux-amd64/shijian-345509-2eb937098cca.json


执行env看环境变量是否生效
```

## 启动
```
./stackdriver_exporter   --google.project-id shijian-345509   --monitoring.metrics-type-prefixes "compute.googleapis.com/instance/cpu,compute.googleapis.com/instance/disk"
```


# cloudwatch_exporter

[[github]]https://github.com/ivx/yet-another-cloudwatch-exporter

### 下载二进制文件yace
```
wget https://github.com/ivx/yet-another-cloudwatch-exporter/releases/download/v0.27.0-alpha/yet-another-cloudwatch-exporter_0.27.0-alpha_Linux_x86_64.tar.gz

```
### 执行yace
```
./yace

```
### 配置config.yml文件
```
discovery:
  jobs:
  - type: ec
    regions:
      - us-west-2
    searchTags:
      - Key: name
        Value: shoplus-redis-primary
    metrics:
      - name: CPUUtilization
        statistics:
        - Average
        period: 600
        length: 60
      - name: FreeableMemory
        statistics:
        - Average
        period: 600
        length: 60
      - name: SwapUsage
        statistics:
        - Average
        period: 600
        length: 60
      - name: NetworkBytesIn
        statistics:
        - Average
        period: 600
        length: 60
      - name: NetworkBytesOut
        statistics:
        - Average
        period: 600
        length: 60
      - name: CurrConnections
        statistics:
        - Average
        period: 600
        length: 60
      - name: Evictions
        statistics:
        - Average
        period: 600
        length: 60
      - name: Reclaimed
        statistics:
        - Average
        period: 600
        length: 60
      - name: CacheHits
        statistics:
        - Average
        period: 600
        length: 60
      - name: CacheHits
        statistics:
        - Average
        period: 600
        length: 60
      - name: ReplicationBytes
        statistics:
        - Average
        period: 600
        length: 60
      - name: ReplicationLag
        statistics:
        - Average
        period: 600
        length: 60
      - name: BytesUsedForCache
        statistics:
        - Average
        period: 600
        length: 60
      - name: CurrItems
        statistics:
        - Average
        period: 600
        length: 60
      - name: CasHits
        statistics:
        - Average
        period: 600
        length: 60
      - name: DatabaseMemoryUsagePercentage
        statistics:
        - Average
        period: 600
        length: 60
      - name: CasMisses
        statistics:
        - Average
        period: 600
        length: 60
  - type: ec
    regions:
      - us-west-2
    searchTags:
      - Key: name
        Value: shoplus-redis-replica
    metrics:
      - name: CPUUtilization
        statistics:
        - Average
        period: 600
        length: 60
      - name: DatabaseMemoryUsagePercentage
        statistics:
        - Average
        period: 600
        length: 60
      - name: FreeableMemory
        statistics:
        - Average
        period: 600
        length: 60
      - name: SwapUsage
        statistics:
        - Average
        period: 600
        length: 60
      - name: NetworkBytesIn
        statistics:
        - Average
        period: 600
        length: 60
      - name: NetworkBytesOut
        statistics:
        - Average
        period: 600
        length: 60
      - name: CurrConnections
        statistics:
        - Average
        period: 600
        length: 60
      - name: Evictions
        statistics:
        - Average
        period: 600
        length: 60
      - name: Reclaimed
        statistics:
        - Average
        period: 600
        length: 60
      - name: CacheHits
        statistics:
        - Average
        period: 600
        length: 60
      - name: CacheHits
        statistics:
        - Average
        period: 600
        length: 60
      - name: ReplicationBytes
        statistics:
        - Average
        period: 600
        length: 60
      - name: ReplicationLag
        statistics:
        - Average
        period: 600
        length: 60
      - name: BytesUsedForCache
        statistics:
        - Average
        period: 600
        length: 60
      - name: CurrItems
        statistics:
        - Average
        period: 600
        length: 60
      - name: CasHits
        statistics:
        - Average
        period: 600
        length: 60
      - name: CasMisses
        statistics:
        - Average
        period: 600
        length: 60
  - type: rds
    regions:
      - us-west-2
    searchTags:
      - Key: name
        Value: shoplus-mysql
    metrics:
      - name: CPUUtilization
        statistics:
        - Average
        period: 600
        length: 60
      - name: DatabaseConnections
        statistics:
        - Average
        period: 600
        length: 60
      - name: FreeStorageSpace
        statistics:
        - Average
        period: 600
        length: 60
      - name: FreeableMemory
        statistics:
        - Average
        period: 600
        length: 60
      - name: ReadThroughput
        statistics:
        - Average
        period: 600
        length: 60
      - name: ReadIOPS
        statistics:
        - Average
        period: 600
        length: 60
      - name: WriteLatency
        statistics:
        - Average
        period: 600
        length: 60
      - name: WriteThroughput
        statistics:
        - Average
        period: 600
        length: 60
      - name: WriteIOPS
        statistics:
        - Average
        period: 600
        length: 60
  - type: rds
    regions:
      - us-west-2
    searchTags:
      - Key: name
        Value: shoplus-mysql-r
    metrics:
      - name: CPUUtilization
        statistics:
        - Average
        period: 600
        length: 60
      - name: DatabaseConnections
        statistics:
        - Average
        period: 600
        length: 60
      - name: FreeStorageSpace
        statistics:
        - Average
        period: 600
        length: 60
      - name: FreeableMemory
        statistics:
        - Average
        period: 600
        length: 60
      - name: ReadThroughput
        statistics:
        - Average
        period: 600
        length: 60
      - name: ReadIOPS
        statistics:
        - Average
        period: 600
        length: 60
      - name: WriteLatency
        statistics:
        - Average
        period: 600
        length: 60
      - name: WriteThroughput
        statistics:
        - Average
        period: 600
        length: 60
      - name: WriteIOPS
        statistics:
        - Average
        period: 600
        length: 60
  - type: rds
    regions:
      - us-west-2
    searchTags:
      - Key: name
        Value: shoplus-data-analyze
    metrics:
      - name: CPUUtilization
        statistics:
        - Average
        period: 600
        length: 60
      - name: DatabaseConnections
        statistics:
        - Average
        period: 600
        length: 60
      - name: FreeStorageSpace
        statistics:
        - Average
        period: 600
        length: 60
      - name: FreeableMemory
        statistics:
        - Average
        period: 600
        length: 60
      - name: ReadThroughput
        statistics:
        - Average
        period: 600
        length: 60
      - name: ReadIOPS
        statistics:
        - Average
        period: 600
        length: 60
      - name: WriteLatency
        statistics:
        - Average
        period: 600
        length: 60
      - name: WriteThroughput
        statistics:
        - Average
        period: 600
        length: 60
      - name: WriteIOPS
        statistics:
        - Average
        period: 600
        length: 60
  - type: cloudfront
    regions:
      - us-east-1
    metrics:
      - name: Requests
        statistics:
        - Sum
        period: 600
        length: 300
      - name: TotalErrorRate
        statistics:
        - Average
        period: 600
        length: 300
      - name: 4xxErrorRate
        statistics:
        - Average
        period: 600
        length: 300
      - name: 5xxErrorRate
        statistics:
        - Average
        period: 600
        length: 300

```
### 第二种配置config.yml文件
```
discovery:
  jobs:
  - type: ec
    regions:
      - us-west-2
    metrics:
      - name: CPUUtilization
        statistics:
        - Average
        period: 600
        length: 300
      - name: FreeableMemory
        statistics:
        - Average
        period: 600
        length: 300
      - name: SwapUsage
        statistics:
        - Average
        period: 600
        length: 300
      - name: NetworkBytesIn
        statistics:
        - Average
        period: 600
        length: 300
      - name: NetworkBytesOut
        statistics:
        - Average
        period: 600
        length: 300
      - name: CurrConnections
        statistics:
        - Average
        period: 600
        length: 300
      - name: Evictions
        statistics:
        - Average
        period: 600
        length: 300
      - name: Reclaimed
        statistics:
        - Average
        period: 600
        length: 300
      - name: CacheHits
        statistics:
        - Average
        period: 600
        length: 300
      - name: CacheHits
        statistics:
        - Average
        period: 600
        length: 300
      - name: ReplicationBytes
        statistics:
        - Average
        period: 600
        length: 300
      - name: ReplicationLag
        statistics:
        - Average
        period: 600
        length: 300
      - name: BytesUsedForCache
        statistics:
        - Average
        period: 600
        length: 300
      - name: CurrItems
        statistics:
        - Average
        period: 600
        length: 300
      - name: CasHits
        statistics:
        - Average
        period: 600
        length: 300
      - name: DatabaseMemoryUsagePercentage
        statistics:
        - Average
        period: 600
        length: 300
      - name: CasMisses
        statistics:
        - Average
        period: 600
        length: 300
  - type: rds
    regions:
      - us-west-2
    metrics:
      - name: CPUUtilization
        statistics:
        - Average
        period: 600
        length: 300
      - name: DatabaseConnections
        statistics:
        - Average
        period: 600
        length: 300
      - name: FreeStorageSpace
        statistics:
        - Average
        period: 600
        length: 300
      - name: FreeableMemory
        statistics:
        - Average
        period: 600
        length: 300
      - name: ReadThroughput
        statistics:
        - Average
        period: 600
        length: 300
      - name: ReadIOPS
        statistics:
        - Average
        period: 600
        length: 300
      - name: WriteLatency
        statistics:
        - Average
        period: 600
        length: 300
      - name: WriteThroughput
        statistics:
        - Average
        period: 600
        length: 300
      - name: WriteIOPS
        statistics:
        - Average
        period: 600
        length: 300
      - name: ReplicaLag
        statistics:
        - Average
        period: 600
        length: 300
  - type: cloudfront
    regions:
      - us-east-1
    metrics:
      - name: Requests
        statistics:
        - Sum
        period: 600
        length: 300
      - name: TotalErrorRate
        statistics:
        - Average
        period: 600
        length: 300
      - name: 4xxErrorRate
        statistics:
        - Average
        period: 600
        length: 300
      - name: 5xxErrorRate
        statistics:
        - Average
        period: 600
        length: 300
      - name: BytesDownloaded
        statistics:
        - Sum
        period: 600
        length: 300
      - name: BytesUploaded
        statistics:
        - Sum
        period: 600
        length: 300

```


## clickhouse-exporter
地址：[[https://github.com/ClickHouse/clickhouse_exporter/blob/master/README.md]]
```
git clone //github.com/ClickHouse/clickhouse_exporter  
cd clickhouse_exporter  
go mod init  
go mod vendor  
go build   
ls ./clickhouse_exporter  
```

### 配置数据库账号密码
```
vim /etc/profile

#加入数据库账号密码
export CLICKHOUSE_USER="itsm_sqladmin" 
export CLICKHOUSE_PASSWORD="S7gXsScXnAEU5i39" 

source /etc/profile

#默认端口9116
nohup ./clickhouse_exporter -scrape_uri=http://cc-wz96xnag709788zx2o.ads.rds.aliyuncs.com:8123/ >nohup.log 2>&1 &

#指定端口
nohup ./clickhouse_exporter -telemetry.address=:9111 -scrape_uri=http://172.27.33.180:3002/ >nohup.log 2>&1 &
```

### 检验指标采集
```
curl localhost:9116/metrics | head 

```
