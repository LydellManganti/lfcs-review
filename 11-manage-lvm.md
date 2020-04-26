## Manage LVM using vgcreate, lvcreate, and lvextend

### Create Physical Volume
- create physical volumes fro `xvdb`, `xvdc`, `xvdd`
- display created physical volumes
- display detailed information for each physical volume

<details><summary>show</summary>
<p>

```bash
sudo pvcreate /dev/xvdb /dev/xvdc /dev/xvdd
sudo pvs
sudo pvdisplay /dev/xvdb
```

</p>
</details>

---

### Create Volume Group
- create volume group from physical volume `xvdb` and `xvdc`
- display the volume group created

<details><summary>show</summary>
<p>

```bash
sudo vgcreate vg00 /dev/xvdb /dev/xvdc
sudo vgdisplay
```

</p>
</details>

---

### Create Logical Volume
- create 2 logical volume `vol_projects`(2GB) and `vol_backups`(remaining space)
- display the logical volumes created
- display individual information for logical volume
- create ext4 filesystem for both logical volume

<details><summary>show</summary>
<p>

```bash
sudo lvcreate -n vol_projects -L `2G` vg00
sudo lvcreate -n vol_backups -l 100%FREE vg00
sudo lvs
sudo lvdisplay vg00/vol_projects
sudo lvdisplay vg00/vol_backups
sudo mkfs.ext4 /dev/vg00/vol_projects
sudo mkfs.ext4 /dev/vg00/vol_backups
```

</p>
</details>

---

### Resizing Logical Volumes and Extending Volume Groups
- reduce `vol_projects` size by 500Mb
- allocate all remaining space to  `vol_backups`
- add `xvdd` to volume group
- display volume group after adding physical volume

<details><summary>show</summary>
<p>

```bash
sudo lvreduce -L -500M -r /dev/vg00/vol_projects
sudo lvextend -l +100%FREE -r /dev/vg00/vol_backups
sudo vgextend vg00 /dev/xvdd
sudo vgdisplay vg00
```

</p>
</details>

---

### Mount Logical Volumes on Boot, and on Demand
- determine uuid of both logical volume
- create directories to mount the logical volumes
- mount logical volumes to directories

<details><summary>show</summary>
<p>

```bash
sudo blkid /dev/vg00/vol_projects
sudo blkid /dev/vg00/vol_backups
sudo mkdir /home/projects
sudo mkdir /home/backups
# mount in /etc/fstab
sudo vi /etc/fstab
# add the lines
UUID=the-result-of-blkid-here /home/projects ext4 defaults 0 0
UUID=the-result-of-blkid-here /home/backups ext4 defaults 0 0
#
sudo mount -a
mount | grep home
```

</p>
</details>

---
