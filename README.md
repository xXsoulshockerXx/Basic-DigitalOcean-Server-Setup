# Basic Digital Ocean Server Setup
A setup guide and files for common server setup that follows my videos.

## Create a Digital Ocean Droplet
These droplets are the Virtual Private Servers that are setup within minutes ready to go.

Go to [Creating a Digital Ocean Droplet Section](tutorials/1-createdroplet.md)

## Install Free Basic Tools
You will need these applications to continue. Here are the names and links.

1. FileZilla
    - A file transfer tool for FTP Access.
    - https://filezilla-project.org/
2. Putty and PuttyGen
    - A SSH Terminal tool and a session key generator.
    - https://the.earth.li/~sgtatham/putty/latest/w64/putty-64bit-0.70-installer.msi
    - https://the.earth.li/~sgtatham/putty/latest/w64/puttygen.exe
3. NotePad++
    - This is an advanced notepad that can handle all types of code. A big improvement over the standard Windows Notepad.
    - https://notepad-plus-plus.org/download/v7.5.6.html

## Setup Apache2 for the Server
After you login we need to setup the server up so it's browseable on the web.

We need to access the hosts files and setup somethings so the server doesn't go crazy because it can't see itself. It's weird, I know. However, tedious it might seem, it's beneficial in the long run.

Use this command to access the hosts file:

```sudo nano /etc/hostname```

Remember to modify the code to fit your server name. It will not work if you use the generic example server name as follows.

At the bottom of the document make sure you add the following:

```
127.0.1.1 servername.domainaddress.com servername
127.0.0.1 servername.domainaddress.com servername
```

So for me that will be:

```
127.0.1.1 HBF-SERVER.example.com HBF-SERVER
127.0.0.1 HBF-SERVER.example.com HBF-SERVER
```

If you run into errors later on, you may need to change the second server name section to the full address like so:

```
127.0.1.1 HBF-SERVER.example.com HBF-SERVER.example.com
127.0.0.1 HBF-SERVER.example.com HBF-SERVER.example.com
```

###  Add this to prevent IP Spoofing
It gets rather annoying when your website goes down and you can't figure out why. Well one step to prevent your website being taken down is to prevent IP Spoofing.

Add this following text to the bottom of the hosts file:

```
order bind,hosts
nospoof on
```

**Control+O** to write out (save the document)

**Control+X** to exit the document.

Server restart will be required.

Use this to restart the server:

``` sudo reboot ```

## Now we need to test apache2 to see if everything is working correctly.

``` sudo apache2ctl configtest ```

It will prompt a message that it can't detect the FQDN. That is ok. We will fix that in the server files.

``` sudo nano /etc/apache2/apache2.conf ```

Find the Directory Section, we need to make some modifications to the /var/www section:

```
<Directory /var/www/>
        Options Indexes FollowSymLinks Includes ExecCGI
        AllowOverride All
        Require all granted
</Directory>
```

Add **ServerName** and set it to **servername.yourdomain.com** to the bottom of the script. Change as needed.

``` ServerName HBF-SERVER.example.com ```

**Control+O** to write out (save the document)

**Control+X** to exit the document.

``` sudo a2enmod rewrite ```

Use **sudo apache2ctl configtest** to check the syntax again and make sure everything is working correctly.
Use **sudo systemctl restart apache2** to restart apache2.

### Index Files
A generic index file template to test to see if the server is working.

### Script Files
Scripts that are used in the tutorial videos. These are for logwatch, tripware, and more.
