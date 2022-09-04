---
title: Pop Os Setup
date: 2022-04-22 23:58:19
description:
tags: 
 - pop_os
 - linux
 - tools
icon: material/emoticon-happy
status: [Todo|Inprogress|Published|Arxived]
external_references: 
urls: 
aliases: 
---
# [[Pop Os Setup]]

## Remove top bar 
https://ubuntuhandbook.org/index.php/2020/08/top-panel-auto-hide-ubuntu-20-04/


```bash
cd ~
# Basic Update
sudo apt-get update && sudo apt-get upgrade -y
sudo apt-get install -y python3-pip htop python-is-python3 python3-venv python3-opencv

# Install Google Chrome
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo apt install -y ./google-chrome-stable_current_amd64.deb
rm ./google-chrome-stable_current_amd64.deb

# Install Sublime
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
sudo apt-get install -y apt-transport-https
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
sudo apt-get update
sudo apt-get install -y sublime-text

# Install VLC
sudo apt-get install -y vlc

# Install Docker
sudo apt-get install -y apt-transport-https ca-certificates curl gnupg lsb-release
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io
 
# Install Heroku CLI
curl https://cli-assets.heroku.com/install.sh | sh 

# Install SQlite3
sudo apt-get install sqlite3
sudo apt-get install -y  sqlitebrowser

# Git config
git config --global user.email "harshmaheshwari3110@gmail.com"
git config --global user.name "Harsh-Maheshwari"

# Zipline-reloded config
sudo apt-get install -y libatlas-base-dev python-dev gfortran pkg-config libfreetype6-dev hdf5-tools

# Install TAlib for zipline
wget http://prdownloads.sourceforge.net/ta-lib/ta-lib-0.4.0-src.tar.gz
tar -xzf ta-lib-0.4.0-src.tar.gz
rm ta-lib-0.4.0-src.tar.gz
cd ta-lib/
sudo ./configure
sudo make
sudo make install
echo 'export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH' >> ~/.bashrc

# Portfolio Setup
Portfolio_dir="/media/harsh/DATA/Harsh/Portfolio" 
mkdir $Portfolio_dir/python_environments/
cd $Portfolio_dir/python_environments/
python -m venv portfolio_env
echo 'source /media/harsh/DATA/Harsh/Portfolio/python_environments/portfolio_env/bin/activate' >> ~/.bashrc 
echo 'export PATH="/home/harsh/.local/bin:$PATH"' >> ~/.bashrc 
echo 'Portfolio_dir="/media/harsh/DATA/Harsh/Portfolio/"'>> ~/.bashrc
source $Portfolio_dir/python_environments/portfolio_env/bin/activate
pip3 install -r $Portfolio_dir/secondary_requirements.txt
pip3 install -r $Portfolio_dir/requirements.txt
source ~/.bashrc

# Install CMake 3.20.5
sudo apt-get install -y build-essential libssl-dev
wget https://github.com/Kitware/CMake/releases/download/v3.20.0/cmake-3.20.0.tar.gz
tar -zxvf cmake-3.20.0.tar.gz
rm cmake-3.20.0.tar.gz
cd cmake-3.20.0
./bootstrap
make
sudo make install


#Adding SSH For Github
ssh-keygen -t ed25519 -C "harshmaheshwari3110@gmail.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
cat ~/.ssh/id_ed25519.pub

```

## Install Pantheon File System 

```
sudo add-apt-repository ppa:elementary-os/stable
sudo apt-get update
sudo apt-get install pantheon-files
```

Github Desktop for linux
https://github.com/shiftkey/desktop/releases
