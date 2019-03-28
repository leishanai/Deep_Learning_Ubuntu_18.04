# A tutorial to set up deep learning environment in Ubuntu 18.04
In this tutorial, I will present how to build up deep learning environment in desktop. In my opinion, linux has much better performance than window 10. Everything should be done in about 2 hours. Vim is my default text editor in linux, so I will first introduce some basics in vim and show how to install Vundle to manage multiple plugins in vim. If you are interested, I also attached my vimrc file. In this meta, I used CUDA 10.0 and python 3.6 which are the latest version supported by tensorflow.

## Install vim
1. Run ```sudo apt-get install vim```
2. Type ```i``` to enter insert mode and press ESC to return to normal mode. In insert mode, one can edit text like regular text editor while normal mode "protects" file from being changed. Type ```:wq``` to save changes and leave vim. These moves should be sufficient to help in the following parts.
2. Run ```git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim``` to manage plugins. [Check here](https://github.com/VundleVim/Vundle.vim)
3. Add the following to ```vim ~/.vimrc```.
```
set nocompatible              " be iMproved, required
filetype off                  " required
" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
" call vundle#begin('~/some/path/here')
" Keep Plugin commands between vundle#begin/end.
" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'
call vundle#end()            " required
filetype plugin indent on    " required
```
4. Launch vim and run ```:PluginInstall``` or run ```vim +PluginInstall +qall``` to install from command line.



## Install gcc



## Install GPU driver
1. Install required dependencies, ```sudo apt-get install freeglut3-dev build-essential libx11-dev libxmu-dev libxi-dev libgl1-mesa-glx libglu1-mesa libglu1-mesa-dev libfreeimage3 libfreeimage-dev```
2. Remove preinstalled driver, ```sudo apt-get remove --purge nvidia*```
3. To diable nouveau, run ```sudo vim /etc/modprobe.d/blacklist.conf``` and add following two lines to it.
```
blacklist nouveau
options nouveau modeset=0
```
4. Make it effective ```sudo update-initramfs -u``` and reboot.
5. System becomes 800*600 resolution after reboot. Run ```lsmod | grep nouveau``` and if nothing shows up, it means nouveau is successfully disabled.
6. To make the file excutable, run ```sudo chmod 777 cuda_9.0.176_384.81_linux.run``` and run ```sudo ./cuda_9.0.176_384.81_linux.run --no-opengl-files –no-x-check –no-nouveau-check``` to install the driver.	Note that OPENGL yields login loop issue. X-check is equivalent to question 'Would you like to run the nvidia-xconfig utility to automatically update your X configuration file…' if this is not diabled.
7. Reboot and run ```nvidia-smi``` or ```nvidia-setting``` to obtain gpu driver info.



## Install CUDA:
1. Download CUDA from Nvidia, choose ```.run``` file since ```.deb``` will replace gpu driver
2. CD to the folder and run ```sudo sh cuda_10.1.105_418.39_linux.run```
3. Questions in the process:

* Do you accept the previously read EULA?
accept/decline/quit: accept
* Install NVIDIA Accelerated Graphics Driver for Linux-x86_64 410.23?
(y)es/(n)o/(q)uit: n # no if you have the latest gpu driver
* Install the CUDA 9.0 Toolkit?
(y)es/(n)o/(q)uit: y
* Enter Toolkit Location
[ default is /usr/local/cuda-9.0 ]: # press enter if default
* Do you want to install a symbolic link at /usr/local/cuda?
(y)es/(n)o/(q)uit: y or n # we will create symbolic link by ourselves
* Install the CUDA 9.0 Samples?	(y)es/(n)o/(q)uit: y or n # depends on whether you want to run the test.
4. Verify the verision installed, ```cat /usr/local/cuda/version.txt```



## Install cuDNN
1. Download [cuDNN Library for Linux](https://developer.nvidia.com/rdp/cudnn-download).
2. Unzip through ```tar -xzvf cudnn-10.0-linux-x64-v7.tgz```
3. Copy files from unzipped folder to CUDA folder (not symbolic one):
```
sudo cp cuda/include/cudnn.h /usr/local/cuda-10.0/include
sudo cp cuda/lib64/libcudnn* /usr/local/cuda-10.0/lib64
sudo chmod a+r /usr/local/cuda-10.0/include/cudnn.h /usr/local/cuda-10.0/lib64/libcudnn*
```
4. Verify version installed, ```cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2```. [Check here](https://docs.nvidia.com/deeplearning/sdk/cudnn-install/index.html).



## Keep multiple versions of CUDA
1. Delete symbolic folder ```sudo rm -rf /usr/local/cuda```
2. Create a symbolic link from cuda-10.0 to a new folder cuda by running ```sudo ln -s /usr/local/cuda-10.0/ /usr/local/cuda```. 
3.Add the following at the end of ```~/.bashrc```(add symblic folder as envir variables)
```
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64
export PATH=$PATH:/usr/local/cuda/bin
export CUDA_HOME=$CUDA_HOME:/usr/local/cuda
```
4. ```source ~/.bashrc``` and restart the terminal
5. Run ```nvcc -V``` to verify the current CUDA version



## Test GPU driver, CUDA and cuDNN
1. If you see 'Result = PASS' for the following, meaning they are installed properly.
```
cd /usr/local/cuda-9.0/samples/1_Utilities/deviceQuery
sudo make
./deviceQuery
```
or
```
cd ../bandwidthTest
sudo make
./bandwidthTest
```
2. You will see a fancy picture if you do,
```
cd ../NVIDIA_CUDA-10.0_Samples/2_Graphics/volume
Rendersudo make
./volumeRender
```


## Install anaconda:
1. Download [anaconda python 3.6](https://repo.continuum.io/archive/)
2. Install anaconda, ```bash Anaconda3-5.2.0-Linux-x86_64.sh```
3. Create environment, ```conda create --name dlpy36 python=3.6```
4. Install required deep learning python packages under dlpy36. [Check here](https://www.digitalocean.com/community/tutorials/how-to-install-anaconda-on-ubuntu-18-04-quickstart).



## Python packages:
```
pip install keras (numpy)
pip install tensorflow-gpu (only support py3.6)
pip install -U scikit-learn
pip install pandas
pip install seaborn (matplotlib)
pip install -U gensim
```
Try to avoid using conda, as packages are not up-to-date and conda-forged tensorflow will install low verisions of CUDA and cuDNN.
