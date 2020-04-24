## Mount Local and Network Filesystem
---

### Mount Local Filesystem
- mount an ext4 with readonly and no exec
- mount with defaults
- umount both filesystem
<details><summary>show</summary>
<p>

```bash
sudo mount -t ext4 /dev/xvdc1 /mnt/disk1 -o ro,noexec
sudo mount -t ext4 /dev/xvdc2 /mnt/disk2 -o defaults
sudo umnount /mnt/disk1
```
</p>
</details>

---

### Mount a samba share
- Install `samba-client`
- Look for availabe smaba shares
- Mount on fstab with credentials on a hidden file
<details><summary>show</summary>
<p>

```bash
sudo apt-get install samba-client
smbclient -L 192.168.1.2
# set password
sudo mkdir /media/samba
echo "username=samba_username" | sudo tee -a /media/samba/.credentials
echo "password=samba_password" | sudo tee -a /media/samba/.credentials
sudo chmod 600 /media/samba/.credentials
#
sudo vi /etc/fstab
# add the following line
//192.168.1.2/share /media/samba cifs credentials=/media/samba/.credentials,defaults 0 0
sudo mount -a
```
</p>
</details>

---

### Mounting NFS share
- mount `192.168.1.2/nfs`
<details><summary>show</summary>
<p>

```bash
sudo vi /etc/fstab
# add the line
192.168.1.2:/nfs /mnt nfs defaults 0 0
```
</p>
</details>

---

