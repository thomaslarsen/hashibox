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
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "bento/centos-7.1"

  config.vm.hostname = "hashibox"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder "/Volumes/Crypt disk/Work/Home Office/git", "/git"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "file", source: "files/alias", destination: "/tmp/ipt_aliases.sh"

  config.vm.provision "shell", inline: <<-SHELL
   yum -y install epel-release
   yum -y install jq git unzip

   if [ ! -e /usr/local/bin/vault ]; then
     wget https://releases.hashicorp.com/vault/0.6.4/vault_0.6.4_linux_amd64.zip
     unzip vault_*
     mv vault /usr/local/bin
     rm -f vault_*
   fi

   if [ ! -e /usr/local/bin/terraform ]; then
     wget https://releases.hashicorp.com/terraform/0.8.2/terraform_0.8.2_linux_amd64.zip
     unzip terraform_*
     mv terraform /usr/local/bin
     rm -f terraform_*
   fi

   if [ ! -e /usr/local/bin/consul ]; then
     wget https://releases.hashicorp.com/consul/0.7.2/consul_0.7.2_linux_amd64.zip
     unzip consul_*
     mv consul /usr/local/bin
     rm -f consul_*
   fi

   if [ ! -e /usr/local/bin/consul-template ]; then
     wget https://releases.hashicorp.com/consul-template/0.16.0/consul-template_0.16.0_linux_amd64.zip
     unzip consul-template_*
     mv consul-template /usr/local/bin
     rm -f consul-template_*
   fi

   if [ ! -e /usr/local/bin/aws ]; then
     wget https://s3.amazonaws.com/aws-cli/awscli-bundle.zip
     unzip awscli-*
     ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
     rm -rf awscli-*
   fi

   if [ ! -e /usr/bin/pip ]; then
     easy_install pip
     pip install boto3
     pip install openpyxl
   fi


   if [ ! -e /etc/profile.d/ipt_aliases.sh ]; then
     cp -f /tmp/ipt_aliases.sh /etc/profile.d/ipt_aliases.sh
     echo "export TF_VAR_created_by=ENV['USER']" >> /etc/profile.d/ipt_aliases.sh
     echo "export PATH=$PATH:/git/DC/aws-tf-generator/bin/" >> /home/vagrant/.bash_profile
   fi
  SHELL
end
