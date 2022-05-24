


<!-- vim-markdown-toc GFM -->

* [zed2i相机ORB_SLAM2容易丢点问题相关说明](#zed2i相机orb_slam2容易丢点问题相关说明)
    * [1 容易丢点的原因：](#1-容易丢点的原因)
    * [2 zed_wrapper默认参数设置](#2-zed_wrapper默认参数设置)
        * [2.1 获取图像的频率](#21-获取图像的频率)
        * [2.2 发布图像的频率](#22-发布图像的频率)
        * [2.3 图像的分辨率](#23-图像的分辨率)
        * [2.4 其它](#24-其它)

<!-- vim-markdown-toc -->



# zed2i相机ORB_SLAM2容易丢点问题相关说明

## 1 容易丢点的原因：

1. 很容易的发现ORB_SLAM2示例中用到的公开数据集的默认fps都是在30~100左右，


而zed2i相机的默认配置参数中，fps值是15

2. 对于ORB_SLAM2来说，比较合适的图像的分辨率应该是在 `640*480 ~ 720p` 之间。


## 2 zed_wrapper默认参数设置

### 2.1 获取图像的频率

在`zed-ros-wrapper/zed_wrapper/params/common.yaml`文件中进行设置：

如下，相机的默认频率设置为了15

```yaml
grab_frame_rate:  15    # Frequency of frame grabbing for internal SDK operations
```

这里改成30或者60即可

这里的频率应该与ORB-SLAM2配置文件中的fps参数保持一致

### 2.2 发布图像的频率

发布频率的话，也可以在这里更改：

```yaml
pub_frame_rate:  15.0   # Dynamic - frequency of publishing of video and depth data
```

如果是在windows下的话，这些设置都是有一个可视化的界面可以去调节的




### 2.3 图像的分辨率

这里主要将两个设置

**1. 分辨率设置**

```yaml
resolution:  2   # '0': HD2K, '1': HD1080, '2': HD720, '3': VGA
```

根据官方帮助手册的说明，zed2i相机的分辨率与常规的尺寸不同，对于单个相机，具体如下：



|VIDEO MODE|OUTPUT RESOLUTIONE|	FRAME RATE (FPS)|
|----------|------------------|-----------------|
|2K        |	2208x1242     |15               |	
|1080p     |	1920x1080     |30, 15           |
|720p      |	1280x720	  |60, 30, 15       |
|VGA       |	672x376	      |100, 60, 30, 15  |




**2. 发布图像的尺寸与本地图像尺寸比例**

```yaml
img_downsample_factor:  0.5   # Resample factor for images [0.01,1.0] The SDK works with native image sizes, but publishes rescaled image.
```

例如，如果我们想要得到一个640x480分辨率的图像，难道我们还要手动处理吗？当然不是这样，我们这里通过设置`img_downsample_factor`这个参数为0.5，并将`resolution`设置为`HD720`，这样发布的图像就是我们想要的

### 2.4 其它

在相机的`zed-ros-wrapper/zed_wrapper/params/common.yaml`配置文件中，还有很多参数的可以设置,这里就不一一列举了

不仅仅是改变相机获取图像的频率，还有其它的设置，这些设置的说明都是可以在官方的帮助文档中看到.

zed2i官网的用户手册中是由对于这些的参数的描述的，具体可以参考[zed官网帮助文档-相机控制](https://www.stereolabs.com/docs/video/camera-controls/)

    
|SETTINGS     |     DESCRIPTION                                                                             |	VALUES                               |
|-------------|---------------------------------------------------------------------------------------------|----------------------------------------|
|Resolution   | Controls camera resolution.                                                                 |	HD2K, HD1080, HD720, VGA             |
|FPS          |	Controls frame rate.                                                                        |	15, 30, 60, 100                      |
|Brightness   |	Controls image brightness.                                                                  |	[0 - 8]                              |
|Contrast     |	Controls image contrast.                                                                    |	[0 - 8]                              |
|Hue          |	Controls image color.                                                                       |	[0 - 11]                             |
|Saturation   |	Controls image color intensity.                                                             |	[0 - 8]                              |
|Gamma        |	Controls gamma correction.                                                                  |	[0 - 8]                              |
|Sharpness    |	Controls image sharpness.                                                                   |	[0 - 8]                              |
|White Balance|	Controls camera white balance.                                                              |	[2800 - 6500]                        |
|Exposure     |Controls shutter speed.<br>Setting a long exposure time leads to an increase in motion blur. |	[0 - 100]<br>(% of camera frame rate)|
|Gain         |	Controls digital amplification of the signal from the camera sensors.                       |	[0 - 100]                            |





