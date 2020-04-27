## Process Resource Usage and Limits Per User Basis

### Systat
- install `sysstat`
- display processor statistics 5 times with 3 second interval
- display processor 0 5 times with 3 second interval

<details><summary>show</summary>
<p>

```bash
sudo apt-get install sysstat
sudo mpstat -P ALL -u 3 4
sudo mpstat -P 0 -u 3 4
```

</p>
</details>

---

### Reporting Linux Proceses
- list processes information (pid, ppid, cmd, %cpu, %mem) and sort by cpu usage
- identify the pid of the highest cpu, use the pid to navigate to `/proc`
- display io statistics for the process, security attributes, and cgroup
- identify if cgroups is enabled
- display filedescriptor for the pid

<details><summary>show</summary>
<p>

```bash
ps -eo pid,ppid,cmd,%cpu,%mem --sort=-%cpu
sudo ls /proc/[id]/io
sudo ls /proc/[id]/attr/current
sudo ls /proc/[id]/cgroup
sudo cat /boot/config-$(uname -r) | grep -i cgroups
sudo ls -l /proc/[id]/fd
```

</p>
</details>

---

### Settring Resource Limits on a Per User Basis
- set limit to number of processes a user can start
- set 1Gb memory limit per process
- set total memory limit per user

<details><summary>show</summary>
<p>

```bash
sudo vi /etc/security/limits.conf
# add the lines for per process limit
user hard nproc 10
user hard as 1000000
# per user total limit
sudo vi /etc/cgconfig.conf
# add the lines
group memlimit {
    memory {
        memory.limit_in_bytes = 4000000000
    }
}
sudo vi /etc/cgrules.conf
# add the line
user memory memlimit/
```

</p>
</details>

---

### Other management tools
- decrease the priority of a pid running
- kill a process
- kill all process of a user
- kill all process of a group
- kill all sub process of a ppid
- list all process of a user

<details><summary>show</summary>
<p>

```bash
sudo renice -n 1 -p pid
sudo kill 123
sudo pkill -u user
sudo pkill -G mygroup
sudo pkill -P 123
sudo pgrep -l -u user
```

</p>
</details>

---
