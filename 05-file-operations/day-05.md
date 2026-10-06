# Day 5 — Viewing, Creating & Editing Files

## 1. Creating Files

### touch

Creates an empty file (or updates timestamp if file exists).

```bash
touch newfile.txt
```

**Example:**

```bash
┌──(root㉿kali)-[~]
└─# touch newfile.txt

┌──(root㉿kali)-[~]
└─# ls
cache  Downloads  hello.txt  linux-cybersecurity-journey  newfile.txt  report.html
```

---

### echo with `>` (overwrite)

Writes output to a file, **overwriting** existing content.

```bash
echo "Pentest" > pentest.txt
```

**Example:**

```bash
┌──(root㉿kali)-[~]
└─# echo "Pentest" > pentest.txt

┌──(root㉿kali)-[~]
└─# ls
cache  Downloads  hello.txt  linux-cybersecurity-journey  newfile.txt  pentest.txt  report.html
```

**Overwrite behavior:**

```bash
┌──(root㉿kali)-[~]
└─# echo "File was created" > newfile.txt

┌──(root㉿kali)-[~]
└─# echo "Pentesting" > pentest.txt
┌──(root㉿kali)-[~]
└─# cat pentest.txt
Pentesting
```

> The second `>` replaced "Pentest" with "Pentesting".

---

### echo with `>>` (append)

Appends output to a file **without overwriting**.

```bash
echo "pentest" >> pentest.txt
```

**Example:**

```bash
┌──(root㉿kali)-[~]
└─# echo "pentest" >> pentest.txt
┌──(root㉿kali)-[~]
└─# cat pentest.txt
Pentesting
pentest
```

---

## 2. Viewing Files

### cat

Displays entire file content.

```bash
cat newfile.txt
```

**Example:**

```bash
┌──(root㉿kali)-[~]
└─# cat newfile.txt
File was created
```

---

## 3. Editing Files

### nano — Terminal-based editor

Simple, beginner-friendly editor.

```bash
nano newfile.txt
```

**Key shortcuts:**
- `Ctrl+O` — Write (save)
- `Ctrl+X` — Exit
- `Ctrl+K` — Cut line
- `Ctrl+U` — Paste line
- `Ctrl+W` — Search

---

### gedit — GUI-based editor

Graphical text editor (requires display environment).

```bash
gedit newfile.txt
```

Opens a window similar to Notepad on Windows.

---

## 4. Commands I Practiced

| Command | Description |
|---------|-------------|
| `touch file` | Create empty file |
| `echo "text" > file` | Write text to file (overwrite) |
| `echo "text" >> file` | Append text to file |
| `cat file` | View file contents |
| `nano file` | Edit file in terminal |
| `gedit file` | Edit file in GUI |

---

## What I Learned Today

- `touch` for quick empty file creation
- `>` vs `>>` redirection — overwrite vs append
- `cat` for viewing file contents
- `nano` as a terminal editor (always available)
- `gedit` as a GUI editor (when display is available)
- Redirection operators are fundamental for scripting and automation