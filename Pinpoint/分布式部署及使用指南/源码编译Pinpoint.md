* 下载源码

用IDEA从github上下载pinpoint的源码，地址：

```
https://github.com/naver/pinpoint.git
```

然后切换到1.6.x分支，由于代码里面的版本是1.6.3-SNAPSHOT，如果需要编译稳定版本1.6.2，需要把代码回退到1.6.2发布版本，其实1.6.3-SNAPSHOT也就是改了一下版本号而已，跟1.6.2一样。

* 设置环境变量

设置环境变量JAVA\__6\_\_HOME、JAVA\_7_\_\_HOME、JAVA\_8_\_\_HOME，可以全部指向同一个JDK，就看你当前环境用的是哪个版本的JDK了。

* 设置MAVEN仓库

一般公司都会自己搭建私服，如果settings.xml里面设置了mirror最好注释掉，保留设置本地仓库地址就可以了，因为pinpoint依赖的好多jar包需要从cloudera上面下载

* 编译

打开IDEA的terminal，输入如下命令编译pinpoint

```
mvn clean install -Dmaven.test.skip=true
```

可能会出现各种找不到jar报的错误，如果本地仓库有，就删掉那个jar包，重新执行上述命令，两次不行那就三次，一直轮回到编译成功。

