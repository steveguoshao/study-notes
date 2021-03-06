1.在master下载并执行pinpoint的HBase初始化表语句并执行

```
wget -c https://raw.githubusercontent.com/naver/pinpoint/1.6.x/hbase/scripts/hbase-create.hbase -P ~/pinpoint/
cd /usr/local/hbbase/bin
./hbase shell ~/pinpoint/hbase-create.hbase
```

1.6.x版本的所有的脚本地址：[https://github.com/naver/pinpoint/tree/1.6.x/hbase/scripts](https://github.com/naver/pinpoint/tree/1.6.x/hbase/scripts)

2.下载pinpoint-collector

下载1.6.2 Releases版本，地址：[https://github.com/naver/pinpoint/releases/tag/1.6.2](https://github.com/naver/pinpoint/releases/tag/1.6.2)

```
wget --no-cookie --no-check-certificate --header "Cookie:oraclelicense=accept-securebackup-cookie" \
    -P /usr/local/src https://github.com/naver/pinpoint/releases/download/1.6.2/pinpoint-collector-1.6.2.war
```

如果网络比较慢的话自己去git clone [https://github.com/naver/pinpoint](https://github.com/naver/pinpoint) 切换到1.6.x 自己编译

3.解压

将 pinpoint-collector-1.6.2.war 解压到 /usr/local/src/apache-tomcat-8.5.20/webapps/ROOT/ 中

```
rm -rf /usr/local/src/apache-tomcat-8.5.20/webapps/*
unzip -oq pinpoint-collector-1.6.2.war -d /usr/local/src/apache-tomcat-8.5.20/webapps/ROOT/
```

4.修改hbase.properties

```
cd /usr/local/src/apache-tomcat-8.5.20/webapps/ROOT/WEB-INF/classes
vim hbase.properties
# zookeeper集群的地址
hbase.client.host=99.48.18.233,99.48.18.235,99.48.18.236
# zookeeper集群的端口
hbase.client.port=2181
```

完整配置项：[https://github.com/naver/pinpoint/blob/master/collector/src/main/resources/hbase.properties](https://www.gitbook.com/book/steveguoshao/study-notes/edit#)

5.修改pinpoint-collector.properties

```
vim pinpoint-collector.properties
## 下面三项都把默认值：0.0.0.0改为本机IP
# TCP监听地址
collector.tcpListenIp=99.48.18.227
# TCP监听端口 对应Agent配置中的 profiler.collector.tcp.port，默认值为9994，不需要修改
collector.tcpListenPort=9994

# UDP状态监听地址
collector.udpStatListenIp=99.48.18.227
# UDP状态监听端口 对应Agent配置中的 profiler.collector.stat.port，默认值为9995，不需要修改
collector.udpStatListenPort=9995

# UDP Span监听地址
collector.udpSpanListenIp=99.48.18.227
# UDP状态监听端口 对应Agent配置中的 profiler.collector.span.port，默认值为9996，不需要修改
collector.udpSpanListenPort=9996

## 集群相关配置
#开启集群
cluster.enable=true
# zookeeper集群地址
cluster.zookeeper.address=99.48.18.233,99.48.18.235,99.48.18.236
# zookeeper会话超时时间
cluster.zookeeper.sessiontimeout=30000
# collector监听IP
cluster.listen.ip=99.48.18.227
# collector监听端口 
cluster.listen.port=9090
##注意：这两个配置对应的pinpoint-web/WEB-INF/classes/pinpoint-web.properties中的cluster.connect.address=cluster.listen.ip:cluster.listen.port

# collector admin 用户的密码
collector.admin.password=1qaz@WSX
```

完整配置项：[https://github.com/naver/pinpoint/blob/master/collector/src/main/resources/pinpoint-collector.properties](https://github.com/naver/pinpoint/blob/master/collector/src/main/resources/pinpoint-collector.properties)

