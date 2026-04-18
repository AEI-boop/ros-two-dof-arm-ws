# ros-two-dof-arm-ws

一个用于 **二自由度机械臂 URDF / Xacro 建模与 RViz 可视化** 的 ROS1 `catkin` 工作空间。

当前工作空间只包含一个核心 package：`two_dof_arm_description`，适合用于：

- 学习机械臂 URDF / Xacro 建模
- 观察关节、连杆、夹爪的层级关系
- 在 RViz 中联动查看关节姿态变化
- 为后续 Gazebo 控制或运动学实验打基础

---

## 1. 目录结构

```text
two_dof_arm_ws/
├── .catkin_workspace
├── .gitignore
├── README.md
└── src/
    ├── CMakeLists.txt
    └── two_dof_arm_description/
        ├── CMakeLists.txt
        ├── config/
        │   └── urdf.rviz
        ├── launch/
        │   └── display.launch
        ├── package.xml
        └── urdf/
            └── two_dof_arm.xacro
```

---

## 2. 包说明

### `two_dof_arm_description`

定义一个二自由度机械臂模型，关键文件包括：

- `src/two_dof_arm_description/urdf/two_dof_arm.xacro:1`
  - 机械臂主体模型
  - 包含 `base_link`、`link1`、`link2`、`gripper_link`
  - 包含 `joint1`、`joint2` 和一个 `gripper_joint`
- `src/two_dof_arm_description/launch/display.launch:1`
  - 加载 Xacro
  - 启动 `joint_state_publisher(_gui)`
  - 启动 `robot_state_publisher`
  - 打开 RViz
- `src/two_dof_arm_description/config/urdf.rviz`
  - RViz 预配置

从 `src/two_dof_arm_description/package.xml:1` 可见，主要依赖包括：

- `catkin`
- `geometry_msgs`
- `roscpp`
- `rospy`
- `std_msgs`
- `urdf`
- `xacro`

---

## 3. 模型说明

根据 `src/two_dof_arm_description/urdf/two_dof_arm.xacro:1`：

- `joint1`：底座旋转关节，绕 Z 轴转动
- `joint2`：第二关节，绕 Y 轴转动
- `gripper_joint`：夹爪伸缩关节，类型为 `prismatic`

模型中还定义了：

- 基础惯性参数宏 `default_inertial`
- 3 个 transmission
- `gazebo_ros_control` 插件入口

这意味着它不仅适合做 RViz 展示，也为后续 Gazebo 控制保留了扩展接口。

---

## 4. 环境要求

建议环境：

- Ubuntu 20.04
- ROS Noetic
- Python 3

至少需要安装：

- `xacro`
- `urdf`
- `joint_state_publisher`
- `joint_state_publisher_gui`
- `robot_state_publisher`
- `rviz`

---

## 5. 构建方式

```bash
cd ~/two_dof_arm_ws
catkin_make
source devel/setup.bash
```

如果你的工作空间目录不在 `~/two_dof_arm_ws`，请替换为实际路径。

---

## 6. 运行方式

### 6.1 RViz 可视化

```bash
roslaunch two_dof_arm_description display.launch
```

这个 launch 会执行以下动作：

1. 通过 Xacro 加载机械臂模型
2. 启动 `joint_state_publisher_gui`
3. 启动 `robot_state_publisher`
4. 打开 RViz 并加载预设配置

对应文件：`src/two_dof_arm_description/launch/display.launch:1`

### 6.2 无 GUI 模式

```bash
roslaunch two_dof_arm_description display.launch gui:=false
```

这样会使用 `joint_state_publisher` 代替 GUI 版本。

---

## 7. 适合扩展的方向

这个工作空间目前聚焦于“描述与可视化”，后续可以继续扩展：

- 加入 Gazebo 世界文件与控制器
- 添加 MoveIt 运动规划配置
- 增加正逆运动学求解脚本
- 将夹爪控制接入实际话题或服务

---

## 8. 常见问题

### 8.1 `xacro` 找不到

请确认已经 source ROS 环境：

```bash
source /opt/ros/noetic/setup.bash
```

### 8.2 RViz 中模型不显示

检查：

- 是否执行了 `source devel/setup.bash`
- `robot_description` 参数是否成功加载
- RViz Fixed Frame 是否与模型 TF 一致

---

## 9. 关键文件索引

- 模型定义：`src/two_dof_arm_description/urdf/two_dof_arm.xacro:1`
- 启动文件：`src/two_dof_arm_description/launch/display.launch:1`
- 包依赖：`src/two_dof_arm_description/package.xml:1`
