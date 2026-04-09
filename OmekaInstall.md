# Installing OMEKA
* update system **sudo apt update** and **sudo apt upgrade**
* check version of PHP and MySQL to make sure they match up with OMEKA requirements
* **mysql --version**
* **php --version**
* both met the requirements
* restart system **sudo systemctl restart**
* check that php extensions are enabled **php -m**
* install ImageMagick for photo files **sudo apt install imagemagick**
* enable mod_rewrite to rewrite URLS **sudo a2enmod rewrite**
* prompt will say to restart apache. use above command again
* switch to MySQL **sudo mysql -u root**
* create new user and password **create user 'omeka'@'localhost' identified by 'Ruthie_Brewer26!';
* create database **create database omeka;**
* grant all privileges to new user for database **grant all privileges on omeka.* to 'omeka'@'localhost';**
* check outputs **show databases;** and quit **\q**
* install newest version of Omeka onto server using **wget**
* Note: here i had issues. trying to use the url like we did when creating WordPress would not work(ie. using wget). entering this kept returning 404 Errors saying it was not found. I downloaded the file and then manually uploaded it onto the server. I then used **cd** to return to home directory, **ls** to find it there. I then used  **sudo unzip omeka-3.2.zip** this created the directory which i then used **sudo mv omeka-3.2 /var/www/html** to move it to the correct directory
* find db.ini **sudo nano db.ini**
* change following
1. host: localhost
2. username: omeka
3. password: Ruthie_Brewer26!
4. dbname: omeka
* use command to give Apache group write access **sudo chmod -R g+w **
 
# Adding Data to Omeka 
* go to admin site **http://35.184.202.219/omeka/admin**
* Note: here i ran into issues with the server as it was taking me to my Wordpress sample page and showing a 404 Error. In the end, i found out that i had missed a configuration/permission step in the installation process so /omeka/admin was being denied. I used **sudo chown -R www-data:www-data /var/www/html/omeka** and refreshed the site and it was working
* i completed the installation process for Omeka
* I then added an edition of Jane Austen's Pride and Prejudice
