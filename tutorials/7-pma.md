## Install and Secure PHPmyAdmin
This is a web based client that allows us to make modifications to the MySQL database easily.

PHPMyAdmin is a web program that allows us to modify our databases in MySQL. Since we have apache we can easily install it with console commands. Ubuntu's default repositories already contain the needed files so we can just use the following commands:

```
sudo apt-get update
sudo apt-get install phpmyadmin php-mbstring php-gettext
```

This will ask you a few questions in order to configure your installation correctly.

Warning: When the first prompt appears, apache2 is highlighted, but not selected. If you do not hit Space to select Apache, the installer will not move the necessary files during installation. Hit Space, Tab, and then Enter to select Apache.

- For the server selection, choose apache2.
- Select yes when asked whether to use dbconfig-common to set up the database
- You will be prompted for your database administrator's password
- You will then be asked to choose and confirm a password for the phpMyAdmin application itself

The installation process actually adds the phpMyAdmin Apache configuration file into the **/etc/apache2/conf-enabled/** directory, where it is automatically read.

The only thing we need to do is explicitly enable the PHP mcrypt and mbstring extensions, which we can do by typing:

```
sudo phpenmod mcrypt
sudo phpenmod mbstring
```

Afterwards, you'll need to restart Apache for your changes to be recognized:

``` sudo service apache2 restart ```

You can now access the web interface by visiting your server's domain name or public IP address followed by /phpmyadmin:

**http://domain.com/phpmyadmin or ###.###.###.###/phpmyadmin**

## Secure your phpMyAdmin Instance

We were able to get our phpMyAdmin interface up and running fairly easily. However, we are not done yet. Because of its ubiquity, phpMyAdmin is a popular target for attackers. We should take extra steps to prevent unauthorized access.

One of the easiest way of doing this is to place a gateway in front of the entire application. We can do this using Apache's built-in .htaccess authentication and authorization functionalities.

First, we need to enable the use of .htaccess file overrides by editing our Apache configuration file.

We will edit the linked file that has been placed in our Apache configuration directory:

``` sudo nano /etc/apache2/conf-available/phpmyadmin.conf ```

We need to add an **AllowOverride All** directive within the **<Directory /usr/share/phpmyadmin>** section of the configuration file, like this:

```
<Directory /usr/share/phpmyadmin>
    Options FollowSymLinks
    DirectoryIndex index.php
    AllowOverride All
    . . .
```

When you have added this line, save and close the file. Use **Control+O** to save and **Control+X** to exit.

To implement the changes you made, restart Apache:

``` sudo service apache2 restart ```

Now that we have enabled .htaccess use for our application, we need to create one to actually implement some security.

In order for this to be successful, the file must be created within the application directory. We can create the necessary file and open it in our text editor with root privileges by typing:

``` sudo nano /usr/share/phpmyadmin/.htaccess ```

Within this file, we need to enter the following information:

```
AuthType Basic
AuthName "Restricted Files"
AuthUserFile /etc/phpmyadmin/.htpasswd
Require valid-user
```

Let's go over what each of these lines mean:

- AuthType Basic: This line specifies the authentication type that we are implementing. This type will implement password authentication using a password file.
- AuthName: This sets the message for the authentication dialog box. You should keep this generic so that unauthorized users won't gain any information about what is being protected.
- AuthUserFile: This sets the location of the password file that will be used for authentication. This should be outside of the directories that are being served. We will create this file shortly.
- Require valid-user: This specifies that only authenticated users should be given access to this resource. This is what actually stops unauthorized users from entering.

When you are finished, save and close the file. Use **Control+O** to save and **Control+X** to exit.

Now that we have specified a location for our password file through the use of the AuthUserFile directive within our .htaccess file, we need to create this file.

We actually need an additional package to complete this process. We can install it from our default repositories:

``` sudo apt-get install apache2-utils ```

Afterward, we will have the htpasswd utility available.

The location that we selected for the password file was "/etc/phpmyadmin/.htpasswd". Let's create this file and pass it an initial user by typing:

``` sudo htpasswd -c /etc/phpmyadmin/.htpasswd [username] ```

Replace [username] with the user you are creating it for.

You will be prompted to select and confirm a password for the user you are creating. Afterwards, the file is created with the hashed password that you entered.

If you want to enter an additional user, you need to do so without the -c flag, like this:

``` sudo htpasswd /etc/phpmyadmin/.htpasswd [additionaluser] ```

Now, when you access your PHPMyAdmin subdirectory, you will be prompted for the additional account name and password that you just configured.

Go to [Install and Setup a Mail Transfer Agent Section](8-mta.md)
