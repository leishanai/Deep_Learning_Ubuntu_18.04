# Several things to do after you installed a fresh ubuntu on your desktop

## Install Gnome-terminal-solarized-theme:
1. Run ```sudo apt-get install dconf-cli```
2. Go to the folder you want to clone the repo and do the following steps,
* ```git clone https://github.com/Anthony25/gnome-terminal-colors-solarized.git```
* ```cd gnome-terminal-colors-solarized```
* ```./install.sh``` or (```set_dark.sh``` or ```set_light.sh```)
3. dir_colors should be installed as well. [Check here](https://github.com/Anthony25/gnome-terminal-colors-solarized)

4. Do the following to install solarized-dir_colors:
* ```cd ~/.dir_colors``` and ```git clone https://github.com/seebi/dircolors-solarized.git```
* add ```eval `dircolors ~/.dir_colors/dircolors-solarized/dircolors.256dark` ``` to ```~/.bashrc``` and source ```~/.bashrc```. [Check here](https://github.com/seebi/dircolors-solarized)

## Intall sougou-pinyin
1. Intall fcitx, run ```sudo apt install fcitx-bin``` and ```sudo apt install fcitx-table```
2. Download [sougou-pinyin](https://pinyin.sogou.com/linux/?r=pinyin) and double click .deb file to install it.
3. Reboot and click the small keyboard icon on the top right corner of the screen. Choose configure and you will see sougou-pinyin is already added.
4. Go to the "Global Config" tab: Use "ctrl + space" to trigger input method. Disable "Extra key for trigger..." and disable "Enable Hotkey to scroll...".
5. Open system setting, go to the Device and choose Keyboard. Remove two shortcuts for input as they conflict with fcitx.
5. Click "Show Advanced Options": change "Interval of Two Key Input" to 5 which decreases the delay of input. And add "Lshift" as the hotkey for "Inactivate Input Method". Click "Program" tab and change "Share State Among Window" to all.


## Gnome Tweaks
Just install it :)

## Something about shell, vim and python
1. Run```gnome-shell --version```to show the version of gnome terminal.
2. Run ```vim --version | grep +python``` to check if vim is python supported, e.g. ```+python3``` meaning support python3 and ```-python``` meaning not support python.
