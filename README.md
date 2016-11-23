# Linux Server Configuration

This document describes the steps I took to configure the configure a Linux server
to run my Item Catalog website.

## Step 1 Update Software

apt-get update
sudo apt-get dist-upgrade

## Step 2 Create User grader

adduser grader
passwd grader

## Step 3 Add User grader to sudo list

adduser grader sudo

## Step 4 Change ssh port

nano /etc/ssh/sshd_config
changed Port 22 to Port 2200
service ssh force-reload

## Step 5 Configure the Uncomplicated Firewall (UFW)

### Only allow incoming traffic from:

- SSH (port 2200)
- HTTP (port 80)
- NTP (port 123)

### Configure ufw to check this rules:

ufw default deny incoming
ufw default allow outgoing
ufw allow 2200/tcp
ufw allow www
ufw allow ntp
ufw enable

## Step 6 Configure the local timezone to UTC

dpkg-reconfigure tzdata

## step 7 install git and clone item list repo

apt-get install git
cd /var/www/html
git clone https://github.com/peng-ling/Udacity-FSWD-P3-Item_Catalog.git

## Step 8 Install and configure Apache to serve a Python mod_wsgi application

apt-get install apache2
apt-get install libapache2-mod-wsgi
nano /etc/apache2/sites-enabled/000-default.conf
added line WSGIScriptAlias / /var/www/html/Udacity-FSWD-P3-Item_Catalog/itemlist.wsgi

## Step 9 Install and configure PostgreSQL

sudo apt-get install postgresql

Ceck if remote connections are prohibited
nano /etc/postgresql/9.3/main/postgresql.conf

## Step 10 Create a new user named catalog that has limited permissions to your catalog application database

Script (Setup/postgre.sql) can be found in the item database repo.

## Step 11 Getting item list to run

apt-get install python-pip
pip install Flask
sudo apt-get install python-psycopg2
pip install httplib2
pip install --upgrade oauth2client
