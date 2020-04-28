## Kernel Runtime Parameters

### Sysctl
- Display all kernel runtime parameters
- The result correspond to the directory in `/proc/sys`
- View `dev.cdrom.autoclose` parameter
- Update `ip_forward=0`
- Apply the changes to the running configuration
<details><summary>show</summary>
<p>

```bash
sudo sysctl -a | less
sudo sysctl dev.cdrom.autoclose
# update
echo 0 | sudo tee /proc/sys/net/ipv4/ip_forward
sudo sysctl -w net.ipv4.ip_forward=0
# apply
sudo sysctl -p
```

</p>
</details>

---

### Useful Kernel Parameters
- Display maximum number of file handles
- Display Sysrq
- Set ignore ping and drop at the kernel level
- Set ipv4.ip_forward=0 in `conf.d`
-
<details><summary>show</summary>
<p>

```bash
sudo sysctl fs.file-max
sudo sysctl kernel.sysrq
echo "net.ipv4.icmp_echo_ingore_all=1" | sudo tee -a /etc/sysctl.d/net.conf
echo "net.ipv4.ip_forward=0" | sudo tee -a /etc/sysctl.d/net.conf
```

</p>
</details>

---
