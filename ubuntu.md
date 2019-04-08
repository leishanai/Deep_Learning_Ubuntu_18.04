# Several things to do after you installed a fresh ubuntu on your desktop

## Install Gnome-terminal-solarized-theme:
* Run ```sudo apt-get install dconf-cli```
* Go to the folder you want to clone the repo and do the following steps,
    ```
    git clone https://github.com/Anthony25/gnome-terminal-colors-solarized.git
    cd gnome-terminal-colors-solarized
    ./install.sh (set_dark.sh, set_light.sh)
    ```
* [Add color for directory](https://github.com/Anthony25/gnome-terminal-colors-solarized). To install solarized-dir_colors:
    1. ```cd ~/.dir_colors``` and ```git clone https://github.com/seebi/dircolors-solarized.git```
    2. add ```eval `dircolors ~/.dir_colors/dircolors-solarized/dircolors.256dark` ``` to ```~/.bashrc``` and source ```~/.bashrc```. [Check here](https://github.com/seebi/dircolors-solarized)

## Intall sougou-pinyin
* Intall fcitx, run ```sudo apt install fcitx-bin``` and ```sudo apt install fcitx-table```
* Download [sougou-pinyin](https://pinyin.sogou.com/linux/?r=pinyin) and double click .deb file to install it.
* Reboot and click the small keyboard icon on the top right corner of the screen. Choose configure and you will see sougou-pinyin is already added.
* Go to the "Global Config" tab: Use "ctrl + space" to trigger input method. Disable "Extra key for trigger..." and disable "Enable Hotkey to scroll...".
* Open system setting, go to the Device and choose Keyboard. Remove two shortcuts for input as they conflict with fcitx.
* Click "Show Advanced Options": change "Interval of Two Key Input" to 5 which decreases the delay of input. And add "Lshift" as the hotkey for "Inactivate Input Method". Click "Program" tab and change "Share State Among Window" to all.


## Gnome Tweaks
Just install it :)

## Something about shell, vim and python
* Run```gnome-shell --version```to show the version of gnome terminal.
* Run ```vim --version | grep +python``` to check if vim is python supported, e.g. ```+python3``` meaning support python3 and ```-python``` meaning not support python.

## mouse acceleration remover

## UEFI install, disable csm, disable secure boot
just set OS type as `other OS` and diable csm

## manual partition

## more to be added

## test
