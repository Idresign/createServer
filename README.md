# createServer

sudo adduser <username>

sudo adduser <username> sudo

su - <username>

Update all currently installed packages

sudo apt-get update        # Fetches the list of available updates

sudo apt-get upgrade       # Strictly upgrades the current packages

sudo apt-get dist-upgrade  # Installs updates (new ones)

Change the SSH port from 22 to 2200

sudo vi /etc/ssh/sshd_config

change the port to the following

# What ports, IPs and protocols we listen for

Port 2200

Configure the Universal Firewall to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123)

sudo ufw allow 2200/tcp

sudo ufw allow 80/tcp

sudo ufw allow 123/tcp

sudo ufw enable

sudo service ssh restart

Install and configure Apache to serve a Python mod_wsgi application

sudo apt-get install apache2

Open terminal and type the following command to install mod_wsgi:

sudo apt-get install libapache2-mod-wsgi python-dev

To enable mod_wsgi, run the following command:

sudo a2enmod wsgi 

cd /var/www 

sudo mkdir FlaskApp

cd FlaskApp/

sudo apt-get install git

sudo git clone https://github.com/Idresign/Catalog-app.git

mv sudo mv Catalog-app FlaskApp

cd FlaskApp/

sudo mv project.py __init__.py

sudo nano __init__.py 

remove debug = true

sudo apt-get install python-pip 

sudo pip install virtualenv 

sudo virtualenv venv

source venv/bin/activate

sudo pip install Flask 

sudo pip install sqlalchemy
  $> sudo pip install requests
  $> sudo pip install oauth2client

follow the instruction in the readme of the git to get a client_secrets.json file for the project.

sudo python __init__.py  to check if everything is fine, or you might be missing something.Please check on the catalog github read me of my app

deactivate

sudo nano /etc/apache2/sites-available/FlaskApp.conf

<VirtualHost *:80>
                ServerName YOUR_IP 
                ServerAdmin admin@YOUR_IP
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


sudo a2ensite FlaskApp

cd ..

sudo nano flaskapp.wsgi

sudo nano 
#!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/FlaskApp/")

from FlaskApp import app as application
application.secret_key = 'Add your secret key'


sudo service apache2 restart 



http://52.11.88.226/
