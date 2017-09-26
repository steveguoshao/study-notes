1.在master下载并执行pinpoint的HBase初始化表语句并执行

```
wget -c https://raw.githubusercontent.com/naver/pinpoint/1.6.x/hbase/scripts/hbase-create.hbase -P ~/pinpoint/
cd /usr/local/hbbase/bin
./hbase shell ~/pinpoint/hbase-create.hbase
```

1.6.x版本的所有的脚本地址：[https://github.com/naver/pinpoint/tree/1.6.x/hbase/scripts](https://github.com/naver/pinpoint/tree/1.6.x/hbase/scripts)

2.下载pinpoint

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
hbase.client.host=99.48.18.233,99.48.18.235,99.48.18.236
hbase.client.port=2181
```

主要是修改两个配置：

hbase.client.host=zookeeper集群的地址

hbase.client.port=zookeeper集群的端口

