# 安装FileRun

## 宝塔建立网站

宝塔内建立新站点命名为filerun, 需同时建立数据库. 关闭该站点的防跨站攻击, 清空站点文件夹下所有文件. 下载[FileRun](https://www.filerun.com/download)并上传至filerun网站根目录下解压, 删除压缩包.

![filerun-00.jpg](https://img.oss.logan.ren/2020/04/17/filerun-00.jpg)

## PHP配置

安装ionCube, imagemagick, exif, opcache扩展

![filerun-01.jpg](https://img.oss.logan.ren/2020/04/17/filerun-01.jpg)

解除exec函数的禁用. 点击删除即可.

![filerun-03.jpg](https://img.oss.logan.ren/2020/04/17/filerun-03.jpg)

重启php, 点击重启即可.

![filerun-0381d3e391753b9839.jpg](https://img.oss.logan.ren/2020/04/17/filerun-0381d3e391753b9839.jpg)

## 安装FFmpeg

以下为debian安装命令.

```
apt update && apt install ffmpeg
```

## 安装FileRun

打开filerun网站的网址, 一路next, 如果检查时有任何一项不为OK, 需查看报错原因, 重新对照教程安装.

![filerun-04.png](https://img.oss.logan.ren/2020/04/17/filerun-04.png)

找到刚刚随网站建立的数据库的数据库名, 用户名, 密码, 然后填入.

![filerun-05.jpg](https://img.oss.logan.ren/2020/04/17/filerun-05.jpg)

记下你的默认账户密码.

![filerun-06.jpg](https://img.oss.logan.ren/2020/04/17/filerun-06.jpg)

好了, 到这里安装完成, 自行设置下网站就可以开始使用了.