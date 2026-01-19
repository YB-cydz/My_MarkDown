使用dockerfie构建镜像(. 表示dockerfile在当前目录下)

```bash
docker build -t imagename:latest .
```

创建新容器

```bash
docker run -it --ipc=host --net=host --privileged -e DISPLAY=unix$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix:rw -e NVIDIA_DRIVER_CAPABILITIES=all --name docker_name image_name:tag /bin/bash
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

宿主机开启X11访问权限

```bash
xhost +local:docker
```

临时修改以太网口

```bash
sudo ifconfig eno1 192.168.1.50 netmask 255.255.255.0 up
```

永久修改以太网ip

```bash
sudo nmcli con add type ethernet ifname eno1 con-name eno1 ipv4.method manual ipv4.addresses 192.168.1.50/24 autoconnect yes
sudo nmcli con up eno1
```







