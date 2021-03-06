#Roll-Your-Own Local Drupal Development Environment On Ubuntu 15.10

![alt text](http://drupal.org/files/images/DrupalDiver.png "Florida Drupal Users Group")
**[Florida Drupal Users Group](https://groups.drupal.org/florida)**
**IRC**: Freenode.org **#drupal-florida**

Complete Ubuntu 15.10 local development environment setup guide for Drupal 8. Includes LAMP, git, Composer, Drush, and RVM. Also, a few optional applications are included. (Sublime Text 3, PhpStorm, Node.js, Gulp.js and HexChat) 

**LOCAL set up only!**  Ubuntu 15.10 / LAMP / Drupal Sites Setup

1. Ubuntu 15.10 Install
2. Lamp Stack Installation
3. Server Applications Setup
4. Apache Configuration
5. vhost Setup and Configuration
6. Hosts File Configuration
7. Database Creation And Drupal Installation
8. IDE Installation
9. Optional Applications

---

#1. Ubuntu 15.10 Install
1. Create a bootable install disk or usb drive, follow directions and install Ubuntu  
2. It is very important to write down or remember your username and password. You will use these on a regular basis  
3. Open a terminal, click on the top left icon and type: `terminal`
4. Run updates (don’t bother with the software updater). This takes a while:

	```bash
$ sudo apt-get update
```

5. If you want to use a text editor, it is called gedit. Open it the same way as the terminal. Once it is in the sidebar launcher, you can left click and lock.  
6. You can also remove most of the other annoying icons from the launcher so they are not in your way.  

####Give Your User Permissions!

1. Add your username to the sudo group:

    ```bash
$ sudo adduser yourusername sudo
```

2. Add your username to the www-data group:

    ```bash
$ sudo adduser yourusername www-data
```

####Permission Issues

If you are having permission issues, you can also do the following. I do not believe this method is recommended.  

1. Edit the sudoers file with the "visudo" text editor:
**(This is much safer than editing the `/etc/sudoers` file manually).**

    ```bash
$ sudo visudo
```

2. Using the arrow keys, or the 'j' key,  scroll down until you see # User privilege specification
3. Place cursor under: `root ALL=(ALL:ALL) ALL`
4. Add: `yourusername ALL=(ALL:ALL) ALL`
5. Press **Esc**
6. Type: `:wq`

___
#2. Lamp Stack Installation
* **[How to install Lamp server on ubuntu 15.10 by Krizna.com](http://www.krizna.com/ubuntu/install-lamp-server-ubuntu-15-10/)**

1. Install Apache:

    ```bash
$ sudo apt-get install apache2
``` 

2. Open Apache main configuration file:

    ```bash
$ sudo nano /etc/apache2/apache2.conf
```

3. Use the arrow key to scroll down to the end of the file and type in: 
`ServerName localhost`  
4. Press **CTRL**+**o** (to save)  
5. Press **Enter**   
6. Press **CTRL**+**x** (to exit)  
7. You can check to make sure that it saved with

    ```bash
$ cat /etc/apache2/apache2.conf
```

8. Restart apache:

    ```bash
$ sudo service apache2 restart
```

9. Change the ownership of /var/www/html to your username

    ```bash
$ sudo chown yourusername:www-data /var/www/html -R
```

10. You can check to make sure it works properly by opening the browser and typing: */var/www/html*

---

###Install MySQL server
1. Install MySQL:

    ```bash
$ sudo apt-get install mysql-server
```

2. It will ask you to create a password. Generally for localhost it is root/root.   
3. Check the service status with:

    ```bash
$ sudo /etc/init.d/mysql status
```

4. If your prompt is `mysql>` after checking the status, you can escape by typing: `exit`

---

###Install PHP
1. Install PHP: 

    ```bash
$ sudo apt-get install php5 php5-mysql
```  

2. Create a PHP file. 

    ```bash
$ sudo nano /var/www/html/phpinfo.php
```

3. Add the following code:  

    ```PHP
    <?php  
    
    phpinfo();
    
    ?>
    ```

4. Press **CTRL**+**o** (to save)  
5. Press **Enter**  
6. Press **CTRL**+**x** (to exit)  
7. Restart Apache:  

    ```bash
$ sudo service apache2 restart
```

8. Open browser and navigate to: *localhost/phpinfo.php*  

---

###Install phpMyAdmin
* **[How To Install and Secure phpMyAdmin on Ubuntu 15.10 by Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-phpmyadmin-on-ubuntu-15-10)**

1. Install phpMyAdmin:

    ```Bash
$ sudo apt-get install phpmyadmin
```

2. **Important!** Press the **spacebar** to choose Apache 2. An ***** will display.   
3. Press **Tab** (to navigate down the menu to <ok>)   
4. Press **Enter**  
5. Select *Yes* to configure with dbconfig-common  
6. Follow the rest with your password, and again *root* is a common password used for a local host
7. Activate the mcrpyt module:

    ```Bash
$ sudo php5enmod mcrypt
```

8. Restart Apache:

    ```Bash
$ sudo service apache2 restart
```

9. Test by navigating in the browser to: *localhost/phpmyadmin*  
10. You can follow the rest of the tutorial for additional securities if you want. I am not sure if this is necessary for local. Of course it would be for a cloud server  
11. Don’t *ever* do this on a live server but local is fine. Drupal php needs access:

    ```Bash
$ chmod 775 /var/www/html
```

####Increase Max Limit In php.ini
1. Open the PHP configuration file:
    
    ```Bash
$ sudo nano /etc/php5/apache2/php.ini
```

2. Press **CTRL**+**w** (to search) and type `memory_limit`
3. Change to: `memory_limit = 512M`
4. Press **CTRL**+**o** (to save)  
5. Press **Enter**   
6. Press **CTRL**+**x** (to exit)  

---

###Create SSH key
**(you can replace keys with your old ones after initial set up)**

1. Change to root directory:

    ```Bash
$ cd ~
```

2. Creat .ssh directory:
    
    ```Bash
$ mkdir .ssh
```

3. Protect the directory against any access from other users, while you still has full access:

    ```Bash
$ chmod 700 .ssh
```

4. Create an RSA Key Pair:

    ```Bash
$ ssh-keygen -t rsa
```

5. Press **Enter** three times (type a passphrase if you wish)  

Note: You will see the following messages to which you can just press enter:
```sh
Enter file in which to save the key (/Users/you/.ssh/id_rsa): [Press enter]
```
```sh
Enter passphrase (empty for no passphrase): [Type a passphrase]
```
```sh
Enter same passphrase again: [Type passphrase again]
```


---

#3. Server Applications Setup 
###Install git

1. Change to root directory:

    ```Bash
$ cd ~
``` 

2. Install git:
    
    ```Bash
$ sudo apt-get install git
```

3. Set your name for git identity:
    
    ```Bash
$ git config --global user.name yourname
```
**NOTE: Yourname may be entered as “firstname lastname”, use apostrophes around yourname if using a space between first and last names.  Example:  `$ git config –global user.name “Jane Smith” `**

4. Set your email for git identity:

    ```Bash
$ git config --global user.email youremail@domain.com
```

5. To add color to git use this command:

    ```Bash
$ git config --global color.ui auto
```

6. See all current values:
    ```Bash
$ git config --list
```
---

###Install Composer
**Note! You must install composer before installing drush**

1. Change to root directory:
    
    ```bash
$ cd ~
```

2. Install cURL:
    
    ```bash
$ sudo apt-get install curl
```

3. Download Composer:
    
    ```bash
$ curl -sS https://getcomposer.org/installer | php
```

4. Move composer.phar to /usr/loca/bin composer"
    
    ```bash
$ sudo mv composer.phar /usr/local/bin/composer
```

5. Adds the proper path to the .bashrc file:
    
    ```bash
$ sed -i '1i export PATH="$HOME/.composer/vendor/bin:$PATH"' $HOME/.bashrc
```

6. Reloading/Rrefreshing the .bashrc file:

    ```bash
source $HOME/.bashrc
```

---

###Install Drush (must install composer first) 
**For additional drush commands visit [www.drushcommands.com](http://www.drushcommands.com)**

1. Change to root directory:

    ```bash
$ cd ~
```

2. Install Drush:

    ```bash
$ composer global require drush/drush:dev-master
```

3. Update everything:

    ```bash
$ composer global update
```

---



#4. Apache Configuration
###Configure Apache To Preference .php Files Over .html Files
1. Open /etc/apache2/mods-enabled/dir.conf for editing:

    ```Bash
$ sudo nano /etc/apache2/mods-enabled/dir.conf
```

2. Add a parameter reading "index.php" as the first item after "DirectoryIndex":

    ```PHP
    <IfModule mod_dir.c>
    
              DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
    
    </IfModule>
    ```

3. Press **CTRL**+**o** (to save)  
4. Press **Enter**   
5. Press **CTRL**+**x** (to exit)
6. Restart Apache:
    
    ```Bash
$ sudo service apache2 restart
```

---

###Enable Global Site Clean URL’s
1. Enable the rewrite module for Apache:

    ```Bash
$ sudo a2enmod rewrite
```

2. Restart Apache:
    
    ```Bash
$ sudo service apache2 restart
```

3. Open /etc/apache2/apache2.conf for editing:

    ```Bash
$ sudo nano /etc/apache2/apache2.conf
```

4. Scroll down until you find:

    ```Apache
    <Directory /var/www/>
    
		    Options Indexes FollowSymLinks
    
	    	AllowOverride None
        
	    	Require all granted
    
    </Directory>
    ```

5. Change `AllowOverride None` to `AllowOverride All`:

    ```Apache
    <Directory /var/www/>

	    	Options Indexes FollowSymLinks
    
		    AllowOverride All
    
		    Require all granted
    
    </Directory>
    ```

5. Restart Apache:

    ```Bash
$ sudo service apache2 restart
```

---

#5. vhost Setup and Configuration

1. Change to the /etc/apache2/sites-available directory:

    ```Bash
$ cd /etc/apache2/sites-available
```

2. Create and open a new vhost config file just for drupal:
    
    ```Bash
$ sudo nano drupal
```

3. Add in the following

    ```Apache
    NameVirtualHost *:80
    
    <VirtualHost *:80>
    
       DocumentRoot /var/www
    
       ServerName localhost
    
    </VirtualHost>
    ```

3. Press **CTRL**+**o** (to save)  
4. Press **Enter**   
5. Press **CTRL**+**x** (to exit)

Create Symlinks for the drupal file in the sites-enabled directory

1. Change to the /etc/apache2/sites-enabled directory:
    
    ```Bash
$ cd /etc/apache2/sites-enabled
```

2. Create a symlink to ../sites-available/drupal in /etc/apache2/sites-enabled:

    ```
$ sudo ln -s ../sites-available/drupal .
```

3. Restart Apache
    
    ```Bash
$ sudo service apache2 restart
```

---

###Easy Navigate To Sites Directory By Creating A Symlink To /var/www/html

1. Change to root directory:

    ```bash
$ cd ~
```

2. Change "foo" to your user. (If you are unsure of the name, type `pwd` in the command line):
    
    ```Bash
$ ln -s  /var/www/html /home/foo/sites
```

Now you can easily cd sites and you will be directly in the html folder

**Future note**: You can create and name a symlink wherever you like
* The first path is **what** you are linking
* The second path is **where** the link will be

Example:
    
```Bash
$ ln -s /what/is/being/linked /where/symlink/goes/nameofsymlink
```

To remove a symlink: unlink **sites** (symlink name)

---
#6. Hosts File Configuration
Configure Apache For Sites **(Follow this for either site creation methods.)**

1. Open /etc/hosts file for editing:

    ```Bash
$ sudo nano /etc/hosts
```

2. Add the follow to the last line:
    
    ```
# Drupal sites
127.0.0.1 newsite.dev
```

3. Press **CTRL**+**o** (to save)  
4. Press **Enter**   
5. Press **CTRL**+**x** (to exit)

**Future note**: You will edit this file for every additional site so it will look like this:

```
# Drupal sites
127.0.0.1 newsite.dev

127.0.0.1 newsite2.dev

127.0.0.1 newsite3.dev
```

---

#7. Database Creation And Drupal Installation
###Create Database And Site Via GUI/git
**(Scroll down for cli/Drush alternate.)**
####Create Database
1. Open browser and navigate to *localhost/phpMyAdmin*
2. Create new database called **newsite**

####Git Clone Site
1. Change to your sites directory: (Hopefully you created a symlink. If you didn’t then use `$ cd /var/www/html`):

    ```Bash
$ cd sites
```
2. Clone Drupal 8

    For Drupal 8:
    
    ```Bash
$ git clone --branch 8.0.x http://git.drupal.org/project/drupal.git
```

4. Change the name of the cloned drupal directory to the name of the new site:
    
    ```Bash
$ mv drupal newsite.dev
```

5. Change to newsite.dev/sites/default directory:
    
    ```
$ cd newsite.dev/sites/default
```

6. Copy default.settings.php
    
    ```Bash
$ cp default.settings.php settings.php
```

####Complete Install
1. Open browser and navigate to *localhost/newsite.dev*
2. Complete install, making sure to fill in database name and password

*Rinse and repeat this section for __new__ drupal sites you may create*

---

###Alternate Database And Site Creation Using Command Line And Drush
####Create Database
1. Create a database, Replace database_name with the name of your choice.
    
    ```Bash
$ mysqladmin -u root -p create database_name
```

2. Enter your MySQL root password at the prompt.

---

####Create database user for site via command line
1. Open the mySQL client using root:
    
    ```Bash
$ mysql -u root -p
```

2. Enter your MySQL root password at the prompt.

3. Create a user:

    ```mySQL
mysql> CREATE USER 'name_of_new_user'@'localhost' IDENTIFIED BY 'password_of_new_user';
```

4. Grant *name_of_new_user* privileges to *database_name*:
    
    ```mySQL
mysql> GRANT ALL PRIVILEGES ON 'database_name'.* TO 'name_of_new_user'@'localhost';
```

5. Reload the grant tables:
    
    ```mySQL
mysql> FLUSH PRIVILEGES;
```

6. Exit mySQL
    
    ```mySQL
mysql> quit
```

---

###Create site via drush (with Drupal 8)
1. Change to your sites directory: (Hopefully you created a symlink. If you didn’t then use $ cd /var/www/html):

    ```Bash
$ cd sites
```

2. Download Drupal (drupal-8.0.x):

    ```Bash
$ drush dl drupal --drupal-project-rename=site_directory_name
```

3. Change to the site_directory_name
    
    ```Bash
$ cd site_directory_name
```

4. Drupal Site Install. This will also create your settings.php file:
    
    ```Bash
$ drush si standard --account-name=admin --account-pass=admin --db-url=mysql://database_user_name:database_user_password@localhost/database_name
```

---

#8. IDE Installation

###Install IDE (Sublime Text 3)
Sublime Text is a sophisticated text editor for code, html and prose. 

1. Change to root directory:

    ```bash
$ cd ~
```

2. Add the WebUpd8 Sublime Text 3 (beta) PPA:
    
    ```bash
$ sudo add-apt-repository ppa:webupd8team/sublime-text-3
```

3. Download the package lists from the repositories and "update" them to get information on the newest versions of packages and their dependencies. It will do this for all repositories and PPAs.

    ```bash
$ sudo apt-get update
```

4. Install Sublime Text 3:
    
    ```bash
$ sudo apt-get install sublime-text-installer
```

5. Add in drupal specific preferences [https://drupal.org/node/1346890](https://drupal.org/node/1346890)

---



#9. Optional Applications


###Install IRC (HexChat)
1. Add the HexChat PPA:
    
    ```Bash
$ sudo add-apt-repository ppa:gwendal-lebihan-dev/hexchat-stable
```

2. Download the package lists from the repositories and "update" them to get information on the newest versions of packages and their dependencies. It will do this for all repositories and PPAs
    
    ```Bash
$ sudo apt-get update
```

3. Install HexChat:
    
    ```Bash
$ sudo apt-get install hexchat
```
