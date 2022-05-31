# consul server + consul_exporter 配置
### 下载文件
```
mkdir -p /data/tarballs /data/database/consul
cd /data/tarballs
wget https://releases.hashicorp.com/consul/1.5.3/consul_1.5.3_linux_amd64.zip
 wget https://github.com/prometheus/consul_exporter/releases/download/v0.5.0/consul_exporter-0.5.0.linux-amd64.tar.gz
unzip consul_1.5.3_linux_amd64.zip
mv consul /usr/local/bin/consul
tar -xvzf consul_exporter-0.5.0.linux-amd64.tar.gz
mv consul_exporter /usr/local/bin/consul_exporter

```
### systemd创建consul配置
```
cat << EOF > /etc/systemd/system/consul.service
[Unit]
Description="HashiCorp Consul - A service mesh solution"
Documentation=https://www.consul.io/
Requires=network-online.target
After=network-online.target
ConditionFileNotEmpty=/etc/consul.d/consul.hcl
 
[Service]
User=consul
Group=consul
ExecStart=/usr/local/bin/consul agent -config-dir=/etc/consul.d/
ExecReload=/usr/local/bin/consul reload
KillMode=process
Restart=on-failure
LimitNOFILE=65536
 
[Install]
WantedBy=multi-user.target
EOF

```
### systemd创建consul_exporter配置
```
cat <<EOF > /etc/systemd/system/consul_exporter.service
[Unit]
Description=Consul Exporter
Wants=network-online.target
After=network-online.target
[Service]
User=consul_exporter
Group=consul_exporter
Type=simple
ExecStart=/usr/local/bin/consul_exporter
[Install]
WantedBy=multi-user.target
EOF

```
### 创建用户并授权
```
useradd --system --home /etc/consul.d --shell /bin/false consul
useradd --no-create-home --shell /bin/false consul_exporter
chown consul_exporter:consul_exporter /usr/local/bin/consul_exporter
chown consul.consul /data/database/consul

#修改配置文件
cat <<EOF > /etc/consul.d/consul.hcl
{
    "bootstrap_expect": 5,
    "client_addr": "0.0.0.0",
    "datacenter": "此处应该修改idc名称",
    "data_dir": "/data/database/consul",
    "domain": "consul",
    "enable_script_checks": true,
    "dns_config": {
        "enable_truncate": true,
        "only_passing": true
    },
    "enable_syslog": true,
    "encrypt": "此处应该修改为新的密码 可以用 consul keygen生成",
    "leave_on_terminate": true,
    "log_level": "INFO",
    "rejoin_after_leave": true,
    "server": true,
    "start_join": [
        "此处应该修改对应的ip列表，最后一个注意不要多逗号",
    ],
    "ui": true,
    "telemetry": {
        "prometheus_retention_time": "2s"
    }
}
EOF
 
#启动
systemctl restart consul
systemctl enable consul
systemctl status consul
 
systemctl restart consul_exporter
systemctl enable consul_exporter
systemctl status consul_exporter

```