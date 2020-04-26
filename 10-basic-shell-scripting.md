## Basic Shell Scripting

### Shell Scripting
- Create a shell script to test the services below if they are active
- `sshd`, `httpd`, `crond`, `firewalld`, `mariadb`
- Set the list of services in a txt file (services.txt)

<details><summary>show</summary>
<p>

```bash
#!/bin/bash

for services in $(cat services.txt); do
    sudo systemctl status $service | grep --quiet "running"
    if [ $? -eq 0 ]; then
        echo $service "is Active"
    else
        echo $service "is Inactive or Not Installed"
    fi
done
```

</p>
</details>

- Update the script to verify the existence of `services.txt`
---

### Filesystem Troubleshooting
- Run fsck on xvdb device
- Display issue of xvdb device
- Fix issues with xvdb device
- Force to automatically repair xvdb device

<details><summary>show</summary>
<p>

```bash
sudo umount /dev/xvdb
sudo fsck -n /dev/xvdb
sudo fsck -y /dev/xvdb
sudo fsck -af /dev/xvdb
```

</p>
</details>
