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

