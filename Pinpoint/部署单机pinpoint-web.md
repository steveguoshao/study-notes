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

3.

