## Package Management
---

### Use low level package management
- install/upgrade a package using `dpkg`
- list installed packages
- check if `mysql` is installed
- find out which package installed a file
<details><summary>show</summary>
<p>

```bash
sudo dpkg -i file.deb
sudo dpkg -l
sudo dpkg --status mysql-common
sudo dpkg --search mysql-common
```

</p>
</details>

---

### Use high level package management
- search for mysql package
- install `mysql` package
- show information of the package
- remove package
- purge package
<details><summary>show</summary>
<p>

```bash
sudo apt-cache search mysql
sudo apt-get install mysql
sudo apt-cache show mysql
sudo apt-get remove mysql
sudo apt-get purge mysql
```

</p>
</details>

---
