## Install Network Services
### Insall NFS (Network File System) Server
<details><summary>show</summary>
<p>

```bash
sudo apt-get update
sudo apt-get install nfs-kernel-server
```
</p>
</details>

---
### Install Apache Web Server
<details><summary>show</summary>
<p>

```bash
sudo apt-get update
sudo apt-get install apache2
```
</p>
</details>

---
### Install Squid and SquidGuard
<details><summary>show</summary>
<p>

```bash
sudo apt-get update
sudo apt-get install squid3 squidguard
```
</p>
</details>

---
### Install Postfix and Dovecot
<details><summary>show</summary>
<p>

```bash
sudo apt-get update
sudo apt-get install postfix dovecot-imapd dovecot-pop3d
```
</p>
</details>

---
### Install NTP Network Time Protocol date
<details><summary>show</summary>
<p>

```bash
sudo apt-get update
sudo apt-get install ntpdate
```
</p>
</details>

---
### Update local time from NTP Server
- Query the server of the NTP Time
 
<details><summary>show</summary>
<p>

```bash
sudo ntpdate 1.ro.pool.ntp.org
sudo -qu 1.root.ntp.org
```
</p>
</details>

---
### Verify iptables package is loaded.
<details><summary>show</summary>
<p>

```bash
lsmod | grep ip_tables
# Load the package if not yet loaded
modprobe -a ip_tables
```
</p>
</details>

---
### Automatically Start Services after reboot
<details><summary>show</summary>
<p>

```bash
sudo systemctl enable apache2
# To remove auto start 
sudo systemctl disable apache2
```
</p>
</details>

