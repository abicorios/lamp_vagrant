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
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.network "forwarded_port", guest: 80, host: 8080

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
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    vb.gui = true
     
    # Customize the amount of memory on the VM:
    vb.memory = "2048"
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
    echo deb http://repo.mysql.com/apt/ubuntu/ bionic mysql-5.7 > /etc/apt/sources.list.d/mysql.list
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 467B942D3A79BD29
    debconf-set-selections <<< "mysql-community-server mysql-community-server/root-pass password 123456"
    debconf-set-selections <<< "mysql-community-server mysql-community-server/re-root-pass password 123456"
    add-apt-repository ppa:ondrej/php
    apt update
    apt install -y php7.3-{cli,pdo,fpm,zip,gd,xml,mysql,cgi,mbstring,curl,xdebug} apache2 libapache2-mod-php7.3 unzip mysql-community-server=5.7* mysql-client=5.7*
    rm /var/www/html/index.html
    cp /vagrant/000-default.conf /etc/apache2/sites-available/000-default.conf
    a2enmod rewrite
    service apache2 restart
    mysql -p123456 -e "create user 'ciuiscrm'@'%' identified with mysql_native_password by '123456';grant usage on *.* to 'ciuiscrm'@'%';alter user 'ciuiscrm'@'%' require none;create database if not exists ciuiscrm;grant all privileges on ciuiscrm.* to 'ciuiscrm'@'%';flush privileges;"
    cd /var/www/html
    wget https://files.phpmyadmin.net/phpMyAdmin/5.1.3/phpMyAdmin-5.1.3-english.zip
    unzip phpMyAdmin-5.1.3-english.zip
    mv phpMyAdmin-5.1.3-english phpmyadmin
    rm phpMyAdmin-5.1.3-english.zip
    cp /vagrant/config.inc.php /var/www/html/phpmyadmin/config.inc.php
    chown -R www-data:www-data /var/www/html
    cat /home/vagrant/.ssh/authorized_keys >> /root/.ssh/authorized_keys
  SHELL
end

