# Koha Install and Configuration

## Create New VM
* Create new VM to ensure enough RAM and disk space. Follow these directions:
	1. Go to SystemsLib repository
	2. Drop down buttom and click **Create VM**, change name **koha-install** and change information:
	3. Machine Configuration: e2-medium (2 vCPU, 1 core, 4 GB)
	4. OS and storage: Ubuntu 24.04 LTS, 20 GB Hard Disk
	5. Networking: Allow HTTP traffic
	6. Network Tag: **koha-staff-8080** and **koha-opac-8081**
* Create firewall rules to open traffic to our two ports
* For Firewall Rule 1: Port 8080
	1. VPC Network
	2. Click Firewall
	3. Choose a firewall rule (do not click policy)
	4. add name: **koha-staff-8080**
	5. add description: **Open port 8080 for the Koha staff interface**
	6. go to Targets then Specified Target Tags and add **koha-staff-8080**
	7. Source IPv4 ranges: 0.0.0.0/0
	8. In Specified Ports: Click TCP and add 8081 in box
	9. Click Create
* For Firewall Rule 2: Port 8081 keep details the same except in the following areas:
	1. add name: **koha-opac-8081**
	2. add descriptions: **open port 8081 for the Koha opac interface**
	3. in target tags: **koha-opac-8081**
	4. in TCP box add 8081
	5. create

## Install Koha Repository
* connect to our terminal multiplexer using command **tmux**
* note: use comman **tmux attach** if ever disconnected from terminal. work will remain there

### Server Setup
* first, update our local repositories using **sudo apt update**
* upgrade **sudo apt upgrade**
* use **sudo apt autoremove -y && sudo apt clean** to remove packages that have been automatically installed. this will help create more disk space

### Add Koha Repository
* we will not add repositories to sync and help install Koha
* use the following:
	1. ** apt install apt-transport-https ca-certificates curl**
	2. **mkdir -p --mode=0755 /etc/apt/keyrings**
	3. **curl -fsSL https://debian.koha/gpg.asc -o /etc/apt/keyrings/koha.asc**
* make self the root user of repo: **sudo su**
* once in terminal, paste the following:
tee /etc/apt/sources.list.d/koha.sources <<EOF
Types: deb
URIs: https://debian.koha-community.org/koha/
Suites: 25.05
Components: main
Signed-By: /etc/apt/keyrings/koha.asc
EOF
* use **cat /etc/apt/sources.list.d/koha.source** to verify info matches
* exit

### Install MariaDB
* this will act similar to MySQL 
* **sudo apt update**
* install with **sudo apt install mariadb-server**

### Install Koha
* update and sync our new repository with the remote Koha repo: **sudo apt update**
* review package info to see if it is correct : **apt show koha-common**
* if correct, install using **sudo apt install koha-common**

### Open Ports
* we now need to update our files to allow access to our 8080(admin) and 8081(public) open access. these match the ports we updated in our firewall rules
* make a copy of our file for safe keeping: **sudo cp /etc/koha/koha-sites.conf /etc/koha/koha-sites.conf.backup**
* open our text editor **sudo nano /etc/koha/koha-sites.conf**
* change INTRAPORT="8080"
* and OPACPORT="8081"

### Apache2 Setup
* apache2 is setup but certain modules need enabled using **sudo a2enmod rewrite cgi headers proxy_http**
* restart with **sudo systemctl restart apache2**
* we now need to configure Apache2 to our ports
* open text editor **sudo nano /etc/apache2/ports.conf**
* under Listen 80, add **Listen 8080** and **Listen 8081**

### Create Koha Instance
* to initiate the install, we will create our library which will be named thefakelibrary
* create **sudo koha-create --create-db thefakelibrary**
* restart system **sudo systemctl restart apache2**

### Apache2 Configurations
* turn off configuration that automatically makes /var/www/html web root: **sudo a2dissite 000-default**
* turn on network compression: **sudo a2enmod deflate**
* enable the new library: **sudo a2ensite thefakelibrary**
* reload **sudo systemctl reload apache2** and restart **sudo systemctl restart apache2** to process the new configurations

### Koha Web Installer 
* find our username and password using **sudo koha-passwd thefakelibrary**
* check both the staff and public interfaces
