### Install ftp client and server
<details><summary>show</summary>
<p>

```bash
sudo apt-get update
sudo apt-get install vsftpd ftp
```
</p>
</details>

---
### Start and Enable ftp daemon

<details><summary>show</summary>
<p>

```bash
sudo systemctl start vsftpd
sudo systemctl enable vsftpd
```
</p>
</details>

---
### Update FTP Server Config
- Allow anonymous access to the server
- Allow Anonymous acces without password
- Set Local directory for anonymous access on `/storage/ftp`

<details><summary>show</summary>
<p>

```bash
# Backup current config file before making changes
sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.orig
sudo vi /etc/vsftpd.conf
# Add the following lines
anonymous_enable=YES
no_anon_password=YES
anon_root=/storage/ftp/
```
</p>
</details>

---
- Allow local users access
- Restrict authenticated users to their home directory
> Do NOT set this when testing `ftp localhost`
<details><summary>show</summary>
<p>

```bash
sudo vi /etc/vsftpd.conf
# Add the following lines
local_enable=YES
chroot_local_user=YES
chroot_list_enable=YES
chroot_list_file=/etc/vsftpd.chroot_list
# create the file chroot_list
sudo touch /etc/vsftpd.chroot_list
```

</p>
</details>

---

- Add available bandwidth for anonymous users and logged in users. Set logged in users twice as fast as anonymous users.
- Add maximum concurrent connections per IP

<details><summary>show</summary>
<p>

```bash
sudo vi /etc/vsftpd/conf
# Add the following lines
anon_max_rate=10240
local_max_rate=20480
max_per_ip=5
```

</p>
</details>

---

- Restrict TCP ports for the server.
- Allow server to listen on IPv4 sockets
<details><summary>show</summary>
<p>

```bash
sudo vi /etc/vsftpd.conf
# Add the following lines
listen=YES
pasv_enable=YES
pasv_max_port=15500
pasv_min_port=15000
```

</p>
</details>

---
### Setup firewalld to add ftpd and its ports

<details><summary>show</summary>
<p>

```bash
sudo apt-get update
sudo apt-get install firewalld
sudo systemctl start firewalld
sudo systemctl enable firewalld
sudo firewall-cmd --add-service=ftp
sudo firewall-cmd --add-service=ftp --permanent
sudo firewall-cmd --add=port=15000-15500
sudo firewall-cmd --add=port=15000-15500 --permanent
```
</p>
</details>

---
### Set ports to be used in IPTables
- Write iptable rules to `/etc/iptables` 
<details><summary>show</summary>
<p>

```bash
sudo iptables --append INPUT --protocol TCP --destination-port 21 -m state --state NEW,ESTABLISHED --jump ACCEPT
sudo iptables --append INPUT --protocol TCP --destination-port 15000:15500 -m state --state ESTABLISHED,RELATED --jump ACCEPT
sudo mkdir /etc/iptables
sudo iptables-save | sudo tee -a /etc/iptables/rules.v4
```
</p>
</details>


---
### Load ip_conntrack_ftp module
- Set module to autoload on restart
<details><summary>show</summary>
<p>

```bash
sudo modprobe ip_conntrack_ftp
echo "ip_conntrack_ftp" | sudo tee -a /etc/modules
```
</p>
</details>

---
### Test ftp
- Make sure `vsftpd` service is running state without error
<details><summary>show</summary>
<p>

```bash
sudo systemctl restart vsftpd
ftp local
ftp> anonymouse
ftp> ls
ftp> get your-filename
```
</p>
</details>
