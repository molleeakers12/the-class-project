# Repository Overview
This repository will be for my Systems Librarianship class. This project will be used to help me understand how library databases and software are used so that one day I can
hopefully operate and manage these systems myself.

# 02/10/2026 Progress
* I have learnt how to setup Git on my remote server
* I have learnt how to setup a local download of my Git repository on my server
* I have learnt how to push my local changes to the remote Git repository through Git

# Reflection 
This has been a very interesting assignment because there was a lot more that went into than I would have
initially thought. This work can be very intricate.
At first, using the text editor was very confusing to me because my brain wants to look at this work in a very abstract way when in fact it can be pretty straightforward.

# 02/15/2026 Progress
* Learnt how to use grep function to search through large files.
* Used an example from World of Science searching for differnt forms of "cats"
* Tried different commands to get different results ie. case sensitivety

# 02/23/2026 Progress
For this section, I learnt how to install yaz and use it to search an OPACs Marc file. I first updated and upgraded my system and then installed yaz.
I then opened yaz-client and then opened the portal. I was then able to run basic commands to find searches based on title, subject headings, author etc. I then was able to
open the results using the "show" command.

# Lamp Stack Introduction
LAMP stands for Linux, Apache, MySQL, and PHP. The LAMP stack represents a suite of open source software commonly used to host websites and appliccations. 
Linux gives the operating system, Apache serves HTTP requests, PHP runs the server code, and MySQL stores the data. It is commonly used because together the applications provide the functionality needed to serve content management systems.

## 03/01/2026 Installing Apache
* I first used **sudo apt update** and **sudo apt -y upgrade** to secure latest version
* **apt search apache2 | head** to find specific package 
* then found the **apache2** package to install
* **apt show apache2** to locate 
* install with **sudo apt install apache2**
* use **systemctl status apache2** to configure, **Active** and **Loaded** showed it was running properly
I then checked the pre-existing browser
* **sudo apt install w3m**
* used my private IP to check site **w3m 34.55.208.21**
* it showed Apache2 Ubuntu Default Page It Works!
Creating Web Page
* **cd /var/www/html** then **sudo mv index.html index.original.html** and **sudo nano index.html**
* first directed to the document root, then create and rename backup file
* open text editor and paste HTML code provided
* use IP with index.original.html to see if it is working
For this section, I had little issues. The one i had was with the last commands because I couldn't quite grasp the changes. But after I got it sorted, it was all fine!

## 03/08/2026 Installing PHP
* I first used basic commands to make sure the server is updated
* then used **sudo apt install php libapache2-mod-php** to install packages
* **sudo systemctl restart apache2** to restart
* to check status **systemctl status apache2**
Check Installation 
* change to doc root **cd /var/www/html/**
* create file **sudo nano info.php**
* add following <?php phpinfo(); ?>
* use IP to view file: *http://34.55.208.21/info.php*
* once its been confirmed running delete **sudo rm /var/www/html/info.php**
Configurations: default to file titled index.php
* copy the file for safety **cd /etc/apache2/mods-available/** **sudo cp dir.conf dir.conf.bak** then **sudo nano dir.conf**
* change line so index.php is first
* check configuration **apachectl configtest**
* Got Syntax Ok message so reload and check status **sudo systemctl reload apache2** and **systemctl status apache2**
Create index.php file
* cd back to doc root and use nano to create index.php file
* add HTML provided
* save and exit 
* view site with **http://34.55.208.21/**
For this installation, I got a bit lost going back and forth between the files. It was hard for me to visualize what I was doing until I saw the website.

## 03/15/2026 Installing mySQL
* Run all basic commands to update server
* install metapackage **sudo apt install mysql-server**
* use **systemctl status mysql** to check status
* Shows **Loaded** and **Active**
* use **sudo mysql_secure_installation** to run security checks 
* Answer Y to all questions
* login to database **sudo mysql -u root**
* now in MySQL database, syntax is different
* **mysql> show databases;** to list all databases available 
* following were returned: information_schema, mysql, performance  schema, sys
* exit **mysql> \q**
Create User Account
* need to reserve root MySQL user for special administrative cases and then a regular one
* Log into **sudo mysql -u root**
* create new user at prompt
Log in as Regular User
* open bash file **nano ~/.bashrc**
* add this to bottom of file **export MYSQL_PS1="[\d]> "** save and exit
* created a new user named opacuser, a new database for opacuser that is called opacdb. 
* use **mysql> show databases;** and **mysql> use opacdb;** to switch to new databases
* input commands to create data fields
* then followed class instructions and practiced different commands
* Install PHP and MySQL Support
* in file add credentials
* create a file for the html display **cd /var/www/html** and **sudo nano opac.php**
* Add the text provided 
* View site from a browser

# Reflection on LAMP Stack
Creating this was very interesting and it finally came together and that was so fun to see. One way I would test to see if everything was working was obviously when checking the websites. If the website was running then I knew I was making progress. 
This correlates with the main challenge I had which was sometimes being confused on what I was working for. 
In both instances, the best solution was to go back to what I do know and make my way from there. 
