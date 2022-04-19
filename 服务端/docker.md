### 1.docker的相关命令

#### 1）docker中镜像

**启动镜像**

```
docker run -p 本机映射端口:镜像映射端口 -d  --name 启动镜像名称 -e 镜像启动参数  镜像名称:镜像版本号
```

   参数释义：

1.    -p  本机端口和容器启动端口映射
2.    -d  后台运行
3.   --name  容器名称
4.   -e  镜像启动参数 

例：

```
docker run -d -p 9092:9092 --name kafka 0eaa187928fa
// 最后面需要加上镜像的 ID  则docker ps 中显示的image 就为这个ID
```

例：

```
 docker run -p 7010:7010 -d allinone-iobp-api:icbc  --name allinone-iobp-api:icbc 
```



**查看当前启动的镜像**

```
docker ps
```

**查看所有镜像(包括未启动的)**

```
docker ps -a
```



**停止镜像**

```
docker stop 镜像实例ID/image（镜像名）
例：
docker stop fe754db626db
```

**删除镜像实例**

```
docker rmi [OPTIONS] IMAGE 
```



#### 2）启动与停止docker服务

```
systemctl stop docker

systemctl start docker
```

#### 3）进入容器内部

```
docker exec -it image/容器ID bash

退出容器
exit
```

**停止容器**

```
docker stop 容器ID/container
```

**重启容器**

```
docker restart 容器ID
```

**删除容器**

```
docker rm 容器ID/CONTAINER
```



#### 4) 查看容器的日志

```
docker 	logs 容器ID
```



### 2.docker镜像打包导入另一台服务器

​	1.找到要打包的镜像，并打包成文件

```
docker save -o kafka.tar kafka:v2.12
```

​	2.导入到另一台服务器上

``` 
scp kafka.tar root@192.168.65.28:/home
///home 为导入到这台服务器的home目录下
```

​	3.导出文件(在/home目录下)

```
docker load < kafka.tar
```

4.进入容器

​	1.进入mongo数据库

```
docker exec -it mongo bash
//找到mongo的运行路径
whereis mongo
//执行导出命令
mysqldump -u 用户名 -p 数据库名 > 保存文件.sql
```

### 3.Dockerfile

通过dockerfile构建镜像：

```
docker build -t nginx:v3 .
```

- -t 
- nginx:v3  镜像名称：镜像标签
- . 代表本次执行的上下文路径

```
docker build -f api.Dockerfile -t allinone-iobp-api:icbc .
```

- -f 指定某个文件为dockerfile文件

### 4.docker从tar包加载image

1.从tar包载入镜像：

docker load -i {image_name}.tar

或者

docker load --input {image_name}.tar

2.查看载入是否成功：

docker images | grep {image_name}

3.如果看到加载的镜像没有tag和镜像名，则手动打tag:

docker tag {image_id} {image_name}:{image_tag}

### 常见问题

#### 1.docker 报错 Container 56b90db5253e  is not running

（docker exec -it kafka bash 进入容器时报错）

```
//开启没有运行的container
docker start 56b90db5253e 
```

基础平台:

init_data: 21.203

http://192.168.21.199:9527/index.html#/datax admin/123456

event是连kafka的

datax-web  
