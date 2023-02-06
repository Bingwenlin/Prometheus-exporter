## linux node监控安装
下载链接：https://github.com/Bingwenlin/Bingwenlin/blob/master/percona.tar.gz

```
wget https://github.com/Bingwenlin/Bingwenlin/blob/master/percona.tar.gz

tar -zxvf percona.tar.gz && cd percona/pmm-client/

cp percona/pmm-client/pmm-linux-metrics-42000.service /etc/systemd/system/

systemctl start pmm-linux-metrics-42000.service
```
## linux mysql监控
```
wget https://github.com/Bingwenlin/Bingwenlin/blob/master/percona.tar.gz

tar -zxvf percona.tar.gz && cd percona/pmm-client/

cp percona/pmm-client/pmm-mysql-metrics-42002.service /etc/systemd/system/

systemctl start pmm-mysql-metrics-42002.service
```
### 欢迎更新其他指标监控

