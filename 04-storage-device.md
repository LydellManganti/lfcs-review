## Storage Device
---

### Partitioning Storage Device
- use `gdisk` to partiion a device into two
- display filesystem supported
- format the partition with ext4

<details><summary>show</summary>
<p>

```bash
sudo gdisk /dev/xvdc1
# partition into 2 then write the changes
ls /sbin/mk*
sudo mkfs.ext4 /dev/xvdc1
sudo mkfs.ext4 /dev/xvdc2
```
</p>
</details>

---


### Create and Configure Swap Partition
- use `xvdc1` as a swap partition
- add in `fstab`
- enable/disable partition
- display swap partition detail

<details><summary>show</summary>
<p>

```bash
sudo vi /etc/fstab
# add the following line
/dev/xvdc1 swap swap sw 0 0
sudo mkswap /dev/xvdc1
sudo swapon -v /dev/xvdc1
# display swap detail
lsblk
cat /proc/swaps
free -h
# disable swap
sudo swapoff /dev/xvdc1
```
</p>
</details>

---
