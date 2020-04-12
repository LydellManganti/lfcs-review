### Install AppArmor Utility package
<details><summary>show</summary>
<p>

```bash
sudo apt-get update
sudo apt-get install apparmor-utils
```
</p>
</details>

---
### Display status of AppArmor
<details><summary>show</summary>
<p>

```bash
sudo apparmor_status
```
</p>
</details>

---
### Display profiles from apparmor directory
<details><summary>show</summary>
<p>

```bash
ls -lah /etc/apparmor.d
```
</p>
</details>

---
### Set `usr.sbin.tcpdump` status to complain
<details><summary>show</summary>
<p>

```bash
sudo aa-complain /etc/apparmor.d/usr.sbin.tcpdump
```
</p>
</details>

---
### Set `usr.sbin.tcpdump` status to enforce
<details><summary>show</summary>
<p>

```bash
sudo aa-enforce /etc/apparmor.d/usr.sbin.tcpdump
```
</p>
</details>

---
### Set all profiles to complain, then to enforce
<details><summary>show</summary>
<p>

```bash
sudo aa-complain /etc/apparmor.d/*
sudo aa-enforce /etc/apparmor.d/*
```
</p>
</details>

---
### Disable `usr.sbin.tcpdump` profile
<details><summary>show</summary>
<p>

```bash
sudo ln -s /etc/apparmor.d/usr.sbin.tcpdump /etc/apparmor.d/disable/
```
</p>
</details>
