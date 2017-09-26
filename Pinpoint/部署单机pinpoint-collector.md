1.在master下载并执行pinpoint的HBase初始化表语句并执行

```
wget -c https://raw.githubusercontent.com/naver/pinpoint/1.6.x/hbase/scripts/hbase-create.hbase -P ~/pinpoint/
cd /usr/local/hbbase/bin
./hbase shell ~/pinpoint/hbase-create.hbase
```

1.6.x版本的所有的脚本地址：https://github.com/naver/pinpoint/tree/1.6.x/hbase/scripts 

2.

