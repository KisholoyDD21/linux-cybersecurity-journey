# Day 3 — Users, Permissions & sudo

## 1. Deep Dive: `ls -la` Output Breakdown

Example from home directory:

```
drwxr-xr-x 18 root root  4096 Oct  2 12:05 ..
```

**Breakdown:**

| Part | Meaning |
|------|---------|
| `d` | Directory |
| `rwx` | Owner permissions: read, write, execute |
| `r-x` | Group permissions: read, execute (no write) |
| `r-x` | Others permissions: read, execute (no write) |

> These permission concepts are critical in pentesting — full access is often required for exploitation and post-exploitation.

---

## 2. Special Permissions: `/tmp` Directory

```bash
ls -la /tmp/
```

Output shows `drwxrwxrwt`:

- `d` → Directory
- `rwx` → Owner full access
- `rwx` → Group full access
- `rwt` → Others: read, write, **sticky bit** (`t`)

The sticky bit (`t`) prevents users from deleting files they don't own, even with write access to the directory.

---

## 3. Changing File Permissions: `chmod`

Created a test file:

```bash
echo "Helloo" > hello.txt
cat hello.txt
# Helloo
```

Initial permissions: `-rw-r--r--` (644)

Changed to full access using numeric method:

```bash
chmod 777 hello.txt
```

Result: `-rwxrwxrwx` (777) — owner, group, and others all have read, write, execute.

**Numeric permission values:**

| Value | Permission |
|-------|------------|
| 4 | Read (r) |
| 2 | Write (w) |
| 1 | Execute (x) |
| 7 (4+2+1) | Read, Write, Execute |

---

## 4. Adding Users: `adduser`

```bash
adduser KISHOLOY
```

Prompts for password and user info (full name, room, phones, etc.).

Verify in `/etc/passwd`:

```bash
cat /etc/passwd
```

Output format:
```
username:x:UID:GID:GECOS:home:shell
```

Example:
```
KISHOLOY:x:1001:1001:Kisholoy,27,6001498558,7002611162:/home/KISHOLOY:/bin/bash
```

---

## 5. Password Hashes: `/etc/shadow`

```bash
cat /etc/shadow
```

Output for the new user:
```
KISHOLOY:$y$j9T$BGhP80q/Xt1ZiFS4wc8eE0$ytPCy5NHvvCPE25xMAKT5hnvavT9XMGv5n.NzqWCUX1:20730:0:99999:7:::
```

- The string after the username is the **hashed password** (yescrypt algorithm, indicated by `$y$`)
- These hashes can be cracked using tools like **hashcat** (offline password cracking)

> Never expose `/etc/shadow` — it's readable only by root.

---

## 6. Switching Users: `su`

```bash
su KISHOLOY
```

Switches to the user. Note: `SU ROOT` fails — commands are case-sensitive (`su root`).

---

## 7. Elevated Privileges: `sudo`

A regular user cannot change root's password:

```bash
passwd root
# passwd: You may not view or modify password information for root.
```

With `sudo`:

```bash
sudo passwd root
# [sudo] password for KISHOLOY:
# New password:
# Retype new password:
# passwd: password updated successfully
```

---

## 8. Granting sudo Access

As root:

```bash
su -
# Enter root password

usermod -aG sudo KISHOLOY
groups KISHOLOY
# KISHOLOY : KISHOLOY sudo
```

The `-aG` flag appends the user to the `sudo` group without removing existing groups.

---

## 9. Commands I Practiced

| Command | Description |
|---------|-------------|
| `ls -la` | Detailed listing with permissions |
| `ls -la /tmp/` | View sticky bit in action |
| `chmod 777 file` | Full permissions (numeric) |
| `adduser username` | Create new user interactively |
| `cat /etc/passwd` | List all users |
| `cat /etc/shadow` | View password hashes (root only) |
| `su username` | Switch user |
| `sudo command` | Run command as root |
| `usermod -aG sudo user` | Add user to sudo group |
| `groups user` | Check user's groups |

---

## What I Learned Today

- Permission triads: owner, group, others (`rwx`)
- Special bits: sticky bit (`t`) on `/tmp`
- Numeric `chmod` (4=read, 2=write, 1=execute)
- User management: `adduser`, `/etc/passwd`, `/etc/shadow`
- Password hashing (yescrypt) and hashcat relevance
- User switching: `su` vs `sudo`
- Granting sudo access via `usermod -aG sudo`