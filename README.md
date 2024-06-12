# Raspberry-Pi-Web-Server

[More Details](https://www.tomshardware.com/news/raspberry-pi-web-server,40174.html)

1. Update your packages by typing

```
sudo apt update
```

2. Install apache2 :
```
sudo apt install apache2 -y 
```

3. Install php :
```
sudo apt install php libapache2-mod-php -y 
```

4. Install mariadb so that mysql database can be used with the website :
```
sudo apt install mariadb-server
```

5. Do formal install and configuration 
```
sudo mysql_secure_installation
```

6. Install the php-mysql connector so php pages can access the DB.
```
sudo apt install php-mysql
```

7. Restart apache2 so all of the changes are running.
```
sudo service apache2 restart
```

8. Access the default apache2 page with the following link
```
http://raspberrypi.local
```

9. The page can be also viewed with local IP. If you do not know the IP, you can enter the `hostname` command into the terminal to retrieve it.
```
hostname -I
```

## Serve a Custom Folder on Apache

10. Allow Execute Permission
```
sudo adduser www-data username
```

11. replace default location with desire location in config file
```
vim /etc/apache2/sites-available/000-default.conf
```
> Replace _`/var/www/html `_ with the path of your choice.

_**or**_

11.1. Configure Apache with different config file
```
sudo nano /etc/apache2/sites-available/example-site.conf
```
11.2 In the configuration file, add the following:
```
<VirtualHost *:80>
      DocumentRoot /var/www/new-folder/
      ErrorLog ${APACHE_LOG_DIR}/new-folder_error.log
      CustomLog ${APACHE_LOG_DIR}/new-folder_access.log combined
</VirtualHost>
```

11.3: Enable Your Site. 
> Disable the default site configuration with:
```
sudo a2dissite 000-default.conf
```
> Enable your new site configuration with:
```
sudo a2ensite example-site.conf
```

11.4 restart the server:
```
service apache2 restart
```

## [Apache Virtual Host](https://pimylifeup.com/raspberry-pi-apache/)
12. 
```
<VirtualHost *:80>
      ServerName example.com
      ServerAlias www.example.com
      DocumentRoot /var/www/example.com/public_html
      ErrorLog ${APACHE_LOG_DIR}/example.com_error.log
      CustomLog ${APACHE_LOG_DIR}/example.com_access.log combined
</VirtualHost>
```
13. watch the log file of the server
```
watch -n 1 tail /var/log/apache2/access.log
```
## Listen on Different [Ports](https://www.baeldung.com/linux/apache-web-server-two-ports)
14. The first step is to update the port configuration file 
```
sudo nano /etc/apache2/ports.conf
```
14.1 Modify Port Config File. Add the post number(example: 8888).
```
... 

Listen 80 
Listen 8888 

<IfModule ssl_module> 
...
```

14.2 Update Virtual Host File `/etc/apache2/sites-enabled/000-default.conf`
> before
```
<VirtualHost *:80>
    ...
</VirtualHost>
```
> after
```
<VirtualHost *:80 *:8888> 
    ... 
</VirtualHost> 
```

14.3 for different `port` serving different `folder`
> Update Virtual Host File `/etc/apache2/sites-enabled/000-default.conf` with new `VirtualHost` tag
```
<VirtualHost *:8888>
      DocumentRoot /var/www/new-port-8888-folder
      ErrorLog ${APACHE_LOG_DIR}/port-8888_error.log
      CustomLog ${APACHE_LOG_DIR}/port-8888_access.log combined
</VirtualHost>

<VirtualHost *:80> 
    ... 
</VirtualHost> 
```

14.4 restart the server:
```
service apache2 restart
```
