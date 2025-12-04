创建新容器

```bash
docker run -it ubuntu:22.04 bash
```

容器改名

```bash
docker rename 原名字 新名字
```

运行已经存在的容器

```bash
docker start -ai mycontainer
```

将容器保存为新镜像

```bash
docker commit test fastlio_image:v1
```

显示所有容器

```bash
docker ps -a
```

下载常用工具

```bash
apt update
apt install -y wget curl git vim nano
```

打开容器的一个终端

```bash
docker exec -it test bash
```

配置终端中文包

```bash
#安装中文语言包
apt update
apt install -y locales locales-all tzdata language-pack-zh-hans
locale-gen zh_CN zh_CN.UTF-8
update-locale LANG=zh_CN.UTF-8 LC_ALL=zh_CN.UTF-8
#编辑 ~/.bashrc  添加
export LANG=zh_CN.UTF-8
export LANGUAGE=zh_CN:zh
export LC_ALL=zh_CN.UTF-8
```



根据ros官网配置ros2

```bash
https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html
```

安装colcon

```bash
apt update
apt install -y python3-pip python3-setuptools python3-colcon-common-extensions
```

在docker中配置网络环境（这里是为了给mid360配置以太网ip192.168.1.5）

```bash
# 1. 创建自定义桥接网络
docker network create   --driver=bridge   --subnet=192.168.1.0/24   fastlio_net
  
 # 2. 启动容器并分配固定 IP
docker run -it --name fastlio   --network fastlio_net   --ip 192.168.1.5   your_fastlio_image /bin/bash
```

查看桥接网络

```bash
docker network ls
```







