# How to setup a PHP development enviroment in Windows 11 WSL2 Ubuntu20.04

1. Install Apache webserver

```sudo apt install apache2```

2. Install PHP
-  ```sudo add-apt-repository ppa:ondrej/php```
-  ```sudo apt-get update```
-  ```sudo apt-get install php8.0 ```
-  ```sudo service apache2 restart```
-  ```sudo apt install php8.0-common php8.0-mysql php8.0-xml php8.0-xmlrpc php8.0-curl php8.0-gd php8.0-imagick php8.0-cli php8.0-dev php8.0-imap php8.0-mbstring php8.0-opcache php8.0-soap php8.0-zip php8.0-intl -y```
 ```sudo service apache2 restart```

 Now you can check your php version
-  ```php -v```

3. In Windows create a folder on your prefered drive and link it to your WSL distribution.
As an example I will use the c drive.

In your Ubuntu Terminal you can list your drives with the command: ```ls /mnt```

You will see then something like this: ```c wsl```

To link your folder you have to type ```ln -s /mnt/c/MyFolder MyFolder```

If you type: ```ll``` you will see something like this (user represents your username):

```
drwxr-xr-x  5 user user  4096 Jan  4 19:55 .vscode-server/
-rw-r--r--  1 user user   183 Jan  4 19:55 .wget-hsts
lrwxrwxrwx  1 user user    15 Jan 20 15:14 MyFolder -> /mnt/c/MyFolder/
-rw-r--r--  1 user user    63 Jan 20 16:53 composer.json
-rw-r--r--  1 user user 21696 Jan 20 16:53 composer.lock
```
You can see that your folder is now linked to the c drive

4. To enable Apache to serve from this folder first you have to enable userdir for Apache
- ``` sudo a2enmod userdir```
5. Restart Apache
- ``` sudo service apache2 restart```
6. Now you have to edit the userdir.conf file, so that apache finds the folder you just created
- ``` sudo vi /etc/apache2/mods-available/userdir.conf```

It will look like this:


```
<IfModule mod_userdir.c>
       UserDir public_html
       UserDir disable root
            <Directory /home/*/public_html>
                AllowOverride FileInfo AuthConfig Limit Indexes
                Options MultiViews Indexes SymLinksOwnerMatch IncludesNoExec
                Require method GET POST OPTIONS
            </Directory>
</IfModule> 
```

You have to edit it like this:


```
<IfModule mod_userdir.c>
       UserDir MyFolder
       UserDir disable root
            <Directory /home/*/MyFolder>
                AllowOverride All
                Options MultiViews Indexes SymLinksOwnerMatch IncludesNoExec
                Require all granted
            </Directory>
</IfModule>
```

In your browser you will find your folder now (create a index.php and call the phpinfo(); method) under:

``` localhost/~user/(user represents your username) ```

Happy coding 😃!




# WSL-PHP-Devenviroment
