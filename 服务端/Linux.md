### 1.Linux命令大全

https://www.runoob.com/linux/linux-command-manual.html

Linux常见命令

文档型：文件相关命令（touch,cat ,echo,rm,vi,cd）

硬件型：硬盘/进程/服务/网络

功能型：压缩/解压，下载，远程

### 2.文档型

#### touch 创建一个新文件

touch test.txt	

#### cat 查看文件内容

cat test.txt

#### echo 

像文件中添加内容

echo 'hello world 123' >> test.txt

覆盖文件中的内容

echo 'hello world' > test.txt

#### rm 

### 3.功能型

#### 1.wget 下载

#### 2.tar 压缩文件

```
打包成tar.gz格式压缩包
sudo tar czvf mongo.tar(被压缩的文件名) mongo（压缩的文件名）

将linux中的包发送到本地电脑中
sz mongo.tar(文件名)
```



#### 3.grep 查看进程

筛选docker进程

ps -ef |grep docker



#### 4.linux中创建用户

```shell
sudo useradd -m "suer"  #创建suer用户
sudo passwd suer  		#设置suer的密码
```

#### 5.linux中免密登陆

ssh免密码登录的原理：serverA 免密码登录到 serverB

**机器A** 向 **机器B** 进行免密码登陆（A上登陆B服务器）

**step1：**

在A中生成私钥和公钥

```shell
ssh-keygen -t rsa
```

此时在 ~/.ssh/ 目录下生成了公钥（id_rsa.pub）和私钥(id_rsa)

**step2:**

把机器A的公钥（id_rsa.pub）复制到机器B ~/.ssh/authorized_keys 文件里，两种常用方法

1.

```shell
scp ~/.ssh/id_rsa.pub username@host:/home/B/id_rsa.pub	#B
//此时scp需要输入 登录机器B username用户的密码
//然后进入机器B内把 /home/B/id_rsa.pub 文件内容加写进 ~/.ssh/authorized_keys 文件：

cat /home/B/id_rsa.pub /home/B/.ssh/authorized_keys
```

2.推荐

```shell
//在机器A中使用 ssh-copy-id 把公钥加写到机器B的 ~/.ssh/authorized_keys 文件
ssh-copy-id username@host
//执行后输入机器B username用户的密码
```

**step3：** (配置完公钥之后，还需要修改需要连接服务器上的.ssh文件的权限，否则免密不成功)

修改机器B ~/.ssh/authorized_keys 文件的权限：

```shell
//.ssh文件夹权限
chmod 700 ~/.ssh/
chmod 600 ~/.ssh/authorized_keys
```

```shell
//用户权限
chmod 700 /home/username	#还不能成功时，设置目标服务器用户权限
```

**step4:**

此时机器A可以进行免验证登录 机器B	

```shell
ssh username@host
```



### 4.防火墙



#### 1.开启端口

```shell
sudo firewall-cmd --zone=public --add-port=3306/tcp --permanent 
sudo firewall-cmd --reload
sudo firewall-cmd --zone=public --list-ports
```