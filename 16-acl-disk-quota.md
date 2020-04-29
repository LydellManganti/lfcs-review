## ACLs and Disk Quotas

### Access Control List - ACL
- verify if system is supporting ACL
- create a group called developer
- create users called user1, user2, and user3
- add user1 and user2 to developer group
- create `/test` directory and change the groupowner to developer
- use user1 to append to file `/test/file.txt`
- use user2 to append to file `/test/file.txt`
- view acl of `/test/file.txt`
- give user3 `rw` permission to `/test/file.txt`
- use use3 to append to file `/test/file.txt`
- add default acl read access to `/test` directory
- view acl of `/test` directory
- remove user acl on `/test/file.txt`
- remove all acl on `/test`
<details><summary>show</summary>
<p>

```bash
sudo tune2fs -l /dev/xvda | grep "Default mount options"
sudo groupadd developer
sudo useradd user1
sudo useradd user2
sudo useradd user3
sudo usermod -aG developer user1
sudo usermod -aG developer user2
sudo mkdir /test
sudo touch /test/file.txt
sudo chgrp -R developer /test
sudo chmod -R 770 /test
#su user1
echo "This is user1" >> /test/file.txt
#su user2
echo "This is user2" >> /test/file.txt
sudo getfacl /test/file.txt
sudo setfacl -m u:user3:rw /test/file.txt
# su user3
echo "This is user3" >> /test/file.txt
sudo setfacl -m d:o:r /test
sudo getfacl /test
sudo setfacl -x u:user3 /test/file.txt
sudo setfacl -b /test
#  
```

</p>
</details>

---

### Disk Quotas on Users and Filesystems
- add grpquota to `/dev/xvdb` 
- add usrquota to `/dev/xvdc`
- mount `/dev/xvdb` to `/app/projects`
- mount `/dev/xdc` to `/app/backups`
- verify quota is displayed for the mounted volumes
- enable quota for groups on `/app/projects`
- enable quota for users on `/app/backups`
- add block quota for user1 for 1000 blockes (1MB)
- test quota is applied
- add inode quota for user2 for 2 files
- test quota is applied
- add grace period for quota
- display quota usage for user and group
- display quota usage for mounted volume

<details><summary>show</summary>
<p>

```bash
sudo vi /etc/fstab
# add the lines
/dev/xvdb /app/projects ext4 defaults,grpquota 0 0
/dev/xvdc /app/backups ext4 defaults,usrquota 0 0
sudo mount -a
sudo mount | grep quota
sudo quotacheck -avugc
sudo quotaon -vg /app/projects
sudo quotaon -vu /app/backups
sudo edquota -u user1
# add 1000 under blockes/hard
#su user1
dd if-/dev/zero of=/app/backups bs=2M count=1
sudo edquota -g developers
touch /app/projects/file1 /app/projects/file2 /app/projects/file3
sudo edquota -t
sudo quota -u user1
sudo quota -g developer
sudo repquota -v /dev/xvdb
```

</p>
</details>

---

