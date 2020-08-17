# Vagrant ready to go for reverse proxy 


# How to install, from the very beginning, a virtual box in which we can run nginx and set up a reverse proxy for Kachin Amber Research Institute
# August, 12, 2020

# Using Visual Studio Code & Powershell, do the following commands, taken from (https://medium.com/@manncyam/ubuntu-18-04-box-installation-on-windows-10-using-vagrant-905bc70355d4)

# In powershell: 

mkdir ubuntu1804
cd ubuntu1804

vagrant init bento/ubuntu-18.04
vagrant up

vagrant ssh


# Install dependencies. (Note: git should already be included)

sudo apt-get update && sudo apt-get upgrade -y
sudo apt install python3-dev -y
sudo apt install python3-pip -y
sudo apt-get install nginx -y


# ###########  EDITING THE VAGRANTFILE

1. Shared

# In order to get the git to work through vagrant, the cloning specifically, must be done on local machine. In local machine, go to vagrantfile, and add this line of code into the vagrantfile. It should be inserted directly beneath "Vagrant.configure("2") do |config|" which you'll find near to the top within the config. This synchronizes a directory that is seen in both vagrant's virtual machine and local. 

config.vm.synced_folder "shared", "/shared"

# once complete, the above line of code in the vagrantfile will enable you to then circle back and git clone the kari4 repo inside the ubuntu18 folder from within the local machine, not within vagrant.

2. Public Network

# This change within the config will allow you to access it remotely through your private or public IP address. It needs to be uncommented in the vagrantfile before you can then use something like nginx. 

Uncomment "config.vm.network "public_network" within the vagrantfile

## End to the config changes

# Start Nginx Configuration

sudo systemctl start nginx
sudo systemctl enable nginx 

# The following command below is unlinking nginx' default config with nginx in order to create a new one, which will be done in the next set of commands and instructions

sudo unlink /etc/nginx/sites-enabled/default

sudo nano /etc/nginx/sites-available/proxy_config.conf

# Write the following in this file while in nano: (where SERVER is your ip)

server {

listen 80;

location / {

proxy_pass http://SERVER;

}

}

# Note: if you're testing on private network the ip would be, obviously, local ip such as 192.168...blabalbabla







