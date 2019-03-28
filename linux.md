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

## Gnome Tweaks
Just install it :)

## Something about shell, vim and python
1. Run```gnome-shell --version```to show the version of gnome terminal.
2. Run ```vim --version | grep +python``` to check if vim is python supported, e.g. ```+python3``` meaning support python3 and ```-python``` meaning not support python.
