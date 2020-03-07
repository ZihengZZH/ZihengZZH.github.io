---
layout: post
title: "How to Re-install Ubuntu 18.04"
date: 2019-02-12
---

Because of the failure during setting CUDA and inadequate partition of hard disk for Ubuntu, I have decided to reinstall the entire Ubuntu 18.04 LTS. 

In this post, I will go through all the steps, including **backup the workspace**, **reinstallation**, and **installing some necessary tools**.


## backup the workspace

This is the most critical part during my entire work. Because of the conflict of incompatible versions of graphics cards drivers, the graphical interface is not accessible and the following work must be done to address this problem.

> Here, I have some complaints about the drivers management of Ubuntu. The default driver is the open-source driver, ```nouveau-display-driver```. It is okay that Ubuntu is based on an open-source driver, which is the core value, but the recommended Nvidia driver, on my machine, conflicts with the GPU MX150. When compared with Windows platform, I expect a easier and less time-consuming driver management system on Ubuntu.

So, to recover all my important work files, we need to enter the recovery mode. Just enter the recovery mode of any one of the available Linux kernels. After this, the following image will pop up.

![](../res/images/recoverymode.png)


It is also noticeable that in the recovery mode, all files are read-only and we need the write permission to proceed. 

Enter the ```fsck``` to check all file systems (or remount filesystem in read/write mode), which allows us to read/write our files on the machine. 

Next, enter the ```root``` to drop to root shell prompt, which allows us to access the files as the root user.

Simply injecting the USB drive does not work, we need to manually mount the USB drive. Run the ```fdisk -l``` to display the USB information. Then run the each one of the following commands to mount USB drive (Note the drive format).
```
$ mount -t vfat /dev/{patition} /media
$ mount -t ntfs /dev/{patition} /media
$ mount -t /dev/{patition} /media
```

After mounting the external drive, run the following command to move or copy the crucial files to the drive.

```
$ mv  -v {workspace}/* /media/{drive_name}
$ cp -v {workspace}/* /media/{drive_name}
```
```-v``` outputs information explaining what the command is exactly doing.

