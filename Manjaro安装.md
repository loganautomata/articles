# 安装Manjaro

## 2020年10月3日更新

video-hybrid-intel-nvidia-450xx-prime hybrid模式可以识别双显示屏, 但外接显示屏掉帧严重, 可以升级尝尝鲜.

## 制作USB启动盘

下载镜像, 制作USB启动盘. 推荐KDE镜像及使用[Rufus](https://rufus.ie/)制作.

> 制作时一定要用DD模式.

## 选择显卡驱动

光标停在Boot选项, 按E键.

![manjaro-0.jpg](https://img.oss.logan.ren/2020/04/13/manjaro-0.jpg)

将driver = free进行修改. 一般intel核显或AMD显卡, 选择free就好了, 也就是不需要改动, 回车开始安装就好了. 但如果是nvidia的显卡你需要将free改为nofree, 然后F10启动安装. 特殊情况: 如果你是游戏本上的intel + nvidia, 此时你需改为driver = intel, 然后F10启动安装.

![manjaro-1.jpg](https://img.oss.logan.ren/2020/04/13/manjaro-1.jpg)

> 无需改其他设置如语言, 时区... 安装时会重新选择.

## 安装系统

点击Launch Installer开始安装, 后面的选择语言, 时区等你看着选就行.

[![manjaro-2.jpg](https://img.oss.logan.ren/2020/04/13/manjaro-2.jpg)](https://gallery.loganren.xyz/image/hhq)

由于我这里是只装manjaro, 如果要安装同磁盘双系统, 需要手动分区, 建议谷歌, BiliBili上也有相关教程. 个人建议8G及以上无须交换分区, 以下的要选择交换分区, 如果还要带休眠的还需选择带休眠的交换分区.

![manjaro-3.jpg](https://img.oss.logan.ren/2020/04/13/manjaro-3.jpg)

> 如果卡在了93%上, 断网即可. 但现在新的安装镜像一般不会卡了.

## 修改pacman镜像源

按F12, 输入以下代码, 选择上海交大的源(全选也行, 随意)

```bash
sudo pacman-mirrors -i -c China -m rank
```

![manjaro-4.jpg](https://img.oss.logan.ren/2020/04/13/manjaro-4.jpg)

按F12, 输入以下代码.

```bash
sudo pacman -Syy
```

在 /etc/pacman.conf 文件中添加以下内容. (没有vim, 自带的是nano, 所以你现在只能用nano.)

```bash
[archlinuxcn]
SigLevel = Optional TrustedOnly
Server = https://mirrors.sjtug.sjtu.edu.cn/archlinux-cn/$arch
```

## 安装其他软件

按F12, 输入以下代码.

```bash
sudo pacman -Syy && sudo pacman -S archlinuxcn-keyring && sudo pacman -Syyu && sudo pacman -S yay vim fcitx-im fcitx-configtool && yay --aururl “https://aur.tuna.tsinghua.edu.cn” --save
```

>安装中途的选择, 选yes或则选默认.

新建 ~/.xprofile 文本. (已经安装vim了, 可以用vim了)

```bash
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```

重启. (系统更新时可能更新内核了, 重启生效.)

```bash
reboot
```

## 安装NVIDIA显卡驱动

单intel的就可以跳过这里了, 两个方法差不多, 懒人适用方法2.

### 方法1

使用manjaro自带的的prime模式, 用optimus-manager来切换显卡模式. 参考的manjaro论坛上的[这篇帖子](https://archived.forum.manjaro.org/t/guide-install-and-configure-optimus-manager-for-hybrid-gpu-setups-intel-nvidia/92196).

> 使用gnome界面的要去看原帖或使用方法2, 方法1仅仅针对kde.

在manjaro硬件驱动管理界面, 安装显卡驱动, 有bumblebee和prime模式, 我选择的prime模式, bumblebee模式你需要查看其它教程.

![显卡驱动](https://img.oss.logan.ren/2020/10/06/manjaro-6.jpg)

去 /etc/X11 /etc/X11/xorg.conf.d 这两个目录下删除所有与显卡有关的配置文件或者重命名为*.bak文件.

> 文件名包含intel, nvidia, mhwd, xorg的都是需要重命名或删除的.

打开 /etc/sddm.conf 文件并将 DisplayCommand DisplayStopCommand 这两行注释掉.

```bash
sudo vim /etc/sddm.conf

[X11]
# DisplayCommand=/usr/share/sddm/scripts/Xsetup
# DisplayStopCommand=/usr/share/sddm/scripts/Xstop
```

安装optimus-manager.

```bash
sudo pacman -Sy optimus-manager
```

使用以下命令切换三种模式.

```bash
optimus-manager --switch hybrid
optimus-manager --switch intel
optimus-manager --switch nvidia
```

修改默认模式, 

```bash
sudo cp /usr/share/optimus-manager.conf /etc/optimus-manager/optimus-manager.conf
sudo vim /etc/optimus-manager/optimus-manager.conf
```

#### 安装optimus-manager-qt(可选)

前提你要已经安装好yay和base-devel, 换了清华AUR源. 如果你是按照我的教程安装的话, 你现在安装base-devel组就OK了.

````bash
yay optimus-manager-qt
````

这个图形界面可以让你方便的改optimus-manager配置和切换显卡.

### 方法2

使用GayHub一个老哥的[脚本](https://github.com/dglt1/optimus-switch-sddm)安装显卡驱动, 懒人适用.

> 我这里把链接放出来, 希望大家用完后记得给个star.

```bash
sudo mhwd -i pci video-nvidia-390xx
```

> 去nvidia官网查询你显卡支持的驱动版本, 将390替换成你自己显卡版本, 不是太旧的390都OK所以一般不用换.

```bash
sudo pacman -S linux54-headers acpi_call-dkms xorg-xrandr xf86-video-intel git && sudo modprobe acpi_call && cd ~ && git clone https://github.com/dglt1/optimus-switch-sddm.git && cd ~/optimus-switch-sddm && chmod +x install.sh && sudo ./install.sh
```

> 如果你的内核版本不是5.4, 就要把54修改为你的内核版本.

使用以下命令后重启修改显卡驱动.

```bash
sudo set-intel.sh
sudo set-nvidia.sh
```

## 调整鼠标滚轮速度

安装imwheel.

```bash
sudo pacman -Sy imwheel
```

新建~/.imwheelrc文件.

```bash
".*"
None,      Up,      Button4, 3 //3为滚动行数
None,      Down,    Button5, 3
Control_L, Up,      Control_L|Button4
Control_L, Down,    Control_L|Button5
Shift_L,   Up,      Shift_L|Button4
Shift_L,   Down,    Shift_L|Button5
None,      Thumb1,  Alt_L|Left
None,      Thumb2,  Alt_L|Right
```

设置/usr/bin/imwheel开机自启动.

## 可选软件

谷歌浏览器

```bash
sudo pacman -Sy google-chrome
```

V2ray

```bash
sudo pacman -Sy v2ray qv2ray
```

VSCode

```bash
sudo pacman -Sy visual-studio-code-bin
```

网易云音乐

```bash
sudo pacman -Sy netease-cloud-music
```

WPS

```bash
sudo pacman -Sy wps-office ttf-wps-fonts archlinuxcn/wps-office-mui-zh-cn
```

谷歌拼音和云拼音

```bash
sudo pacman -Sy fcitx-googlepinyin fcitx-cloudpinyin
```

Typora

```bash
sudo pacman -Sy typora
```

NextCloud

```bash
sudo pacman -Sy nextcloud-client
```

Dock栏

```bash
sudo pacman -Sy latte-dock
```

## 注意事项

1. 时间不同步是因为Linux把主板里的时间当作的是UTC时间, 所以你需要用npt同步时间, 再将其写入主板即可. 此时Windows时间不对是因为Windows把它当作的是现在时间, 所以进Windows注册表将其修改为使用UTC时间. 建议统一使用UTC时间, 以后安装黑苹果的话就不用调整了.

2. 调整分辨率不要在显示屏设置那里改全局缩放, 去字体设置那里调固定字体DPI. 否则双屏时会出现各种意外情况.

3. 换官方源后要执行以下命令:

   ```bash
   sudo pacman -Syy
   ```

   换archlinuxcn源后要执行以下命令:

   ```bash
   sudo pacman -Syy && sudo pacman -S archlinuxcn-keyring
   ```

   换AUR源前需要提前安装好yay包管理器, 安装base-devel组.

   ```bash
   sudo pacman -Sy yay && base-devel
   ```

   >一般不用AUR源, 因为Arch的包更新比较激进, manjaro一般会慢几个版本更新来保持稳定.