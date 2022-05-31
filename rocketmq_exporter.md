### github rocketmq_exporter
https://github.com/apache/rocketmq-exporter
### 下载
```
wget https://github.com/apache/rocketmq-exporter/archive/refs/heads/master.zip

```
### 解压
```
unzip master.zip

cd rocketmq-exporter-master

```
### 构建二进制或docker
```
#二进制
mvn clean install


#docker

mvn package -Dmaven.test.skip=true docker:build

```

```
nohup sudo java -jar target/rocketmq-exporter-0.0.2-SNAPSHOT.jar &



docker container run -itd --rm  -p 5557:5557  docker.io/rocketmq-exporter

```