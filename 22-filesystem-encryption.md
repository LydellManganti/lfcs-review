## Encrypt File System and Swap Space
### Determine if encryption is supported by kernel
- Load `dm_crypt` modules
<details><summary>show</summary>
<p>

```bash
# Result below is CONFIG_DM_CRYPT=m
grep -i config_dm_crypt /boot/config-$(uname -r)
lsmod | grep dm_crypt
# Load module
sudo modprobe dm_crypt
```
</p>
</details>

---

### Install cryptsetup
<details><summary>show</summary>
<p>

```bash
sudo apt-get update
sudo apt-get install cryptsetup
```
</p>
</details>

---

### Setup Encrypted Partition
- Delete all files on a device to be encrypted
- Setup Encrypted Partition
- Open the LUKS Partition
- Format Encrypted Partition
<details><summary>show</summary>
<p>

```bash
sudo dd if=/dev/urandom of=/dev/xvdb bs=4096
sudo cryptsetup luksFormat /dev/xvdb
sudo cryptsetup luksOpen /dev/xvdb my_encrypted_partition
sudo mkfs.ext4 /dev/mapper/my_encrypted_partition
```
</p>
</details>

---

### Mount and Test Encrypted Partition
- Umount the Encrypted Partition
- Close the Encrypted Partition
<details><summary>show</summary>
<p>

```bash
# Create folder to mount
sudo mkdir -p /mnt/encrypted
# Mount the Encrypted Partition
sudo mount /dev/mapper/my_encrypted_partition /mnt/encrypted
mount | grep partition
# Write a file to the folder
sudo touch /mnt/encrypted/test.file
# Unmount and Close the Partition
sudo umount /mnt/encrypted
sudo cryptsetup luksClose my_encrypted_partition
# Test mount as a regular file system
sudo mount /dev/xvdb /mnt/encrypted
# Should display an error
```
</p>
</details>

---

### Setup Encrypted Swap Space
<details><summary>show</summary>
- Create an Encrypted Partition as above
- Enable Swap for the Partition
- Verify Swap Status
<p>

```bash
# Create a new Encrypted Partition named my_encrypted_swap
sudo mkswap /dev/mapper/my_encrypted_swap
sudo swapon /dev/mapper/my_encrypted_swap
# Update /etc/fstab with
/dev/mapper/my_encrypted_swap none swap sw 0 0
# Update /etc/crypttab with
swap /dev/xvdf /dev/urandom swap
# Reboot to apply the changes
sudo reboot
# Verify Swap Status
sudo cryptsetup status my_encrypted_swap
```
</p>
</details>

