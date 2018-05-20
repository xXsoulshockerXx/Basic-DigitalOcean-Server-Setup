## Create Directories and Set Permissions
Once Apache2 has been configured, now we need to setup the server root directory so you can upload/create/manage web files and view them from the web.

If we use the **cat /etc/group** command, we can see that apache2 created a group for use to add our users in to modify data. So our documents are still owned by root. We need to make sure that we have access.
Use the following command to add our user account to that group. Remember to replace **[your username]** with the username that you have created.

```sudo adduser [your username] www-data```

Now we need to create two folders. One is the **servername.yourdomain.com** and the other is **yourdomain.com**. Please replace accordingly.

```
sudo mkdir -p /var/www/servername.yourdomain.com/
sudo mkdir -p /var/www/yourdomain.com/
```

What we are doing is creating a server folder for our servername (which is a domain name) and an actual folder for our domain name.

### Now to Add Permissions to Our Domain Folders

```
sudo chown root:www-data -R /var/www/servername.yourdomain.com/
sudo chown root:www-data -R /var/www/yourdomain.com/

sudo chmod -R 775 /var/www/servername.yourdomain.com/
sudo chmod -R 775 /var/www/yourdomain.com/

sudo setfacl -d -R -m u:root:rwx,g:www-data:rwx,o::rwx /var/www/html
sudo setfacl -d -R -m u:root:rwx,g:www-data:rw,o::r /var/www/html
```

### Create index.html files

We need to create two files and save them in the proper directory. Remember that **Control+O** is to save and **Control+X** is to exit the text editor. Replace accordingly.

```
sudo nano /var/www/servername.yourdomain.com/index.html
sudo nano /var/www/yourdomain.com/index.html
```

Replace **[domain name]** with the proper address.

Use this Index Template that I have created [Index Template](../templates/index_template.html)

### Create Virtual Hosts for Our Domains

Now to copy the default config file to a new config file for our domain setup. We will need to do this for every domain we set up with directories. Replace the names accordingly.

```
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/[replace_with_domain].conf
sudo nano /etc/apache2/sites-available/[domain].conf
```

Should look something like this:

<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

Make sure to replace the ServerAdmin with your real email address. Also replace DocumentRoot with the correct site directory.

Add **ServerName [domain name]** below **ServerAdmin**. Also add **ServerAlias www.[domain]** under **DocumentRoot** so that the site is also browseable when someone types in www. in front of the domain.

Scroll down, and just before **</VirtualHost>** add this:

```
<Directory /var/www/html/[domain]/public_html>
	Options -Indexes +FollowSymLinks +MultiViews
	AllowOverride All
	Require all granted
</Directory>
```

Yours should look something similar to this:

```
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        ServerName example.com

        ServerAdmin webmaster@example.com
        DocumentRoot /var/www/html/examplecom
        ServerAlias www.example.com

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf

        <Directory /var/www/html/examplecom>
                Options -Indexes +FollowSymLinks +MultiViews
                AllowOverride All
                Require all granted
        </Directory>
</VirtualHost>
```

Let's enable the config files now. Make sure to replace the **[domain]** with the proper name.

``` sudo a2ensite [domain].conf ```

The default config file is still enabled. In order for this to properly work, we need to disable that config file.

``` sudo a2dissite 000-deafult.conf ```

Now we need to use **sudo service apache2 reload** to activate the new configuration. Then use **sudo service apache2 restart*** to restart all apache2 services.

Go to [PHP Section](5-php.md)
