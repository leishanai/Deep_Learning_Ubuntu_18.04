# A tutorial to set up deep learning environment in Ubuntu 18.04

In this tutorial, I will present how to build up deep learning environment in desktop. In my opinion, linux has much better performance than window 10. Everything should be done in about 2 hours. Vim is my default text editor in linux, so I will first introduce some basics in vim. If you are interested, I also attached my vimrc file. In the current meta, I installed, CUDA 10.0 and anaconda-python 3.6 which are the latest versions supported by tensorflow-gpu-1.13 (ts2.0 is not stable yet). 

## Table of content
<!-- vim-markdown-toc GFM -->

* [Intall basic tools](#intall-basic-tools)
    * [1. Vim](#1-vim)
    * [2. Git](#2-git)
    * [3. GCC](#3-gcc)
* [GPU driver, CUDA and cuDNN](#gpu-driver-cuda-and-cudnn)
    * [1. Install the latest Nvidia driver](#1-install-the-latest-nvidia-driver)
    * [2. Install CUDA](#2-install-cuda)
    * [3. Install cuDNN](#3-install-cudnn)
    * [4. Keep multiple versions of CUDA](#4-keep-multiple-versions-of-cuda)
    * [5. Verify GPU driver, CUDA and cuDNN](#5-verify-gpu-driver-cuda-and-cudnn)
* [Python environment](#python-environment)
    * [1. Install anaconda](#1-install-anaconda)
    * [2. Python packages](#2-python-packages)

<!-- vim-markdown-toc -->
## Intall basic tools

### 1. Vim
Personally, I prefer to use vim as default text editor.
* Run ```sudo apt-get install vim``` to install vim from terminal.
* Type ```i``` to enter insert mode and press ESC to return to normal mode. In insert mode, one can edit text like regular text editor while normal mode "protects" file from being changed. Type ```:wq``` to save changes and leave vim. These moves should be sufficient to help in the following parts.
* For a more detailed introduction of vim, please refer to [here](https://github.com/slme1109/myConfigFiles).

### 2. Git
As we need to clone some repos from github in some installations, it is necessary to install git.
1. Run ```sudo apt-get install git``` to install git from shell
2. Optionally intall [git prompt for bash](https://github.com/magicmonty/bash-git-prompt). It brings you informative git prompt when you are in a git repo.
3. Run ```git config --global user.email "you@example.com"``` to add your github account
4. Run ```git config --global user.name "Your Name"``` to add your name (not necessarily your github name)
5. Run ```git config credential.helper store``` if you do not want to type username and password everytime you do git push.

### 3. GCC
Install corresponding GCC version for CUDA. For Ubuntu-18.04 and CUDA-10.0, GCC-7.3 is the [highest version supported](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html)

## GPU driver, CUDA and cuDNN
### 1. Install the latest Nvidia driver
It would just be one line of code, if you do not care about using up-to-date driver. I am sure newer driver makes notable difference and it is worth spending time on.
* Install required dependencies, ```sudo apt-get install freeglut3-dev build-essential libx11-dev libxmu-dev libxi-dev libgl1-mesa-glx libglu1-mesa libglu1-mesa-dev libfreeimage3 libfreeimage-dev```
* Remove preinstalled driver, ```sudo apt-get remove --purge nvidia*```
* To diable nouveau, run ```sudo vim /etc/modprobe.d/blacklist.conf``` and add following two lines to it.
    ```
    blacklist nouveau
    options nouveau modeset=0
    ```
* Make it effective ```sudo update-initramfs -u``` and reboot.
* System becomes 800*600 resolution after reboot. Run ```lsmod | grep nouveau``` and if nothing shows up, it means nouveau is successfully disabled.
* To make the file excutable, run ```sudo chmod 777 cuda_9.0.176_384.81_linux.run``` and run ```sudo ./cuda_9.0.176_384.81_linux.run --no-opengl-files –no-x-check –no-nouveau-check``` to install the driver.	Note that OPENGL yields login loop issue. X-check is equivalent to question 'Would you like to run the nvidia-xconfig utility to automatically update your X configuration file…' if this is not diabled.
* Reboot and run ```nvidia-smi``` or ```nvidia-setting``` to obtain gpu driver info.



### 2. Install CUDA
* Download CUDA from Nvidia, choose ```.run``` file since ```.deb``` will replace gpu driver with lower version
* cd to the folder and run ```sudo sh cuda_10.1.105_418.39_linux.run```
* Questions in the process:

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
* Verify the verision installed, ```cat /usr/local/cuda/version.txt```



### 3. Install cuDNN
* Download [cuDNN Library for Linux](https://developer.nvidia.com/rdp/cudnn-download).
* Unzip through ```tar -xzvf cudnn-10.0-linux-x64-v7.tgz```
* Copy files from unzipped folder to CUDA folder (not symbolic one):
    ```
    sudo cp cuda/include/cudnn.h /usr/local/cuda-10.0/include
    sudo cp cuda/lib64/libcudnn* /usr/local/cuda-10.0/lib64
    sudo chmod a+r /usr/local/cuda-10.0/include/cudnn.h /usr/local/cuda-10.0/lib64/libcudnn*
    ```
* Verify version installed, ```cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2```. [Check here](https://docs.nvidia.com/deeplearning/sdk/cudnn-install/index.html).



### 4. Keep multiple versions of CUDA
* Delete symbolic folder ```sudo rm -rf /usr/local/cuda```
* Create a symbolic link from cuda-10.0 to a new folder cuda by running ```sudo ln -s /usr/local/cuda-10.0/ /usr/local/cuda```. 
* Add the following at the end of ```~/.bashrc```(add symblic folder as envir variables)
    ```
    export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64
    export PATH=$PATH:/usr/local/cuda/bin
    export CUDA_HOME=$CUDA_HOME:/usr/local/cuda
    ```
* ```source ~/.bashrc``` and restart the terminal
* Run ```nvcc -V``` to verify the current CUDA version



### 5. Verify GPU driver, CUDA and cuDNN
* If you see 'Result = PASS' from the following test, they are installed properly.
    ```
    cd /usr/local/cuda-9.0/samples/1_Utilities/deviceQuery
    sudo make
    ./deviceQuery
    ```
* or
    ```
    cd ../bandwidthTest
    sudo make
    ./bandwidthTest
    ```
* You will see a fancy picture if you do the test below,
    ```
    cd ../NVIDIA_CUDA-10.0_Samples/2_Graphics/volume
    Rendersudo make
    ./volumeRender
    ```

## Python environment

### 1. Install anaconda
Anaconda is a great python tool to manage packages.
* Download [anaconda python 3.6](https://repo.continuum.io/archive/)
* Install anaconda, ```bash Anaconda3-5.2.0-Linux-x86_64.sh```
* Create environment, ```conda create --name dlpy36 python=3.6```
* Install required deep learning python packages under dlpy36. [Check here](https://www.digitalocean.com/community/tutorials/how-to-install-anaconda-on-ubuntu-18-04-quickstart).
* For freshly opened terminal, run ```source activate dlpy36``` to switch on the environment with all required package installed.

### 2. Python packages
Make sure you are in the dlpy36 environment and then do the following,
```
pip install keras (numpy)
pip install tensorflow-gpu (only support py3.6)
pip install -U scikit-learn
pip install pandas
pip install seaborn (matplotlib)
pip install -U gensim
```
Try to avoid using conda, as packages are not up-to-date and conda-forged tensorflow will install low verisions of CUDA and cuDNN.
