#!/usr/bin/bash
# Installs the required components to get a working workstation install for development
# Ubuntu Focus Focal
# Go, Docker, Git, Visual Code

# Update
sudo apt update && sudo apt upgrade -y

# Install oh-my-zsh
sudo apt-get install fonts-powerline -y
sudo apt install zsh -y
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
# Install custom files for oh-my-zsh (bound to user: dgee)
wget https://raw.githubusercontent.com/davedotdev/dotfiles/master/agnoster.zsh-theme
wget https://raw.githubusercontent.com/davedotdev/dotfiles/master/.zshrc
mv agnoster.zsh-theme ~/.oh-my-zsh/themes/agnoster.zsh-theme
mv .zshrc ~/.zshrc
chsh -s /usr/bin/zsh

# Install Docker pre-reqs
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    git \
    gnupg-agent \
    software-properties-common -y

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

sudo apt-get update -y

sudo apt-get install docker-ce docker-ce-cli containerd.io -y

sudo groupadd docker

sudo usermod -aG docker $USER

sudo systemctl enable docker

# Install Go
wget -c https://dl.google.com/go/go1.15.2.linux-amd64.tar.gz -O - | sudo tar -xz -C /usr/local
echo -n 'export PATH=$PATH:/usr/local/go/bin' >> ~/.zshrc
mkdir ~/go/{bin,pkg,src} -p
