## Troubleshooting Tools

---

### Diskspace usage
- Display disk usage in human readable space
- Display Inodes usage
<details><summary>show</summary>
<p>

```bash
df -h
df -hTi
```
</p>
</details>

---

### Find Files
- Find empty files
- Find empty directories
- Delete empty files
- Delete empty directories
<details><summary>show</summary>
<p>

```bash
find /home -type f -empty
find /home -type f -empty --delete
find /home -type d -empty
find /home -type d -empty --delete
```
</p>
</details>

---

### Display Directory size
- Display directory sizes
<details><summary>show</summary>
<p>

```bash
du -sch /var/*
```
</p>
</details>

---

### Display CPU and memory usage
- display running process
    <details><summary>show</summary>
<p>

```bash
top
free -h
ps -ef
ps -eo user,comm,pid,ppid,%mem --sort -%mem

```
</p>
</details>

---


### Running a process in the background
- Start process in the backround
- Pause a running process and continue in the background
<details><summary>show</summary>
<p>

```bash
process &
Ctrl + Z
kill -18 PID
```
</p>
</details>

---
