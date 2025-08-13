## 参考文档

- [**古月居**](https://book.guyuehome.com/)

- [**鱼香ROS**](https://fishros.com/d2lros2/#/?id=%e5%8a%a8%e6%89%8b%e5%ad%a6ros2)



## 一键下载

``` bash
wget http://fishros.com/install -O fishros && . fishros
```



## 正式开始学习

### 		1. 创建工作空间

```bash
mkdir -p ~/workspace_ws/src
cd ~/workspace_ws/src
```

### 2.创建功能包

```bash
ros2 pkg create pkg_name --build-type ament-python --dependencies rclpy
```

--build-type 后面的参数指明**功能包的类型**(python用ament-python,c/c++用ament_cmake)

--dependencies 后面指明**依赖(古月居的文档中未提到这一点，不知道是什么原因)**

### 3.在功能包目录下的同名文件夹中写一个节点

```python
#一个简单的一直输出的helloworld节点
import rclpy
from rclpy.node import Node
import time

def main(args = None):
    rclpy.init(args=args)
    node = Node("Hello_world_node")
    i = 0
    while rclpy.ok():
        i+=1
        node.get_logger().info(f"hell_world {i}")
        time.sleep(0.5)
    node.destroy_node()
    rclpy.shutdown()
```

### 4. 在功能包目录下的setup.py中添加程序入口

```python
entry_points={
        'console_scripts': [
                'hello_world_node = hello_world.hello_world_node:main'
        ],
    },
```

**格式**：包名.节点名:入口函数(一般是main)

### 5.编译工作空间

```bash
cd ~/workspace_ws
colcon build
```

### 6.source一下

```bash
source install/local_setup.sh	#更新环境变量，让电脑能识别到节点(仅在当前终端有效,每次都得source)

echo "source /hoem/your_usr_name/workspace_ws/install/local_setup.sh" >> ~/.bashrc
#source后面加setup.sh的绝对路径,添加到bashrc后就不用每次source了，注意这里用的>>表示追加到末尾,而>表示覆写
```

注意这里的用的local_setup.sh与setup.sh的区别,一般使用local

- local_setup.sh：只加载当前工作空间的配置
- setup.sh：除了当前工作空间的配置还会加载更底层的相关配置(比如最基本的/opt/ros/humble/setup.sh)

### 7.运行节点

```bash
ros2 run pkg_name node_name
```

可以用` ros2 node list`查看节点

---



## 创建接口功能包编接口

### 1.创建功能包

```bash
ros2 pkg create example_ros2_interfaces --build-type ament_cmake --dependencies rosidl_default_generators geometry_msgs
```

注意功能包类型必须为：ament_cmake

依赖`rosidl_default_generators`必须添加，`geometry_msgs`视内容情况添加（我们这里有`geometry_msgs/Pose pose`所以要添加）。

### 2.写入接口

服务接口`MoveRobot.srv`

```bash
# 前进后退的距离
float32 distance
---
# 当前的位置
float32 pose
```

----

话题接口，采用基础类型  `RobotStatus.msg`

```bash
uint32 STATUS_MOVEING = 1
uint32 STATUS_STOP = 1
uint32  status
float32 pose
```

----

话题接口，混合包装类型 `RobotPose.msg`

```shell
uint32 STATUS_MOVEING = 1
uint32 STATUS_STOP = 2
uint32  status
geometry_msgs/Pose pose
```

---

>注意话题接口放到`msg`文件夹下，以`.msg`结尾。服务接口放到`srv`文件夹下，以`srv`结尾。

```bash
.
├── CMakeLists.txt
├── msg
│   ├── RobotPose.msg
│   └── RobotStatus.msg
├── package.xml
└── srv
    └── MoveRobot.srv
```

### 3.修改`CMakeLists.txt`

```cmake
find_package(rosidl_default_generators REQUIRED)
find_package(geometry_msgs REQUIRED)
# 添加下面的内容
rosidl_generate_interfaces(${PROJECT_NAME}
  "msg/RobotPose.msg"
  "msg/RobotStatus.msg"
  "srv/MoveRobot.srv"
  DEPENDENCIES geometry_msgs
)
```

### 4.修改`package.xml`

```xml
  <buildtool_depend>ament_cmake</buildtool_depend>

  <depend>rosidl_default_generators</depend>
  <depend>geometry_msgs</depend>
  
  <member_of_group>rosidl_interface_packages</member_of_group> #添加这一行

  <test_depend>ament_lint_auto</test_depend>
  <test_depend>ament_lint_common</test_depend>
```

### 5.编译

```shell
colcon build --packages-select example_ros2_interfaces
```

编译完成后在`chapt3_ws/install/example_ros2_interfaces/include`下应该可以看到C++的头文件。在`chapt3_ws/install/example_ros2_interfaces/local/lib/python3.10/dist-packages`下应该可以看到Python版本的头文件。

接下来的代码里我们就可以通过头文件导入和使用我们定义的接口了。

- 明天需要测试用别的工作空间的功能包下面的接口是否需要source
