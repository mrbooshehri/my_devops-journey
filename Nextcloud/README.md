# HTTPS for nextcloud

1. Create the following path:
```bash
mkdir /etc/apache2/ssl
``` 
2. Run the following command:
```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl/nextcloud.key -out /etc/apache2/ssl/nextcloud.crt
```
3. Enter required information

**Note:** To read a ```.crt``` file as human-readable run the following command:
```bash
openssl x509 -in /PATH/TO/CERTIFICATE.crt -noout -text
```
4. Make sure port ```443``` already exists in ```/etc/apache2/ports.con```
5. Edit ```/etc/apache2/sites-available/00-default.conf``` with your favorit editor and make the following changes
```bash
<VirtaulHost *:80>
Redirect "/" "https://YOUR_IPADDRESS/"
#Redirect "/" "https://YOUR_DOMAINNAME/"
</VirtaulHost>


<VirtaulHost *:443>
...
SSLCertificateFile /etc/apache2/ssl/nextcloud.crt
SSLCertificateKeyFile /etc/apache2/ssl/nextcloud.key
...
ErrorLog ${APPACHE_LOG_DIR}/error.log
```
6. Restart ```appache2``` service
```bash
systemctl restart appache2
```
7. Open the nextcloud addess on your borwser and add the cert into your machine
