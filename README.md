# robot

---
# 2026 RAICOM 睿抗机器人开发者大赛 - 魔力元宝 (工业组)

## 📖 项目简介

本仓库包含了 2026 年睿抗机器人开发者大赛（RAICOM）CAIR 强体赛道轮式机器人赛项“魔力元宝”赛题工业组的完整 ROS 仿真工程项目 。

本项目基于**元创兴 Oryx_A 智能复合机器人**进行开发，涵盖了智能机器人环境建模、自主导航、避障、机械臂精准抓取、AR 码识别与定位，以及基于 YOLO 算法的目标检测等核心功能 。

## 🎯 核心任务说明

本项目主要实现了工业组选拔赛的两个核心任务：


**任务一：基于 AR 码的仓储机器人开发** 


* 机器人导航至指定加工中心 。


* 通过视觉识别指定的 AR 码（如 ID 8 和 ID 3），机械臂准确抓取对应物块并放置到中转箱 。


* 完成物料出库后，机器人自主导航至充电桩区域进行模拟回充，最后返回出发区待机 。




  
**任务二：基于目标检测的仓储机器人开发** 


* 集成 YOLOv5 目标检测算法，识别不同类别的工业产品物块（伺服电机、转子、控制芯片等半成品） 。


* 机器人依次前往一号和二号加工中心抓取半成品物块 。


* 前往三号加工中心进行物料装配，抓取装配成品并最终导航回出发区出库 。





## ⚙️ 环境依赖

**操作系统**: Ubuntu (ROS 环境) 


**核心框架**: ROS 1 (通过 `catkin_make` 编译) 


**仿真与视觉**: Gazebo, rqt_image_view 


**深度学习框架**: YOLOv5 (用于目标检测，运行 `yolo_node.py`) 



## 🚀 快速开始 (Quick Start)

请确保你的所有终端都已在 `~/ros_workspace` 目录下打开 。

### 运行任务一（AR 码抓取与搬运）

1. **启动基础仿真环境与摄像头节点** 


```bash
source devel/setup.bash
roslaunch oryxbot_cruise_ex runtask.launch
rqt_image_view

```


2. **生成赛题场景与物块** 


* 新建终端运行抽签脚本：


```bash
./reinovo_oryxbot_sim

```


* 在终端提示中选择题目 `1` 。




3. **编译并启动任务一控制节点** 


* 新建终端，依次输入：


```bash
catkin_make
source devel/setup.bash
roslaunch oryxbot_cruise_ex runtask1.launch

```


4. **触发任务执行** 


* 再新建一个终端，发送启动指令：


```bash
rosservice call /start_stop_nav "mode: 'start'
level: 0"

```



### 运行任务二（YOLO 目标检测与装配）

1. **启动基础仿真环境** 


```bash
source devel/setup.bash
roslaunch oryxbot_cruise_ex runtask.launch
rqt_image_view

```


2. **生成赛题场景** 


```bash
./reinovo_oryxbot_sim

```


* 在终端提示中选择题目 `2` 。




3. **启动任务二控制节点** 


* 新建终端编译并启动：


```bash
catkin_make
source devel/setup.bash
roslaunch oryxbot_cruise_ex runtask2.launch

```


4. **启动 YOLOv5 识别节点** 


* 新建终端启动深度学习节点：


```bash
rosrun yolov5_ros yolo_node.py

```


5. **触发任务执行** 


* 新建终端，发送启动指令：


```bash
rosservice call /start_stop_nav "mode: 'start'
level: 0"

```



## 🛠️ 配置与参数修改

在比赛调试阶段，如果抽签获得了不同的加工中心路线或 AR 码 ID，需要修改以下源文件：

* **修改目标 AR 码 ID** 


* 文件路径：`oryxbot_cruise_ex.cpp` 


* 修改内容：在第 24 行左右，找到 `ar_id_order_pick = {8, 3};`，将其修改为你抽到的实际物块 ID 组合 。




* **修改导航路径点** 


* 文件路径：`task1.yaml` 


* 修改内容：根据需要前往的场馆顺序（如先去 4 号再去 5 号馆），在 yaml 文件中修改目标点的基坐标及执行先后顺序 。





> **⚠️ 注意事项**：
> * 每次修改完 `.cpp` 或参数文件后，**必须**重新执行 `catkin_make` 和 `source devel/setup.bash` 以更新环境 。
> * 启动选题脚本时，请勿关闭当前的地图环境终端 。
> 
> 

---
