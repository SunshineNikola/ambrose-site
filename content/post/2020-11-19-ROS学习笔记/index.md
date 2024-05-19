---
layout:      post
title:       "ROS 初级教程学习笔记"
subtitle:    "The Robot Operating System (ROS) is a set of software libraries and tools that help you build robot applications."
description: " "
date:        2024-05-19T23:47:00+08:00
author:      "Ambrose"
image:       "/img/ros.png"
published: true 
tags:        ["ROS", "机器人"]
URL: "/2024/05/19/ros/"
categories:  [ 🛰️Tech ]
# [ 🚘Auto ] [ 🛰️Tech ] [ 🏕️Life ] [ 🎥Video ]
---


# ROS 初级教程学习笔记

a  环境的配置及软件包的创建和编译

b  ROS 节点，话题，服务和参数  (rqt_console和roslaunch)调试

c  创建ROS消息和ROS 服务(rosmsg, rossrv, roscp 命令行使用)，使用python&C++编写消息发布器和订阅器，并测试

d  编写Service和Client，并测试

e  录制和回放bag 包

## 一. 安装配置

### 1. 参考CSDN链接[山水Ubuntu ROS Kinetic安装](https://blog.csdn.net/weixin_43159148/article/details/83375218)

### 2. 管理空间

查看环境变量：

> $ printenv | grep ROS

