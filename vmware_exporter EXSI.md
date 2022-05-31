# vmware_exporter EXSI

##### 二进制部署
```shell
yum -y install python36 python36-devel
yum -y install gcc libffi-devel python-devel openssl-devel
mkdir -p /opt/prometheus
cd /opt/prometheus
wget https://github.com/pryorda/vmware_exporter/archive/v0.7.0.tar.gz
tar -xzvf v0.7.0.tar.gz 
mv vmware_exporter-0.7.0 vmware_exporter
cd vmware_exporter/
python36 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install cryptography
python setup.py install
whereis vmware_exporter 

cat > /etc/systemd/system/vmware_exporter.service <<'EOF'
[Unit]
Description=Prometheus VMWare Exporter
After=network.target
[Service]
User=prometheus
Group=prometheus
WorkingDirectory=/opt/prometheus/vmware_exporter/
ExecStart=/opt/prometheus/vmware_exporter/venv/bin/vmware_exporter -c /etc/vmware_exporter/config.yml
Type=simple
[Install]
WantedBy=multi-user.target
EOF

systemctl start vmware_exporter
systemctl status vmware_exporter

```

##### docker部署
```shell
mkdir -p /data/vmware/
vim /data/vmware/config.yml

default:
    vsphere_host: "vcenter"
    vsphere_user: "user"
    vsphere_password: "password"
    ignore_ssl: False
    collect_only:
        vms: True
        vmguests: True
        datastores: True
        hosts: True
        snapshots: True

```

##### 执行vmware-exporter docker程序
```
docker run -d   -v /data/vmware:/data/ -it --rm  -p 9272:9272 --name vmware_exporter pryorda/vmware_exporter   -c /data/vmware/config.yml
```

##### 编译prometheus配置文件
 - 编辑esx.yml文件，以便prometheus中sd_file配置文件使用
 ```shell
 vim /etc/prometheus/esx.yml
 
---
- targets:
  - 192.168.21.91
  - 192.168.21.92
  - 192.168.21.93
  - 192.168.21.95
  - 192.168.21.96
  labels:
    job: vmware_esx
 ```
 
 - 编辑prometheus文件，其中section：[esx] 对应config.yml文件中的ESX片段
 ```shell
 vim /etc/prometheus/prometheus.yml
 
   - job_name: 'vmware_vcenter'
    metrics_path: '/metrics'
    static_configs:
      - targets:
        - 'vcenter.company.com'
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: localhost:9272

  - job_name: 'vmware_esx'
    metrics_path: '/metrics'
    file_sd_configs:
      - files:
        - /etc/prometheus/esx.yml
    params:
      section: [esx]
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: localhost:9272

 ```
