---
title: hyper-v虚拟机操作-docker
tags: 
	- 笔记
---

> 查看Centos版本
```
cat /etc/redhat-release
```
<!--more-->
> hyper查询ip地址
```
ipconfig 查询IP地址（或 ip addr）
```

> 创建文件夹
```
mkdir  文件夹名
```

> 创建文件
```
touch 文件名
```

> 关闭防火墙
```
1
    //停止
    systemctl stop firewalld.service
    //禁止开机启动
    systemctl disable firewalld.service
2
    //这里发现防火墙是开启的，再来查看防火墙控制的端口
    systemctl status firewalld
    //查看防火墙控制的端口，发现我想要监听的8081端口没有开启
    firewall-cmd --list-all
    //永久开启3306端口
    sudo firewall-cmd --zone=public --add-port=3306/tcp --permanent
    //重载防火墙
    sudo firewall-cmd --reload   

参考见：https://www.cnblogs.com/TimLiuDream/p/9993625.html
```

> 删除文件夹名/文件名
```
rm -rf 文件夹名（文件名）
```

> 退出
```
exit
```

> 当前路径
```
pwd
```

> 重启docker
```
启动        systemctl start docker
守护进程重启   sudo systemctl daemon-reload
重启docker服务   systemctl restart  docker
重启docker服务  sudo service docker restart
关闭docker   service docker stop   
关闭docker  systemctl stop docker
参考见：https://blog.csdn.net/easternunbeaten/article/details/80463837
```

> docker
```
镜像images(类) 容器container(对象) 仓库repository  标签tag(版本号)
```

> 查看镜像
```
docker images

> REPOSITORY: 镜像仓库源
> TAG: 镜像标签（版本）
> IMAGE ID: 镜像ID
> CREATED: 镜像创建时间
> SIZE: 镜像大小
  
  docker images -a (列出本地所有镜像)
                -q (只显示镜像ID)
                -qa (显示所有镜像ID)
                --digests (显示镜像的摘要信息)
                --no-trunc (显示完整的镜像信息)
```

> 搜索镜像
```
docker search tomcat  
(docker search -s 30 tomcat  只查找下载数超过30)
```

> 拉取镜像
```
docker pull 镜像名:版本号（不写版本号默认最新的）
```

> 运行容器
```
docker run -it imagesID        以交互式启动（启动后进入命令终端）
            i                   交互
             t                  终端
docker run -d 容器名（imagesID） 后台式运行
docker run -d --name 自定义容器名 容器名（imagesID） 后台式运行
docker run -it -p 8888:8080 容器名（容器ID）
               主机端口:容器端口
              （可以运行多个容器）
docker run -it -P 容器名（容器ID）随机分配端口
> 停止容器
docker stop 容器ID
docker kill 容器ID （强制停止）
```

> 镜像运行状态
```
docker ps -a 列出所有正在运行的容器+历史运行过的
          -l 显示最近创建的容器
          -n 3 显示最近n创建的容器
          -q 静默模式，只显示容器编号
          --no-trunc 不截断输出
```

> 退出容器
```
exit 停止并退出
ctrl+p+q 退出不停止
```

> 启动容器
```
docker start 容器ID
```

> 重启容器
```
docker restart 容器ID
```

> 删除镜像
```
docker rmi IMAGE ID 
docker rmi -f IMAGE ID 强制删除
docker rm CONTAINER ID 
想要删除运行过的images必须首先删除它的container
```

> 查看日志
```
docker logs 容器ID
docker logs -t 容器ID
docker logs -t -f 容器ID  持续打印
docker logs -t -f --tail 3 容器ID  最后三行
```

> 停止容器运行
```
docker top 容器ID
```

> 查看容器内细节
```
docker inspect 容器ID
```

> 进入容器
```
docker exec -it 容器ID
docker attach 容器ID （重新进入容器）
```

> 容器数据卷
```
容器与主机文件共享
1 docker cp
  docker run -it -v /宿主机绝对路径目录:/容器内目录 容器ID     文件夹不存在会自动创建
  docker run -it -v /宿主机绝对路径目录:/容器内目录:ro 容器ID   只读
```

> Dockerfile
```
...
```

> 容器中安装vim
```
apt-get update
apt-get install vim
```

> 容器中网络不通
```
重启容器，还是不行的话
测试 ping www.baidu.com
访问不了
在主机中 /etc/resolv.conf 文件末尾中添加
nameserver 8.8.8.8
nameserver 202.102.224.68

参考见：http://dockone.io/question/248
```

> 安装mysql
```
docker run -p 3301:3306 --name testmysql -v /test/mysql/conf:/etc/mysql/conf.d -v /test/mysql/logs:logs -v /test/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql

docker exec -it 容器ID /bin/bash 
mysql -uroot -p
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'root';  设置数据库账号密码

iptables -t nat -A  DOCKER -p tcp --dport 3302 -j DNAT --to-destination 172.18.111.185:3301
（外网连接3302）
```

> 安装node
```
dokcer run -it -p 8081:808 --name testnode -v /test/node:/test/node node
```

> nginx
```
默认访问页面路径
/usr/share/nginx/html nginx
```

> 安装宝塔控制面板
```
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && bash install.sh
查询账号密码
/etc/init.d/bt default
username: trwwj7kr
password: 17f68cb8
```

> docker容器中安装 vim
```
bash: vim: command not found

apt-get install vim
apt-get update
apt-get install vim
```

> windows挂载文件到dockers容器中
```
C:\Program Files\Docker\Docker\Resources\bin\docker.exe: Error response from daemon: C: drive is not shared. Please share it in Docker for Windows Settings.
See 'C:\Program Files\Docker\Docker\Resources\bin\docker.exe run --help'.
docker软件Settings鼠标右键->Shared Drives->把你想挂载的文件所有的盘符勾上->Apply

参考见：https://blog.csdn.net/m0_37477061/article/details/82217525
```
