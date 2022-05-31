# zookeeper exporter
#wget下载
```

  wget https://github.com/carlpett/zookeeper_exporter/releases/download/v1.0.2/zookeeper_exporter
  
```
#添加执行权限
```
   chmod +x zookeeper_exporter
```
#下载screen
```
   yum -y install screen
   
```
#创建会话
```
    #创建会话 
	screen -S zookeeper_exporter
	
	#进入会话
	screen -r zookeeper_exporter
```
#执行zookeeper_exporter二进制文件
```
	#查看信息
	./zookeeper_exporter --help
	
	#执行(默认访问/metrics ip:9141)
	
	./zookeeper_exporter
	
```