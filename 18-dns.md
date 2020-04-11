## EC2 Ubuntu 18 setup
- Follow the [Ubuntu DNS Server setup](https://aws.amazon.com/premiumsupport/knowledge-center/ec2-static-dns-ubuntu-debian/)
- Add the below to `/etc/netplan/99-custom-dns.yaml`
```bash
network:
    version: 2
    ethernets:
        eth0:         
            nameservers:
                    addresses: [1.2.3.4, 5.6.7.8]
            dhcp4-overrides:
                    use-dns: false
```
- Run `systemd-resolve --status` to confirm the changes.

> More Resources In: 
> <br>- [nslookup not expanding host](https://askubuntu.com/questions/1040595/dns-at-systemds-127-0-0-53-is-ignoring-some-lookups/1072127#1072127)
> <br>- [bind permission error fix](https://serverfault.com/questions/447913/bind-permission-errors/811841#811841)
> <br>- [setup dns server in ec2 ubuntu](https://aws.amazon.com/premiumsupport/knowledge-center/ec2-static-dns-ubuntu-debian/)
---
### Install DNS Server
<details><summary>show</summary>
<p>

```bash
sudo apt-get update
sudo apt-get install bind9 bind9utils
```
</p>
</details>

---
### Configure DNS Server
- Create a caching server with your host IP
- Use Google name servers
<details><summary>show</summary>
<p>

```bash
sudo vi /etc/bind/named.conf.options
# Add the following lines inside options block
listen-on port 53 { 127.0.0.1; 172.31.14.134; };
allow-query { localhost; 172.31.14.0/24; };
recursion yes;
forwarders {
    8.8.8.8;
    8.8.4.4;
}
```
</p>
</details>

---
### Configure DNS Zones
- configure `projects.com` zone
- configure `14.31.172.in-addr.arpa.zone`
<details><summary>show</summary>
<p>

```bash
sudo vi /etc/bind/named.conf
# Add the following lines
zone "example.com." IN {
    type master;
    file "/var/lib/bind/example.com.zone";
};
zone "14.31.172.in-addr.arpa.zone" {
    type master;
    file "/var/lib/bind/14.31.172.in-addr.arpa.zone"
};
```
</p>
</details>

---
### Validate Configuration
<details><summary>show</summary>
<p>

```bash
sudo named-checkconf /etc/bind/named.conf
```
</p>
</details>

---

### Create DNS Zone
- DNS name of example.com
- Add an `A` record for `projects`, `mail`, 
- Add a `CNAME` for `www.projects`
<details><summary>show</summary>
<p>

```bash
sudo vi /var/lib/bind/example.com.zone
# Add the following lines
$TTL    604800
@       IN      SOA     example.com. root.example.com. (
                        2016051101 ; Serial
                        10800 ; Refresh
                        3600 ; Retry
                        604800 ; Expire
                        604800) ; Negative TTL
;
@       IN      NS      dns.example.com.
dns     IN      A       172.31.14.134
projects    IN      A       172.31.14.29
mail    IN      A       172.31.14.28
@       IN      MX      10 mail.example.com.
www.projects        IN      CNAME   projects
```
</p>
</details>

---
### Create Reverse Zone Configuration
- Create Pointer record to `projects.example.com`, `mail.example.com`
<details><summary>show</summary>
<p>

```bash
sudo vi /var/lib/bind/14.31.172.in-addr.arpa.zone
# Add the following lines
$TTL    604800
@       IN      SOA     example.com. root.example.com. (
                        2016051101 ; Serial
                        10800 ; Refresh
                        3600 ; Retry
                        604800 ; Expire
                        604800) ; Minimum TTL
;
@       IN      NS      dns.example.me.com.
29      IN      PTR     projects.example.com.
28      IN      PTR     mail.example.com.
```

</p>
</details>

---
### Testing the DNS Server
- Query dns for `example.com`, `projects.example.com`, `www.example.com`
- Determine email handler for `example.com`
- Reverse Query hosts `172.31.14.28` and `172.31.14.29`
- Query outside hosts `8.8.8.8`
<details><summary>show</summary>
<p>

```bash
host www.example.com
host example.com
host projects.example.com

host -t mx example.com

host 172.31.14.28
host 172.31.14.29

host 8.8.8.8
```
</p>
</details>
