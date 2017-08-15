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
        - Ubuntu@ip-172-26-12-122:~$
