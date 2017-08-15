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
 # Install Apache
   * Run sudo apt-get install apache2
    - In this Stage, we can see the apache2 default page in http://34.230.84.216/
 # Install mod_wsgi
   * Run sudo apt-get install libapache2-mod-wsgi python-dev
   * Run sudo a2enmod wsgi
       - Enable mod_wsgi
   * Start the web server with sudo service apache2 start
 # Clone the Item Catalog app from Github 
   * install git using: sudo apt-get install git
   * cd /var/www
   * sudo mkdir catalog
   * Change owner of the newly created catalog folder sudo chown -R grader:grader catalog
   * cd /catalog
   * Clone your project from github git clone https://github.com/Aarthyravi/Item-catalog.git catalog
   * Create a catalog.wsgi file, then add this inside:
       
          import sys
          import logging
          logging.basicConfig(stream=sys.stderr)
          sys.path.insert(0, "/var/www/catalog/")
      
          from catalog import app as application
          application.secret_key = 'supersecretkey'
    * Rename finalproject.py to init.py mv finalproject.py __init__.py  
 # Install virtual environment
   - Install the virtual environment         --> sudo pip install virtualenv
   - Create a new virtual environment with   --> sudo virtualenv venv
   - Activate the virutal environment source --> venv/bin/activate
   - Change permissions sudo                 --> chmod -R 777 venv 
 # Install Flask and other dependencies
   - Install pip with   ----> sudo apt-get install python-pip
   - Install Flask      ----> sudo pip install Flask
   - Install other project dependencies ----> sudo pip install httplib2 oauth2client sqlalchemy psycopg2 sqlalchemy_utils   
 # Configure and enable a new virtual host
   - Run this: sudo nano /etc/apache2/sites-available/catalog.conf
   - Paste this code:
         
         <VirtualHost *:80>
             ServerName 34.230.84.216
             ServerAlias ec2-34-230-84-216.compute-1.amazonaws.com
             ServerAdmin admin@34.230.84.216
             WSGIDaemonProcess catalog python-path=/var/www/catalog:/var/www/catalog/venv/lib/python2.7/site-packages
             WSGIProcessGroup catalog
             WSGIScriptAlias / /var/www/catalog/catalog.wsgi
            <Directory /var/www/catalog/catalog/>
                Order allow,deny
                Allow from all
            </Directory>
            Alias /static /var/www/catalog/catalog/static
            <Directory /var/www/catalog/catalog/static/>
                Order allow,deny
                Allow from all
            </Directory>
            ErrorLog ${APACHE_LOG_DIR}/error.log
            LogLevel warn
            CustomLog ${APACHE_LOG_DIR}/access.log combined
         </VirtualHost>   
   - Enable the virtual host sudo a2ensite catalog   
 # Install and configure PostgreSQL
   * sudo apt-get install libpq-dev python-dev
   * sudo apt-get install postgresql postgresql-contrib
   * sudo su - postgres
   * psql
   * CREATE USER catalog WITH PASSWORD 'password';
   * ALTER USER catalog CREATEDB;
   * CREATE DATABASE catalog WITH OWNER catalog;
   * \c catalog
   * REVOKE ALL ON SCHEMA public FROM public;
   * GRANT ALL ON SCHEMA public TO catalog;
   * \q
   * exit
   * Change create engine line in your __init__.py and database_setup.py to: 
       engine = create_engine('postgresql://catalog:password@localhost/catalog')
   * python /var/www/catalog/catalog/database_setup.py
   * Make sure no remote connections to the database are allowed. 
      - Check if the contents of this file ---> sudo nano /etc/postgresql/9.3/main/pg_hba.conf looks like this:
          
            local   all             postgres                                peer
            local   all             all                                     peer
            host    all             all             127.0.0.1/32            md5
            host    all             all             ::1/128                 md5
   * Restart Apache
       - sudo service apache2 restart
   * Visit site at http://34.230.84.216  
