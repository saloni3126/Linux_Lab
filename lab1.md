# Linux Basic Commands

##  1. **Navigation Commands**

### `pwd` – Print Working Directory

Shows the current location in the filesystem.

```bash
pwd
```
![image](<Screenshot (7).png>)
---
Output example:
```
saloni@saloni:~/Documents/linux lab 2$ pwd
/home/saloni/Documents/linux lab 2

```

---

### `ls` – List Directory Contents

Lists files and folders in the current directory.

```bash
ls
```

* `ls -l` → Detailed list (permissions, size, date)
* `ls -a` → Shows hidden files (those starting with `.`)
* `ls -la` → Combined

output example:
```
saloni@saloni:~/Documents/linux lab 2$ ls
lab1.md

saloni@saloni:~/Documents/linux lab 2$ ls -l
total 4
-rw-rw-r-- 1 saloni saloni 2898 Sep  8 16:28 lab1.md

saloni@saloni:~/Documents/linux lab 2$ ls -la
total 12
drwxrwxr-x 2 saloni saloni 4096 Sep  8 16:28 .
drwxr-xr-x 7 saloni saloni 4096 Sep  8 16:09 ..
-rw-rw-r-- 1 saloni saloni 2898 Sep  8 16:28 lab1.md   

saloni@saloni:~/Documents/linux lab 2$ ls -la
total 12
drwxrwxr-x 2 saloni saloni 4096 Sep  8 16:28 .
drwxr-xr-x 7 saloni saloni 4096 Sep  8 16:09 ..
-rw-rw-r-- 1 saloni saloni 2898 Sep  8 16:28 lab1.md
saloni@saloni:~/Documents/linux lab 2$ ls -a
.  ..  lab1.md
```

![image](<Screenshot (8).png>)

---

### `cd` – Change Directory

Moves into a directory.

```bash
cd folder_name
```

Examples:

```bash
cd Documents        # Go to Documents
cd ..               # Go up one level
cd /                # Go to root
cd ~                # Go to home directory
```

---

## 2. **File and Directory Management**

### `mkdir` – Make Directory

Creates a new folder.

```bash
mkdir new_folder
```

---

### `touch` – Create File

Creates an empty file.

```bash
touch file.txt
```

---

### `cp` – Copy Files or Directories
this command copies files or directories.
```bash
cp source.txt destination.txt
```

* Copy folder:

```bash
cp -r folder1 folder2
```

---

### `mv` – Move or Rename Files

```bash
mv oldname.txt newname.txt
```

```bash
mv file.txt ~/Documents/     # Move file
```

---

### `rm` – Remove Files

```bash
rm file.txt          # Delete file
rm -r folder_name    # Delete folder (recursively)
```

 **Be careful!** There is no undo.

---

## 3. **File Viewing & Editing**

### `cat` – View File Contents

Displays content in terminal.

```bash
cat file.txt
```

---

### `nano` – Edit Files in Terminal

A basic terminal-based text editor.

```bash
nano file.txt
```

* Use arrows to move
* `CTRL + O` to save
* `CTRL + X` to exit

---

### `clear` – Clears the Terminal

```bash
clear
```

Shortcut: `CTRL + L`

---

## 4. **System Commands**

### `echo` – Print Text

Useful for debugging or scripting.

```bash
echo "Hello, World!"
```

---

### `whoami` – Show Current User

```bash
whoami
```

---

### `man` – Manual for Any Command

```bash
man ls
```

Use `q` to quit the manual.

---

## 5. **Searching and Finding**

### `find` – Locate Files

```bash
find . -name "*.txt"
```

Finds all `.txt` files in current folder and subfolders.

---

### `grep` – Search Inside Files

```bash
grep "hello" file.txt
```

 Searches for the word `hello` inside `file.txt`.

---

## 6. **Helpful Shortcuts**

| Shortcut   | Action                      |
| ---------- | --------------------------- |
| `Tab`      | Auto-complete files/folders |
| `↑ / ↓`    | Browse command history      |
| `CTRL + C` | Stop a running command      |
| `CTRL + L` | Clear screen                |

---
## File Permissions with `chmod` and `chown`

---

## 🔹 1. Understanding File Permissions in Linux

Each file/directory in Linux has:

* **Owner** → The user who created the file.
* **Group** → A group of users who may share access.
* **Others** → Everyone else.

### Permission Types

* **r** → Read (4 in numeric)
* **w** → Write (2 in numeric)
* **x** → Execute (1 in numeric)

### Permission Layout

Example from `ls -l`:

```
-rwxr-xr--
```
output:
```
total 8
-rw-rw-r-- 1 saloni saloni    0 Sep  8 16:34 file1.txt
drwxrwxr-x 2 saloni saloni 4096 Sep  8 16:33 file.txt
-rw-rw-r-- 1 saloni saloni 3623 Sep  8 16:33 lab1.md
```

Breakdown:

* `-` → Regular file (`d` = directory, `l` = symlink, etc.)
* `rwx` → Owner has read, write, execute
* `r-x` → Group has read, execute
* `r--` → Others have read only

