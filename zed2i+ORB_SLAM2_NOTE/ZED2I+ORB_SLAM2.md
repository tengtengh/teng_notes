### 1.安装cuda10.2


### 2.安装sdk

### 3.编译ORB_SLAM2

### 4.编译zed2i



### 5.

第1个终端：


```bash

# cd到catkin_ws文件夹
devel/setup.bash
catkin_ws$ roscore
```



第2个终端：


```bash
# cd到catkin_ws文件夹
source devel/setup.bash 
roslaunch zed_wrapper zed2i.launch
```





第3个终端：


**对于双目来说：**

先输入
```bash
rostopic list
```
查看节点，可以在里面找到矫正后的左右目节点

```bash
/zed2i/zed_node/left/image_rect_color
/zed2i/zed_node/right/image_rect_color
```

将 .../ORB_SLAM2/Examples/ROS/ORB_SLAM2/src/ros_stereo.cc文件中，做出如下修改：


```c++
    // message_filters::Subscriber<sensor_msgs::Image> left_sub(nh, "/camera/left/image_raw", 1);
    // message_filters::Subscriber<sensor_msgs::Image> right_sub(nh, "camera/right/image_raw", 1);
    message_filters::Subscriber<sensor_msgs::Image> left_sub(nh, "/zed2i/zed_node/left/image_rect_color", 1);
    message_filters::Subscriber<sensor_msgs::Image> right_sub(nh, "/zed2i/zed_node/right/image_rect_color", 1);
```

重新`./build_ros.sh`编译ORB_SLAM2 ROS



```bash

cd xx/xx/ORB_SLAM2

source Example/ROS/ORB_SLAM2/build/devel/setup.bash

rosrun ORB_SLAM2 Stereo Vocabulary/ORBvoc.txt Examples/ROS/ORB_SLAM2/zed2i_MonoHD2k.yaml false

```
参考ORB_SLAM2的README.md文件，如果输入的是已经矫正过的图像，这一步最后的值可以设置为false，那么将不会读这个`.yaml`文件，这个文件是什么也就无所谓了，可以随便选一个例如`Example/Sereo/Examples/Stereo/EuRoC.yaml `

双目节点即可运行。


**对于单目**

单目图像的相机标定文件则是必要的：
`rostopic list`查看
比如这里我选择的是`/zed2i/zed_node/rgb/image_rect_color`,在ros_mono.cc中做修改,并重新编译。

`rostopic echo /zed2i/zed_node/rgb/camera_info`查看相机的参数



```
```





