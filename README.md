# Tutorial-Owncloud

COMO MONTAR SEU PRÓPRIO SERVIDOR DE NÚVEM!

apt-get install apache2

echo 'deb http://download.opensuse.org/repositories/isv:/ownCloud:/community/Debian_7.0/ /' >> /etc/apt/sources.list.d/owncloud.list

wget http://download.opensuse.org/repositories/isv:ownCloud:community/Debian_7.0/Release.key

apt-key add - < Release.key

apt-get update

apt-get install owncloud

######Isira seu ip no navegador /owncloud e logo você verá sua nuvem owncloud#######

Criando um banco de dados:

mysql -u root -h localhost -p

CREATE DATABASE owncloud_DB;

CREATE USER 'SEU LOGIN'@'localhost' IDENTIFIED BY 'SUA SENHA';

GRANT ALL PRIVILEGES ON owncloud_DB.* TO 'SEU LOGIN'@'localhost';

FLUSH PRIVILEGES;

####CONFIDURANDO SSL HTTPS#######

mkdir /etc/apache2/ssl

openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt

nano /etc/apache2/conf.d/owncloud.conf

****************************************************************

Alias /owncloud /var/www/owncloud

<VirtualHost 192.168.1.107:80>
    RewriteEngine on
    ReWriteCond %{SERVER_PORT} !^443$
    RewriteRule ^/(.*) https://%{HTTP_HOST}/$1 [NC,R,L]
</VirtualHost>

<VirtualHost 192.168.1.107:443>
    SSLEngine on
    SSLCertificateFile /etc/apache2/ssl/apache.crt
    SSLCertificateKeyFile /etc/apache2/ssl/apache.key
    DocumentRoot /var/www/owncloud/
<Directory /var/www/owncloud>
    Options Indexes FollowSymLinks MultiViews
    AllowOverride All
    Order allow,deny
    Allow from all
</Directory>
</VirtualHost>

******************************************************************
a2enmod ssl

a2enmod rewrite

service apache2 restart



Isira seu ip no navegador, logo você será redirecionado para o HTTPS mas sem identidade confiramda!

Para que essa menssagem desapareça terá que ter comprar um certificado auto assinado
