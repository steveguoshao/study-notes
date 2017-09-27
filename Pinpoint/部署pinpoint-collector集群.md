部署pinpoint-collector集群需要注意的有：

1.每台部署pinpoint-cllector的`pinpoint-collector.properties`

```
## 把下面三项从默认值0.0.0.0改为本机IP
collector.tcpListenIp
collector.udpStatListenIp
collector.udpSpanListenIp
```

`pinpoint-web.properties`

```
cluster.connect.address=collectorIp1:host1,collectorIp2:host2
```

`pingpoint.config`

```
profiler.collector.ip=nginx服务器所在ip
```

2.nginx配置

如果服务器上已经有nginx了，要用`nginx –V`查看是否有stream模块，如果没有，需要重新编译安装，编译的时候增加参数`--with-stream`开启，

编译命令：

```
./configure --prefix=/usr/local/nginx --user=nginx --group=nginx --with-http_ssl_module --with-http_stub_status_module --with-stream
```

Stream配置是顶层配置，如果原先已经有http配置，那么就在http前面增加`stream{}`，

单纯的TCP、UDP负载均衡nginx.conf如下：

```
user nginx;
worker_processes  2;

error_log  /usr/local/nginx/logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

pid        /usr/local/nginx/logs/nginx.pid;


events {
    use epoll;
    worker_connections  1024;
}

#配置TCP和UDP负载均衡
stream {

    proxy_protocol_timeout 120s;
    log_format main '$remote_addr $remote_port - [$time_local] '
        '$status $bytes_sent $protocol $server_addr $server_port'
        '$proxy_protocol_addr $proxy_protocol_port';

    access_log  /usr/local/nginx/logs/access.log  main;

    upstream 9994_tcp_upstreams {
        #least_time first_byte;
        #server 99.48.18.227:9994 max_fails=3 fail_timeout=15s;
        server 99.48.18.227:9994;
        server 99.48.18.229:9994;
    }

    upstream 9995_udp_upstreams {
        #least_time first_byte;
        server 99.48.18.227:9995;
        server 99.48.18.229:9995;
    }

    upstream 9996_udp_upstreams {
        #least_time first_byte;
        server 99.48.18.227:9996;
        server 99.48.18.229:9996;
    }

    upstream 9997_tcp_upstreams {
        #least_time first_byte;
        server 99.48.18.227:9997;
        server 99.48.18.229:9997;
    }

    server {
        listen 9994;
        proxy_pass 9994_tcp_upstreams;
        #proxy_timeout 1s;
        proxy_connect_timeout 5s;
    }

    server {
        listen 9995 udp;
        proxy_pass 9995_udp_upstreams;
        proxy_timeout 1s;
        #proxy_responses 1;
    }

    server {
        listen 9996 udp;
        proxy_pass 9996_udp_upstreams;
        proxy_timeout 1s;
        #proxy_responses 1;
    }

    server {
        listen 9997;
        proxy_pass 9997_tcp_upstreams;
        #proxy_timeout 1s;
        proxy_connect_timeout 5s;
    }

}
```