> On Linux and UNIX operating system, you can use the ```mount``` command to attach (mount) file systems and removable devices such as USB flash drives at a particular mount point in the directory tree. -- [Linuxize](https://linuxize.com/post/how-to-mount-and-unmount-file-systems-in-linux/)

More information may refer to this [blog](https://blog.csdn.net/u013451048/article/details/52344362) and this [webpage](https://www.howtoforge.com/linux-mv-command/).


## reinstall Ubuntu 18.04 (already downloaded)

Extend the volume on the disk for Ubuntu 18.04.
Only after deleting one of the volume, the other one can extend on this deteled volume, which can actually be extended to any volume. Remember to format.

https://morvanzhou.github.io/tutorials/others/linux-basic/1-2-install/

After zuole upan qidongqi

With the Upan inserted, reboot the computer to enter the tryout interface of Ubuntu. In this interface, go directly to install Ubuntu 18.04 (because I have already used Ubuntu for a long time). Notice here that the pre-partitioned disk partition will be again formatted to install Ubuntu on this partition. 

## install some necessary tools (in my opinion)

### git

```
$ sudo apt update
$ sudo apt install git
```

The difference between ```apt``` and ```get-apt``` can refer to this [post](https://www.maketecheasier.com/apt-vs-apt-get-ubuntu/).

Next critical step is to setup the ```git-ssh``` to configure with Github for example.

1. Setup username and email for Git
```
$ git config --global user.name "{your_name}"
$ git config --global user.email "{your_email}"
```
2. Generate the keys
```
$ ssh-keygen -t rsa -C "{your_email}"
```
Two files will be generated: ```id_rsa``` and ```id_rsa.pub```, which corresponds to the private key and the public key.

An **optional** step needs to be done to ensure that ```ssh-agent``` works. ```ssh-agent``` is a management tool to store private keys and we need to add the keys to ```ssh-agent``` if necessary.
```
$ ssh-add ~/.ssh/id_rsa
```

4. Configure ssh on [Github](https://github.com/)

After logging in the Github, copy and paste the content of ```id_rsa.pub``` to the ```Setting -> SSH and GPG keys```, as follows. Note that the end of the public key should be your email that entered during key generation. 
![](../res/images/sshkeys.png)

5. Test
```
$ ssh -T git@github.com
```
After running this command, you will see:
```
The authenticity of host 'github.com (some_IP)' can't be established.
RSA key fingerprint is xx:xx:xx:....:xx:xx:xx.
Are you sure you want to continue connecting (yes/no)? yes
Hi your_name! You've successfully authenticated, but GitHub does not provide shell access.
```

Here, all done to setup and configure your git.


[Reference](https://segmentfault.com/a/1190000002645623) here.

### zsh
```
$ apt-get update
$ apt-get upgrade
$ apt-get install zsh
$ sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

source code pro


https://ohmyz.sh

powerline font

```
apt-get install fonts-powerline
```

### chrome



### CUDA & OpenCL

Nvidia driver 
nvidia-smi

> do not forget to add /usr/local/cuda/bin to PATH in both bash and zsh

verify installation 
cat /proc/driver/nvidia/version
nvcc -V

https://www.pugetsystems.com/labs/hpc/How-to-install-CUDA-9-2-on-Ubuntu-18-04-1184/
```
$ cp -a /usr/local/cuda-10.0/include/CL /usr/local/include
$ cp -a /usr/local/cuda-10.0/lib64/* /usr/lib
$ sh -c "echo '/usr/local/lib' >> /etc/ld.so.config"
$ ldconfig
```
https://zhuanlan.zhihu.com/p/53393267

### anaconda3 (Python3)

remember to add PATH to ~/.bashrc & source ~/.zshrc
Also, to zsh, add PATH to ~/.zshrc & source ~/.zshrc

numpy pandas matplotlib sklearn all built-in

when installing tensorflow, it turned out that tensorflow is not compatiable on Python3.7. So, it is necessary to downgrade from Python 3.7 to Python 3.6.
```
$ conda search python
$ conda install python=3.6.5

$ conda install tensorflow(gpu) 
$ conda install keras(gpu)
$ conda install pytorch torchvision cudatoolkit=10.0 -c pytorch
```

### VS code

http://shanalikhan.github.io/2015/12/15/Visual-Studio-Code-Sync-Settings.html

access token 
gist id

zsh terminal font
```
$ cd /usr/share/fonts/truetype/
$ git clone https://github.com/powerline/fonts.git
$ fc-cache -f -v
```

https://github.com/powerline/fonts

### ATOM 

with julia


### Pycharm & IDEA
```
$ tar -zxvf ipycharm-community-xxx.tar.gz -C /opt
$ cd /opt/pycharm-community-xxx/bin
$ ./idea.sh

$ tar -zxvf ideaIU-xxx.tar.gz -C /opt
$ cd /opt/idea-IC-xxx/bin
$ ./idea.sh
```

###  stacer

```
$ sudo add-apt-repository ppa:oguzhaninan/stacer
$ sudo apt-get update
$ sudo apt-get install stacer
```
![](../res/images/stacer.png)

https://github.com/oguzhaninan/Stacer

### Gnome themes
```
$ apt-get install gnome-tweak-tool
$ apt-get install gnome-shell-extensions
```
```
$ mv <theme_name> /usr/share/themes
```

### Spotify

```
# 1. Add the Spotify repository signing keys to be able to verify downloaded packages
apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 931FF8E79F0876134EDDBDCCA87FF9DF48BF1C90

# 2. Add the Spotify repository
echo deb http://repository.spotify.com stable non-free | sudo tee /etc/apt/sources.list.d/spotify.list

# 3. Update list of available packages
apt-get update

# 4. Install Spotify
apt-get install spotify-client
```

https://www.spotify.com/uk/download/linux/