# Day 1 — Linux Basics

## 1. Getting Root Access

```bash
sudo su -
```

This switches to the root user.

> Note: `sudo -i` is another common way to start a root login shell.

---

## 2. Understanding the Terminal Prompt

Example:

```
kali@kali:~$
```

- First `kali` → username
- Second `kali` → hostname
- `~` → current user's home directory
- `$` → normal user
- `#` → root user

Example root prompt:

```
root@kali:~#
```

---

## 3. Linux Directory Structure

A normal user's home directory is:

```
/home/kali
```

The `/` directory is the root of the entire Linux filesystem.

To check the current directory:

```
pwd
```

To move one directory upward:

```
cd ..
```

For example:

```
/home/kali
    ↓ cd ..
/home
```

---

## 4. Commands I Practiced

### pwd

Shows the current working directory.

```
pwd
```

### cd

Changes the current directory.

```
cd Desktop
```

### cd ..

Moves one directory up.

```
cd ..
```

### ls

Lists files and directories.

```
ls
```

### ls -la

Shows hidden files along with detailed file information.

```
ls -la
```

### echo

Prints text to the terminal.

```
echo "Hello Linux"
```

---

## 5. Creating a File

I created a file using:

```
echo "Starting my linux journey" > msg.txt
```

The `>` operator redirects the output into a file.

I then checked the directory:

```
ls
```

The file appeared as:

```
msg.txt
```

---

## 6. Removing a File

I removed the file using:

```
rm msg.txt
```

Then verified it with:

```
ls
```

The file was no longer present.

---

## 7. Moving a File

I practiced moving a file into the `.cache` directory:

```
mv words.txt .cache
```

This moves `words.txt` into `.cache`.

---

## What I Learned Today

- Linux users and hostnames
- Root vs normal users
- Linux directory structure
- `/` as the filesystem root
- `/home/kali` as my user's home directory
- `pwd`
- `cd`
- `ls`
- `ls -la`
- `echo`
- `rm`
- `mv`
- Output redirection using `>`