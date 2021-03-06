---
layout: post
title: Ubuntu Install Tips
date: 2017-7-5
categories: blog
tags: [技术总结]
description: Linux重装系统&软件
---

## First part for reinstall Ubuntu

### Ubuntu partition

         \boot (primary)  300M
         \
         \swap  memory * 2
         \home

如果将/boot单独分区，则/boot为主分区，而/分区不必是主分区。  
分区完成后，选择*/home*作为Ubuntu安装位置.  
**device for boot loader installation:**  
选择*Ubuntu安装的/boot位置*.

**Note that**: reinstall ubuntu, partition should have something different from before!!

### 如果需要安装系统的电脑是dell-Inspiron-7559

当安装完Windows后，确认BIOS中的**安全启动**和**快速启动**关闭，进入Ubuntu安装盘，如果长时间没有进入，卡在Ubuntu这几个字的紫色背景页面，请跳转到以下链接
[connorkuehl](https://connorkuehl.github.io/dell-inspiron-7559-linux-guide/index.html)

- Highlight the Install Ubuntu entry and press e on your keyboard.  
- Modify the line that contains quiet splash at the end and change it to:
```
nomodeset i915_bpo.nomodeset=1 quiet splash
```
这是为了关闭显卡，使用BIOS安装功能，这是由于电脑本身的缺陷.  

- Press F10 to boot.
- Proceed with the installation as normal.
- Reboot.

开机后如果无法关机，请按照【Ubuntu unmet problem】显卡安装的方法，安装显卡驱动，或者参考以上链接  
Now I think I should do it, write them down and log everything. I am exciting now.  

### 如果需要安装系统的电脑是XPS15-i5

You should be able to get away with switching from RAID to AHCI without re-imaging windows using [these instuctions](http://triplescomputers.com/blog/uncategorized/solution-switch-windows-10-from-raidide-to-ahci-operation/).

1. Right-click the Windows Start Menu. Choose Command Prompt (Admin).
* If you don’t see Command Prompt listed, it’s because you have already been updated to a later version of Windows.  If so, use this method instead to get to the Command Prompt:  
- Click the Start Button and type cmd  
- Right-click the result and select Run as administrator  
2. Type this command and press ENTER: ```bcdedit /set {current} safeboot minimal```  
* If this command does not work for you, try ```bcdedit /set safeboot minimal```(useful for me)  
3. Restart the computer and enter BIOS Setup (the key to press varies between systems).  
4. Change the SATA Operation mode to **AHCI** from either **IDE or RAID** (again, the language varies).  
5. Save changes and exit Setup and Windows will automatically boot to Safe Mode.  
6. Right-click the Windows Start Menu once more. Choose Command Prompt (Admin).  
7. Type this command and press ENTER: ```bcdedit /deletevalue {current} safeboot```  
* If you had to try the alternate command above, you will likely need to do so here also: ```bcdedit /deletevalue safeboot```  
8. Reboot once more and Windows will automatically start with **AHCI** drivers enabled.  

## Install Sougou pinyin

Ubuntu 14.04 install sougoupinyin

- First, download .deb install package and install, then go to language support and modify keyboard input system to fcitx and apply all system.
- Go to fcitx configure (in windows and search a penguin called fcitxconfigure) and click + and cancle only show current language , at the same time find Sogou Pinyin and add.
- Reboot!

## SSH Installation

```sudo apt-get install openssh-server```后，可以使用ssh连接其他电脑，即
```ssh bryan@192.168.1.254```.

## Setting github ssh

1. Install git | Sign up an account for github

2. 配置Name和Email
```
命令格式：   git config --global user.name "your name"
            git config --global user.email "your email address"
```
3. Create Public/Private RSA Key
```
命令格式：    ssh-keygen -C "your email address" -t rsa
```
注意图中红色数字标注：  
        - 设置Public RSA Key的保存位置，直接回车采用默认地址  
        - 设置一个密码，并再次输入确认(这里不建议设置，方便本地使用)  
        - Public RSA Key的保存路径：*c:\users\username\.ssh\id_rsa.pub*(windows) and */home/inin/.ssh/id_rsa.pub*(linux)  
4. 将Public Key告知Github
```
请首先注册一个github账号，Home Page：https://github.com/ 。然后进入Account Settings页面，打开SSH Keys，
点击“Add SSH Key”。打开c:\users\username\.ssh\id_rsa.pub，把里面的内容全部Copy到Key对应的输入框内，点击“Add Key”。
```

5. Now you can try push a project now:  
在 github.com 上*new repository* (选add readme）  
在本地文件夹下
```
git clone git@github.com:bryanibit/pySfM.git
git init
touch README.md (由于选择了readme)
git add README.md
git commit -m 'first_commit'
git remote add origin https://github.com/findingsea/myRepoForBlog.git
git push -u origin master (-u is the first time to upload)
```

## Install Cmake 3.0 +

```
sudo -E add-apt-repository -y ppa:george-edison55/cmake-3.x
sudo -E apt-get update
sudo apt-get install cmake
```
I still recommand to use the official tutorial as shown the following:
```sh
wget https://github.com/Kitware/CMake/releases/download/v3.15.2/cmake-3.15.2.tar.gz
tar -zxvf cmake-3.15.2.tar.gz
cd cmake-3.15.2
./bootstrap
make
sudo make install
cmake --version
```

## Install meshlab

```
$ sudo add-apt-repository ppa:zarquon42/meshlab
$ sudo apt-get update
$ sudo apt-get install meshlab
```
Optional, to **remove** meshlab first, do:

```
$ sudo apt-get remove meshlab
```

Or, if you want to uninstall meshlab, disable the recently added PPA and downgrade all the packages that got updated via the PPA, do:

```
$ sudo apt-get install ppa-purge
$ sudo ppa-purge ppa:zarquon42/meshlab
```

## Install pycharm and crack it

:panda_face: Install from PPA
```
sudo add-apt-repository ppa:mystic-mirage/pycharm
sudo apt-get update
sudo apt-get install pycharm  ## professional version
```

* Download official version professional
* go to [crack](http://idea.lanyus.com/)
* add “0.0.0.0 account.jetbrains.com” to /etc/hosts file

:dragon_face: Activate Pycharm

**1. Active server:** http://www.activejetbrains.gq (2017.3.4以上紧急车道 其他车辆请避让)  
**2. 激活server：** http://idea.imsxm.com/

:monkey_face: Append exe to path env
```
select 'for all users'
PATH=$PATH:/home/inin/{installation path}/bin
```

### 配置pycharm/clion使用ros

设置快捷键，使从快捷方式启动PyCharm的同时加载ROS环境变量
```
gedit ~/.local/share/applications/jetbrains-pycharm.desktop   #如果选择安装为当前用户
```

或者
```
gedit /usr/share/applications/jetbrains-pycharm.desktop       #如果选择为全部用户
```

修改Exec变量一行，在=后添加 bash -i -c 即改为（若为zsh，则改为zsh)
```
Exec= bash -i -c "/home/ubu/tools/pycharm-professional-2016.2.3/bin/pycharm.sh" %f
```

在clion中 Opening catkin packages (remember to ```source devel/setup.bash```)
```
File > Open Select the src/CMakeLists.txt (```catkin_init_workspace``` produces cmakelists.txt and cp it into your catkin_ws)
and open as Project (or open the folder containing CMakeLists.txt)
```

在clion调试中include经常找不到生成的自定义message，在clion-file-settings-build-CMake中，在CMake Default populate option
```
-DCATKIN_DEVEL_PREFIX=/home/bryan/catkin_ws/devel
-DCMAKE_INSTALL_PREFIX=/home/bryan/catkin_ws/install
```
选择build目录为*../build*.

### 使用pycharm时import第三方库

Pycharm 和 Clion 2018.1 版本下载地址 (链接: https://pan.baidu.com/s/1mniZLPgLqr9ViwfM7QfBGA 密码: bqwc)

```
settings--project interpreter--more(the bottom of add local)--show path for the selected
```
```
interpreter-add+--/home/inin/OpenDroneMap/Superbuild/install/python2.7/dist-package
```

### Configure VitualEnv

**To create a virtual environment**  
    1. In the *Settings/Preferences* dialog (*Ctrl+Alt+S*), select Project: ```<project name> | Project Interpreter```.  
    2. In the *Project Interpreter* page, click icons :gear: and select *Add*.  
    3. In the left-hand pane of the *Add Python Interpreter* dialog box, select *Virtualenv Environment*. The following actions depend on whether the virtual environment existed before.

If **New environment** is selected (not run ```virtualenv ENV```):

       1. Specify the location of the new virtual environment in the text field, or click (virtual environment location) and find location in your file system. Note that the directory where the new virtual environment should be located, must be empty!

       2. Choose the base interpreter from the drop-down list, or click (choose the base interpreter) and find the base interpreter in the your file system.

       3. Select the *Inherit global site-packages* check-box if you want to inherit your global site-packages directory. This check-box corresponds to the *--system-site-packages* option of the **virtualenv** tool.

       4. Select the *Make available to all projects* check-box, if needed.

If **Existing environment** is selected (have run ``` virtualenv ENV```):

       1. Specify the required interpreter: use the drop-down list, or click (Select an interpreter) and find ```bin/python2.7``` in your file system.

       2. Select the check-box *Make available to all projects*, if needed.

       3. Click OK to complete the task.

## install zsh

```
sudo apt-get install zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
chsh -s /bin/zsh
sudo reboot
```

## install vim

```
sudo apt-get install vim
```

Renew a file called .vimrc:
```
set nu
set confirm
set mouse=a
set tabstop=4
set cindent
syntax on
```

## 安装NVIDIA驱动方法

卸载可能存在的旧版本 nvidia 驱动（对没有安装过 nvidia 驱动的主机，这步可以省略，但推荐执行，无害）
```
         sudo apt-get remove --purge nvidia*
```
安装驱动可能需要的依赖(可选)
```
         sudo apt-get update

         sudo apt-get install dkms build-essential
```
把 nouveau 驱动加入黑名单
```
         sudo vim /etc/modprobe.d/blacklist-nouveau.conf
```
  在文件 blacklist-nouveau.conf 中加入如下内容：

```
  blacklist nouveau
  blacklist lbm-nouveau
  blacklist nvidia-173
  blacklist nvidia-96
  blacklist nvidia-current
  blacklist nvidia-173-updates
  blacklist nvidia-96-updates
  options nouveau modeset=0
  alias nouveau off
  alias lbm-nouveau off
  alias nvidia nvidia_current_updates
```

```sudo update-initramfs -u```重启后再次进入字符终端界面，并关闭图形界面

          sudo service lightdm stop

### (Thinkpad T480/T470P)

安装上面的教程将nouveau关闭,然后按照following:

```
$ sudo add-apt-repository ppa:graphics-drivers/ppa
$ sudo apt update (re-run if any warning/error messages)
$ sudo apt-get install nvidia- (press tab to see latest). 384 (do not use 378, may cause login loops) ## 查找对应的驱动型号
```

### (Dell 7557)

1. 去官网下载驱动后，给驱动run文件赋予执行权限：

        sudo chmod +x NVIDIA-Linux-x86_64-384.59.run

2. 后面的参数非常重要，不可省略：

        sudo ./NVIDIA-Linux-x86_64-384.59.run --no-opengl-files
        //no-opengl-files表示只安装驱动文件，不安装OpenGL文件，避免login loop。

3. 安装cuda

        sudo ./cuda_8.0.61_375.26_linux.run --no-opengl-libs

**Table 1. CUDA Toolkit and Compatible Driver Versions**

| CUDA Toolkit | Linux x8664 Driver Version | Windows x86_64 Driver Version |
|:-----------------:|:-----------------------:|:--------------------------------:|
| CUDA 10.0.130      |  >= 410.48  | 	>= 411.31 |
| CUDA 9.2 (9.2.148 Update 1) |  	>= 396.37 |  	>= 398.26 |
| CUDA 9.2 (9.2.88)  | 	>= 396.26  | 	>= 397.44 |
| CUDA 9.1 (9.1.85)  | 	>= 390.46  | 	>= 391.29 |
| CUDA 9.0 (9.0.76)  | 	>= 384.81  | 	>= 385.54 |
| CUDA 8.0 (8.0.61 GA2) |  >= 375.26  | >= 376.51 |
| CUDA 8.0 (8.0.44)  | 	>= 367.48  | 	>= 369.30 |
| CUDA 7.5 (7.5.16)  |  >= 352.31  | 	>= 353.66 |
| CUDA 7.0 (7.0.28)  | 	>= 346.46  |  	>= 347.62 |  

Last but not least: Add Path and Env
```
export PATH=$PATH:/usr/local/cuda-10.0/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-10.0/lib64 ## 64-bit
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-10.0/lib ##32-bit
```
关于安装驱动和CUDA的更详细官方指导请查看[链接](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html)

## Install tensorflow

推荐两种安装方式：**VirtualEnv**，**Docker**  
查看[官网](https://www.tensorflow.org/install/)的安装指导，注意tensorflow >=1.5需要cuda9.0版本，如果已经安装cuda8.0，可以采用下面方法

```
pip3 uninstall tensorflow-gpu
pip3 install --upgrade tensorflow-gpu==1.4
```

- ImportError: libcudnn.so.6: cannot open shared object file: No such file or directory

到[官网](https://developer.nvidia.com/cudnn)下载相应的*cudnn*库，在注册完帐号之后即可下载（应该选择自己对应的版本）.  
从下载完的文件中，将*libcudnn.so.6.XXX*（具体可修改到自己下载的版本）文件放在*/usr/local/cuda/lib64/*文件目录下  
从下载完的文件中，将*cudnn.h*（具体可修改到自己下载的版本）文件放在*/usr/local/cuda/include/*文件目录下

## Launch a Website with a Custom URL using Github Pages

1. Add ```connorleech.info``` and ```www.connorleech.info``` to **CNAME**
2. Add the changes to git and push them to your repo.
3. Add two “@” type A records that point to the GitHub ips *192.30.252.153* and *192.30.252.154* and one “www” CNAME record that points to your **USERNAME.github.io** url.

![Custom URL](https://github.com/bryanibit/bryanibit.github.io/raw/master/img/doc/customURLSetting.png)

## vscode installation

编译的规则由tasks.json和launch.json确定，launch.json——告诉VS Code如何执行启动任务，task.json——告诉launch或者编译器需要执行什么操作。  
其中的可以确定的环境变量描述为：  
| 变量名 | 含义 |
|:-----------------:|:-----------------------:|
| ${workspaceRoot} |  当前打开的文件夹的绝对路径+文件夹的名字  |
| ${workspaceRootFolderName} | 当前打开的文件夹的名字 |
| ${file}  | 	当前打开正在编辑的文件名，包括绝对路径，文件名，文件后缀名  |
| ${relativeFile}  | 从当前打开的文件夹到当前打开的文件的路径，如当前打开的是test文件夹，当前的打开的是main.c，并有test/first/second/main.c，那么此变量代表的是  first/second/main.c |
| ${fileBasename}  | 当前打开的文件名+后缀名，不包括路径  |
| ${fileBasenameNoExtension} |  当前打开的文件的文件名，不包括路径和后缀名  |
| ${fileDirname}  | 当前打开的文件所在的绝对路径，不包括文件名 |
| ${fileExtname}  | 当前打开的文件的后缀名 |
| ${cwd}  | 任务开始运行时的当前工作目录  |
|${lineNumber} |  当前打开的文件，光标所在的行数  |


## Donation

**If you think this useful for you, you can donate for me. Thank you for your support!**    
![zhifubao](https://github.com/bryanibit/bryanibit.github.io/raw/master/img/zfb.jpg)
