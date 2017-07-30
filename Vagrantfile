# -*- mode: ruby -*-
# vi: set ft=ruby :

# API version for confguration file, we can specify which version we need depending on requirement.
Vagrant.configure(2) do |config|
# Specifying the base vagrant box
config.vm.box = "ubuntu/xenial64"
# Specifying network confgurations and port forwarding
config.vm.network "forwarded_port", guest: 80, host: 8080
config.vm.network "private_network", ip: "192.168.33.10"
# configuring virtualbox
config.vm.provider "virtualbox" do |v|
v.memory = 4096
v.cpus = 2
end


# Using Shell script for provisioning Packages
config.vm.provision "shell", inline: <<-SHELL

# Update server
apt-get update
apt-get upgrade -y
# Creating new user called as pythian
sudo useradd -m pythian -p pythian
su - pythian

# Installing required Packages

# Installing Git
echo "------------------------------------------------------"
echo "<<<<<<<<          Installing Git            >>>>>>>>>>"
echo "------------------------------------------------------"
apt-get -y install build-essential binutils-doc git -y

# Installing apache
echo "------------------------------------------------------"
echo "<<<<<<<<         Installing Apache          >>>>>>>>>>"
echo "------------------------------------------------------"
apt-get install apache2 -y

# Installing MySQL
echo "------------------------------------------------------"
echo "<<<<<<<<         Installing MySQL           >>>>>>>>>>"
echo "------------------------------------------------------"

echo "mysql-server mysql-server/root_password password root" | sudo debconf-set-selections
echo "mysql-server mysql-server/root_password_again password root" | sudo debconf-set-selections
apt-get install mysql-client mysql-server -y

# Restarting Apache server
service apache2 restart

# Cloning Git Repository to local system
echo "-------------------------------------------------------------------------"
echo "<<<<<<<<        Cloning Git Repository to /opt/code            >>>>>>>>>>"
echo "-------------------------------------------------------------------------"
git clone https://github.com/pythian/DevOps_Cloud_Automation_Engineer_Project.git /opt/code

echo "-----------------------------------------------------------------------"
echo "<<<<<<<<        Checking Ownership and Permissions           >>>>>>>>>>"
echo "-----------------------------------------------------------------------"
# Changing Ownership and Permissions to pythian
sudo chown -R pythian:pythian /opt/code
cd /opt
ls -l

# Testing if all the service is working
echo "---------------------------------------------------------------"
echo "<<<<<<<<        Checking Installed Packages          >>>>>>>>>>"
echo "---------------------------------------------------------------"

dpkg --get-selections git
dpkg --get-selections mysql-server
dpkg --get-selections apache2

echo "Done installing your virtual machine!"
SHELL

end
