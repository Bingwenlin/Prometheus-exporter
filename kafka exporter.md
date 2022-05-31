# kafka exporter

#下载二进制包
```
wget "https://github.com/danielqsj/kafka_exporter/releases/download/v1.2.0/kafka_exporter-1.2.0.linux-amd64.tar.gz"

```
#解压
```
[root@master kafka]# ls
kafka_exporter-1.2.0.linux-amd64.tar.gz

[root@master kafka]# tar -xzvf kafka_exporter-1.2.0.linux-amd64.tar.gz
[root@master kafka]# ls

kafka_exporter-1.2.0.linux-amd64  kafka_exporter-1.2.0.linux-amd64.tar.gz

```
#将kafka_exporter-1.2.0.linux-amd64 目錄下的 kafka_exporter 二進制文件複製到 /usr/local/bin 路徑下
```
[root@master kafka]# cd kafka_exporter-1.2.0.linux-amd64/
[root@master kafka_exporter-1.2.0.linux-amd64]# ls
kafka_exporter  LICENSE
[root@master kafka_exporter-1.2.0.linux-amd64]# cp kafka_exporter /usr/local/bin/
[root@master kafka_exporter-1.2.0.linux-amd64]# ls -l /usr/local/bin/kafka_exporter
-rwxr-xr-x. 1 root root 13578776 Oct 23 11:57 /usr/local/bin/kafka_exporter

```
#創建systemd service文件启动
```
cat <<EOF > /etc/systemd/system/kafka_exporter.service
[Unit]
Description=kafka_exporter
After=network.target 

[Service]
User=nobody
Group=nobody
Type=simple
ExecStart=/usr/local/bin/kafka_exporter\\
          --kafka.server=127.0.0.1:9092\\
          --web.listen-address=:9308

[Install]
WantedBy=multi-user.target
EOF

```
#重載系統 systemd 配置
```
systemctl daemon-reload
```
#开机自启
```
[root@master kafka_exporter-1.2.0.linux-amd64]# systemctl enable --now kafka_exporter
Created symlink from /etc/systemd/system/multi-user.target.wants/kafka_exporter.service to /etc/systemd/system/kafka_exporter.service.

```
#查看启动状态
```
[root@master kafka_exporter-1.2.0.linux-amd64]# systemctl status kafka_exporter
● kafka_exporter.service - kafka_exporter
   Loaded: loaded (/etc/systemd/system/kafka_exporter.service; enabled; vendor preset: disabled)
   Active: active (running) since Fri 2020-10-23 15:15:54 CST; 19s ago
 Main PID: 16549 (kafka_exporter)
    Tasks: 6
   Memory: 1.3M
   CGroup: /system.slice/kafka_exporter.service
           └─16549 /usr/local/bin/kafka_exporter --kafka.server=127.0.0.1:9092 --web.listen-address=:9308

Oct 23 15:15:54 master systemd[1]: Started kafka_exporter.
Oct 23 15:15:54 master kafka_exporter[16549]: time="2020-10-23T15:15:54+08:00" level=info msg="Starting kafka_exporter (version=1.2.0, branch=HEAD, revision=830660212e6c109e69dcb1cb58f5159fe3...porter.go:474"
Oct 23 15:15:54 master kafka_exporter[16549]: time="2020-10-23T15:15:54+08:00" level=info msg="Build context (go=go1.10.3, user=root@981cde178ac4, date=20180707-14:34:48)" source="kafka_exporter.go:475"
Oct 23 15:15:54 master kafka_exporter[16549]: time="2020-10-23T15:15:54+08:00" level=info msg="Done Init Clients" source="kafka_exporter.go:213"
Oct 23 15:15:54 master kafka_exporter[16549]: time="2020-10-23T15:15:54+08:00" level=info msg="Listening on :9308" source="kafka_exporter.go:499"
Hint: Some lines were ellipsized, use -l to show in full.

```
#测试接口
```
[root@master kafka_exporter-1.2.0.linux-amd64]# curl  -s 192.168.150.166:9308/metrics | grep kafka_brokers
# HELP kafka_brokers Number of Brokers in the Kafka Cluster.
# TYPE kafka_brokers gauge
kafka_brokers 1
```