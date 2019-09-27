####### Full Stack Web Developer Nanodegree ######
########### Linux Configuration Project ##########

IP Address : 3.229.33.82
SSH Port : 2200
URL : http://www.topcatalogapp.net


## Introduction
Catalog is an application that displays list of catagories adn items for each
category. general public can only view the catagories and items but user that register
can add/modify/delete catagories and items they own.

Application is securely hosted on Amazon Lightsail linux servers rendered in
Apache web server using wsgi module. It used python in a virtual environment.

## Accessing environment

IP Address : 3.229.33.82

SSH Port : 2200

User : grader (Server can be accessed using the user `grader`. This user has been
       setup to access the server using ssh, key separately shared. Additionally,
       grader is givern sudo access to run specific sudo commands.)

## Environment Setup

The application is set up on Amazon Lightsaid linux server using ubuntu as core OS.
Default user ubuntu is used to perform the setup. Public access to server is
disabled and server can be accessed only via customer Port

Server Setup : set up the server with latest packages
>>sudo apt-get update
>>sudo apt-get upgrade

TimeZone setup : set up the server time zone to UTC
>>sudo timedatectl set-timezone UTC
>>timedatectl status (verify time has changed)

## Securing server
- Change ssh port from 22 to 2200
- generate ssh_key from your local machine and copy the public key into ~/.ssh/authorized_keys
- change configuration not to allow password login. access should allowed only through ssh_key


## Firewall Setup
UFW (Uncomplicated Firewall) is front end for managing firewall rules in Linux.
UFW is used to secure the environment by shutting down all access except for the
ones that the server is meant to serve.

the steps to secure the server is to shut all access and open the ports for required
service one at a time

>> sudo ufw status (Check ufw status)
>> sudo ufw default deny incoming (Deny incoming traffic to all ports)
>> sudo ufw default allow outgoing (Allow outgoing traffic thru all ports)
>> sudo ufw allow 2200/tcp (allow ssh thru pot 2200)
>> sudo ufw allow www (Allow http port)
>> sudo ufw allow 123/udp (Allow ntp port)
>> sudo ufw enable (Start firewall)

## DNS Setup using AWS Route53

AWS Route53 (https://aws.amazon.com/route53/) is used to setup the DNS for the server.


## Summary of installed software

- apt-get install --assume-yes linux-aws
- apt-get install --assume-yes hibagent
- apt-get upgrade
- apt install git
- apt install python
- apt install apache2
- apt-get install postgresql postgresql-contrib
- apt install python-pip
- apt install python3-pip
- apt-get upgrade python3
- apt-get install build-essential libssl-dev libffi-dev python-dev
- apt install -y python3-venv
- apt-get install libapache2-mod-wsgi
- apt-get install python-psycopg2
- apt-get install libpq-dev
- apt install libapache2-mod-wsgi-py3
- apt-get install ntp

## Apache2 installation

- Apache2 installed using the `apt install apache2` command.
- Additionally install the wsgi using `apt install libapache2-mod-wsgi-py3`

 apache installation will create directory /var/www

## Python installation

- Python3 installed using `apt install python3-pip`
- install python virtual environment in /var/www/catalogdir/catalogapp using
  command `python3 -m venv catalogvenv`
- activate virtual enviroment using command
  >> source /var/www/catalogdir/catalogapp/catalogvenv/bin/activate
- Install python packages inside virtual Environment
  pip install
  - flask
  - sqlalchemy
  - oauth2client.client
  - json
  - httplib2
  - random

## PostgreSQL installation

- Postgresql is installed using command
  >> apt-get install postgresql postgresql-contrib
- postgres user is created in linux and a password created for the same.
  This information will be used for Database URI for python program to connect to
  database
- A new database named catalogdb created in postgresql.

## Application installation & configuration

- To create tables and load initial data run >> python lotsofitems.py. This
  program will create tables users, category and item in catalogdb database
- git clone application code into /var/www/catalogdir/catalogapp. All py files
  will be in catalogapp directory, html files in catalogapp/templates and
  css file in /catalogapp/static
- create catalogapp.wsgi file in /var/www/catalogdir
- create apache config file in /etc/apache2/sites-enabled directory called
  catalogapp.config. define Following
  WSGIDaemonProcess catalogapp python-path=/var/www/catalogdir/catalogapp/catalogvenv/lib/python3.6/site-packages python-home=/var/www/catalogdir/catalogapp/catalogvenv
  WSGIScriptAlias / /var/www/catalogdir/catalogapp.wsgi
- existing default site is dissited
- catalogapp site is ensited
- apache is restarted after the above changes 'sudo apache2ctl restart'

## Linux User Management

- A new user named grader is created in the server, and can access th server
  only using ssh enabled port. `ssh-keygen` is used to generate the ssk key
  pair for grader, shared separately.
- Verify the sshd_config file to confirm if password authentication is disabled
  by checking the property <b>PasswordAuthentication no</b>

## Security

Security is handled at multiple levels
- Root login is not allowed by updating /etc/ssh/sshd_config file to change
  'PermitRootLogin no'
- Blocking password login by updating /etc/ssh/sshd_config file to change
  'PasswordAuthentication no'
- Allowing logging in only by ssh_key, generating secret key and inserting
  publickey in ~/.ssh/authorized_keys
- creating grader as new user and granting sudo privileges
- implemented Ubuntu's Uncomlicated Firewall (ufw) rules by allowing access to
  specific ports.
  SSH at port 2200
  HTTP at port 80
  NTP at port 123
- Application security is configured using google's oauth login and CRUD
  operations are enabled only when a user is logged in. As well, current user
  can perform CRUD only on the catalogs owned by them.


## Files for the application

  lotsofitems.py -> creation of tables and inserting initial data
  catalogapp.wsgi -> apache wsgi file
  database_setup.py -> database setup file for the application
  README.md -> Application manual
  client_secrets.json -> googleplus connect secret key file
  requirements.txt -> list of packages required
  static -> directory containing style.css file
  tempaltes -> directory containing html files  
  __init__.py -> application program


## Access the application

  Catalog application is hosted in www.topcatalogapp.net running in apache
  web server using the web service gateway interface.


### References

- [Amazon Lightsail DNS](https://lightsail.aws.amazon.com/ls/docs/en_us/articles/lightsail-how-to-create-dns-entry)
- [Apache wsgi configuration](http://flask.pocoo.org/docs/1.0/deploying/mod_wsgi/)
- [Python Flask Virtual Environment](http://flask.pocoo.org/docs/1.0/installation/)
- [Ubuntu UFW Setups](https://help.ubuntu.com/community/UFW)
- [Postgresql Setup](https://help.ubuntu.com/lts/serverguide/postgresql.html.en)
