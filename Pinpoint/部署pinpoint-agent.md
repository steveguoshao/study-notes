1.下载pinpoint-agent

下载1.6.2 Releases版本，地址：[https://github.com/naver/pinpoint/releases/tag/1.6.2](https://github.com/naver/pinpoint/releases/tag/1.6.2)

```
wget --no-cookie --no-check-certificate --header "Cookie:oraclelicense=accept-securebackup-cookie" \
    -P /usr/local/src https://github.com/naver/pinpoint/releases/download/1.6.2/pinpoint-agent-1.6.2.tar.gz
```

如果网络比较慢的话自己去git clone [https://github.com/naver/pinpoint](https://github.com/naver/pinpoint) 切换到1.6.x 自己编译

2.解压

```
cd /usr/local/src/
mkdir /usr/local/src/pinpoint-agent && tar --strip-components=1 –xzvf zookeeper-3.4.9.tar.gz –C /usr/local/zookeeper
```

将pinpoint-agent-1.6.2.tar.gz 解压到 /usr/local/src/pinpoint-agent 中

2.修改pinpoint.config  
配置

```
 目录下，vim pinpoint.config
```

```
cd /usr/local/src/pinpoint-agent/
```

```
 profiler.collector.ip // 单个collector情况下对应的是对应pinpoint-collector中的pinpoint-collector.properties中的cluster.listen.ip，如果是集群部署cellector，并且用了nginx做了TCP、UDP负载均衡，那么这里就要配置nginx服务器所在IP

 profiler.sampling.rate=2 // 采样率  1/n，2就是50%
```



