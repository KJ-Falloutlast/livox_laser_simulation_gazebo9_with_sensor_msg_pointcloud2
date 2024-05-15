# Introduction

* 解决livox系列雷达在gazebo9仿真环境中无法输出sensor_msgs::PointCloud2和livox_ros_driver/CustomMsg点云问题
* 修复`https://github.com/lvfengchi/livox_laser_simulation`代码中gazebo7和9无法适配问题

# Install

* 安装ode: https://bitbucket.org/odedevs/ode/src/master/
* catkin_make
* 将livox雷达的xacro文件导入到自己机器人中,其中publish_pointcloud_type = 3为发布livox_ros_driver/CustomMsg格式点云

```xml
  <xacro:macro name="Livox_gazebo_sensor" params="visualize:=True update_rate:=10 resolution:=0.002 noise_mean:=0.0 noise_stddev:=0.01 name:=livox ros_topic:=/livox/lidar publish_pointcloud_type:=1">
    <gazebo reference="${name}">
      <sensor type="ray" name="${name}">
        <pose>0 0 0 0 0 0</pose>
        <visualize>True</visualize>
        <update_rate>${update_rate}</update_rate>
        <!-- This ray plgin is only for visualization. -->
        <plugin name="gazebo_ros_laser_controller" filename="liblivox_laser_simulation.so">
        <robotNamespace>urobot</robotNamespace>
        <bodyName>livox_base</bodyName>
          <ray>
            <scan>
            <horizontal>
            <samples>100</samples>
            <resolution>1</resolution>
            <min_angle>${-horizontal_fov/360*M_PI}</min_angle>
            <max_angle>${horizontal_fov/360*M_PI}</max_angle>
            </horizontal>
            <vertical>
            <samples>50</samples>
            <resolution>1</resolution>
            <min_angle>${-vertical_fov/360*M_PI}</min_angle>
            <max_angle>${vertical_fov/360*M_PI}</max_angle>
            </vertical>
            </scan>
            <range>
            <min>${laser_min_range}</min>
            <max>${laser_max_range}</max>
            <resolution>0.002</resolution>
            </range>
            <noise>
            <type>gaussian</type>
            <mean>${noise_mean}</mean>
            <stddev>${noise_stddev}</stddev>
            </noise>
          </ray>
          <visualize>${visualize}</visualize>
          <samples>${samples}</samples>
          <downsample>1</downsample>
          <csv_file_name>package://livox_laser_simulation/scan_mode/avia.csv</csv_file_name>
          <publish_pointcloud_type>${publish_pointcloud_type}</publish_pointcloud_type>
          <ros_topic>${ros_topic}</ros_topic>
        </plugin>
      </sensor>
      <sensor name="livox_imu" type="imu">
        <always_on>true</always_on>
        <update_rate>50</update_rate>
        <visualize>true</visualize>
        <topic>"/livox/imu"</topic>
        <plugin filename="libgazebo_ros_imu_sensor.so" name="imu_plugin">
          <topicName>/livox/imu</topicName>
          <bodyName>imu_base_link</bodyName>
          <updateRateHZ>10.0</updateRateHZ>
          <xyzOffset>0 0 0</xyzOffset>
          <gaussianNoise>0.0026399240948208127</gaussianNoise>
          <rpyOffset>0 0 0</rpyOffset>
          <frameName>imu_base_link</frameName>
          <initialOrientationAsReference>false</initialOrientationAsReference>
        </plugin>
        <pose>0.05512, 0.02226, -0.0297 0 0 0</pose>
      </sensor>
    </gazebo>
  </xacro:macro>
  <xacro:Livox_gazebo_sensor visualize = "True" name = "livox_base" publish_pointcloud_type = "3"/>
```

# Result

* 将雷达加入到机器人中

![image-20240411103225912](https://ubuntu-desktop-pics.oss-cn-beijing.aliyuncs.com/image-20240411103225912.png)

* gazebo仿真

![image-20240411103148495](https://ubuntu-desktop-pics.oss-cn-beijing.aliyuncs.com/image-20240411103148495.png)
