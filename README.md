*LINUX SERVER CONFIGURATION - PROJECT 5 - Version 1 - January 2016*
****
**1. PROJECT DESCRIPTION**

This is Project 5 "Linux Server Configuration" of the Udacity Nanodegree "Full Stack Web Developer". In this project the web application "Lisa's Grocery Store" developed for Project 3 "Item Catalog" is deployed on a virtual server in Amazon's Elastic Compute Cloud (EC2) provided by Udacity and Amazon to be accessible to the public. 

This README.md file contains the information needed to access the hosted web application and also a step-by-step walkthrough how the specification tasks required to complete the project where executed, what software was installed, what configuration changes where made and what third-party resources where made use of to complete these steps. 

*Note: All extra specification has been implemented.*  

*1. Security:*  
*The firewall has been configured to monitor for unsuccessful login attempts and appropriately bans attackers(see step 10: fail2ban).*  
*Cron scripts have been included to automatically manage package updates (see step 11).*

*2. Application Functionality:*  
*The VM includes monitoring applications that provide automated feedback on application availability status* *and/or system security alerts (see step 10: fail2ban and step 24: Glances).*
  
****
**2. ACCESS THE HOSTED WEB APPLICATION**

***2.3  Access Information***  

Public IP address: *52.35.124.29*  
SSH port: *2200*  
Complete URL: *http://ec2-52-35-124-29.us-west-2.compute.amazonaws.com/*

***2.2 Access via browser***

