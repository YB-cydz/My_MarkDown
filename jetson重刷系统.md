### nano安装

```bash
sudo apt install nano/opt/ros/humble/share/mavros/launch

```

### ros2安装

```bash
wget http://fishros.com/install -O fishros && . fishros
```

### [nomachine](https://www.nomachine.com/)

点击上面链接进入官网下载deb包，注意要用arm64的

### mavros安装与配置

apt安装相关的包

```bash
sudo apt-get install ros-humble-mavros
sudo apt-get install ros-humble-mavros-extras
```

下载并安装 GeographicLib 的地理数据集，以支持 MAVROS 正确处理 GPS 坐标、地球椭球模型和高度转换。

```bahs
cd /opt/ros/humble/lib/mavros
sudo ./install_geographiclib_datasets.sh
```

更改连接飞控的串口和连接地面站的url

```bash
cd /opt/ros/humble/share/mavros/launch
sudo nano px4.launch
```

修改px4.launch（波特率改为921600,修改串口和udp）

```bash
<launch>
#=========修改==========
        <arg name="fcu_url" default="/dev/ttyTHS1:921600" />
        <arg name="gcs_url" default="udp://@10.21.174.34:14550" />
#======================
        <arg name="tgt_system" default="1" />
        <arg name="tgt_component" default="1" />
        <arg name="log_output" default="screen" />
        <arg name="fcu_protocol" default="v2.0" />
        <arg name="respawn_mavros" default="false" />
        <arg name="namespace" default="mavros"/>

        <include file="$(find-pkg-share mavros)/launch/node.launch">
                <arg name="pluginlists_yaml" value="$(find-pkg-share mavros)/launch/px4_pluginlists.yaml" />
                <arg name="config_yaml" value="$(find-pkg-share mavros)/launch/px4_config.yaml" />
                <arg name="fcu_url" value="$(var fcu_url)" />
                <arg name="gcs_url" value="$(var gcs_url)" />
                <arg name="tgt_system" value="$(var tgt_system)" />
                <arg name="tgt_component" value="$(var tgt_component)" />
                <arg name="log_output" value="$(var log_output)" />
                <arg name="fcu_protocol" value="$(var fcu_protocol)" />
                <arg name="respawn_mavros" value="$(var respawn_mavros)" />
                <arg name="namespace" value="$(var namespace)"/>
        </include>
</launch>
```

关于如何知道那个串口连接飞控

> 如果看到乱码（mavlink数据流）说明就成功了，此时将上面的/dev/ttyTHS1改成你的串口就可以了	

```bash
ls /dev/tty*	#查看所有串口
sudo apt install screen	#下载一个工具方便查看串口数据
sudo screen /dev/ttyTHS1 921600	#一定要加上sudo不然看不到数据
```

***给当前用户访问`dialout`（一般是串口所在的组）的权限（重要）***

```bash
sudo usermod -a -G dialout $USER
```

成功启动

```bash
ros2 launch mavros px4.launch
```





