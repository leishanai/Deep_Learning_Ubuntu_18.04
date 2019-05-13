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
Trust me, there is no decent way to install it.
* Intall fcitx, run ```sudo apt install fcitx-bin``` and ```sudo apt install fcitx-table```
* Download [sougou-pinyin](https://pinyin.sogou.com/linux/?r=pinyin) and double click .deb file to install it.
* Go settings, Region & Language, manage Installed Languages and add Chinese.
* Reboot and click the small keyboard icon on the top right corner of the screen. Choose configure, click '+' icon, uncheck 'Only Show Current Language' and you will be able to add sougou-pinyin.
* Go to the "Global Config" tab: Use "ctrl + space" to trigger input method. Disable "Extra key for trigger..." and disable "Enable Hotkey to scroll...".
* Click "Show Advanced Options": change "Interval of Two Key Input" to 5 which decreases the delay of input. And add "Lshift" as the hotkey for "Inactivate Input Method". Click "Program" tab and change "Share State Among Window" to all.
* Open Ubuntu system setting, go to the Device and choose Keyboard. Remove two shortcuts for input as they conflict with fcitx.


## Something about shell, vim and python
* Run```gnome-shell --version```to show the version of gnome terminal.
* Run ```vim --version | grep +python``` to check if vim is python supported, e.g. ```+python3``` meaning support python3 and ```-python``` meaning not support python.
* Gvim seems to be the best vim in ubuntu as it supports everything.

## Kill mouse acceleration
* Open file ```sudo vi /usr/share/X11/xorg.conf.d/50-mouse-acceleration.conf```
* Add following
    ```
    Section "InputClass"
        Identifier "My Mouse"
        MatchIsPointer "yes"
        Option "AccelerationProfile" "-1"
        Option "AccelerationScheme" "none"
        Option "AccelSpeed" "-1"
    EndSection
* Restart the session (logout/login)

## UEFI boot with Windows 10
UEFI is an alternative to BIOS booting. To install ubuntu along with windows10: Firstly, go to the BIOS setting and disable fast startup, CSM and secure boot(delete all keys or select other OS). And follow this [tutorial](http://myviewsonfoss.blogspot.com/2018/05/this-article-willshow-you-how-you-can.html) to complete the rest. But if you have a dedicated graphic card, you will encounter black screen when you install and boot your system at the first time. To resolve this, for installation, go to the 'install ubuntu' option and press 'e', find 'quite splash', remove '---', add 'nomodeset' and press 'F10'. You will be directed to the installation normally. For the first time booting system, press 'e' at the first option, find 'splash', add 'nomodeset', and press F10. This bug is due the cause that ubuntu is not able to load Nvidia graphic card correctly.

## Miscellanea
* Enable case-insensitive autocomplete add ```set completion-ignore-case on``` to ~/.inputrc and restart the terminal.
* Disable edge tilting, ```$ dconf write /org/gnome/mutter/edge-tiling false```
* Install Gnome tweaks, Panel OSD, sublime, slack, discord from built-in ubuntu software.
