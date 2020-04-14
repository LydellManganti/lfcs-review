## Apache Web Server

### Verify if webserver is installed or running
<details><summary>show</summary>
<p>

```bash
ps -ef | grep '(httpd|apache)' | grep -v grep
```
</p>
</details>

---

### Setup Name-Based Virtual Hosts
- Create two public html files with different domain
- Set Permission to `755` to display the html page
- Create the Config files for the site
- Enable the site
- Update `/etc/hosts` file to point ip to the host
- Test the site are working
<details><summary>show</summary>
<p>

```bash
sudo vi /var/www/html/myfirstsite.com/public_html/index.html
sudo vi /var/www/html/mysecondsite.com/public_html/index.html
# Add the html like
<html>
  <h1> My First Site </h1>
</html>
sudo chmod -R 755 /var/www/html/myfirstsite.com/public_html
sudo chmod -R 755 /var/www/html/mysecodnsite.com/public_html
# Create the config file for the host
sudo vi /etc/apache2/sites-available/myfirstsite.com.conf
# Add The following lines
<VirtualHost>
  ServerAdmin admin@myfirstsite.com
  DocumentRoot /var/www/html/myfirstsite.com/public_html
  ServerName www.myfirstsite.com
  ServerAlias www.myfirstsite.com myfirstsite.com
  ErrorLog /var/www/html/myfirstsite.com/error.log
  LogFormat "%v %l %u %t \"r\" %s> %b" myvhost
  CustomLog /var/www/html/myfirstsite.com/access.log 
</VirtualHost>
# Enable the Site
sudo a2ensite myfirstsite.com
sudo a2ensite mysecondsite.com
# Reload the config
sudo systemctl reload apache2
sudo systemctl restart apache2
# Update /etc/hosts and add the line
123.45.56.78 www.myfirstsite.com
123.45.56.78 www.mysecondsite.com
# Test the hosts
curl www.myfirstsite.com
curl www.mysecondsite.com
```
</p>
</details>

---

### Setup SSL with Apache
- Enable Apache Module SSL
- Generate certificate and key file using `openssl`
- Update VirtualHosts to enable https
- Test the secured connection
<details><summary>show</summary>
<p>

```bash
sudo a2enmod ssl
sudo mkdir /etc/apache2/ssl-certs
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl-certs/apache.key -out /etc/apache2/ssl-certs/apache.crt
sudo vi /etc/apache2/sites-available/myfirstsite.com.conf
# Copy the VirtualHost and add the following lines
SSLEngine on
SSLCertificateFile /etc/apache2/ssl-certs/apache.crt
SSLCertificateKeyFile /etc/apache2/ssl-certs/apache.key
# Add the following lines on top of the myfirstsite.com.conf file
NameVirtualHost *:80
NameVirtualHost *:443
```
</p>
</details>

