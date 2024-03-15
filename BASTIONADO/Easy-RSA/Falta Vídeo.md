	cp vera-server.crt /home/tux/intermedia/ /etc/ssl/certs/

En el Server falta vera-server.key
scp /home/tux/ca/easy-rsa/vera-server.key /etc/apache2/

sudo aenmod ssl

scp /home/tux/ca/easy-rsa/vera-server.key /etc/ssl/private/

Modificar /etc/apache2/sites-avaliable/default-ssl.conf

SSLCertificateFile /etc/ssl/certs/vera-server.crt
SSLCertificateKeyFile /etc/ssl/private/vera-server.key


///
sudo mkdir -p /var/www/vera.local/public_html/
#sudo mkdir -p /var/www/pruebas.local/public_html/

sudo chown -R $USER:$USER /var/www/ vera.com/public_html
#sudo chown -R $USER:$USER /var/www/ pruebas.com/public_html

sudo chmod -R 755 /var/www

subl /var/www/vera.local/public_html/index.html
#subl /var/www/ejemplo.com/public_html/index.html

cp /var/www/ejemplo.com/public_html/index.html /var/www/ vera.local /public_html/index.html

Crear el archivo Virtual Host
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-avaliable/vera.local

sudo subl /etc/apach2/sites-available/vera.local.conf
Virtualhost
```
<VirtualHost *:80>
    ServerAdmin webmaster@localhost -> admin@vera.org
    ServerName vera.local
    ServerAlias www.vera.local
    DocumentRoot /var/www/html -> /var/www/vera.local/public_html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```


sudo a2ensite vera.local.conf
#sudo a2ensite ...

sudo service apache2 reload
sudo service apache2 restart
sudo service apache2 status

sudo nano /etc/hosts
192.168.1.135 vera.local

http://vera.local
https://vera.local
