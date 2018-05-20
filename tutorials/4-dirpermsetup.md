## Create Directories and Set Permissions
Once Apache2 has been configured, now we need to setup the server root directory so you can upload/create/manage web files and view them from the web.

If we use the **cat /etc/group** command, we can see that apache2 created a group for use to add our users in to modify data. So our documents are still owned by root. We need to make sure that we have access.
Use the following command to add our user account to that group. Remember to replace **[your username]** with the username that you have created.

```sudo adduser [your username] www-data```

Now we need to create two folders. One is the **servername.yourdomain.com** and the other is **yourdomain.com**. Please replace accordingly.

```
sudo mkdir -p /var/www/servername.yourdomain.com/public_html
sudo mkdir -p /var/www/yourdomain.com/public_html
```
