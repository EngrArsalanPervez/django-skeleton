# django-skeleton

### Features

1. Complete File structure except media
2. Project-level Static files (CSS, js, img)
3. App-level Static files (CSS, js, img)


### Demo

![alt text](https://github.com/EngrArsalanPervez/django-skeleton/blob/main/static/img/screen.jpg?raw=true)

### HowTo Run with Python
```python
mkdir mysite
cd mysite
git clone https://github.com/EngrArsalanPervez/django-skeleton.git
python3 -m venv venv
source venv/bin/activate/
pip install -r requirements.txt
python3 manage.py runserver
```


### HowTo Deploy with Apache2

##### Install LAMP stack
```bash
# Apache2
sudo apt update
sudo apt install apache2 libapache2-mod-wsgi-py3
sudo a2enmod wsgi
sudo ufw app list
sudo ufw allow in "Apache"
sudo ufw status
http://localhost

# MySQL
sudo apt install mysql-server
sudo mysql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
exit
sudo mysql_secure_installation
sudo mysql -uroot -p
exit

# PHP
sudo apt install php libapache2-mod-php php-mysql
php -v

# Phpmyadmin
sudo apt install phpmyadmin php-mbstring php-zip php-gd php-json php-curl
(abort)
mysql -u root -p
UNINSTALL COMPONENT "file://component_validate_password";
exit
sudo apt install phpmyadmin
mysql -u root -p
INSTALL COMPONENT "file://component_validate_password";
exit
sudo phpenmod mbstring
sudo systemctl restart apache2
https://localhost/phpmyadmin
```

##### Upload Web content
```python
sudo mkdir /var/www/mysite.com
sudo chown -R $USER:$USER /var/www/mysite.com

cd mysite.com
git clone https://github.com/EngrArsalanPervez/django-skeleton.git

python3 -m venv venv
source venv/bin/activate/

pip install -r requirements.txt

python3 manage.py collectstatic
python3 manage.py createsuperuser
python3 manage.py runserver

(Stop site and deactivate venv)
```

##### Create a new virtual host
```bash
sudo nano /etc/apache2/sites-available/mysite.com.conf
    <VirtualHost *:80>
        ServerName mysite.com
        ServerAlias www.mysite.com 
        ServerAdmin webmaster@mysite.com
        DocumentRoot /var/www/mysite.com
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        Alias /static /var/www/mysite.com/assets
        <Directory /var/www/mysite.com/assets>
                Require all granted
        </Directory>

        Alias /media /var/www/mysite.com/media
        <Directory /var/www/mysite.com/media>
            Require all granted
        </Directory>

        <Directory /var/www/mysite.com/mysite>
            <Files wsgi.py>
                Require all granted
            </Files>
        </Directory>

        WSGIDaemonProcess mysite python-path=/var/www/mysite.com python-home=/var/www/mysite.com/venv
        WSGIProcessGroup mysite
        WSGIScriptAlias / /var/www/mysite.com/mysite/wsgi.py
    </VirtualHost>

sudo a2ensite mysite.com
sudo a2dissite 000-default

sudo nano /etc/apache2/apache2.conf
    # Add at the end of file
    ServerName 127.0.0.1

sudo apache2ctl configtest
sudo systemctl reload apache2

nano /etc/hosts
127.0.0.1 mysite.com
127.0.0.1 www.mysite.com

http://mysite.com
http://mysite.com/static
http://mysite.com/media
http://mysite.com/admin
```