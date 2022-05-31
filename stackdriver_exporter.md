### stackdriver_exporter
https://github.com/prometheus-community/stackdriver_exporter#google-stackdriver-prometheus-exporter
```
#下载二进制包 
wget https://github.com/prometheus-community/stackdriver_exporter/releases/download/v0.12.0/stackdriver_exporter-0.12.0.linux-amd64.tar.gz 

tar -xf stackdriver_exporter-0.12.0.linux-amd64.tar.gz 

cd stackdriver_exporter-0.12.0.linux-amd64
```
### 启动方式
 - 前期添加服务账号认证权限
```
vim /etc/environment 
GOOGLE_APPLICATION_CREDENTIALS=/opt/gcp/stackdriver_exporter-0.12.0.linux-amd64/shijian-345509-2eb937098cca.json


执行env看环境变量是否生效
```

### 启动
```
./stackdriver_exporter   --google.project-id shijian-345509   --monitoring.metrics-type-prefixes "compute.googleapis.com/instance/cpu,compute.googleapis.com/instance/disk"
```
