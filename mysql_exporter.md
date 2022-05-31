### 需要先授权用户密码
```
GRANT SELECT, RELOAD, PROCESS, REPLICATION CLIENT ON *.* TO 'pmm'@'%' IDENTIFIED BY 'w-p2049,5MKx;EPw;0Rq';flush privileges;
GRANT SELECT ON `performance_schema`.* TO 'pmm'@'%' ;
```

### 监控安装包
![[percona.tar.gz]]

### 配置
- 复制文件到启动目录
```
cp pmm-mysql-metrics-42002.service /etc/systemd/system/
```

- 修改配置文件
```shell
vim /etc/systemd/system/pmm-mysql-metrics-42002.service

[Unit]
Description=PMM Prometheus mysqld_exporter 42002
ConditionFileIsExecutable=/usr/local/percona/pmm-client/mysqld_exporter
After=network.target
After=syslog.target

[Service]
StartLimitInterval=5
StartLimitBurst=10
ExecStart=/bin/sh -c '/usr/local/percona/pmm-client/mysqld_exporter -collect.auto_increment.columns=true -collect.binlog_size=true -collect.global_status=true -collect.global_variables=true -collect.info_schema.innodb_metrics=true -collect.info_schema.innodb_cmp=true -collect.info_schema.innodb_cmpmem=true -collect.info_schema.processlist=true -collect.info_schema.query_response_time=true -collect.info_schema.tables=true -collect.info_schema.tablestats=true -collect.info_schema.userstats=true -collect.perf_schema.eventswaits=true -collect.perf_schema.file_events=true -collect.perf_schema.indexiowaits=true -collect.perf_schema.tableiowaits=true -collect.perf_schema.tablelocks=true -collect.slave_status=true -web.listen-address=0.0.0.0:42002 -web.auth-file=/usr/local/percona/pmm-client/pmm.yml -web.ssl-cert-file=/usr/local/percona/pmm-client/server.crt -web.ssl-key-file=/usr/local/percona/pmm-client/server.key >> /var/log/pmm-mysql-metrics-42002.log 2>&1'


Environment="DATA_SOURCE_NAME=pmm:w-p2049,5MKx;EPw;0Rq@tcp(127.0.0.1:3306)/?parseTime=true&time_zone='%%2b00%%3a00'&loc=UTC"

Restart=always
RestartSec=120
EnvironmentFile=-/etc/sysconfig/pmm-mysql-metrics-42002

[Install]
WantedBy=multi-user.target

```


### 启动即可
```
systemctl start pmm-mysql-metrics-42002.service
```

