# 有关ROS工作空间source的问题


参考的博客

[ROS中多个工作空间同时工作](https://wenku.baidu.com/view/5a17192515fc700abb68a98271fe910ef12dae04.html)

[解决ROS工作空间在bashrc中覆盖的问题](https://blog.csdn.net/weixin_42284263/article/details/108084682)

## 1、关于ROS工作空间覆盖的问题


ROS在查找包的时候，使用的是ROS_PACKAGE_PATH环境变量，此环境变量下的路径是ROS查找包的所有路径， 如果一个工作空间的路径不在不在ROS_PACKAGE_PATH环境变量中，ROS是找不到这个工作空间中的ROS功能包的，


当你已经执行过设置过ROS_PACKAGE_PATH的时候，例如已经执行过`source /opt/ros/melodic/setup.bash`的命令,这个命令就是将`/opt/ros/melodic/setup.bash # (ROS系统路径) `设置到ROS_PACKAGE_PATH中，通过echo命令可以查看当前的ROS_PACKAGE_PATH值，如下：

```bash
teng@tengh:~$ echo $ROS_PACKAGE_PATH 
/opt/ros/melodic/share
```


如果这时，再运行另一个工作空间的setup.bash脚本会怎么样呢？ 是覆盖掉之前的路径，还是共存呢？答案是都有可能，取决于这个工作空间执行catkin_make生成devel文件夹时的环境。

举例如下，我们已经运行过了`source /opt/ros/melodic/setup.bash`，这时ROS_PACKAGE_PATH的值为`opt/ros/melodic/share`。
然后我们再创建一个工作空间，代码完成后，执行`catkin_make`编译，编译完成后，我们再执行`source devel/setup.bash`，执行完成后，再次查看ROS_PACKAGE_PATH，结果如下：

```bash
teng@tengh:~/Atengh/zed2_ws$ echo $ROS_PACKAGE_PATH 
/opt/ros/melodic/share:/home/teng/Atengh/zed2_ws/src
```


并且，查看此时的`devel/_setup_util.py`文件

```python
  if not args.local:
      # environment at generation time
      CMAKE_PREFIX_PATH = r'/opt/ros/melodic'.split(';')
  else:
      # don't consider any other prefix path than this one

```


## 2.两个工作空间同时工作


明白了上面将的原理，那么让两个工作空间同时工作就很容易了，无非就是确保两个工作空间的目录都出现在ROS_PACKAGE_PATH环境变量。假设有两个工作空间`/home/teng/Ateng/work1_ws`和`/home/teng/Ateng/work2_ws`。

(1) 在work1_ws目录下执行catkin_make, 编译完成后`source devel/setup.bash`
(2) 在work2_ws目录下,先`source /home/teng/Ateng/work1_ws/devel/setup.bash`,再执行catkin_make编译，编译完成后`source devel/setup.bash`


此时再查看ROS_PACKAGE_PATH，结果如下

```bash
teng@tengh:~/Atengh/work2_ws$ echo $ROS_PACKAGE_PATH
/opt/ros/melodic/share:/home/teng/Atengh/work1_ws/src:/home/teng/Atengh/work2_ws/src
```


或者是直接修改`work2_ws/devel/_setup_util.py`，在里面加上`work1_ws/devel`的路径


```python
  if not args.local:
      # environment at generation time
      CMAKE_PREFIX_PATH = r'/opt/ros/melodic;/home/teng/Ateng/work1_ws/devel'.split(';')
  else:
      # don't consider any other prefix path than this one

```

然后重新source work2_ws，即可。


