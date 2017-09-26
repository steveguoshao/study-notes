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
mkdir /usr/local/src/pinpoint-agent \
&& tar --strip-components=1 –xzvf pinpoint-agent-1.6.2.tar.gz –C /usr/local/src/pinpoint-agent
```

3.修改pinpoint.config配置

```
cd /usr/local/src/pinpoint-agent/
vim pinpoint.config
```

```
#单个collector情况下对应的是对应pinpoint-collector中的pinpoint-collector.properties中的cluster.listen.ip
#如果是集群部署cellector，并且用了nginx做了TCP、UDP负载均衡，那么这里就要配置nginx服务器所在IP
profiler.collector.ip

# 采样率  1/n，2就是50%
profiler.sampling.rate=2
```

4.在web应用中配置

在`tomcat/bin/catalina.sh`的开头加上

```
#pinpoint-agent config
#agent版本号
AGENT_VERSION="1.6.2"
#分布式应用名称 全局唯一
APPLICATION_NAME="wechat"
#agentID必须在所有分布式应用中唯一，这里用分布式应用名开头加上pinpoint-agent
AGENT_ID="$APPLICATION_NAME-pinpoint-agent"
AGENT_PATH="/usr/local/src/pinpoint-agent"
AGENT_OPTS=" -javaagent:$AGENT_PATH/pinpoint-bootstrap-${AGENT_VERSION}.jar -Dpinpoint.agentId=$AGENT_ID -Dpinpoint.applicationName=$APPLICATION_NAME "
CATALINA_OPTS="$CATALINA_OPTS $AGENT_OPTS"
```

5.在Dubbo应用中配置

在`dubbo/bin/start.sh`的开头加上

```
#pinpoint-agent config
#agent版本号
AGENT_VERSION="1.6.2"
#分布式应用名称 全局唯一
APPLICATION_NAME="wechat"
#agentID必须在所有分布式应用中唯一，这里用分布式应用名开头加上pinpoint-agent
AGENT_ID="$APPLICATION_NAME-pinpoint-agent"
AGENT_PATH="/usr/local/src/pinpoint-agent"
AGENT_OPTS=" -javaagent:$AGENT_PATH/pinpoint-bootstrap-${AGENT_VERSION}.jar -Dpinpoint.agentId=$AGENT_ID -Dpinpoint.applicationName=$APPLICATION_NAME "
JAVA_OPTS="$JAVA_OPTS $AGENT_OPTS"
```

然后找到下面的`JAVA_OPTS`，把

```
JAVA_OPTS="-Djava.awt.headless=true -Djava.net.preferIPv4Stack=true "
```

修改为

```
JAVA_OPTS="$JAVA_OPTS -Djava.awt.headless=true -Djava.net.preferIPv4Stack=true "
```



