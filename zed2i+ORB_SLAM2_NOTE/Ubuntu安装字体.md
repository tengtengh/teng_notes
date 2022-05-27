## Ubuntu系统安装字体

将下载的字体（*.ttf、*.TTF等）或者字体文件夹拷贝（`sudo cp -rf xxx`）到`/usr/share/fonts/`文件夹中。

执行如下命令

```bash
cd /usr/share/fonts/

# 赋予权限
sudo chmod -R 755 xxx 

# 创建雅黑字体的fonts.scale文件，它用来控制字体旋转缩放
sudo mkfontscale 

# 创建雅黑字体的fonts.dir文件，它用来控制字体粗斜体产生
sudo mkfontdir

# 建立字体缓存信息，也就是让系统认识雅黑
sudo fc-cache -fv 
sudo fc-cache 
```
