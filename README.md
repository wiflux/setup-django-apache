# How to Serve Django Applications with Apache and mod_wsgi on Ubuntu 16.04 LST
Author : <wiflux@gmail.com> Dharminder Singh Negi
### Project home:
   https://wiflux.github.io/setup-django-apache/
   
### Step 1 : Install Apache
```sh
$ sudo apt-get update
$ sudo apt-get install apache2
```
### Step 2 : Install apache2 mod wsgi and pip
##### For Python 2.x
```sh
$ sudo apt-get update
$ sudo apt-get install libapache2-mod-wsgi
$ sudo apt-get install python-pip
```
##### For Python 3.x
```sh
$ sudo apt-get update
$ sudo apt-get install libapache2-mod-wsgi-py3
$ sudo apt-get install python3-pip
```

### Step 3 : Install virtualenv and setup
##### For Python 2.x
```sh
$ sudo pip install virtualenv
$ cd /var/www/html/ 
$ virtualenv -p python myenv //if deafult python version is default 2.x
```
##### For Python 3.x
```sh
$ sudo pip3 install virtualenv
$ cd /var/www/html/ 
$ virtualenv -p python3 myenv 

```
### Step 4 : Activate virtual environment
```sh
$ source venv/bin/activate
```

### Step 5 : Install Django
```sh
(myenv) $ pip install -e django
```
### Step 6 : Check Django Version
```sh
(myenv) $ python -m django --version
```
### Step 7 : Create new Django Application
```sh
(myenv) $ django-admin startproject mysite
```
### Step 8 : Check your Django Application 
```sh
(myenv) $ cd mysite
(myenv) $ python manage.py runserver
November 03, 2017 - 13:01:32
Django version 1.11.6, using settings mysite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```
### Step 9 : Test application on browser 
Open http://127.0.0.1:8000/ Url into your browser if you will see a webpage then congratulation Django is working properly. 
### Step 10 : Configure Django Project with Apache. 
First you should exit from virtual environment by using “deactivate” command. Then you need to edit apache virtual host file.
```sh
(myenv) $ deactivate
$ sudo nano /etc/apache2/sites-available/000-default.conf 
```
### Step 11: Now Add below listed settings. 
```sh
<VirtualHost *:80>
        ServerName mysite.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html/mysite
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined


       WSGIDaemonProcess mysite python-home=/var/www/html/myenv/python-path=/var/www/html/mysite
WSGIProcessGroup mysite
       WSGIScriptAlias / /var/www/html/mysite/mysite/wsgi.py process-group=mysite

      <Directory /var/www/html/mysite/mysite>
           <Files wsgi.py>
                  Require all granted
            </Files>
      </Directory>
</VirtualHost>
```
Referance : https://docs.djangoproject.com/en/1.11/howto/deployment/wsgi/modwsgi/#using-mod-wsgi-daemon-mode
### Step 11: Final Test 
Now open your domain name i.e http://mysite.com or your server ip address i.e http://192.168.100.23 into your browser. 




