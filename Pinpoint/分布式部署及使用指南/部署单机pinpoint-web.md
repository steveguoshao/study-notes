1.下载pinpoint-web

下载1.6.2 Releases版本，地址：[https://github.com/naver/pinpoint/releases/tag/1.6.2](https://github.com/naver/pinpoint/releases/tag/1.6.2)

```
wget --no-cookie --no-check-certificate --header "Cookie:oraclelicense=accept-securebackup-cookie" \
    -P /usr/local/src https://github.com/naver/pinpoint/releases/download/1.6.2/pinpoint-web-1.6.2.war
```

如果网络比较慢的话自己去git clone [https://github.com/naver/pinpoint](https://github.com/naver/pinpoint) 切换到1.6.x 自己编译

2.解压

将 pinpoint-web-1.6.2.war 解压到 /usr/local/src/apache-tomcat-8.5.20/webapps/ROOT/ 中

```
rm -rf /usr/local/src/apache-tomcat-8.5.20/webapps/*
unzip -oq pinpoint-web-1.6.2.war -d /usr/local/src/apache-tomcat-8.5.20/webapps/ROOT/
```

3.修改hbase.properties

```
cd /usr/local/src/apache-tomcat-8.5.20/webapps/ROOT/WEB-INF/classes
vim hbase.properties
# zookeeper集群的地址
hbase.client.host=99.48.18.233,99.48.18.235,99.48.18.236
# zookeeper集群的端口
hbase.client.port=2181
```

完整配置项：[https://github.com/naver/pinpoint/blob/master/web/src/main/resources/hbase.properties](https://www.gitbook.com/book/steveguoshao/study-notes/edit#)

4.修改pinpoint-web.properties

```
vim pinpoint-web.properties

## 集群相关配置
# 开启集群
cluster.enable=true
# web tcp监听端口
cluster.web.tcp.port=9997
# zookeeper集群地址
cluster.zookeeper.address=99.48.18.233,99.48.18.235,99.48.18.236
# zookeeper的会话超时时间
cluster.zookeeper.sessiontimeout=30000
# zookeeper的重试时间间隔
cluster.zookeeper.retry.interval=60000
#连接地址 对应pinpoint-collector中的pinpoint-collector.properties中的cluster.listen.ip：cluster.listen.port，
#如果collector是集群配置可以是ip1:host1,ip2:host2格式
cluster.connect.address=99.48.18.227:9090

# 管理员的密码，一定要修改
admin.password=1qaz@WSX
```

完整配置：https://github.com/naver/pinpoint/blob/master/web/src/main/resources/pinpoint-web.properties

