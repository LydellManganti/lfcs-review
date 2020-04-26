## Configure and Troubleshoot GRUB

### GRUB
- navigate to "Simple Configuration" on `grub` info page.
- display list of OS in grub initial screen
- view `GRUB_DEFAULT` value in config
- set single-user in config

<details><summary>show</summary>
<p>

```bash
info -f grub -n 'Simple configuration'
awk -F\' '$1=="menuentry" {print $2} ' /boot/grub/grub.cfg
cat /etc/default/grub
# add the line in /etc/default/grub
GRUB_CMDLINE_LINUX="single"
sudo update-grub
```

</p>
</details>

---

### Fixing GRUB Issues
- enter a grub command line
- list available hard drive
- find grub folder in hard drive
- set configuration on the correct hard drive
- bring up grub menu and choose entry
- update grub and set device after boot

<details><summary>show</summary>
<p>

```bash
# press `c` to get a GRUB commandline
ls
ls (hd0,mdos1)/
# set configuration
set prefix=(hd0,msdos1)/grub2
set root=(hd0,msdos1)
insmod normal
normal
# choose an entry from grub menu
# after boot, execute the command
grub2-install /dev/xvda
```

</p>
</details>

---
