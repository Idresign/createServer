# configuration and deploying the app in the server

## creating a linux user

on your terminal type
  ```bash
  $> sudo adduser grader
  $> sudo adduser grader sudo
  $> su - grader
  ```
  
  the passowrd is grader
## Update all currently installed packages
  
  ```bash
  $> sudo apt-get update
  $> sudo apt-get upgrade
  $> sudo apt-get dist-upgrade
  $> sudo unattended-upgrade
  $> sudo apt-get install unattended-upgrades
  ```
## secure ssh connection

```bash
  $> sudo vi /etc/ssh/sshd_config
  ```
  
  change this line
  
  ```vim
    # What ports, IPs and protocols we listen for
    Port 22
  ```
  
  to
  
  ```vim
    # What ports, IPs and protocols we listen for
    Port 2200
  ```
  
  change the value of PermitRootLogin to no
  
  ```vim
    # Authentication:
    LoginGraceTime 120
    PermitRootLogin no
    StrictModes yes
  ```

## Configure the Universal Firewall to only allow incoming connections

  ```bash
    $> sudo ufw allow 2200/tcp
    $> sudo ufw allow 80/tcp
    $> sudo ufw allow 123/tcp
    $> sudo ufw enable
    $> sudo service ssh restart
  ```
  
##Install and configure Apache to serve a Python mod_wsgi application

  ```bash
    $> sudo apt-get install apache2
    $> sudo apt-get install libapache2-mod-wsgi python-dev
    $> sudo a2enmod wsgi
  ```

#Install and configure PostgreSQL:
  ```bash
    $> sudo apt-get install postgresql postgresql-contrib
    $> sudo -i -u postgres
    $> createuser catalog --interactive
  ```
  
  do not allow the user any priviledges
  
  ```bash
    $> vi /etc/postgresql/9.3/main/pg_hba.conf
  ```
  
  and remove any remote connection from the file
  
## Install git, clone and setup your Catalog App project 

  ```bash
    $> cd /var/www 
    $> sudo mkdir FlaskApp
    $> cd FlaskApp/
    $> sudo apt-get install git 
    $> sudo git clone https://github.com/Idresign/Catalog-app.git 
    $> mv sudo mv Catalog-app FlaskApp
    $> cd FlaskApp/
    $> sudo mv project.py __init__.py
    $> sudo nano __init__.py 
  ``` 

remove
  ```python
    debug = true
  ```
  
  ```bash
    $> sudo apt-get install python-pip 
    $> sudo pip install virtualenv 
    $> sudo virtualenv venv
    $> source venv/bin/activate
    $> sudo pip install Flask 
    $> sudo pip install sqlalchemy
    $> sudo pip install requests
    $> sudo pip install oauth2client
  ```
    
follow the instruction in the readme of the git to get a client_secrets.json file for the project.

Check if everything is fine.

```bash
  $> sudo python __init__.py
  ``` 

```bash
  $> sudo deactivate
  ```

```bash
  $> sudo nano /etc/apache2/sites-available/FlaskApp.conf
  ```
Add the following

```
<VirtualHost *:80>
                ServerName 54.191.223.118 
                ServerAdmin admin@54.191.223.118
                WSGIScriptAlias / /var/www/FlaskApp/flaskapp.wsgi
                <Directory /var/www/FlaskApp/FlaskApp/>
                        Order allow,deny
                        Allow from all
                </Directory>
                Alias /static /var/www/FlaskApp/FlaskApp/static
                <Directory /var/www/FlaskApp/FlaskApp/static/>
                        Order allow,deny
                        Allow from all
                </Directory>
                ErrorLog ${APACHE_LOG_DIR}/error.log
                LogLevel warn
                CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

```bash
  $> sudo a2ensite FlaskApp
  $> cd ..
  $> sudo nano flaskapp.wsgi
  ```

Add the following

```bash
sudo nano 
#!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/FlaskApp/")

from FlaskApp import app as application
application.secret_key = 'Add your secret key'
```
```bash
  $> sudo service apache2 restart
  ```

###this is the ip of the project 54.191.223.118
