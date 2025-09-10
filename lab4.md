 ## SCRIPT : BACKUP.SH
```bash
#!/bin/bash
# backup.sh – Automates backup of .txt files with timestamp

# --- Setup Section ---
backup_dir="backup"
```

# Create backup folder if it doesn’t exist
```bash
mkdir -p "$backup_dir"
```

# --- Timestamp Section ---
```bash
timestamp=$(date +"%Y%m%d_%H%M%S")
```

# --- File Processing Section ---
```bash
echo " Searching for .txt files in current directory..."

count=0
for file in *.txt; do
    if [ -f "$file" ]; then
        new_name="${file%.txt}_$timestamp.txt"
        cp "$file" "$backup_dir/$new_name"
        echo "✅ Backed up: $file → $backup_dir/$new_name"
        ((count++))
    fi
done
```

# --- Result Section ---
```bash
if [ $count -eq 0 ]; then
    echo " No .txt files found to back up."
else
    echo " Backup completed! ($count files copied)"
fi
```

🔹 Don’t forget to give permission:
```bash
chmod +x backup.sh
```


---


#  File & Backup Automation

---

##  Objective
Automate file management by backing up all .txt files in the current directory with *timestamps* in their names.

---

## Script: backup.sh

### 🔹 How it Works
1. *Create Backup Folder* → If backup/ does not exist, it is created automatically.  
2. *Generate Timestamp* → Uses date +"%Y%m%d_%H%M%S" to ensure unique filenames.  
3. *Find & Copy Files* → Loops through all .txt files in the current directory.  
4. *Rename While Copying* → Appends timestamp to each file name before saving in backup/.  
5. *Show Results* → Prints messages for each copied file, or warning if no .txt found.  

---

##  Example Run

###  Step 1: Create Sample Files
```bash

$ echo "Hello" > a.txt 
$ echo "Backup script test" > notes.txt
 $ echo "Assignment data" > lab.txt
```

###  Step 2: Run Script
```bash

$ ./backup.sh  Searching for .txt files in current directory... 
 Backed up: a.txt → backup/a_20250908_142501.txt 
 Backed up: notes.txt → backup/notes_20250908_142501.txt 
 Backed up: lab.txt → backup/lab_20250908_142501.txt 
 Backup completed! (3 files copied)
```

###  Step 3: Check Backup Folder
```bash

$ ls
backup/ a_20250908_142501.txt 
notes_20250908_142501.txt 
lab_20250908_142501.txt
```
---

## Extra questions
1. What is the difference between 'cp', 'mv', and 'rsync'?
---
| Command | Function                            | Notes                                                           |
| ------- | ----------------------------------- | --------------------------------------------------------------- |
| `cp`    | Copies files/directories            | Source remains after copy                                       |
| `mv`    | Moves/renames files                 | Source is removed (move instead of copy)                        |
| `rsync` | Syncs files/directories efficiently | Supports incremental updates, compression, and remote transfers |
---


2. How can you schedule scripts to run automatically?
You can use cron jobs in Linux:

* Edit the crontab file:
```bash
crontab -e
```
* Add a line to schedule the script (e.g., run daily at 2 AM):
```bash

0 2 * * * /path/to/backup.sh
```
Use crontab -l to list current jobs.
