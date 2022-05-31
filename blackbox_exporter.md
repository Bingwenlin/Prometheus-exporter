### github地址
https://github.com/prometheus/blackbox_exporter

### 下载
```

# 下载
wget https://github.com/prometheus/blackbox_exporter/releases/download/v0.18.0/blackbox_exporter-0.18.0.darwin-amd64.tar.gz
# 解压
tar -zxvf blackbox_exporter-0.18.0.darwin-amd64.tar.gz
# 重命名
mv blackbox_exporter-0.18.0.darwin-amd64/ blackbox_exporter
# 进入 blackbox_exporter 目录
cd blackbox_exporter
```

### 2、编写 blackbox.yml 配置文件
```yml
modules:
  http_2xx:
    prober: http
    http:
      method: GET
      preferred_ip_protocol: "ip4"
  http_post_2xx:
    prober: http
    http:
      method: POST
      preferred_ip_protocol: "ip4"
  tcp:
    prober: tcp
  ping:
    prober: icmp
    timeout: 3s
    icmp:
      preferred_ip_protocol: "ip4"
  dns:
    prober: dns
    timeout: 5s
    dns:
      query_name: "pnginx01.hk.batmobi.cn"
      query_type: "A"
      preferred_ip_protocol: "ip4"
```

### 3、启动 blackbox_exporter
```
#!/usr/bin

nohup /Users/huan/soft/prometheus/blackbox_exporter/blackbox_exporter \
--config.file="/Users/huan/soft/prometheus/blackbox_exporter/blackbox.yml" \
--web.listen-address=":9098" \
--log.level=debug \
> logs/blackbox.out 2>&1 &
```

### 4、和 prometheus 集成
```yaml
scrape_configs:
  - job_name: 'blackbox_http_2xx' # 配置get请求检测
    scrape_interval: 30s
    metrics_path: /probe
    params:
      module: [http_2xx]
    static_configs:
      - targets:         # 测试如下的请求是否可以访问的通
        - 127.0.0.1:10005
        - http://127.0.0.1:10005/hello/zhangsan
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: 127.0.0.1:9098 # blackbox-exporter 服务所在的机器和端口
  - job_name: 'blackbox_http_post_2xx' # 配置post请求检测
    scrape_interval: 30s
    metrics_path: /probe
    params:
      module: [http_post_2xx]
    static_configs:
      - targets:              # 测试如下的post请求是否可以访问的通，该post请求不带参数
        - 127.0.0.1:10005
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: 127.0.0.1:9098 # blackbox-exporter 服务所在的机器和端口
  - job_name: 'blackbox_http_ping' # 检测是否可以ping通某些机器
    scrape_interval: 30s
    metrics_path: /probe
    params:
      module: [icmp]
    static_configs:
      - targets:
        - 127.0.0.1
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: 127.0.0.1:9098 # blackbox-exporter 服务所在的机器和端口
  - job_name: 'blackbox_tcp_connect' # 检测某些端口是否在线
    scrape_interval: 30s
    metrics_path: /probe
    params:
      module: [tcp_connect]
    static_configs:
      - targets:
        - 127.0.0.1:10006
        - 127.0.0.1:10005
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: 127.0.0.1:9098 # blackbox-exporter 服务所在的机器和端口
```


### k8s 部署配置文件
```yaml
kind: ConfigMap
apiVersion: v1
metadata:
  name: blackbox-exporter-config
data:
  blackbox.yaml: |
    modules:
      http_2xx:
        http:
          follow_redirects: true
          preferred_ip_protocol: ip4
          valid_http_versions:
          - HTTP/1.1
          - HTTP/2.0
        prober: http
        timeout: 5s
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: blackbox-exporter
  labels:
    app: blackbox-exporter
spec:
  selector:
    matchLabels:
      app: blackbox-exporter
  template:
    metadata:
      name: blackbox-exporter
      labels:
        app: blackbox-exporter
    spec:
      containers:
      - args:
        - --config.file=/config/blackbox.yaml
        image: prom/blackbox-exporter
        name: blackbox-exporter
        imagePullPolicy: IfNotPresent
        livenessProbe:
          failureThreshold: 3
          httpGet:
            path: /health
            port: http
            scheme: HTTP
          periodSeconds: 10
          successThreshold: 1
          timeoutSeconds: 1
        readinessProbe:
          failureThreshold: 3
          httpGet:
            path: /health
            port: http
            scheme: HTTP
          periodSeconds: 10
          successThreshold: 1
          timeoutSeconds: 1
        ports:
        - containerPort: 9115
          name: http
          protocol: TCP
        volumeMounts:
        - mountPath: /config
          name: config
      volumes:
      - configMap:
          defaultMode: 420
          name: blackbox-exporter-config
        name: config
---
apiVersion: v1
kind: Service
metadata:
  name: blackbox-exporter
  labels:
    app: blackbox-exporter
spec:
  ports:
    - port: 9115
      protocol: TCP
  selector:
    app: blackbox-exporter

```

### prometheus job配置
```
- job_name: blackbox-ssl-hk
  metrics_path: /probe
  params:
    module: [http_2xx]
  file_sd_configs:
  - files:
    - /etc/batmobi/prometheus/json/blackbox-ssl-hk.json
  relabel_configs:
    - source_labels: [domain]
      target_label: __param_target
    - target_label: instance
      replacement: blackbox-ssl-hk8s

```

### json文件执行`/etc/batmobi/prometheus/json/blackbox-ssl-hk.json`
```json
[
    {
        "labels": {
            "domain": "https://static.shoplus.net"
        },
        "targets": [
            "10.253.9.81:9115"
        ]
    },
    {
        "labels": {
            "domain": "https://resare.vipshopbuy.com"
        },
        "targets": [
            "10.253.9.81:9115"
        ]
    },
    {
        "labels": {
            "domain": "https://bruse-myshoplus.algobuy.net"
        },
        "targets": [
            "10.253.9.81:9115"
        ]
    }
]
```