Welcome to the Ubuntu Deep Learning System Environment Setting wiki!

## Requirement
- [Ubuntu gnome (14.04LTS)](https://wiki.ubuntu.com/UbuntuGNOME/GetUbuntuGNOME/LTS)
- [cuda8.0 (.deb)](https://developer.nvidia.com/compute/cuda/8.0/prod/local_installers/cuda-repo-ubuntu1404-8-0-local_8.0.44-1_amd64-deb)
- [opencv (2.4.13)](https://github.com/Itseez/opencv/archive/2.4.13.zip)
- [Qt5.4](http://download.qt.io/official_releases/qt/5.4/5.4.0/)
- cmake 

## Preparation
1. change the grub to suit the monitor(make sure there will no black screen when we install GPU driver)
  - sudo vi /etc/default/grub
  - Find the line: `GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"`
  - add `nomodeset` after `splash`
  - `sudo update-grub`

2. change the source of ubuntu
  - `http://mirrors.ustc.edu.cn/ubuntu`

3. `sudo apt-get update`

4. install libraries
  - `sudo apt-get -y remove ffmpeg x264 libx264-dev`
  - `sudo apt-get -y install libopencv-dev`
  - `sudo apt-get -y install build-essential checkinstall cmake pkg-config yasm`
  - `sudo apt-get -y install libtiff4-dev libjpeg-dev libjasper-dev`
  - `sudo apt-get -y install libavcodec-dev libavformat-dev libswscale-dev libdc1394-22-dev libxine-dev libgstreamer0.10-dev libgstreamer-plugins-base0.10-dev libv4l-dev`
  - `sudo apt-get -y install python-dev python-numpy`
  - `sudo apt-get -y install libtbb-dev`
  - `sudo apt-get -y install libqt4-dev libgtk2.0-dev`
  - `sudo apt-get -y install libfaac-dev libmp3lame-dev libopencore-amrnb-dev libopencore-amrwb-dev libtheora-dev libvorbis-dev libxvidcore-dev`
  - `sudo apt-get -y install x264 v4l-utils ffmpeg`
  - `sudo apt-get -y install libgtk2.0-dev`

## cuda
- `sudo dpkg -i xxx.deb`
- `sudo apt-get update`
- `sudo apt-get install cuda`
- check if installed
  - `nvidia-smi`
- reboot

## Qt
- `chmod +x xxx.run`
- `./xxx.run`
 
## opencv
- `unzip opencv-xxx.zip`
- `cd opencv-xxx/`
- `mkdir build`
- `cd build`
- `cmake -D CUDA_ARCH_BIN=6.1 -D CUDA_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local  -D WITH_TBB=ON -D BUILD_NEW_PYTHON_SUPPORT=ON -D WITH_V4L=ON -D INSTALL_C_EXAMPLES=ON -D INSTALL_PYTHON_EXAMPLES=ON -D BUILD_EXAMPLES=ON -D WITH_CUDA=ON -D WITH_OPENGL=ON ..`
- `make -j8`
- `sudo make install`
- add the lib path
  - `sudo sh -c 'echo "/usr/local/lib" > /etc/ld.so.conf.d/opencv.conf' `
  - `sudo ldconfig`
  - `sudo vi /etc/profile `
  - add `this` to the last line: `export  PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig`
  - source /etc/profile
- Error solve
  - CUDA version is too high

```
error: ‘NppiGraphcutState’ has not been declared
error: ‘NppiGraphcutState’ does not name a type
```
edit the graphcuts.cpp file

change the 
```
#include "precomp.hpp"
#if !defined (HAVE_CUDA) || defined (CUDA_DISABLER)
````
to 
```
#include "precomp.hpp"
#if !defined (HAVE_CUDA) || defined (CUDA_DISABLER)  || (CUDART_VERSION >= 8000)
```

  - other

```
modules/gpu/src/nvidia/core/NCVPixelOperations.hpp(51): error: a storage class is not allowed in an explicit specialization
```

edit the NCVPixelOperations.hpp

`:%s/temp;ate<> static inline/template<> inline/g`

## Other personal setting
- Java

```
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer
sudo apt-get install oracle-java8-set-default
```

- shadowsocks

```
sudo add-apt-repository ppa:hzwhuang/ss-qt5
sudo apt-get update
sudo apt-get install shadowsocks-qt5
```

- SwitchOmega
  - install: https://github.com/FelisCatus/SwitchyOmega/releases/
  - online rules: https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt

- Numix theme

```
sudo add-apt-repository ppa:numix/ppa
sudo apt-get update
sudo apt-get install numix-gtk-theme numix-icon-theme
```

- OpenTerminalInFloder

```
sudo apt-get install nautilus-open-terminal
```

- NumberLock

```
sudo apt-get install numlockx
sudo vim /etc/gdm/Init/Default
```

add `following` before `exit 0`

```
if [ -x /usr/bin/numlockx ]; then
numlockx on
fi
```

- keyboard-configuration

`sudo dpkg-reconfigure keyboard-configuration`

for my keyboard, i just pick 104 keys

- Input Error

`ibus-daemon -drx`

- Matlab opengl

`alias matlab=matlab -softwareopengl`

- Change the folder name
```
为了使用起来方便，装了Ubuntu中文版，自然在home文件里用户目录的“桌面”、“图片”、“视频”、“音乐”……都是中文的。很多时候都喜欢在桌面上放一些要操作的文件，linux里命令行操作又多，难免会用命令行操作桌面上的东西，那么就要 “cd 桌面”，打“桌面”的时候要输入法切换，麻烦……所以就想办法把用户目录下的路径改成英文，而其他的中文不变， 方法如下：

 

　　打开终端，在终端中输入命令:

　　export LANG=en_US

　　xdg-user-dirs-gtk-update

 

　　跳出对话框询问是否将目录转化为英文路径,同意并关闭.

 

　　在终端中输入命令:

　　export LANG=zh_CN

 

　　关闭终端,并重起.下次进入系统,系统会提示是否把转化好的目录改回中文.选择不再提示,并取消修改.主目录的中文转英文就完成了.

===============================================================================  

　　在有些linux发行板中，上面的命令无法使用，不过我们可以动过手动修改的方式达到，具体方法路下：

找到～/.config/user-dirs.dis文件

（注：~/代表当前用户目录   .config是个隐藏文件）

将该文件中的中文改成对应的英文

 

再在～/目录下创建对应的英文文件夹，重启就可以了
```
