# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

current_dir    = File.dirname(File.expand_path(__FILE__))
configs        = YAML.load_file("#{current_dir}/config.yaml")

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "ubuntu/focal64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  config.vm.network "forwarded_port", guest: 80, host: 80 #, host_ip: "127.0.0.1"
  config.vm.network "forwarded_port", guest: 8080, host: 8080 #, host_ip: "127.0.0.1"
  config.vm.network "forwarded_port", guest: 4040, host: 4040 #, host_ip: "127.0.0.1" # ngrok inspection port

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"
 

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  config.vm.provider "virtualbox" do |vb|
    vb.name = "dev-vm"
    vb.memory = "5120"
    vb.cpus = 1
  end
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "file", source: "~/.ssh/id_rsa", destination: "$HOME/.ssh/id_rsa"
  config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "$HOME/.ssh/id_rsa.pub"
  config.vm.provision "file", source: "~/.ssh/safeguard.pem", destination: "$HOME/.ssh/safeguard.pem"
  config.vm.provision "file", source: "~/.aws", destination: "$HOME/.aws"
  config.vm.provision "shell", privileged: false, inline: <<-SHELL

      ########################
      ### Install packages ###
      ########################
      # Install packages
      sudo apt-get update && sudo apt-get upgrade -y
      sudo apt-get install -y \
          autojump \
          awscli \
          build-essential \
          nodejs \
          npm \
          python3-pip \
          ruby \
          ruby-dev \
          silversearcher-ag \
          tig \
          zsh

      # Install docker
      [[ -f /usr/bin/docker ]] ||
        curl -fsSL https://get.docker.com | sh
      sudo usermod -aG docker vagrant

      # Install docker-compose
      [[ -f /usr/local/bin/docker-compose ]] ||
        sudo curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
      sudo chmod +x /usr/local/bin/docker-compose

      # Install oh-my-zsh
      if [[ ! -d /home/vagrant/.oh-my-zsh ]]; then
        sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
        rm -f ~/.zshrc
        mv ~/.zshrc.pre-oh-my-zsh ~/.zshrc
      fi

      # Add gem path to remove gem executables warning
      PATH="$(ruby -r rubygems -e 'puts Gem.user_dir')/bin:$PATH"

      # Install gems
      gem install --user-install \
          homesick \
          rubocop:0.42.0 \
          solargraph

      # Install vim-plug
      [[ -f ~/.vim/autoload/plug.vim ]] ||
        curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
          https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim

      # Install asdf
      #  [[ -d ~/.asdf ]] ||
      #    git clone https://github.com/asdf-vm/asdf.git ~/.asdf

      # Install beanstalk-shell
      if ! [[ -f /usr/local/bin/beanstalk-shell ]]; then
        pip3 install --user boto3
        git clone https://oauth2:#{configs['github_access_token']}@github.com/Vizir/beanstalk-shell.git /tmp/beanstalk-shell
        sudo cp /tmp/beanstalk-shell/beanstalk-shell /usr/local/bin/.
        rm -rf /tmp/beanstalk-shell
      fi

      #####################
      ### Link homesick ###
      #####################

      # Clone homesick castle
      if ! [[ -d /home/vagrant/.homesick/repos/dotfiles ]]; then
        homesick clone dsakuma/dotfiles
        homesick link dotfiles
        git --git-dir=$HOME/.homesick/repos/dotfiles/.git remote remove origin
        git --git-dir=$HOME/.homesick/repos/dotfiles/.git remote add origin git@github.com:dsakuma/dotfiles.git
        git --git-dir=$HOME/.homesick/repos/dotfiles/.git branch --set-upstream-to=origin/master master
      fi
      
      ##############################################
      ### Install packages depending on dotfiles ###
      ##############################################

      # Install vim plugins
      vim -E -s -u "$HOME/.vimrc" +PlugInstall +qall

      ######################
      ### Other settings ###
      ######################
      
      # Use zsh as default shell
      sudo chsh vagrant --shell /bin/zsh
      
      # Set timezone
      sudo timedatectl set-timezone America/Sao_Paulo
  SHELL
  
  # Define VM
  config.vm.define "dev-vm"

  # Vagrant plugins
  # config.vagrant.plugins = %w(vagrant-disksize)
  # Increase disk size
  # config.disksize.size = '40GB'
end
