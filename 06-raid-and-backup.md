## Setup RAID Devices, managing backups.
---

### Setup a raid device
- Install `mdadm`
- Create a raid1 device with 2 devices
- verify the raid array creation status
- set autostart for the service
- format the device
- monitor the raid array device
- add new device to the raid array
- disassemble the raid array and erase their data
- add email notification to raid monitor
<details><summary>show</summary>
<p>

```bash
sudo apt-get update
sudo apt-get install -y mdadm
sudo mdadm --create /dev/md1 --level=1 --raid-devices=2 /dev/xvdb /dev/xvdc
# check the raid creation
cat /proc/mdstat
sudo mdadm --detail /dev/md1
# add autostart
sudo vi /etc/default/mdadm
# add the line
AUTOSTART=true
# format
sudo mkfs.ext4 /dev/md1
# setup monitoring
sudo mdadm --detail --scan
# add the result of detail scan to /etc/mdadm/mdadm.conf
sudo vi /etc/mdadm/mdadm.conf
sudo mdadm --assemble --scan
# allow service to start after restart
sudo systemctl start mdmonitor
sudo systemctl enable mdmonitor
# add new devic
sudo mdadm /dev/md1 --add /dev/xvdd
# stop and disassembe raid
sudo mdadm --stop /dev/md1
sudo mdadm --remove /dev/md1
sudo mdadm --zero-superblock /dev/xvdb
# add email notification
sudo vi /etc/mdadm/mdadm.conf
# add the line
MAILADDR root
```
</p>
</details>

---

### Creating an Image from a device
- Create backup of /dev/xvdb
- Create a compressed backup of /dev/xvdb
- Restore a backup of /dev/xvdb
- Restore a compressed backup of /dev/xvdb
<details><summary>show</summary>
<p>

```bash
sudo dd if=/dev/xvdb of=/backup/xvdb.img
sudo dd if=/dev/xvdb | gzip -c > /backup/xvdb.img.gz
sudo dd if=/backup/xvdb.img of=/dev/xvdb
gzip -dc /backup/xvdb.img.gz | suoo dd of=/dev/xvdb
```
</p>
</details>

---

### Backup directories using rsync
- backup to a local directory
- backup to a remote host using compression
- restore from a local directory
- restore from a remote host
<details><summary>show</summary>
<p>

```bash
sudo rsync -av /home/ubuntu /backup/ubuntu
sudo rsync -avzhe ssh /home/ubuntu ubuntu@remote_host:/backups
# reverse the source and destination directory to restore
```
</p>
</details>

---