To access the hosted web application via browser, open a browser and go to:  
*[Lisa's Grocery Store](http://ec2-52-35-124-29.us-west-2.compute.amazonaws.com/)*

***2.3 Acess via ssh***

To access the virtual server via ssh, open a bash (e.g. Git Bash) and type in:  
*$ ssh grader@52.35.124.29 -p 2200 -i ~/.ssh/udacity_key.rsa*
  
*Note: You must have the private key file udacity_key.rsa in the folder ~/.ssh (where ~ is your environment's home directory)!*
 **** 
 **3. STEP-BY-STEP WALKTHROUGH**

***STEP 1: Getting Started with the Virtual Machine***

- Precondition: [VirtualBox](https://www.virtualbox.org/wiki/Downloads) and [Vagrant](https://www.vagrantup.com/downloads.html) are downloaded and installed  
- Create a new folder on your computer, then open that folder within your terminal.  
- Tell Vagrant what kind of Linux virtual machine you would like to run:  
   *$ vagrant init ubuntu/trusty64*
- Download and start running the virtual machine:  
  *$ vagrant up*
- Log into you virtual machine:  
   *$ vagrant ssh*
  
   Source: [Udacity: Configuring Linux Web Servers](https://www.udacity.com/courses/ud299)   

***STEP 2: Create a new Development Environment***

- Go to:  *https://www.udacity.com/account#!/development_environment*
- Download the Private Key
- Move the private key file into the folder *~/.ssh* (where ~ is your environment's home directory)
- Open your terminal and type in:  
   *$ chmod 600 ~/.ssh/udacity_key.rsa*
- Now you can log in as root:  
   *$ ssh -i ~/.ssh/udacity_key.rsa root@52.35.124.29*
     
   Source: [Udacity: Configuring Linux Web Servers](https://www.udacity.com/courses/ud299)   

***STEP 3: Create a New User and Grant this User Sudo Permissions***   

- Create the new user by typing in your terminal:  
   *$ adduser grader*  
     
   *Note: You may add some additional information (e.g. full name), but you do not have to.*  
     
- Check if the new user exists:  
   *$ sudo apt-get install finger*  
   *$ finger grader*  
      
- Grant the new user sudo permissions by creating file *grader* in */etc/sudoers.d*:  
   *$ touch /etc/sudoers.d/grader*  
   *$ nano /etc/sudoers.d/grader*  
   Add to file:     
   *grader ALL=(ALL:ALL)ALL*      
   
   Sources:  
   - [DigitalOcean: How to edit sudoers file](https://www.digitalocean.com/community/tutorials/how-to-edit-the-sudoers-file-on-ubuntu-and-centos)  
   - [PlusBryan: 5 Minutes on a Server](http://plusbryan.com/my-first-5-minutes-on-a-server-or-essential-security-for-linux-servers)  
   - [DigitalOcean: Initial Server Setup with Ubuntu](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-14-04)  
   - [TheGeekStuff: SSH Login](http://www.thegeekstuff.com/2008/11/3-steps-to-perform-ssh-login-without-password-using-ssh-keygen-ssh-copy-id/)

***STEP 4: Update all currently installed packages***  

- Update the list of available packages and versions and install them:  
   *$ sudo apt-get update*  
   *$ sudo apt-get upgrade*  

  Source: [AskUbuntu: Update/Upgrade Packages:](http://askubuntu.com/questions/94102/what-is-the-difference-between-apt-get-update-and-upgrade)
  
***STEP 5: Configure the local timezone to UTC***  
  
- Change the local timezone to UTC by:  
   *$ sudo dpkg-reconfigure tzdata*  
    Choose *'None of the above'* and then *'UTC'*

  Source: [AskUbuntu: Change Time Zone Settings](http://askubuntu.com/questions/3375/how-to-change-time-zone-settings-from-the-command-line)

***STEP 6: Change SSH Port***
  
- *$ nano /etc/ssh/sshd_config*  
- Change *Port* from *22* to *2200*
- Temporarily enable password authentication (just in case): *PasswordAuthentication yes*  
- *$ stop ssh* 
- *$ start ssh*  

   Source: [Udacity: Configuring Linux Web Servers](https://www.udacity.com/courses/ud299)   

***STEP 7: Create SSH Key***
    
On you local machine:    
  
- Create the public and private key:  
   *$ ssh-keygen*  
- Copy the public key on the remote server:  
   *$ ssh-copy-id grader@52.35.124.29 -p 2200*  
- Login:  
   *$ ssh grader@52.35.124.29 -p 2200*
   
   Sources:    
   - [DigitalOcean: Initial Server Setup with Ubuntu](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-14-04)  
   - [PlusBryan: 5 Minutes on a Server](http://plusbryan.com/my-first-5-minutes-on-a-server-or-essential-security-for-linux-servers)

***STEP 8: Disable Root Login and Password Authentication***  
  
- *$ sudo nano /etc/ssh/sshd_config*    
    *PasswordAuthentication no*  
    *PermitRootLogin no*
    
   Sources:    
   - [DigitalOcean: Initial Server Setup with Ubuntu](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-14-04)  
   - [PlusBryan: 5 Minutes on a Server](http://plusbryan.com/my-first-5-minutes-on-a-server-or-essential-security-for-linux-servers)  

***STEP 9: Configure the Uncomplicated Firewall (UFW)***  
  
  - *$ sudo ufw default deny incoming*  
  - *$ sudo ufw allow 2200/tcp*  
  - *$ sudo ufw allow 80/tcp*  
  - *$ sudo ufw allow 123/udp*   
  - *$ sudo ufw enable*  

   Source: [Udacity: Configuring Linux Web Servers](https://www.udacity.com/courses/ud299)     

***STEP 10: Install and Configure fail2ban (extra credit Security, extra credit Application Functionality)***  
 
  - Install *fail2ban*, a daemon that monitors login attempts and blocks suspicious activities:  
     *$ sudo apt-get install fail2ban*  
  - Configure (use a copy and leave *jail.conf* as it is):  
     *$ sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local*  
     *$  sudo nano /etc/fail2ban/jail.local*  
      - Change port in [SSH] section to *2200*
      - Configure automated email alerts:  
         *destemail = lisa.kugler@gmail.com*  
         *action = %(action_mwl)s*  
  - Restart:  
     *$ sudo service fail2ban restart*
  - Check if *fail2ban* is running:  
     *$ sudo /etc/init.d/fail2ban status* 
  - If someone was blocked, it is logged in:  
     */var/log/fail2ban.log*
  - To test fail2ban change the email address and restart fail2ban
     
     Sources:   
     - [DigitalOcean: Protect SSH with fail2ban on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-protect-ssh-with-fail2ban-on-ubuntu-14-04)  
     - [DigitalOcean: Protect Apache Server with fail2ban on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-protect-an-apache-server-with-fail2ban-on-ubuntu-14-04)
     - [MakeTechEasier: Protect SSH with fail2ban on Ubuntu](https://www.maketecheasier.com/protect-ssh-server-with-fail2ban-ubuntu)
     - [HowToForge: fail2ban](https://www.howtoforge.com/fail2ban_debian_etch)
     - [HelpUbuntu: fail2ban](https://help.ubuntu.com/community/Fail2ban)
     - [GistGithub: Install and configure sendmail](https://gist.github.com/adamstac/7462202)
  
***STEP 11: Cron scripts to automatically manage package updates (extra credit Security)***
  - Install the cron scripts:  
     *$ sudo apt-get install unattended-upgrades*    
     *$ sudo dpkg-reconfigure unattended-upgrades*    
  - Check if automatic updates are enabled:  
     *$ sudo cat /etc/apt/apt.conf.d/20auto-upgrades*  
         APT::Periodic::Update-Package-Lists "1";
         APT::Periodic::Unattended-Upgrade "1";
     
     Sources:
     - [HelpUbuntu: Automatic Security Updates](https://help.ubuntu.com/community/AutomaticSecurityUpdates)
     - [AskUbuntu: How To Enable Security Updates](http://askubuntu.com/questions/9/how-do-i-enable-automatic-updates)
     - [How To Check if Automatic Security Updates are Enabled](http://askubuntu.com/questions/172524/how-can-i-check-if-automatic-updates-are-enabled)

***STEP 12: Apache Web Server***  
  
- Install:  
   *$ sudo apt-get install apache2*  
- Check in the browser if the default page appears:  
  *[http://52.35.124.29/](http://52.35.124.29/)*

***STEP 13: mod_wsgi***
  
- *$ sudo apt-get install libapache2-mod-wsgi*

***STEP 14:  Install and Configure PostgreSQL***

- Install PostgreSQL:  
  *$ sudo apt-get update*
  *$ sudo apt-get install postgresql postgresql-contrib*
  
- Check that remote connections are not allowed:  
  *$ sudo nano /etc/postgresql/9.3/main/pg_hba.conf*
 
- Login and create user *catalog* with limited permissions:  
  *$ sudo su - postgres*  
  *$ psql*
      # CREATE USER catalog WITH PASSWORD '[THE_PASSWORD]' CREATEDB;  
      # \du  
      # CREATE DATABASE grocery_store WITH OWNER catalog;  
      # \c grocery_store  
      # REVOKE ALL ON SCHEMA public FROM public;  
      # GRANT ALL ON SCHEMA public TO catalog;  
      # \q  
  *$ exit*  

  Sources:
  - [DigitalOcean: Postgresql on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-secure-postgresql-on-an-ubuntu-vps)  
  - [Postgresql: Create Role](http://www.postgresql.org/docs/8.2/static/sql-createrole.html)


***STEP 15:  Install and Configure Git***  

- Install:  
  *$ sudo apt-get update*  
  *$ sudo apt-get install git*

- Set user name and email:  
   *$ git config --global user.name "Lisa Kugler"*  
   *$ git config --global user.email "lisa.kugler@gmail.com"*  

  Source: [DigitalOcean: Git on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-install-git-on-ubuntu-14-04)  


***STEP 16:  Install pip and all Needed Python Packages***  

- *$ sudo apt-get install python pip*  
- *$ sudo pip install flask*  
- *$ sudo pip install sqlalchemy*  
- *$ sudo pip install oauth2client*  
- *$ sudo apt-get install python-psycopg2*  
  
*** STEP 18:  Clone Catalog App Project and make .git directory publicly inaccessible***

- *$ cd /var/www*  
- *$ sudo git clone https://github.com/LisaJK/grocerystore*  
- *$ cd /var/www/grocerystore/.git*  
- *$ sudo touch .htaccess*  
- *$ sudo nano .htaccess*  
       Order allow,deny  
       Deny from all  

  Source: [StackOverflow: Make Git Directory Web Inaccessible](http://stackoverflow.com/questions/6142437/make-git-directory-web-inaccessible)

***STEP 19: Change application.py***
- *$ sudo nano /var/www/grocerystore/application.py*  
        # this has to be changed in production...
        # app.secret_key = 'super_secret_key'
        app.debug = False
        # app.run(host='0.0.0.0', port=8000)
  
***STEP 20: Configure wsgi***

- *$ cd /var/www/grocerystore*  
- *$ sudo touch grocerystore.wsgi*  
- *$ sudo nano grocerystore.wsgi*  
       import sys, os  
       sys.path.insert(0, '/var/www/grocerystore')  
       from app import {app global variable in app.py} as application  
       application.secret_key = os.urandom(64)  
       
  Sources:    
  - [Alex Nisnevich: Setting up Flask on EC2](http://alex.nisnevich.com/blog/2014/10/01/setting_up_flask_on_ec2.html)
  - [DigitalOcean: Flask on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)

***STEP 21: Change engine from SQLite to Postgresql***

- *$ sudo nano /var/www/grocerystore/load_grocery_store.py*  
       engine = create_engine('postgresql://catalog:[THE_PASSWORD]@localhost/grocery_store')  
  
- *$ sudo nano /var/www/grocerystore/data_base_setup.py*  
       engine = create_engine('postgresql://catalog:[THE_PASSWORD]@localhost/grocery_store')  

- *$ sudo nano /var/www/grocerystore/application.py*  
       engine = create_engine('postgresql://catalog:[THE_PASSWORD]@localhost/grocery_store')

- Fill Database:  
   *$ python /var/grocerystore/load_grocery_store.py*  

- Restart Apache:  
  *$ sudo service apache2 restart*  

- Check for Errors:  
   *$ sudo tail -50 /var/log/apache2/error.log*

  Source: [StackOverflow: Apache Error Logs](http://stackoverflow.com/questions/8007176/500-error-without-anything-in-the-apache-logs)

***STEP 22: Configure Apache to use the WSGI***

- *$ cd /etc/apache2/sites-available/*  
- *$ sudo touch grocerystore.conf*  
- *$ sudo nano grocerystore.conf*  
       <VirtualHost *:80>
         ServerName http://ec2-52-35-124-29.us-west-2.compute.amazonaws.com/
         ServerAlias http://ec2-52-35-124-29.us-west-2.compute.amazonaws.com
         WSGIScriptAlias / /var/www/grocerystore/grocerystore.wsgi
         <Directory /var/www/grocerystore/>
                Order allow,deny
                Allow from all
         </Directory>
         ErrorLog ${APACHE_LOG_DIR}/error.log
         LogLevel info
         CustomLog ${APACHE_LOG_DIR}/access.log combined
      </VirtualHost>

- *$ sudo /etc/init.d/apache2 reload*  
- *$ sudo apachectl restart*  

***STEP 23: OAuth Google+ and Facebook***

Google+:
- Go to [Google Developers Console](https://console.developers.google.com/apis/credentials?project=lisas-catalog-app)
- Add *http://ec2-52-35-124-29.us-west-2.compute.amazonaws.com* to *Authorized JavaScript origins* and *Authorized redirect URIs*

Facebook:
- Go to [Facebook Developers Console](https://developers.facebook.com/apps/1610860372517361/settings/)
- Change *Site URL* to *http://ec2-52-35-124-29.us-west-2.compute.amazonaws.com*
- Add *http://ec2-52-35-124-29.us-west-2.compute.amazonaws.com* to *OAuth Redirect URI*

***STEP 24: Install Glances (extra credit Application Functionality)***

- *$ apt-get install -y python-pip build-essential python-dev*  
- *$ pip install Glances*  
- To start Glances and monitor the application:
   *$ sudo glances*
- To stop Glances press *q*

Sources:
- [Vultr: Monitoring with Glances](https://www.vultr.com/docs/monitoring-your-server-with-glances)
 - [Udacity Forum: P5 How I got through it](https://discussions.udacity.com/t/p5-how-i-got-through-it/15342/12)
 - [Stueken:FSND-P5 Linux Server Configuration](https://github.com/stueken/FSND-P5_Linux-Server-Configuration)
 - [Cyberciti: Glances](http://www.cyberciti.biz/faq/linux-install-glances-monitoring-tool/)


**4. ADDITIONAL NOTES**

***4.1 System Restart Required***
  
After login, sometimes *System Restart Required* appears. As these updates are usually security updates, do a *$ sudo reboot*, wait and login again.

Source: [AskUbuntu: System Restart](http://askubuntu.com/questions/258297/should-i-always-restart-the-system-when-i-see-system-restart-required)

***4.2 Sudo Unable to Resolve Host Name***

If the sudo command is executed, but the message *sudo unable to resolve host name* appears, append the hostname (which can be found in */etc/hostname*) in the first line of */etc/hosts*.

Source: [AskUbuntu: Sudo Unable to Resolve Host Name](http://askubuntu.com/questions/59458/error-message-when-i-run-sudo-unable-to-resolve-host-none)

***4.3. Apache Could Not Reliably Determine the Servers Fully Qualified Domain Name***

 - *$ echo "ServerName localhost" | sudo tee /etc/apache2/conf-available/fqdn.conf*  
 - *$ sudo a2enconf fqdn*  
 - *$ service apache2 reload*  
 
Source: [AskUbuntu: Could Not Reliably Determine the Servers Fully Qualified Domain Name](http://askubuntu.com/questions/256013/could-not-reliably-determine-the-servers-fully-qualified-domain-name)


**5. LIST OF THIRD PARTY RESOURCES**

- [Alex Nisnevich: Setting up Flask on EC2](http://alex.nisnevich.com/blog/2014/10/01/setting_up_flask_on_ec2.html)
- [AskUbuntu: Update/Upgrade Packages:](http://askubuntu.com/questions/94102/what-is-the-difference-between-apt-get-update-and-upgrade)
- [AskUbuntu: Change Time Zone Settings](http://askubuntu.com/questions/3375/how-to-change-time-zone-settings-from-the-command-line)
- [AskUbuntu: How To Check if Automatic Security Updates are Enabled](http://askubuntu.com/questions/172524/how-can-i-check-if-automatic-updates-are-enabled)
- [AskUbuntu: How To Enable Security Updates](http://askubuntu.com/questions/9/how-do-i-enable-automatic-updates)
- [AskUbuntu: System Restart](http://askubuntu.com/questions/258297/should-i-always-restart-the-system-when-i-see-system-restart-required)
- [AskUbuntu: Sudo Unable to Resolve Host Name](http://askubuntu.com/questions/59458/error-message-when-i-run-sudo-unable-to-resolve-host-none)
-  [AskUbuntu: Could Not Reliably Determine the Servers Fully Qualified Domain Name](http://askubuntu.com/questions/256013/could-not-reliably-determine-the-servers-fully-qualified-domain-name)
- [DigitalOcean: Flask on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)
- [DigitalOcean: How to edit sudoers file](https://www.digitalocean.com/community/tutorials/how-to-edit-the-sudoers-file-on-ubuntu-and-centos) 
- [DigitalOcean: Protect SSH with fail2ban on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-protect-ssh-with-fail2ban-on-ubuntu-14-04)  
- [DigitalOcean: Protect Apache Server with fail2ban on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-protect-an-apache-server-with-fail2ban-on-ubuntu-14-04)
- [DigitalOcean: Git on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-install-git-on-ubuntu-14-04)  
- [DigitalOcean: Initial Server Setup with Ubuntu](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-14-04) 
- [DigitalOcean: Postgresql on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-secure-postgresql-on-an-ubuntu-vps) 
- [HelpUbuntu: Automatic Security Updates](https://help.ubuntu.com/community/AutomaticSecurityUpdates)
- [MakeTechEasier: Protect SSH with fail2ban on Ubuntu](https://www.maketecheasier.com/protect-ssh-server-with-fail2ban-ubuntu)
- [HelpUbuntu: fail2ban](https://help.ubuntu.com/community/Fail2ban)
- [HowToForge: fail2ban](https://www.howtoforge.com/fail2ban_debian_etch)
- [PlusBryan: 5 Minutes on a Server](http://plusbryan.com/my-first-5-minutes-on-a-server-or-essential-security-for-linux-servers) 
- [Postgresql: Create Role](http://www.postgresql.org/docs/8.2/static/sql-createrole.html)
- [StackOverflow: Make Git Directory Web Inaccessible](http://stackoverflow.com/questions/6142437/make-git--directory-web-inaccessible)
- [StackOverflow: Apache Error Logs](http://stackoverflow.com/questions/8007176/500-error-without-anything-in-the-apache-logs)
- [Stueken:FSND-P5 Linux Server Configuration](https://github.com/stueken/FSND-P5_Linux-Server-Configuration)
- [TheGeekStuff: SSH Login](http://www.thegeekstuff.com/2008/11/3-steps-to-perform-ssh-login-without-password-using-ssh-keygen-ssh-copy-id/)
- [Udacity: Configuring Linux Web Servers](https://www.udacity.com/courses/ud299) 
- [Udacity Forum: P5 How I got through it](https://discussions.udacity.com/t/p5-how-i-got-through-it/15342/12)
- [Udacity Forum: Markedly underwhelming ... resource list for P5](https://discussions.udacity.com/t/markedly-underwhelming-and-potentially-wrong-resource-list-for-p5/8587)  
- [Udacity Forum: Project 5 Resources](https://discussions.udacity.com/t/project-5-resources/28343)
- [Vultr: Monitoring with Glances](https://www.vultr.com/docs/monitoring-your-server-with-glances)
