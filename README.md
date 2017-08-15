# Linux Server Configuration
   Take a baseline installation of a Linux distribution on a virtual machine and prepare it to host your web applications, to include installing updates, securing it from a number of attack vectors and installing/configuring web and database servers.
   * The EC2 URL is : http://ec2-34-230-84-216.compute-1.amazonaws.com
   * Local IP address http://34.230.84.216 
   * SSH Port : 2200
   * Login with : $ ssh -i ~/.ssh/privatekey.pem -p 2200 grader@34.230.84.216
 # Get started on Lightsail
  Login to LightSail:
   * If you don't already have an Amazon Web Services account, you'll be prompted to create one.
      * Create an instance
        - When You create an instance,LightSail asks what kind you want. For this project,choose an "OS Only" instance with Ubuntu.
      * Choose your instance plan
        - For this project, pick the lowest one to get free-tier access.
      * Give your instance a hostname
        - I have named my instance 'Ubuntu-512MB-Virginia-1'
      * After comimg the main page for my 'Ubuntu-512MB-Virginia-1' instance, connect using SSH button is the next step.
      * SSh window logged into the server instance
        - Ubuntu@ip-xxx-xx-xx-xxx:~$
 # How to get private key :
   * Go to https://lightsail.aws.amazon.com/ls/webapp/account/keys ,then click download button. 
       - I have stored this privatekey under .ssh folder (C:\Users\Ravi\.ssh\privatekey.pem) 
 # Create new user named grader and give it the permission to sudo
   * SSH into the server through --> ssh -i ~/.ssh/privatekey.pem ubuntu@34.230.84.216
   * Run $ sudo adduser grader to create a new user named grader
        - ubuntu@ip-xxx-xx-xx-xxx:~$ sudo adduser grader
   * Create a new file in the sudoers directory with sudo nano /etc/sudoers.d/grader
        - Add the following text grader ALL=(ALL:ALL) ALL   
 # Update all currently installed packages
   * sudo apt-get update - command will update list of packages and their versions on your machine.
   * sudo apt-get upgrade - command will install the packages
 # Add SSH port 
   * Run sudo nano /etc/ssh/sshd_config
      - If I have Changed the port from 22 to 2200, I couldn't enter the server later.
      - So, I have added port 2200 in sshd_config file. Don't remove the line port 22. 
 # Configure the Uncomplicated Firewall (UFW)     
   * sudo ufw allow 2200/tcp
   * sudo ufw allow 80/tcp
   * sudo ufw allow 123/udp
   * sudo ufw enable
   * sudo ufw status
       - Status: active
 # Configure the local timezone to UTC
   * Run sudo dpkg-reconfigure tzdata,
     - Choose US in Geographic area, then choose Pacific ocean in Time Zone.
 # Configure key-based authentication for grader user
   * Run this command cp /ubuntu/.ssh/authorized_keys /home/grader/.ssh/authorized_keys 
   * Now you are only able to login using ssh -i ~/.ssh/privatekey.pem -p 2200 grader@34.230.84.216
     
         

   
   
   
   
   

