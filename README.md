# udacity-linux-configuration
Udacity project for the linux-server configuration
* Address: 18.223.116.156
* SSH-Port: 2200
* Password for Grader: "udacity"

## Server configuration
### Update all currently installed packages.
* ```sudo apt-get update```
* ```sudo apt-get full-upgrade```
* ```sudo apt-get autoremove```
* ```sudo reboot```

### Change the SSH port from 22 to 2200.
* Edit file /etc/ssh/sshd_config
* Update Port 22 to 2200
* Restart ssh ```sudo service ssh restart```

### Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123).
* Before configuring server Firewall, Lightsail Ports must be enabled for 2200 and http
* ```sudo ufw default deny incoming```
* ```sudo ufw allow 2200/tcp```
* ```sudo ufw allow http```
* ```sudo ufw allow ntp```
* Enable Firewall ```sudo ufw enable```

### Create a new user account named grader.
* ```sudo adduser grader```, password = udacity

### Give grader the permission to sudo.
* Create file grader at /etc/sudoers.d
* Add to grader file ```grader ALL=(ALL) NOPASSWD:ALL```

### Create an SSH key pair for grader using the ssh-keygen tool.
* Server
  * Login as grader and navigate to home-directory
  * ```mkdir .ssh```
  * ```touch .ssh/authorized_keys```
  * ```nano .ssh/authorized_keys```
  * Copy public key from Client into this file
  * ```chmod 700 .ssh```
  * ```chmod 644 .ssh/authorized_keys```
* Client
  * ```ssh-keygen```
  
### Configure the local timezone to UTC.
* sudo timedatectl set-timezone Etc/UTC

### Install and configure Apache to serve a Python mod_wsgi application.
* Install apache ```sudo apt-get install apache2```
* Install wsgi ```sudo apt-get install libapache2-mod-wsgi```
* Edit /etc/apache2/sites-available/000-defaut.conf and add ```WSGIScriptAlias / /var/www/html/myapp.wsgi``` before the closing tag of Virtual Host.
* Install python dependencies for catalog app
  * ```sudo apt-get install python-flask```
  * ```sudo apt-get install python-sqlalchemy```
  * ```sudo apt-get install python-oauth2client```
  * ```sudo apt-get install python-httplib2```
  * ```sudo apt-get install python-requests```
  * ```sudo apt-get install python-psycopg2```

### Install and configure PostgreSQL
* Disable remote connections 'listen_addresses' in ```/etc/postgresql/9.5/main/postgresql.conf``` needs to be commented out, or set to localhost only.
* Switch to postgres user
* ```psql```
* ```CREATE USER catalog_dbuser PASSWORD udacity```
* ```CREATEDB catalog```
* ```GRANT ALL PRIVILEGES ON DATABASE catalog TO catalog_dbuser```

 ### Clone and setup your Item Catalog project from the Github repository you created earlier in this Nanodegree program.
 * Navigate to /var/www/html
 * ```git clone https://github.com/FaboSc/Udacity-catalog```
 
 ### Refactor Code
 * Changed database from sqlite to postgresql
 * Adjusted client-secrets.json path