**注意：** 在所有教程中你将会经常看到分别针对 [rosbuild](http://wiki.ros.org/rosbuild) 和 [catkin](http://wiki.ros.org/catkin) 的不同操作说明，这是因为目前有两种不同的方法可以用来组织和编译ROS应用程序。一般而言，rosbuild 比较简单也易于使用，而 catkin 使用了更加标准的 CMake 规则，所以比较复杂，但是也更加灵活，特别是对于那些想整合外部现有代码或者想发布自己代码的人。关于这些如果你想了解得更全面请参阅 [catkin or rosbuild（英文页面）](http://wiki.ros.org/catkin_or_rosbuild)。

### 3. 创建ROS工作空间(以catkin 为准)

> ```
> $ mkdir -p ~/catkin_ws/src
> $ cd ~/catkin_ws/
> $ catkin_make
> ```

一般创建完成后需要刷新环境，使用source命令

> source devel/setup.bash

## 二. ROS文件系统介绍 

### 1. 安装ros-kinetic-ros-tutorials程序包

> sudo apt-get install ros-kinetic-ros-tutorials

### 2. 文件系统概念

* Packages  软件包，ROS程序代码的组织单元，包括：程序库，可执行文件，脚本&other file

* Manifest(package.xml):清单，对 ‘软件包’ 相关信息的描述。定义依赖关系，包括版本，维护者，许可协议等

### 3. 文件系统工具

rospack

> rospack find [包名称]
>
> $ rospock find roscpp      输出：YOUR_INSTALL_PATH/share/roscpp 

roscd

roscd是rosbash 命令集的一部分，允许直接切换工作目录至软件包或软件包集.

> roscd  [本地包名称[/子目录]]
>
> roscd roscpp

roscd log

> 也属于rosbash 命令集,直接切换到日志文件夹下

rosls

> rosbash 命令集可以直接按照软件包名称ls，不需要绝对路径

Tab自动补全

> Tab补全等同于linux   Tab

## 三. 创建ROS 程序包

### 1. catkin程序包的构成（catkin程序包必须符合以下文件）

* 程序包必须包含package.xml文件
* 程序必须包含CMakeLists.txt 文件，而Catkin metapackages中必须包括一个对CMakeList.txt 文件的引用
* 每个目录下只能有一个程序包，不能嵌套或者多个程序包共存

### 2. catkin工作空间中的程序包

工作空间中的程序包组织形式

```
workspace_folder/        -- WORKSPACE
  src/                   -- SOURCE SPACE
    CMakeLists.txt       -- 'Toplevel' CMake file, provided by catkin
    package_1/
      CMakeLists.txt     -- CMakeLists.txt file for package_1
      package.xml        -- Package manifest for package_1
    ...
    package_n/
      CMakeLists.txt     -- CMakeLists.txt file for package_n
      package.xml        -- Package manifest for package_n
```

### 3. 创建catkin程序包

切换到前边创建的catkin工作空间中的src目录下

> ```
> # You should have created this in the Creating a Workspace Tutorial
> $ cd ~/catkin_ws/src
> ```
>
> 

使用catkin_create_pkg 创建新程序包，依赖于std_msgs, roscpp rospy

> ```
> $ catkin_create_pkg beginner_tutorials std_msgs rospy roscpp
> ```

### 4. 程序包的依赖关系

程序包通常拥有一级依赖和间接依赖可以用以下命令查询

> ```
> $ rospack depends1 beginner_tutorials 
> ```

间接依赖查询

> ```
> $ rospack depends beginner_tutorials
> ```

### 5. 自定义程序包

package.xml   &    CMakeLists.txt 的自定义编写

## 四. 编译ROS程序包

*如果通过apt及软件包管理工具安装系统默认安装好依赖项*

### 1. catkin_make工具的使用

在catkin工作空间下

> ```
> $ catkin_make [make_targets] [-DCMAKE_VARIABLES=...]
> ```

工作流程一般为,也可通过--source my_src  指定外部源码位置

> $ catkin_make                #编译程序包
>
> $ catkin_make install    #安装程序包

### 2. 编译自己的程序包

切换到catkin工作空间下

> $ cd ~/catkin_ws/
>
> $ ls src

使用catkin_make 编译

> $ catkin_make

编译完成可使用ls 查看工作空间下自动生成的文件，包括：build     devel   src

## 五. 理解ROS 节点

### 1. 安装轻量级模拟器

> ```
> $ sudo apt-get install ros-<distro>-ros-tutorials
> ```

概念术语

> - [Nodes](http://wiki.ros.org/Nodes):节点,一个节点即为一个可执行文件，它可以通过ROS与其它节点进行通信。
> - [Messages](http://wiki.ros.org/Messages):消息，消息是一种ROS数据类型，用于订阅或发布到一个话题。
> - [Topics](http://wiki.ros.org/Topics):话题,节点可以发布消息到话题，也可以订阅话题以接收消息。
> - [Master](http://wiki.ros.org/Master):节点管理器，ROS名称服务 (比如帮助节点找到彼此)。
> - [rosout](http://wiki.ros.org/rosout): ROS中相当于stdout/stderr。
> - [roscore](http://wiki.ros.org/roscore): 主机+ rosout + 参数服务器 (参数服务器会在后面介绍)。

### 2. ROS命令行

* 列出当前运行的ROS 节点信息

> rosnode list

* 返回某个节点的信息，rosnode list 列出活跃节点后使用下面命令查看

> rosnode info /rosout

### 3. rosrun运行包内节点(小乌龟模拟器启动)

> rosrun turtlesim turtlesim_node

可使用上边rosnode list 查看

可以使用  rosnode  ping  my_turtle 测试

## 六. ROS topic 话题

### 1. 键盘控制乌龟行走

> $ roscore
>
> $ rosrun turtlesim turtlesin_node
>
> $ rosrun turtlesim turtle_teleop_key

开启ROS ,开启ROS 乌龟模拟器，开启键盘监听节点使用键盘控制乌龟行走

### 2. rostopic命令工具介绍

安装rqt_graph     kinetic版本

> ```
> $ sudo apt-get install ros-<distro>-rqt
> $ sudo apt-get install ros-<distro>-rqt-common-plugins
> ```

新终端中运行rqt,rqt可通过可视化的形式方便的查看各节点之间的话题

> rosrun rqt_graph rqt_graph

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210211010717445.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JvdGhzY2hpbGR4aWFvYm8=,size_16,color_FFFFFF,t_70)



rostopic命令工具下的子命令，

> rostopic -h

> ```
> rostopic bw     display bandwidth used by topic
> rostopic echo   print messages to screen
> rostopic hz     display publishing rate of topic
> rostopic list   print information about active topics
> rostopic pub    publish data to topic
> rostopic type   print topic type
> ```

命令的使用(使用echo命令即可将节点上的消息打印在屏幕上)

> rostopic  echo [topic]  
>
> rostopic scho /turtle1/cmd_vel

### 3. ROS Message

节点之间的通讯通过在topic上发送消息实现



rostopic type 命令查看发布话题的消息类型,应该会显示：geometry_msgs/Twist

> rostopic type /turtle1/cmd_vel

rosmsg 可以查看消息的详细情况

> rosmsg show geometry_msgs/Twist

显示如下：

> ```
> geometry_msgs/Vector3 linear
>   float64 x
>   float64 y
>   float64 z
> geometry_msgs/Vector3 angular
>   float64 x
>   float64 y
>   float64 z
> ```



### 4. rostopic 上的 rostopic pub 数据推送

推送乌龟行进坐标到话题上

> ```
> rostopic pub -1 /turtle1/cmd_vel geometry_msgs/Twist -- '[2.0, 0.0, 0.0]' '[0.0, 0.0, 1.8]'
> ```

rostopic hz  数据发送频率的查询

> rostopic hz /turtle1/pose

### 5. rqt_plot 数据变化的绘图

教程中为$  rosrun rqt_plot rqt_plot 命令，实际直接使用rqt 即可调出plot窗口，如下

![rqt_plot.png](https://img-service.csdnimg.cn/img_convert/d79ed36b29c502071dc2a6f406b3cde7.png)

## 七. ROS服务和参数

服务（services）是节点之间通讯的另一种方式。服务允许节点发送**请求（request）** 并获得一个**响应（response）**

### 1. rosservice 命令

> ```
> rosservice list         输出可用服务的信息
> rosservice call         调用带参数的服务
> rosservice type         输出服务类型
> rosservice find         依据类型寻找服务find services by service type
> rosservice uri          输出服务的ROSRPC uri
> ```

工作流总结

> rosservice list      查询节点可用服务
>
> rosservice type  clear   查询服务类型
>
> rosservice call clear     调用clear服务，清空turtle_node  轨迹
>
> rosservice type spawn| rossrv show   查看spawn类型
>
> rosservice call spawn 2 2 0.2 ""      生成新的乌龟

### 2.  Using rosparam

rosparam 使得我们能够存储并操作ROS参数服务器上的数据

> ```
> rosparam set            设置参数
> rosparam get            获取参数
> rosparam load           从文件读取参数
> rosparam dump           向文件中写入参数
> rosparam delete         删除参数
> rosparam list           列出参数名
> ```

rosparam list 列出turtlesim节点上的参数

rosparam set and rosparam get     设置背景及查询

> ```
> rosparam set background_r 150
> rosparam get background_g
> ```

rosparam  dump & load   参数文件的写出及载入

> rosparam dump params.yaml
>
> rosparam dump params.yaml

将yaml文件重载进新的命名空间，如copy空间

> rosparam load params.yaml copy

get 出来看下

> rosparam get copy/background_b

## 八. 使用 rqt_console 和 roslaunch

### 1. 安装rqt & turtlesim

> ```
> sudo apt-get install ros-<distro>-rqt ros-<distro>-rqt-common-plugins ros-<distro>-turtlesim
> ```

使用命令行打开rqt_console  和 rqt_logger_level查看报警等级

```
$ rosrun rqt_console rqt_console
```

```
$ rosrun rqt_logger_level rqt_logger_level
```

### 2. 使用roslaunch

>  roslaunch beginner_tutorials

### 3. 编写&解析launch 文件

* 算是标记语言

### 4. 使用launch启动两个小乌龟并发送速度

> ```
> roslaunch beginner_tutorials turtlemimic.launch
> 
> ```

发送移动命令

> ```
> ostopic pub /turtlesim1/turtle1/cmd_vel geometry_msgs/Twist -r 1 -- '[2.0, 0.0, 0.0]' '[0.0, 0.0, -1.8]'
> ```

## 九. 使用rosed 编辑ros文件

### 1. 使用rosed编辑 文件

> rosed roscpp Logger.msg

### 2.  Tab 补全

tab

## 十. 创建ros消息和ros服务

 ### 1. 创建msg

创建msg文件夹及写入消息

> ```
> $ cd ~/catkin_ws/src/beginner_tutorials
> $ mkdir msg
> $ echo "int64 num" > msg/Num.msg
> ```

修改packagexml.确保可以被转换为C++，Python

> ```
>   <build_depend>message_generation</build_depend>
>   <exec_depend>message_runtime</exec_depend>
> ```

配置运行依赖，添加 message_runtime  修改add_message_file 函数

### 2. 使用rosmsg

> rosmsg show beginnet_tutorials/Num

## 十一. 编写消息发送器和订阅器

在beginn_tuturial下创建src文件夹，在里边编写talker.cpp listening.cpp文件

在CMakeList.txt文件中为可执行文件添加生成消息的依赖

编译文件  catkin_make

## 十二. 测试消息发布器和订阅器

### 1. source catkin工作空间下的setup.sh 文件

> ```
> # In your catkin workspace
> $ cd ~/catkin_ws
> $ source ./devel/setup.bash
> ```

2. 启动发布器和订阅器

> ```
> $ rosrun beginner_tutorials talker      (C++)
> $ rosrun beginner_tutorials talker.py   (Python) 
> $ rosrun beginner_tutorials listener     (C++)
> $ rosrun beginner_tutorials listener.py  (Python) 
> ```

## 十三. rosbag数据录制及回放

### 1. 录制数据

> rostopic list -v   显示所有话题
>
> mkdir ~/bagfiless
>
> cd ~/bagfiles
>
> rosbag record -a

### 2. 检查并回放数据

> rosbag info <your bagfile>      检查数据
>
> rosbag play <your bagfile>     回放数据

## 十四. roswtf工具的使用

### 1. ros的安装检查及运行时检查

ros运行时和不运行时均可通过指令检查

> roscd
>
> roswtf

### 2. ros错误报告

> roscd
>
> ROS_PACKAGE_PATH=bad:$ROS_PACKAGE_PATH roswtf

