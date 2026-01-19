### mid360配置

#### 底层驱动

```bash
# 安装依赖
sudo apt update
sudo apt install cmake build-essential git

# 克隆并编译Livox SDK2
git clone https://github.com/Livox-SDK/Livox-SDK2.git
cd Livox-SDK2
mkdir build && cd build
cmake .. && make -j4
sudo make install
```

#### 配置`Livox ROS2`

```bash
# 确保安装了必要的依赖
sudo apt update
sudo apt install -y \
    ros-humble-pcl-ros \
    ros-humble-pcl-conversions \
    ros-humble-tf2-sensor-msgs \
    ros-humble-tf2-geometry-msgs \
    ros-humble-visualization-msgs \
    libpcl-dev \
    libeigen3-dev
```

**这里是官方推荐的修改版本**


```bash
# 创建一个独立的工作空间给livox驱动
mkdir -p ~/livox_driver_ws/src
cd ~/livox_driver_ws/src

# 克隆官方推荐的修改版本
git clone https://github.com/Ericsii/livox_ros_driver2.git -b feature/use-standard-unit

# 或者使用官方版本
# git clone https://github.com/Livox-SDK/livox_ros_driver2.git

cd ~/livox_driver_ws
source /opt/ros/humble/setup.bash
colcon build
source install/setup.bash
```

#### `fast-lio` 配置

```bash
# 创建FAST-LIO工作空间
mkdir -p ~/fastlio_ws/src
cd ~/fastlio_ws/src

# 克隆官方ROS2分支
git clone https://github.com/Ericsii/FAST_LIO.git --recursive

cd ~/fastlio_ws
# 先source livox驱动（重要！）
source ~/livox_driver_ws/install/setup.bash
# 安装依赖
sudo apt install -y python3-rosdep
rosdep install --from-paths src --ignore-src -y
# 编译
colcon build --symlink-install --packages-select fast_lio
source install/setup.bash
```

#### 修改对应的以太网口

代码中

修改`src/livox_ros_driver2/config/MID360_config.json` 中的

```json
"lidar_configs" : [
    {
      "ip" : "192.168.1.189",################# 修改这里最后的两个数字
      "pcl_data_type" : 1,
      "pattern_mode" : 0,
      "extrinsic_parameter" : {
        "roll": 0.0,
        "pitch": 0,
        "yaw": 0.0,
        "x": 0,
        "y": 0,
        "z": 0
      }
    }
  ]
```

#### 修改以太网口（临时）

```bash
sudo ifconfig eno1 192.168.1.5 netmask 255.255.255.0 up
```

#### 修改以太网口（永久）

详见[修改以太网](./修改以太网)

#### 运行方式

```bash
ros2 launch livox_ros_driver2 msg_MID360_launch.py
```

```bash
ros2 launch fast_lio mapping.launch.py config_file:=/root/fastlio_ws/src/FAST_LIO/config/mid360.yaml
```

```bash
ros2 topic echo /Odometry
```

---

