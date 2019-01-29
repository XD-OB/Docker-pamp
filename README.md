# PAMP Docker for dump at 42 AND Camagru

Dockerized version of the good old PAMP stack to get you up and running in no time. With PHP 7.1 and phpmyadmin

## What's in the box

- PHP7.1
- Apache Httpd v2.2 (with mod_php)
- MySQL 5.7
- phpMyAdmin

## Getting started

1. Create a symlink named `src` from the PHP project into the `docker-pamp` directory:    ln -s /Path/to/PHP/project docker-pamp/src

2. Copy default configuration files into the PHP docker directory:     cp docker-pamp/defaults/* docker-pamp/php71
        
3. Bring up the AMP7.1 stack. It use the same port 80:      docker-compose up php71
        
4. Checkout your website at `127.0.0.1`

5. Checkout your DB with phpMyAdmin at `localhost:8081`

# Initializing a database

SQL files inside the `mysql` directory will be executed on docker-compose up (with some caveats). This is particularly useful to create an initial database and/or tables.
If you want to use another db's name (other than Camagru), modify the environment in docker-compose.yaml

# Edit vhost file to solve Forbidden errors:

<VirtualHost *:80>
  DocumentRoot /docker-pamp/src
  <Directory /docker-pamp/src>
  		AllowOverride All
		Require all granted
		Allow from all
    <IfModule mod_rewite.c>
      Options -Multiviews
      RewriteEngine On
      RewriteCond %{REQUEST_FILENAME} !-f
      RewriteRule ^(.*)$ index.php [QSA,L]
    </IfModule>
  </Directory>

  ErrorLog /var/log/apache2/docker_pamp_error.log
  CustomLog /var/log/apache2/docker_pamp_access.log combined
</VirtualHost>
