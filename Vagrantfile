# -*- mode: ruby -*-
# vi: set ft=ruby :

# Box / OS
VAGRANT_BOX = 'ubuntu/bionic64'

# Memorable name for your
VM_NAME = 'dev-vm'

# Host folder to sync
HOST_PATH = '~/' + VM_NAME

# Where to sync to on Guest — 'vagrant' is the default user name
GUEST_PATH = '/home/vagrant/' + VM_NAME

Vagrant.configure(2) do |config|
  # Vagrant box from Hashicorp
  config.vm.box = VAGRANT_BOX
  
  # Actual machine name
  config.vm.hostname = VM_NAME

  # Set VM name in Virtualbox
  config.vm.provider "virtualbox" do |v|
    v.name = VM_NAME
    v.memory = 8192
  end

  # Attempt to match timezone of host
  if Vagrant.has_plugin?("vagrant-timezone")
    config.timezone.value = :host
  end

  # Set disk size
  config.disksize.size = '50GB'

  # DHCP — comment this out if planning on using NAT instead
  config.vm.network "private_network", type: "dhcp"

  # Sync folder
  config.vm.synced_folder HOST_PATH, GUEST_PATH

  # Sync C drive
  config.vm.synced_folder 'C:/', '/c'

  # Sync D drive
  config.vm.synced_folder 'D:/', '/d'

  # Sync G drive
  config.vm.synced_folder 'G:/', '/g'

  # Disable default Vagrant folder, use a unique path per project
  config.vm.synced_folder '.', '/home/vagrant', disabled: true

  # Allow access to host's ssh agent for Github access
  config.ssh.forward_agent = true

  # Add public key for ssh from WSL
  config.vm.provision "file", source: "vagrant_init/.ssh/id_rsa_dev-vm.pub", destination: "~vagrant/.ssh/me.pub"
  config.vm.provision "shell", inline: "cat ~vagrant/.ssh/me.pub >> ~vagrant/.ssh/authorized_keys"

  # Install Docker
  config.vm.provision "docker"

  # Install docker-machine
  config.vm.provision "shell", inline: <<-SHELL
    base=https://github.com/docker/machine/releases/download/v0.16.0 && \
      curl -L $base/docker-machine-$(uname -s)-$(uname -m) >/tmp/docker-machine && \
      sudo install /tmp/docker-machine /usr/local/bin/docker-machine
  SHELL

  # Configure port forwarding to support remote access to Docker Engine
  config.vm.network "forwarded_port", guest: 2376, host: 2376

  # Make host docker VM available to guest
  #   For Windows:
  #     * docker-machine env will give a roughly usable shell script. Windows path must be converted, however.

  # Install packages
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get upgrade -y
    apt-get install -y git tmux
    apt-get autoremove -y
    apt-get autoclean -y
    apt-get clean -y
  SHELL

  # Configure personal environment
  config.vm.provision "shell", inline: <<-SHELL
    git clone https://github.com/austinkeller/dotfiles .dotfiles
    ln -s .dotfiles/.gitconfig
    ln -s .dotfiles/.tmux.conf
    ln -s .dotfiles/.vimrc
    ln -s .dotfiles/.inputrc
    mkdir ~vagrant/.ssh
    chmod 700 ~vagrant/.ssh
    cp .dotfiles/.ssh/rc .ssh/rc
    cat .dotfiles/.ssh/config >> .ssh/config
    git clone https://github.com/tmux-plugins/tpm ~vagrant/.tmux/plugins/tpm
    cat .dotfiles/.bashrc_components/wsl_colors >> .bashrc
  SHELL

  # Add bin to path
  config.vm.provision "shell", inline: <<-SHELL
    echo >> .bashrc
    echo PATH='$PATH:~/dev-vm/bin' >> .bashrc
  SHELL

  # Add secrets
  config.vm.provision "file", source: "~/.aws", destination: "~/.aws"

end
