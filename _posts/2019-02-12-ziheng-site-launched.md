
# reinstall Ubuntu 18.04 LTS

Because of the failure during setting CUDA and inadequate partition of hard disk for Ubuntu, I have decided to reinstall entire Ubuntu 18.04 LTS. 


1. backup the workspace


https://blog.csdn.net/u013451048/article/details/52344362

mv  -v ~/Downloads/* ~/Videos/

2. reinstall Ubuntu 18.04 (already downloaded)

Extend the volume on the disk for Ubuntu 18.04.
Only after deleting one of the volume, the other one can extend on this deteled volume, which can actually be extended to any volume. Remember to format.

https://morvanzhou.github.io/tutorials/others/linux-basic/1-2-install/

After zuole upan qidongqi

With the Upan inserted, reboot the computer to enter the tryout interface of Ubuntu. In this interface, go directly to install Ubuntu 18.04 (becuase I have already used Ubuntu for a long time). Notice here that the pre-partitioned disk parition will be again formatted to install Ubuntu on this parition. 

1. git

sudo apt update
sudo apt install git

https://segmentfault.com/a/1190000002645623

2. zsh

sudo apt-get update
sudo apt-get upgrade
sudo apt-get install zsh

$ sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

source code pro


https://ohmyz.sh

powerline font

sudo apt-get install fonts-powerline


3. chrome



4. CUDA & OpenCL

Nvidia driver 
nvidia-smi

> do not forget to add /usr/local/cuda/bin to PATH in both bash and zsh

verify installation 
cat /proc/driver/nvidia/version
nvcc -V

https://www.pugetsystems.com/labs/hpc/How-to-install-CUDA-9-2-on-Ubuntu-18-04-1184/

sudo cp -a /usr/local/cuda-10.0/include/CL /usr/local/include
sudo cp -a /usr/local/cuda-10.0/lib64/* /usr/lib
sudo sh -c "echo '/usr/local/lib' >> /etc/ld.so.config"
sudo ldconfig

https://zhuanlan.zhihu.com/p/53393267

5. anaconda3 (Python3)

remember to add PATH to ~/.bashrc & source ~/.zshrc
Also, to zsh, add PATH to ~/.zshrc & source ~/.zshrc

numpy pandas matplotlib sklearn all built-in

when installing tensorflow, it turned out that tensorflow is not compatiable on Python3.7. So, it is necessary to downgrade from Python 3.7 to Python 3.6.

conda search python
conda install python=3.6.5

conda install tensorflow(gpu) 
conda install keras(gpu)
conda install pytorch torchvision cudatoolkit=10.0 -c pytorch


5. VS code

http://shanalikhan.github.io/2015/12/15/Visual-Studio-Code-Sync-Settings.html

access token 
gist id

zsh terminal font
cd /usr/share/fonts/truetype/
sudo git clone https://github.com/powerline/fonts.git
sudo fc-cache -f -v



https://github.com/powerline/fonts

7. ATOM 

8. Pycharm & IDEA

sudo tar -zxvf ipycharm-community-xxx.tar.gz -C /opt
cd /opt/pycharm-community-xxx/bin
./idea.sh

sudo tar -zxvf ideaIU-xxx.tar.gz -C /opt
cd /opt/idea-IC-xxx/bin
./idea.sh


1.  stacer

sudo add-apt-repository ppa:oguzhaninan/stacer
sudo apt-get update
sudo apt-get install stacer

https://github.com/oguzhaninan/Stacer

13. theme

sudo apt-get install gnome-tweak-tool
sudo apt-get install gnome-shell-extensions

sudo mv <theme_name> /usr/share/themes

