### Archive/Compress Files & Directories
---

## Archive files
- Archive files using gzip
- Archive files using bzip2
- Archive files using xzip

<details><summary>show</summary>
<p>

```bash
mkdir test
cd test
touch file0 file1 file2 file3 file4 file5 file6 file7 file8 file9
tar czf myfiles.tar.gz file[0-9]
tar cjf myfiles.tar.bz2 file[0-9]
tar cJf myfiles.tar.xz file[0-9]
```
</p>
</details>

---

## Append/Delete file from tarball
- Decompress the tarball
- Delete file4 and Add file10
- Tar all files which are `file` ascii
- Restore backup preserving permissions

<details><summary>show</summary>
<p>

```bash
gzip -d myfiles.tar.gz
bzip2 -d my myfiles.tar.bz2
xz -d myfiles.tar.xz
# Delete file
tar --delete --file myfiles.tar file4
# Add file
tar --update --file myfiles.tar file4
# Compress tar
gzip myfiles.tar
# command to tar all txt files 
tar X <(for i in ./*; do file $i | grep -i ascii; if [ $? -eq 1 ]; then echo ${i:2}; fi; done) -cjf backupfile.tar.bz2 ./*
tar zjf backupfile.tar.bz2 --directory user_restore --same-permissions
```
</p>
</details>

---

## Find Command to Search for Files
- Find files with 2 sub directories level and size greater than 2mb
- Find conf file in /etc modified 6 months ago
- Find conf file in /etc accessed less than 6 months ago

<details><summary>show</summary>
<p>

```bash
find . -maxdepth 3 -type f -size +2M
find /etc -iname "*.conf" -mtime +180
find /etc -iname "*.conf" -atime -180
```

---
## Set file permission
- Set Octal permission for owner, group, user
- Remove execute permission for all user
- Change ownership of file0 to ubuntu user and group
- Change only the group of file1 to ubuntu

<details><summary>show</summary>
<p>

```bash
chmod 744 file1
chmod a-x file2
chown ubuntu:ubuntu file0
chown :ubuntu file1
```
</p>
</details>

---
