# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.network "private_network", ip: "192.168.33.10"
  
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "512"
    vb.cpus = 1
  end
  
  config.vm.synced_folder "./html", "/var/www/html"
  
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y apache2
    systemctl enable apache2
    systemctl start apache2
    echo "ServerName localhost" >> /etc/apache2/apache2.conf
    systemctl restart apache2
  SHELL
end
