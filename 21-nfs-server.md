## Setup Tips for ec2
- [Fix service is masked error for nfs-common](https://askubuntu.com/a/946315)
- [Install NFS Server and Client on Ubuntu 18](https://vitux.com/install-nfs-server-and-client-on-ubuntu/)
---

### Ensure idmapd Daemon is running and configured.
<details><summary>show</summary>
<p>

```bash
sudo vi /etc/default/nfs-common
# add the line
NEED_IDMAPD=YES
sudo vi /etc/idmapd.conf
# add the line
Domain = yourdomain.com
# Unmask and start idmapd
sudo systemctl unmask nfs-common
sudo rm /lib/systemd/system/nfs-common.service
sudo systemctl start nfs-common
```
</p>
</details>

---
### Setup the directories for sharing
- `/NFS-SHARE`
- `/NFS-SHARE/mydir` 
<details><summary>show</summary>
<p>

```bash
sudo mkdir -p /NFS-SHARE/mydir
sudo vi /etc/exports
# Add the following lines
/NFS-SHARE *(fsid=0,no_subtree_check,rw,root_squash,sync,anonuid=1000,anongid=100)
/NFS-SHARE/mydir *(ro,sync,no_subtree_check)
# Update Shared folders to have rw permission
sudo chmod 777 /NFS-SHARE
# Restart nfs-server
sudo systemctl restart nfs-server
# Verify folders can be mounted
showmount -e
```
</p>
</details>

---
### Test NFS mount
- Install nfs-common
- Mount nfs
<details><summary>show</summary>
<p>

```bash
sudo apt-get install nfs-common
sudo mount -t nfs 172.31.11.0:/NFS-SHARE /mnt/nfs
```
</p>
</details>

---
### Setup NFS client with autofs
<details><summary>show</summary>
<p>

```bash
sudo apt-get install autofs
sudo vi /etc/auto.master
# Add local directory mapping to nfs config file
/media/nfs  /etc/auto.fs-share  --timeout=60
# Create nfs-share config file with mapping to shared folders
sudo vi /etc/auto.nfs-share
# Add the following lines to map shared directories
writeable_share -fstype=nfs4 172.31.13.255:/
non_writeable_share -fstype=nfs4 172.31.13.255:/mydir
# restart autofs service
sudo systemctl restart autofs
```
</p>
</details>

---
### Verify mounted NFS is working
- test if nfs-share is mounted
- test a file can be written to a writeable folder
- test a file can be read from a readonly folder
<details><summary>show</summary>
<p>

```bash
mount | grep nfs-share
touch /media/nfs/writeable_share/test-file
ls -lah /media/nfs/non_writeable_share
# Verify mount is showing the shared folders
mount | grep non_writeable
mount | gre writeable
```
</p>
</details>
