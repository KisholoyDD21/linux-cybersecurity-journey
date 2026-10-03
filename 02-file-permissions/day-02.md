# Day 2 — File Permissions & System Commands

## 1. Locate Command

The `locate` command finds files by name using a pre-built database.

```bash
locate bash
```

Before using `locate`, update the database:

```bash
updatedb
```

This updates the file index used by `locate`.

---

## 2. Changing Password

The `passwd` command changes the user password.

```bash
passwd
```

> Note: On Kali Linux, the default root password is typically `Unix` (or may vary by installation).

---

## 3. Manual Pages

**NAME:** `man` — an interface to the system reference manuals

Usage:

```bash
man ls
```

Alternative: Use `--help` flag with commands:

```bash
ls --help
```

---

## 4. Understanding `ls -la` Output

Example output:

```
┌──(root㉿kali)-[~]
└─# ls -la
total 88
drwx------  9 root root  4096 Oct  3 12:27 .
drwxr-xr-x 18 root root  4096 Oct  2 12:05 ..
-rw-r--r--  1 root root  5578 Jun 16 10:23 .bashrc
-rw-r--r--  1 root root   607 Jun 16 10:23 .bashrc.original
drwx------  5 root root  4096 Sep 13 05:50 .cache
-rw-r--r--  1 root root    57 Sep 13 05:54 cache
drwxr-xr-x  2 root root  4096 Oct  3 12:20 Downloads
-rw-r--r--  1 root root 11656 Jun 16 10:25 .face
lrwxrwxrwx  1 root root    11 Jun 16 10:25 .face.icon -> /root/.face
-rw-------  1 root root    20 Oct  3 12:27 .lesshst
drwxr-xr-x  4 root root  4096 Oct  2 12:21 linux-cybersecurity-journey
drwx------  3 root root  4096 Sep 13 05:25 .local
-rw-r--r--  1 root root   132 Jun 16 06:14 .profile
drwxr-xr-x  4 root root  4096 Sep 30 09:45 report.html
drwx------  2 root root  4096 Jun 16 10:23 .ssh
drwxr-xr-x  3 root root  4096 Sep 30 09:44 .wapiti
-rw-------  1 root root   404 Oct  2 11:40 .zsh_history
-rw-r--r--  1 root root 10882 Jun 16 10:23 .zshrc
```

### File Permission Breakdown

The leftmost characters indicate the file type and permissions:

| Character | Meaning |
|-----------|---------|
| `-` | Regular file |
| `d` | Directory |
| `l` | Symbolic link |

The next 9 characters represent permissions in three groups of three:

| Group | Position | Meaning |
|-------|----------|---------|
| Owner | 1-3 | `rwx` = read, write, execute |
| Group | 4-6 | `rwx` = read, write, execute |
| Others | 7-9 | `rwx` = read, write, execute |

**Examples from the output:**

- `drwx------` → Directory, owner has `rwx`, group and others have no permissions
- `-rw-r--r--` → Regular file, owner has `rw`, group has `r`, others have `r`
- `-rw-------` → Regular file, owner has `rw`, group and others have no permissions
- `lrwxrwxrwx` → Symbolic link, all have `rwx` (links typically show full permissions)

---

## 5. Commands I Practiced

### locate

Find files by name quickly:

```bash
locate bash
```

### updatedb

Update the locate database:

```bash
updatedb
```

### passwd

Change user password:

```bash
passwd
```

### man

View manual pages:

```bash
man ls
```

### ls -la

List all files with detailed permissions:

```bash
ls -la
```

### ls --help

Quick help for a command:

```bash
ls --help
```

---

## What I Learned Today

- `locate` and `updatedb` for fast file searching
- `passwd` for changing passwords
- `man` for reading manual pages
- `--help` flag for quick command help
- File type indicators: `-` (file), `d` (directory), `l` (link)
- Permission triads: owner, group, others
- `r` = read, `w` = write, `x` = execute
- How to interpret `ls -la` output