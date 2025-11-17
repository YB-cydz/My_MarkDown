#### 检查ROS2环境

```bash
ros2 doctor
ros2 wtf
```

#### 新建功能包

```bash
ros2 pkg create <包名> --build-type ament-python --dependencies rclpy
```

#### 编译

```bash
colcon build #编译整个工作空间
colcon build --packages-select <包名> #只编译一个功能包
```

#### 运行一个节点

```bash
ros2 run <包名> <节点名>
```

#### 查看节点

``` bash
ros2 node list
```

#### 查看话题

``` bash
ros2 topic list
```

#### 查看话题中信息

```bash
ros2 topic echo <话题名>
```

#### 查看参数

```bash
ros2 param list
```

#### 查看一个参数的详细信息

``` bash
ros2 param describe <节点名> <参数名>
```

#### 查看一个参数的值

``` bash
ros2 param get <节点名> <参数名>
```

#### 设置参数

``` bash
ros2 param set <节点名> <参数名> <value>
```

#### 保存节点参数数据

``` bash
ros2 param dump <节点名> #将节点的参数保存到yaml格式的文件
```

#### 恢复节点参数数据

``` bash
ros2 param load <节点名> <yaml文件路径>#将节点的参数保存到yaml格式的文件
```

























