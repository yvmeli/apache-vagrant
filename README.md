# Apache Web Server with Vagrant

## Overview
This project implements an Apache web server using Vagrant to automatically create and configure a virtual machine. The web server serves a simple HTML page with CSS styling.

## Prerequisites
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- [Vagrant](https://www.vagrantup.com/downloads)

## Project structure
```
apache-vagrant/
├── Vagrantfile          # Virtual machine configuration
├── html/                # Directory shared with the web server
│   └── index.html       # "Hello World" web page with styles
└── README.md            # This file
```

## Features
- Ubuntu 20.04 LTS virtual machine
- Apache 2 web server
- Static IP: 192.168.33.10
- Allocated resources: 512MB RAM, 1 CPU
- Shared folder between host and virtual machine

## Installation and usage
1. Clone this repository:
   ```bash
   git clone https://github.com/YOUR_USERNAME/apache-vagrant.git
   cd apache-vagrant
   ```

2. Start the virtual machine:
   ```bash
   vagrant up
   ```

3. Access the web page:
   - Open a browser and visit `http://192.168.33.10`

4. To access the virtual machine via SSH:
   ```bash
   vagrant ssh
   ```

5. To stop the virtual machine:
   ```bash
   vagrant halt
   ```

6. To remove the virtual machine:
   ```bash
   vagrant destroy
   ```

## Customization
To modify the web page, simply edit the files in the `html/` folder. Changes will be automatically reflected on the web server.

## Vagrantfile configuration
The `Vagrantfile` contains the following configuration:

```ruby
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
```

## Troubleshooting
If you experience issues with special characters in the Vagrantfile, ensure it is saved with ASCII encoding or UTF-8 without BOM.