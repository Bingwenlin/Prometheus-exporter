# clickhouse-exporter
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