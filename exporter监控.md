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
