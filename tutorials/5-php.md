# PHP Config Setup
Now that we have Apache2 configured, we need to setup PHP.

``` sudo apt-get install php php-curl php-gd php-mbstring php-mcrypt php-xml php-xmlrpc php-cli php-mysql libapache2-mod-php ```

This should install PHP without any problems. We'll test this in a moment.

In most cases, we'll want to modify the way that Apache serves files when a directory is requested. Currently, if a user requests a directory from the server, Apache will first look for a file called index.html. We want to tell our web server to prefer PHP files, so we'll make Apache look for an index.php file first.

To do this, type this command to open the dir.conf file in a text editor with root privileges:

``` sudo nano /etc/apache2/mods-enabled/dir.conf ```

It will look something like this:

```
<IfModule mod_dir.c>
    DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
</IfModule>
```

We want to make it look like this:

```
<IfModule mod_dir.c>
    DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```

Go ahead and make the change, then use **Control+O** to save and **Control+X** to exit.

Now we need to restart apache2 to apply the changes we just made.

``` sudo service apache2 restart ```

Now in one of our directories we need to create a file that will check to see if our PHP system is working correctly.

``` sudo nano /var/www/html/[domain]/info.php ```

Once opened type in the following, save and exit the editor.

```
<?php
phpinfo();
?>
```

Afterwards, open up your web browser and navigate to **domain.com/info.php** or if you do not have a domain setup then you need to use the IP address: **###.###.###.###/info.php**. If you are greeted with a screen that has information about the PHP server, then everything is good to go.

Go to [MySQL Section](6-mysql.md)
