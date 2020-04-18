## Configure SquidGuard
---

### Integrate Blacklist
- Install Shalla Blacklists (hacking, chat)
- Setup SquidGuard Database

<details><summary>show</summary>
<p>

```bash
# create a placeholder folder for the BlackLists
sudo mkdir -p /opt/3rdparty
sudo cd /opt/3rdparty
sudo wget http://www.shallalist.de/Downloads/shallalist.tar.gz
# Extract from same folder
tar xvf shallalist.tar.gz
# Verify BL foler is created
cd BL
ls
# copy categories to be blacklisted
cp -a /opt/3rdparty/BL/hacking /var/lib/squidguard/db
cp -a /opt/3rdparty/BL/chat /var/lib/squidguard/db
# create the .db file from the categories
sudo squidGuard -c all
# Set proxy user to the db
sudo chown -R proxy:proxy /var/lib/squidguard/db/
```
</p>
</details>

---

### Configure Squid with SquidGuard
- Add squidguard in squid configuration
- Create squidguard cnfiguration

<details><summary>show</summary>
<p>

```bash
echo "url_rewrite_program $(which squidGuard)" | sudo tee -a /etc/squid/squid.conf
sudo vi /etc/squidguard/squidGuard.conf
# add the following lines
src localnet {
    ip 172.13.0.0/16
}

dest hacking {
    domainlist  hacking/domains
    urllist     hacking/urls
}

dest chat {
    domainlist  chat/domains
    urllist     chat/urls
}

acl {
    localnet {
        pass !hacking !chat !in-addr all
        redirect http://www.google.com
    }
    default {
        pass local none
    }
}
sudo systemctl restart squid
sudo tail -f -n10 /var/log/squid/access.log
sudo tail -f -n10 /var/log/squidguard/squidGuard.conf
```
</p>
</details>

---

### Removing Restriction
<details><summary>show</summary>
<p>

```bash
sudo rm -rf /var/lib/squidguard/db/chat
sudo vi /etc/squidguard/squidGuard.conf
# delete dest and acl entry for chat
sudo systemctl restart squid
```
</p>
</details>

---

### Whitelist specific domains
<details><summary>show</summary>
<p>

```bash
# create a db domains and urls for whitelisting
sudo mkdir -p /var/lib/squidguard/db/whitelist/domains
sudo mkdir -p /var/lib/squidguard/db/whitelist/urls
# append whitelisted domains
sudo vi /var/lib/squidguard/db/whitelist/domains
sudo vi /var/lib/squidguard/db/whitelist/urls
# create db file
sudo squidGuard -C all
sudo chown -R proxy:proxy /var/lib/squidguard/db
# add the entries to squidguard.conf
sudo vi /etc/squidguard/squidGuard.conf
dest whitelist {
    domainslist whitelist/domains
    urllist     whitelist/urls
}
acl {
    localnet {
        pass whitelist !hacking !chat !in-addr all
        redirect www.google.com
    }
}
```
</p>
</details>
