### Iptables
---

## List Current rules
<details><summary>show</summary>
<p>

```bash
sudo iptables -L
```
</p>
</details>

---

## Add New Rules
- drop ping input connection
- flush iptables rules
- update to reject ping input connection
<details><summary>show</summary>
<p>

```bash
sudo iptables -A INPUT --protocol icmp --in-protocol eth0 -j DROP
sudo iptables -F
sudo iptables -A INPUT --protocol icmp --in-protocol eth0 -j REJECT
# Test the rule by
ping -c 3 172.31.1.1
```
</p>
</details>

---

## Add Rules for NFS  clients
- Add rule on nfs server to prevent client from mounting
- Add rule on nfs server to allow client to mount
<details><summary>show</summary>
<p>

```bash
sudo iptables -A INPUT -i eth0 -s 0/0 -p tcp --dport 111 -j REJECT
sudo iptables -A INPUT -i eht0 -s 0/0 -p udp --dport 111 -j REJECT
sudo iptables -A INPUT -i eth0 -s 0/0 -p tcp --dport 2049 -j REJECT
# test connection with client
sudo mount -t nfs4 173.31.1.1:/nfs-share /mnt
#
sudo iptables -A INPUT -i eth0 -s 0/0 -p tcp --dport 111 -j ACCEPT
sudo iptables -A INPUT -i eht0 -s 0/0 -p udp --dport 111 -j ACCEPT
sudo iptables -A INPUT -i eth0 -s 0/0 -p tcp --dport 2049 -j ACCEPT
```
</p>
</details>

---

## Inserting/Deleting rules
- Insert a New Rule with priority 2
- Delete third rule
- Replace rule from ACCEPT to REJECT
<details><summary>show</summary>
<p>

```bash
sudo iptables -I INPUT 2 -p tcp --dport 80 -j ACCEPT
sudo iptables -D INPUT 3
# display iptables data
sudo -nL -v --line-numbers
#
sudo iptables -R INPUT 2 -p tcp --dport 80 REJECT
```
</p>
</details>

---

## Persist Iptables rules after reboot
- Save iptables into `/etc/iptables`
- Restore iptables from `/etc/iptables`
- Persist rules during reboot
<details><summary>show</summary>
<p>

```bash
sudo iptables-save | sudo tee /etc/iptables/rules.v4
sudo iptables-restore < /etc/iptables/rules.v4
sudo apt-get install update
sudo apt-get install -y iptables-persistent
```
</p>
</details>

---
