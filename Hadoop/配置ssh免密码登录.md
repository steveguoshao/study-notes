1.在master上生成公钥和私钥

以root身份登录，然后进入home目录

```
cd ~
ssh-keygen -t rsa -P ''
```

然后会在home目录下生成.ssh目录，里面会有二个文件私钥：id\_rsa，公钥：id\_rsa.pub

2.导入公钥 

```
cat .ssh/id_rsa.pub >> .ssh/authorized_keys
ssh master
```

执行完以后，可以在本机上测试下，用ssh连接自己，即：ssh master,然后再测试下 ssh master ，如果不需要输入密码，就连接成功，表示ok，一台机器已经搞定了。

3.在其他机器上生成公钥和私钥，并将公钥文件复制到master

以root身份登录其它二台机器slave01、slave02，执行 `ssh-keygen -t rsa -P ''` 生成公钥、密钥，然后用scp命令，把公钥文件发放给master，

slave01上：`scp .ssh/id_rsa.pub root@master:/root/.ssh/id_rsa_01.pub`

slave02上：`scp .ssh/id_rsa.pub root@master:/root/.ssh/id_rsa_02.pub`

这二行执行完后，回到master中，查看下/root/.ssh/目录，应该有二个新文件id\_rsa\_01.pub、id\_rsa\_02.pub，然后在master上，导入这二个公钥

```
cat id_rsa_01.pub >> .ssh/authorized_keys
cat id_rsa_02.pub >> .ssh/authorized_keys
```

这样，master这台机器上，就有所有3台机器的公钥了。

4.将master上的最全的公钥复制到其他机器

在master上执行

```
scp .ssh/authorized_keys root@slave01:/root/.ssh/authorized_keys
scp .ssh/authorized_keys root@slave02:/root/.ssh/authorized_keys
```

5.验证 

在每个虚拟机上，均用 ssh 其它机器的hostname 验证下，如果能正常无密码连接成功，表示ok

