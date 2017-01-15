# udacity-linux-server-configuration

### Project Description

You will take a baseline installation of a Linux distribution on a virtual machine and prepare it to host your web applications, to include installing updates, securing it from a number of attack vectors and installing/configuring web and database servers.

- IP address: 35.167.27.204

- Accessible SSH port: 2200

- Application URL: http://ec2-35-167-27-204.us-west-2.compute.amazonaws.com/

### Walkthrough

1. Create new user named grader and give it the permission to sudo
  - SSH into the server through `ssh -i ~/.ssh/udacity_key.rsa root@35.167.27.204`
  - Run `$ sudo adduser grader` to create a new user named grader
  - Create a new file in the sudoers directory with `sudo nano /etc/sudoers.d/grader`
  - Add the following text `grader ALL=(ALL:ALL) ALL`
  - Run `sudo nano /etc/hosts`
  - Prevent the error `sudo: unable to resolve host` by adding this line `127.0.1.1 ip-10-20-52-12`
   
2. Update all currently installed packages
  - Download package lists with `sudo apt-get update`
  - Fetch new versions of packages with `sudo apt-get upgrade`

3. Change SSH port from 22 to 2200
  - Run `sudo nano /etc/ssh/sshd_config`
  - Change the port from 22 to 2200
  - Confirm by running `ssh -i ~/.ssh/udacity_key.rsa -p 2200 root@35.167.27.204`
  
4. Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123)
  - `sudo ufw allow 2200/tcp`
  - `sudo ufw allow 80/tcp`
  - `sudo ufw allow 123/udp`
  - `sudo ufw enable`
  
5. Configure the local timezone to UTC
  - Run `sudo dpkg-reconfigure tzdata` and then choose UTC
 
6. Configure key-based authentication for grader user
  - Run this command `cp /root/.ssh/authorized_keys /home/grader/.ssh/authorized_keys`

Source: [Udacity Forums](https://discussions.udacity.com/t/not-able-to-login-using-grader-login/161357/3)

7. Disable ssh login for root user
  - Run `sudo nano /etc/ssh/sshd_config`
  - Change `PermitRootLogin without-password` line to `PermitRootLogin no`
  - Restart ssh with `sudo service ssh restart`
  - Now you are only able to login using `ssh -i ~/.ssh/udacity_key.rsa -p 2200 grader@35.167.27.20`
 
8. Install Apache
  - `sudo apt-get install apache2`

9. Install mod_wsgi
  - Run `sudo apt-get install libapache2-mod-wsgi python-dev`
  - Enable mod_wsgi with `sudo a2enmod wsgi`
  - Start the web server with `sudo service apache2 start`

  
  
  
