## Squid Proxy Configuration
---

### Create Squid Configuration to Internet access by IP and IP range
- Backup existing conf file
- Create new conf file from existing one and remove documentation and empty lines
- Create acl elements for IP and IP range
- Create acl list rules to allow access
<details><summary>show</summary>
<p>

```bash
sudo mv /etc/squid/squid.conf /etc/squid/squid.conf.bkp
grep -Eiv '(^#|^$)' /etc/squid.conf | sudo tee /etc/squid/squid.conf
sudo vi /etc/squid/squid.conf
# Add acl elements
acl ubuntuOS src 172.1.1.1/32
acl homeNetwork src 192.168.0.0/24
# Add acl list rule
http_access ubuntuOS allow
http_access homeNetwork allow
sudo systemctl restart squid
```
</p>
</details>

---

### Test Proxy Configuration
- Install `elinks` on Ubuntu Client
- Setup Proxy Configuration. Press `o` -> Protocols -> HTTP -> Proxy Configuration
<details><summary>show</summary>
<p>

```bash
sudo apt-get install elinks
# Setup Proxy Config
elinks
# Press `o` -> Protocols -> HTTP -> Proxy Configuration
# Add server ip, add 3128 port
# Press `g` -> www.google.com
```
</p>
</details>

---

### Create Config to Restrict Domain, Restrict IP
- Restrict access to Facebook and LinkedIn
<details><summary>show</summary>
<p>

```bash
sudo vi /etc/squid/squid.conf
# Add acl element
acl forbidden dstdomain "/etc/squid/forbidden_dommains"
# Create Forbidden Domain file
sudo vi /etc/squid/forbidden_domains
# Add the following entries
.facebook.com
.linkedin.cm
# Add acl list rule to restrict an IP
http_access homeNetwork allow !ubuntuOS
# Add acl list rule to restrict domain access
http_access homeNetwork allow !forbidden
sudo systemctl restart squid
```
</p>
</details>

---

### Create Config to allow access on forbidden sites on a specific time of week
- Create rule to allow access during Friday 8am to 5pm
<details><summary>show</summary>
<p>

```bash
sudo vi /etc/squid/squid.conf
# Add the followng lines
acl friday time F 8:00-17:00
http_access allow forbidden friday
http_access deny forbidden
```
</p>
</details>

---

### Setup BasicAuth on Proxy Server
- Install `apache2` to allow `htpasswd` usage
<details><summary>show</summary>
<p>

```bash
sudo vi /etc/squid/squid.comf
# Add the following lines
auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic credentialstts 30 minutes
auth_param basic casesensitive on
auth_param basic realm Squid proxy-caching web server for testing
acl ncsa proxy_auth REQUIRED
http_access allow ncsa
# set basic auth user/password
sudo htpasswd -c /etc/squid/passwd testuser
sudo systemctl restart squild
# Setup elinks with user/password
# Test internet connectivity
```
</p>
</details>

---

### Steup Caching to Increase Data Transfer
<details><summary>show</summary>
<p>

```bash
sudo vi /etc/squid/squid.conf
# create cache dir
sudo mkdir /var/cache/squid
sudo chown -R proxy:proxy /va/cache/squid
# Add the following lines
cache_dir ufs /var/cache/squid 1000 16 256
maximum_object_size 100 MB
refresh_pattern .*\.(mp4|iso) 2880
# setup client by creating env var http_proxy="http://host:port/" 
# restart then test wget
sudo systemctl restart squid
```
</p>
</details>

---

