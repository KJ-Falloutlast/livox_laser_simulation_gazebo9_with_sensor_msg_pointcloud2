# Introduction

* 解决livox系列雷达在gazebo9仿真环境中无法输出sensor_msgs::PointCloud2和livox_ros_driver/CustomMsg点云问题
* 修复https://github.com/lvfengchi/livox_laser_simulation代码中gazebo7和9无法适配问题

# Install

* 安装ode: https://bitbucket.org/odedevs/ode/src/master/
* catkin_make
* 将livox雷达的xacro文件导入到自己机器人中,其中publish_pointcloud_type = 3为发布livox_ros_driver/CustomMsg格式点云

```xml
  <xacro:Livox_gazebo_sensor visualize = "True" name = "livox_base" publish_pointcloud_type = "3"/>
```

# Result

* 将雷达加入到机器人中

![image-20240411103225912](https://ubuntu-desktop-pics.oss-cn-beijing.aliyuncs.com/image-20240411103225912.png)

* gazebo仿真

![image-20240411103148495](https://ubuntu-desktop-pics.oss-cn-beijing.aliyuncs.com/image-20240411103148495.png)
