# 使用Node Exporter采集主机数据

 ### 下载二禁止文件
```
curl -OL https://github.com/prometheus/node_exporter/releases/download/v0.15.2/node_exporter-0.15.2.darwin-amd64.tar.gz
tar -xzf node_exporter-0.15.2.darwin-amd64.tar.gz

```
### 运行node exporter
```
cd node_exporter-0.15.2.darwin-amd64

cp node_exporter-0.15.2.darwin-amd64/node_exporter /usr/local/bin/

./node_exporter


nohup ./node_exporter > nohup.log 2>&1 & 
```
### 访问地址
```
	http://localhost:9100/metrics

```