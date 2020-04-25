## Managing Users and Groups, File Permissions and Attributes
---

### User
- Create a user user1
- Set expiry date for user1
- Set shell to bash for user1
- Create group developer
- Assign user1 to developer
- Display groups for user1
- Lock user1 account
- Unlock user1 account
- Delete group sample
<details><summary>show</summary>
<p>

```bash
sudo useradd user1
sudo usermod --expiredate 2020-12-31 user1
sudo usermod --shell /bin/bash user1
sudo groupadd developer
sudo usermod -aG developer user1
groups user1
id user1
sudo usermod --locl user1
sudo usermod --unlock user1
sudo groupdel sample
```

</p>
</details>

---

### setuid, setgid and sticky bit
- Set setuid on an executable file with execute permission.
- Set setgid on a directory
- Create `project` folder and and sticky bit
<details><summary>show</summary>
<p>

```bash
sudo setuid u+s executable
sudo setgid g+s common_folder
sudo mkdir /project
sudo chmod o+t /project
```

</p>
</details>

---

### Special File Attributes
- Create new text files named testing1.txt and testing2.txt
- Display special attributes for file testing.txt
- Add immutable attribute to testing1.txt
- Add append-only attribute to testing2.txt
- Test attributes by delete testing1 and adding new line to testing2
<details><summary>show</summary>
<p>

```bash
touch testing1.txt testing2.txt
sudo lsattr testing1.txt
sudo lsattr testing2.txt
sudo chattr +i testing1.txt
sudo chattr +a testing2.txt
```

</p>
</details>

---

### sudo
- update user1 to run apt-get update
- update user2 to run /bin/updatedb without password
- give developer group all access
<details><summary>show</summary>
<p>

```bash
visudo
# add the following lines
user1 ALL=/usr/bin/apt-get update
user2 ALL=NOPASSWD:/bin/upatedb
%developer ALL=(ALL) ALL
```

</p>
</details>

---

### PAM Pluggable Authentication Module
- Verify if `login` is using pam
- Verify if `top` is using pam
- Configure password to remember last 2 password
<details><summary>show</summary>
<p>

```bash
sudo ldd $(which login) | grep libpam
sudo ldd $(which top) | grep libpam
sudo vi /etc/pam.d/common-password
# add setting to line with password
remember=2
```

</p>
</details>

---