---

## 🔹 2. `chmod` – Change File Permissions

### Syntax

```bash
chmod [options] mode filename
```
output:
```
saloni@saloni:~/Documents/linux lab 2$ touch file.sh
saloni@saloni:~/Documents/linux lab 2$ chmod 777 file.sh
saloni@saloni:~/Documents/linux lab 2$ ls -l
total 8
-rw-rw-r-- 1 saloni saloni    0 Sep  8 16:34 file1.txt
-rwxrwxrwx 1 saloni saloni    0 Sep  8 16:45 file.sh
drwxrwxr-x 2 saloni saloni 4096 Sep  8 16:33 file.txt
-rw-rw-r-- 1 saloni saloni 3623 Sep  8 16:33 lab1.md
saloni@saloni:~/Documents/linux lab 2$ chmod 700 file.sh
saloni@saloni:~/Documents/linux lab 2$ ls -l
total 8
-rw-rw-r-- 1 saloni saloni    0 Sep  8 16:34 file1.txt
-rwx------ 1 saloni saloni    0 Sep  8 16:45 file.sh
drwxrwxr-x 2 saloni saloni 4096 Sep  8 16:33 file.txt
-rw-rw-r-- 1 saloni saloni 3623 Sep  8 16:33 lab1.md
saloni@saloni:~/Documents/linux lab 2$ chmod 400 file.sh
saloni@saloni:~/Documents/linux lab 2$ ls -l
total 8
-rw-rw-r-- 1 saloni saloni    0 Sep  8 16:34 file1.txt
-r-------- 1 saloni saloni    0 Sep  8 16:45 file.sh
drwxrwxr-x 2 saloni saloni 4096 Sep  8 16:33 file.txt
-rw-rw-r-- 1 saloni saloni 3623 Sep  8 16:33 lab1.m
```
Modes can be set in **numeric (octal)** or **symbolic** form.

---

### (A) Numeric (Octal) Method

Each permission is represented as a number:

* Read = 4
* Write = 2
* Execute = 1

Add them up:

* `7 = rwx`
* `6 = rw-`
* `5 = r-x`
* `4 = r--`
* `0 = ---`

#### Example:

```bash
chmod 755 script.sh
```

Meaning:

* Owner: 7 → `rwx`
* Group: 5 → `r-x`
* Others: 5 → `r-x`

---

### (B) Symbolic Method

Use `u` (user/owner), `g` (group), `o` (others), `a` (all).
Operators:

* `+` → Add permission
* `-` → Remove permission
* `=` → Assign exact permission

#### Examples:

```bash
chmod u+x script.sh     # Add execute for owner
chmod g-w notes.txt     # Remove write from group
chmod o=r file.txt      # Set others to read only
chmod a+r report.txt    # Everyone gets read access
```

---

### (C) Recursive Changes

```bash
chmod -R 755 /mydir
```

* `-R` → applies changes recursively to all files/subdirectories.

---

##  3. `chown` – Change File Ownership

### Syntax

```bash
chown [options] new_owner:new_group filename
```

### Examples:

```bash
chown saloni file.txt           # Change owner to user 'vibhu'
chown saloni:dev file.txt       # Change owner to 'vibhu' and group to 'dev'
chown :saloni file.txt            # Change only group to 'dev'
chown -R saloni:dev /project    # Recursive ownership change
```

---

##  4. Putting It All Together

### Example Scenario

```bash
touch project.sh
ls -l project.sh
```

Output:

```
-rw-r--r-- 1 saloni 0 Aug 19 12:00 project.sh
```

Now:

```bash
chmod 700 project.sh       # Only owner has rwx
chmod u+x,g-w project.sh   # Add execute for user, remove write for group
chown root:admin project.sh # Change owner to root and group to admin
```

---

##  5. Quick Reference Table

| Numeric | Permission | Meaning      |
| ------- | ---------- | ------------ |
| 0       | ---        | No access    |
| 1       | --x        | Execute only |
| 2       | -w-        | Write only   |
| 3       | -wx        | Write + Exec |
| 4       | r--        | Read only    |
| 5       | r-x        | Read + Exec  |
| 6       | rw-        | Read + Write |
| 7       | rwx        | Full access  |

---

**Key Tip**: Use **numeric for quick settings** (e.g., 755, 644) and **symbolic for fine adjustments** (`u+x`, `g-w`).

---
### extra questions:
# 1.What is the difference between chmod and chown?
* Chmod: change the file permissions
controls who can read ,write ,or execute a file/directory.
works with permission bits (rwx=read,write,execute).
example:
```chmod 755 file.sh
```
- owner :read,write,execute
- group: read,execute
- others: read,execute


* chown :change file ownership
controls who owns the file (user and/or group).
example:
```
chown saloni:developers project.txt
```
user saloni becomes the owner
group developers becomes the group owner.


# 2.How do you check current directory and user?
* to check the current directory we use the 'pwd' command
example output:
```
/home/saloni/~documents/~linux lab
```

* to check the current user we use 'whoami' or 'echo $USER' command
example output:
```
saloni
```
---
