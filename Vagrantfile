# -*- mode: ruby -*-
# vi: set ft=ruby :

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
  config.vm.network "forwarded_port", guest: 80, host: 80

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
    vb.memory = "3072"
    vb.cpus = 1
  end
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
      # Install docker
      [[ -f /usr/bin/docker ]] ||
        curl -fsSL https://get.docker.com | sh
      sudo usermod -aG docker vagrant

      # Install docker-compose
      [[ -f /usr/local/bin/docker-compose ]] ||
        sudo curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
      sudo chmod +x /usr/local/bin/docker-compose

      # Install dependencies
      sudo apt-get update && sudo apt-get install -y \
          autojump \
          awscli \
          build-essential \
          nodejs \
          npm \
          ruby \
          ruby-dev \
          silversearcher-ag \
          zsh

      # Upgrade packages
      sudo apt upgrade -y

      # Install asdf
      git clone https://github.com/asdf-vm/asdf.git ~/.asdf

      # Install oh-my-zsh
      [[ -d /home/vagrant/.oh-my-zsh ]] ||
        sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

      # Install gems
      gem install --user-install homesick
      gem install --user-install rubocop --version='~>0.42.0'

      # Clone homesick castle (using complete path, since .zshrc is not linked yet)
      $(ruby -r rubygems -e 'puts Gem.user_dir')/bin/homesick clone dsakuma/dotfiles
      $(ruby -r rubygems -e 'puts Gem.user_dir')/bin/homesick link dotfiles
      $(ruby -r rubygems -e 'puts Gem.user_dir')/bin/homesick pull
      git --git-dir=/home/vagrant/.homesick/repos/dotfiles remote remove origin
      git --git-dir=/home/vagrant/.homesick/repos/dotfiles remote add origin git@github.com:dsakuma/dotfiles.git

      # Use zsh as default shell
      sudo chsh vagrant --shell /bin/zsh

      # Install vim-plug and plugins
      [[ -f ~/.vim/autoload/plug.vim ]] ||
        curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
          https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
      vim +PlugInstall +qall
  SHELL

  # Define VM
  config.vm.define "dev-vm"

  # Vagrant plugins
  config.vagrant.plugins = %w(vagrant-disksize)
  # Increase disk size
  config.disksize.size = '40GB'
end
