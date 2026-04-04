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

# 03/29/26 Creating Barebones OPAC and Cataloging Module
* An OPAC (Online Public Access Catalog) is the search system you use in a library to find materials.
* The OPAC can tell you if an item is available, where it is located, etc.
* Ultimately, it helps you find, locate, and access library materials.

## Intro to Relational Databases
### Relational Database Structure
* A relational database splits bibliographic records into multiple related tables (ie. authors, subjects, etc.)
* This keeps things organized and efficient (ie. books can have multiple authors).
* The OPAC is the interface in which users search for the materials.
* Relational databases are the storage system behind it. 
* They combine information from different tables quickly and give results.

### Code Documentation: 
* Login to to opacuser using **sudo mysql -u root**
* Create database called DinnerDB **mysql> create database DinnerDB;**
* Grant privileges to opacuser **grant all privileges on DinnerDB * to 'opacuser'@'localhost';**
* Exit
* Log in as opacuser **-u opacuser -p**
* Check if we can see DinnerDB database **show databases;** and **use DinnerDB;**
* Create Meals Table create table Meals (
    meal_id int auto_increment primary key,
    meal_name varchar(100) not null,
    cuisine varchar(50),
    cooking_time int not null default 1 check (cooking_time > 0),
    vegetarian boolean
* Create table Ingredients create table Ingredients (
    ingredient_id int auto_increment primary key,
    meal_id int not null,
    ingredient_name varchar(100) not null,
    quantity varchar(50),
    foreign key (meal_id) references Meals(meal_id) on delete cascade
* Insert Data provided
* Use **SELECT**, **JOIN**, **WHERE**, **ORDER BY** and **GROUP** statement to practice filtering data

### OPAC Creation: Code Documentation
* First update copyright column from previous table creation to use a **DATE** data type
* Use following code 
	1. **mysql -u opacuser -p**
	2. **use opacdb;**
	3. **alter table books add publication_date date;**
	4. **update books set publication_date = str_to_date(concat(copyright, '-01-01'), '%Y-%m-%d');**
	5. **alter table books drop column copyright;**
	6. **alter table books change publication_date copyright date not null;**
* First create basic HTML page that contains a form for entering queries.
* **cd var/www/html/** and then create HTML page using **sudo nano mylibrary.html**
* Add provided HTML to page:
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>MySQL Server Example</title>
    </head>
<body>

    <h1>A Basic OPAC</h1>

    <p>In the form below, <b>optionally</b> enter text in the search field.
    Your search query will search by author, title, or publisher.
    Capitalization is usually not necessary on default case-insensitive MySQL collations.
    It's okay to enter partial information, like part of an author's, title's, or publisher's name.</p>

    <p>You can leave the search field empty and only enter dates.
    Regardless, both start and end dates are required for all searches.
    You can use the date fields to limit results, too.
    I added some extra records, which you can view to know what you can query:</p>

    <p><a href="opac.php">OPAC</a></p>

    <p>This is very much a toy, stripped down
    <a href="https://en.wikipedia.org/wiki/Online_public_access_catalog">OPAC</a>.
    The records are basic.
    Not only do they not conform to <a href="https://www.loc.gov/marc/">MARC</a>,
    they don't even conform to something as simple as <a href="https://www.dublincore.org/">Dublin Core</a>.</p>

    <p>I also don't provide options to select different fields, like author, title, or publisher fields.
    Instead the search field below searches key bibliographic fields (author, title, publisher) in our <b>books</b> table.</p>

    <p>The key idea is to get a sense of how an OPAC works, though.</p>

    <h2>My Basic Library OPAC</h2>

    <form method="post" action="search.php">
        <label for="search">Search Terms (optional):</label>
        <input type="text" name="search" id="search">
        
        <br>
        
        <label for="start_date">Start Date:</label>
        <input type="date" name="start_date" id="start_date" required>
        
        <br>
        
        <label for="end_date">End Date:</label>
        <input type="date" name="end_date" id="end_date" required>
        
        <br>
        
        <input type="submit" value="Search">
    </form>

</body>
</html>
* Save and exit
* Add PHP script to **/var/www/html** using **sudo nano search.php**
* Add following script:
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Search Results</title>
<style>
    table {
        border-collapse: collapse;
        width: 100%;
    }
    th, td {
        border: 1px solid black;
        padding: 8px;
        text-align: left;
    }
</style>
</head>
<body>

    <h1>Search Results</h1>

    <?php
    // Load MySQL credentials
    require_once '/var/www/login.php';

    // Enable MySQL error reporting
    mysqli_report(MYSQLI_REPORT_ERROR | MYSQLI_REPORT_STRICT);

    // Establish connection
    $conn = new mysqli($db_hostname, $db_username, $db_password, $db_database);
    if ($conn->connect_error) {
        die("Connection failed: " . $conn->connect_error);
    }

    if ($_SERVER["REQUEST_METHOD"] == "POST") {
        $search = trim($_POST['search']);
        $start_date = $_POST['start_date'];
        $end_date = $_POST['end_date'];

        // Prepared statement to prevent SQL injection
        $stmt = $conn->prepare("SELECT id, author, title, publisher, copyright FROM books 
                                WHERE (author LIKE ? OR title LIKE ? OR publisher LIKE ?) 
                                AND copyright BETWEEN ? AND ?");

        // Use wildcard search
        $search_param = "%$search%";
        $stmt->bind_param("sssss", $search_param, $search_param, $search_param, $start_date, $end_date);
        $stmt->execute();
        $result = $stmt->get_result();

        if ($result->num_rows > 0) {
            echo "<table>";
            echo "<tr><th>ID</th><th>Author</th><th>Title</th><th>Publisher</th><th>Copyright</th></tr>";

            while ($row = $result->fetch_assoc()) {
                echo "<tr>";
                echo "<td>" . htmlspecialchars($row["id"]) . "</td>";
                echo "<td>" . htmlspecialchars($row["author"]) . "</td>";
                echo "<td>" . htmlspecialchars($row["title"]) . "</td>";
                echo "<td>" . htmlspecialchars($row["publisher"]) . "</td>";
                echo "<td>" . htmlspecialchars($row["copyright"]) . "</td>";
                echo "</tr>";
            }

            echo "</table>";
        } else {
            echo "<p>No results found.</p>";
        }

        $stmt->close();
    }

    $conn->close();
    ?>

    <p><a href="mylibrary.html">Return to search page</a></p>

</body>
</html>
* Add more records to data:
* connect to MySQL server **mysql -u opacuser -p**
* Select database **use opacdb;**
* Insert data.

### Creating Cataloging Module: Code Documentation
* Create new directory **cd /var/www/html** and **sudo mkdir cataloging**
* Create new index.html file **cd cataloging** and **sudo nano index.html**
* Add following script
<!DOCTYPE html>
<html>
<head>
    <title>Enter Records</title>
</head>
<body>
    <h1>OPAC Library Administration</h1>

    <p>This is the library administration page for entering records into the OPAC.</p>
    <p>Please do not use this page unless you are an authorized cataloger.</p>

    <form action="insert.php" method="post">
        <label for="author">Author:</label>
        <input type="text" name="author" id="author" required><br><br>

        <label for="title">Book Title:</label>
        <input type="text" name="title" id="title" required><br><br>

        <label for="publisher">Publisher:</label>
        <input type="text" name="publisher" id="publisher" required><br><br>

        <label for="copyright">Copyright:</label>
        <input type="date" name="copyright" id="copyright" required>

        <input type="submit" value="Submit">
    </form>
</body>
</html>
  
* Now create a form for entering bibliographic data **sudo nano insert.php**
* Add following:
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cataloging: Data Entry</title>
</head>
<body>

<h1>Cataloging: Data Entry</h1>

<?php

// Load MySQL credentials
require_once '/var/www/login.php';

// Enable MySQL error reporting
mysqli_report(MYSQLI_REPORT_ERROR | MYSQLI_REPORT_STRICT);

// Establish connection
$conn = new mysqli($db_hostname, $db_username, $db_password, $db_database);
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

if ($_SERVER["REQUEST_METHOD"] === "POST") {
    $author = trim($_POST["author"] ?? "");
    $title = trim($_POST["title"] ?? "");
    $publisher = trim($_POST["publisher"] ?? "");
    $copyright = $_POST["copyright"] ?? "";

    if ($author === "" || $title === "" || $publisher === "" || $copyright === "") {
        echo "All fields are required.";
    } elseif (!preg_match('/^\d{4}-\d{2}-\d{2}$/', $copyright)) {
        echo "Copyright date must use YYYY-MM-DD format.";
    } else {
        // Prepare and bind SQL statement
        $stmt = $conn->prepare("INSERT INTO books (author, title, publisher, copyright) VALUES (?, ?, ?, ?)");
        $stmt->bind_param("ssss", $author, $title, $publisher, $copyright);

        if ($stmt->execute() === TRUE) {
            echo "New record created successfully";
        } else {
            echo "Error: " . $stmt->error;
        }
        $stmt->close();
    }
} else {
    echo "Please submit records using the cataloging form.";
}

// Close connection
$conn->close();
?>

<p><a href='index.html'>Return to Cataloging Page</a></p>
<p><a href='../mylibrary.html'>Return to Library Home Page</a></p>
</body>
</html>

* Create a security system to make sure unauthorized individuals are not adding to catalog
* Create new authentication file in **/etc/apache2** directory
* Use **sudo htpasswd -c /etc/apache2/.htpasswd libcat** 
* libcat will be username and LibScience our password
* Tell Apache2 web server that we will use htpasswd to control access to module
* **sudo nano /etc/apache2/apache2.conf**
* Go to **<Directory /var/www/** and replace with the following
<Directory /var/www/html/cataloging/>
  Options Indexes FollowSymLinks
  AllowOverride AuthConfig
  Require all granted
</Directory>

* Change cataloging directory and create file **cd /var/www/html/cataloging** then **sudo nano .htaccess**
* Add following:
AuthType Basic
AuthName "Authorization Required"
AuthUserFile /etc/apache2/.htpasswd
Require valid-user

* Check configuration **sudo apachectl configtest**
* Should receive **Syntax OK**
* Restart **sudo systemctl restart apache2**
* Check status **sudo systemctl status apache2**
* Change group ownership of /var/www/html **sudo chown :www-data /var/www/html**
* Set setgid bit. Any new files and directories created within the directory will inherit group ownership of parent directory 
* **sudo find /var/www/html -type d -exec chmod g+s {} +**

### Visit Modules to See if They Work
1. http://34.55.208.21/cataloging/index.html
2. Test PHP page: http://34.55.208.21/cataloging/insert.php

## Important Details
* Always make sure you are in the correct directory. This tripped me up because I would mistakenly create new files outside of the directory and couldn't find them. I think this hindered me because I was creating the files but wasn't getting results because it was outside of the correct directory. Then I would have to go through that process again.
* Make sure, as well, that you are entered the HTML and PHP script accurately. I had issues with the <Directory> changes because I didn't read close enough. I changed one part of the text and not the rest. I went through all of my script twice before realizing this small detail.
* Taking a wrong step with any of these small details can hinder the function of your system; this happened to me. 

## Importance of Using Documentation 
* When doing any of this work, it was important for me to review documentation in many forms.
* I heavily rely on that which is provided in our textbook but I also use my own previous documentation.
* This is especially useful when I need to remember a command from past installations.
* For example, when trying to remember how to create the text file in the /var/www/html directory, I reviewed my own documentation as a refresher. 
* When I review my servers at the end of the installation process, I always go to our discussion boards to compare my results to others in the class. 
* For this assignment, that was very useful because by comparing, I realized I had an error.
* My cataloging module was not showing the username and password requirement.
* Viewing others' work helped me realize I had an error and subsequently solve it!
* In terms of gaps in documentation material, I felt that when creating the files and adding the script was hard to figure out because there was not much detail in how to run that. 
* This was a big issue I encountered. 
* To address this, I reviewed past documentation from myself and within the textbook.
* I also believe at some point I searched Google to help get an answer.

# Installing WordPress 04//04/26

## Check Version Requirements for PHP and MySQL
* Need PHP version of 8.3 or higher
* Run **php --version** : our version is 8.3.6 so we are fine
* Need MySQL version 8.0 or greater
* run **mysql --version** : ours is 8.0.45 so we are fine
* can use **cat /etc/issue.net** to check our Ubuntu release : 24.04.4
* next add additional PHP modules to let WordPress operate fully
* run **sudo apt install php-curl php-xml php-imagick php-mbstring php-zip php-intl**
* restart Apache2 and MySQL **sudo systemctl restart apache2** and **sudo systemctl restart mysql**

## Download and Extract
* change directory **cd /var/www/html**
* download WordPress as zip file **sudo wget https://wordpress.org/latest.zip**
* extract package **sudo unzip latest.zip**

## Create Database and User
* Log in to MySQL root **sudo mysql -u root**
* create new user for WordPress **create user 'wordpress'@'localhost' identified by 'Ruthie_Brewer26!;**
* create new database **create database wordpress;**
* grant all privileges to new user: grant all privileges on wordpress.* to 'wordpress'@'localhost';
* examine output **show databases** and exit **\q**

## Set up wp-config.php
* Change to wordpress directory **cd wordpress** or **cd /var/www/html/wordpress**
* copy and rename file **sudo cp wp-config-sample.php wp-config.php**
* edit file and add database name, user name, and password **sudo nano wp-config.php**
* in **DB_NAME** add **wordpress**
* **DB_USER** add **wordpress**
* **DB_PASSWORD** add **LibScience26!**
* change site name **sudo mv /var/www/html/wordpress /var/www/html/thefakelibrary**
* visit WordPress site **http://34.68.229.119/wp-admin/install.php**

## Install WordPress
* Create site name **The Fake Library**
* username: **mollss12**
* password: **Ruthie_Brewer26!**
* email: **molleeakers12@gmail.com**

## Process Reflection
* installing this was a very interesting process. I had the same issue as last week except way worse. 
* last week's issue was solved in the Directory file of the apache.conf file
* my authorizations were not correct and it was throwing errors
* this week, i finished the whole process in the lecture notes and a Not Authorized error was showing
* it was difficult to find the error because it was located in the file from last week
* i think the issue lied in the Directory area.
* there was no directory for /var/www/html or /var/www/html/thefakelibrary which was the name i changed the wordpress file to
* i had to look up so many commands to run to find the issue.
* i ran ones to find the permissions, who was authorized. 
* i ran the ls -l command to see if the file was located in the right place
* i used it also to see if the index.php and index.html files were correct which they were
* ultimately, i had to insert new directory permissions for both /var/www/html and /var/www/html/thefakelibrary
* the last i think solved the issue
* this took about 2 hours to resolve and thefakelibrary isn't even the root of the web address

## Designing Page
