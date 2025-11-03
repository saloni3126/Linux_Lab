# LOGIN USING SECURE SHELL SCRIPTING ON A SAME NETWORK 
## OVERVIEW
this document explains, in detail, how to *securely transfer files* between two linux systems using *secure shell (SSH)* and related tools(scp,rsync,and sftp)
all commands in thid guide are *tested on ubuntu* but apply to most linux distributions.
 
 ---
## 1. Prerequisites

Before starting, ensure you have the following:

* Two Linux/Ubuntu machines:

  * *Local Machine (Sender):* where your files currently are.
  * *Remote Machine (Receiver):* the system that will receive the files.
* Network connection between the two systems (they must be reachable by IP or hostname).
* User credentials (username and password or SSH key).
* Administrative (sudo) privileges on both machines.

---

## 1. *SSH client: remote login syntax*
to initiate a remote SSH session fron a client machine:
```bash
ssh [user]@[host] [-p port]
```
* user: remote system username
* host:IP adress or FQDN of remote host
* -p port: (optional)non default SSH port

*example*
``` bash
ssh saloni@10.0.15.2
```
---
## 2. *Install and configure OpenSSH server(on Ubuntuhost)*
SSH allows secure communication between systems
## install SSH server pakage
```bash
sudo apt updte
sudo apt install -y openssh-server
```
---

## Enable and start the ssh daemon
```bash
sudo systemctl enable ssh
sudo systemctl start ssh
```
## verify SSH service status 
```bash
sudo systemctl status ssh
```
If it shows *“active (running)”*, SSH is working.

----
## Find the remote machine’s IP address
On the *receiver PC*, run:
```bash
ip addr show
```
or
```bash
hostname -I
```
Copy the IP address — you’ll use it to connect later (e.g., 192.168.1.10).

---

## 3. *SSH Key-Based Authentication(recommended)*
Using SSH keys is more secure than passwords.
### Step 1: Generate an SSH key on the *local (sender)* machine

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
When prompted:

* Press *Enter* to accept default file location (~/.ssh/id_ed25519).
* Optionally, set a passphrase for extra security.

### Step 2: Copy your public key to the *remote machine*

```bash
ssh-copy-id username@192.168.1.10
```

Replace username and IP with your actual details.

If ssh-copy-id isn’t available:

```bash
cat ~/.ssh/id_ed25519.pub | ssh username@192.168.1.10 "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```
### Step 3: Test the SSH connection

```bash
ssh username@192.168.1.10
```

 You should log in without typing your password.

---

##  4. Transfer Files Securely with scp

### Copy a file from local → remote

```bash
scp /path/to/local/file.txt username@192.168.1.10:/path/to/remote/
```

### Copy a folder recursively

```bash
scp -r /path/to/local/folder username@192.168.1.10:/path/to/remote/
```

### Copy from remote → local

```bash
scp username@192.168.1.10:/path/to/remote/file.txt /local/destination/
```
### Use a custom SSH port

If SSH runs on port 2222 instead of 22:

```bash
scp -P 2222 file.txt username@192.168.1.10:/remote/path/
```

---

##  5. Efficient Transfers with rsync (Recommended for large or repeated syncs)

rsync is faster and can resume interrupted transfers.

### Step 1: Install rsync

```bash
sudo apt install rsync -y
```
### Step 2: Basic file transfer

```bash
rsync -avz /local/path/ username@192.168.1.10:/remote/path/
```

*Options explained:*

* -a → archive mode (preserves permissions, timestamps)
* -v → verbose
* -z → compress data
* --progress → show transfer progress

### Step 3: Resume interrupted transfers

```bash
rsync -avzP /local/largefile.iso username@192.168.1.10:/remote/path/
```

(-P = partial + progress)

### Step 4: Sync and delete removed files
```bash
rsync -avz --delete /local/dir/ username@192.168.1.10:/remote/dir/
```

*Be cautious:* Files deleted locally will also be deleted remotely.

---
##  6. Use sftp for Interactive File Management

### Connect via SFTP

```bash
sftp username@192.168.1.10
```

### Common SFTP commands

| Command       | Description                   |
| ------------- | ----------------------------- |
| ls          | List remote files             |
| pwd         | Show remote working directory |
| cd <dir>    | Change remote directory       |
| lcd <dir>   | Change local directory        |
| put <file>  | Upload file                   |
| get <file>  | Download file                 |
| mkdir <dir> | Create remote directory       |
| exit        | Close SFTP session            |

---
##  7. Mount Remote Directory Locally using sshfs

This lets you work with remote files as if they were local.

### Step 1: Install sshfs

```bash
sudo apt install sshfs -y
```

### Step 2: Mount remote folder

```bash
mkdir -p ~/mnt/remote
sshfs username@192.168.1.10:/remote/path ~/mnt/remote
```

Now, navigate to ~/mnt/remote and access remote files.

### Step 3: Unmount

```bash
fusermount -u ~/mnt/remote
```

---

##  8. Transfer with tar over SSH (streaming)

This avoids creating temporary archives.

### Send folder and extract directly on remote

bash
tar -czf - /local/dir | ssh username@192.168.1.10 "tar -xzf - -C /remote/dir"

### Copy remote → local

```bash
ssh username@192.168.1.10 "tar -czf - -C /remote/dir ." > local-backup.tar.gz
```

---

##  9. Verify File Integrity

To ensure no corruption during transfer:

### Step 1: Generate checksum on local

```bash
sha256sum file.tar.gz > file.tar.gz.sha256
```

### Step 2: Copy both files

```bash
scp file.tar.gz file.tar.gz.sha256 username@192.168.1.10:/remote/path/
```
### Step 3: Verify checksum on remote

```bash
cd /remote/path
sha256sum -c file.tar.gz.sha256
```

Output should say:
file.tar.gz: OK

---
##  10. Troubleshooting Tips

| Issue                           | Possible Cause                 | Solution                             |
| ------------------------------- | ------------------------------ | ------------------------------------ |
| Connection refused            | SSH server not running         | sudo systemctl start ssh           |
| Permission denied (publickey) | Wrong key permissions          | chmod 600 ~/.ssh/id_ed25519        |
| Transfer too slow               | No compression / Wi-Fi latency | Use rsync -avz --progress          |
| Host not reachable              | Network or firewall issue      | Check IP, ping, or ufw allow ssh |

---
##  11. Security Best Practices

1. Use *SSH keys* instead of passwords.
2. Disable password authentication once key-based login works:

```bash
   sudo nano /etc/ssh/sshd_config
   ```

   Set:

   
   PasswordAuthentication no
   

   Then restart:

   ```bash
   sudo systemctl restart ssh
   ```
3. Use ed25519 keys for better security.
4. Protect private keys:
```bash
   chmod 600 ~/.ssh/id_ed25519
   ```
   ---
5. Use a firewall to allow only trusted IPs:

```bash
   sudo ufw allow from <your_local_ip> to any port 22
   ```

---

##  12. Quick Command Reference

| Action           | Command                               |
| ---------------- | ------------------------------------- |
| Generate SSH key | ssh-keygen -t ed25519               |
| Copy SSH key     | ssh-copy-id user@remote             |
| Transfer file    | scp file.txt user@remote:/path/     |
| Transfer folder  | scp -r folder user@remote:/path/    |
| Sync files       | rsync -avz /src/ user@remote:/dest/ |
| Mount remote dir | sshfs user@remote:/dir ~/mnt/remote |
| Unmount          | fusermount -u ~/mnt/remote          |

---

##  Summary

| Tool        | Description                 | Use Case                       |
| ----------- | --------------------------- | ------------------------------ |
| *SSH*     | Secure connection           | Login remotely                 |
| *SCP*     | Simple copy tool            | Quick transfers                |
| *RSYNC*   | Smart synchronization       | Large or repeat transfers      |
| *SFTP*    | Interactive file management | Manual uploads/downloads       |
| *SSHFS*   | Mount remote directory      | Work with remote files locally |
| *TAR+SSH* | Stream compressed data      | Backup / large folders         |

----





#  Secure File Transfer Between Ubuntu Systems Over SSH (Across Different Networks)

---

##  Overview

Transferring files securely between two Ubuntu systems on **different networks** (e.g., your home computer and a remote server) is best achieved using **SSH (Secure Shell)**.

SSH ensures:
-  **Encryption** of data during transmission
-  **Credential protection**
-  **Authenticated and tamper-proof** connections

This guide provides a **deep technical breakdown** of SSH-based file transfer tools (`scp`, `rsync`, `sftp`) and explains **how SSH encryption works internally**.

---

##  Understanding SSH Communication

SSH creates a **secure tunnel** between systems using **encryption and key exchange** to ensure confidentiality and integrity.

###  SSH Handshake and Encryption Flow

```
┌──────────────┐                                   ┌────────────────────┐
│ Local Client │                                   │ Remote SSH Server  │
└──────┬───────┘                                   └────────┬───────────┘
       │ 1️. Client initiates SSH connection on port 22             │
       ├─────────────────────────────────────────────────────────────►│
       │                                                             │
       │ 2️. Server responds with supported SSH versions & algorithms │
       ◄──────────────────────────────────────────────────────────────┤
       │                                                             │
       │ 3️. Key Exchange (Diffie-Hellman or Elliptic Curve)         │
       ├─────────────────────────────────────────────────────────────►│
       │                                                             │
       │ 4️. Server sends host key; client verifies it                │
       ◄──────────────────────────────────────────────────────────────┤
       │                                                             │
       │ 5️. User Authentication (Password or Public Key)             │
       ├─────────────────────────────────────────────────────────────►│
       │                                                             │
       │ 6️. Encrypted Channel Established                          │
       ▼                                                             ▼
┌──────────────────┐                                 ┌──────────────────┐
│ Data Transmission│ ←────── Encrypted via SSH ─────→│ Data Reception   │
└──────────────────┘                                 └──────────────────┘
```

###  Encryption Details

- **Symmetric Encryption:** AES-256 or ChaCha20
- **Asymmetric Authentication:** RSA or Ed25519
- **Hashing:** SHA-2 for integrity
- **Key Exchange:** Diffie-Hellman or ECDH

---

##  Prerequisites

### 1. Install and Enable SSH

On **both systems**:

```bash
sudo apt update
sudo apt install openssh-client openssh-server -y
sudo systemctl enable ssh
sudo systemctl start ssh
```

### 2. Configure Firewall

```bash
sudo ufw allow 22/tcp
sudo ufw enable
sudo ufw status
```

### 3. Obtain Remote IP

On the remote server:

```bash
curl ifconfig.me
```

Example output:

```
203.0.113.50
```

---

##  File Transfer Methods

###  Method 1: `scp` (Secure Copy)

**Syntax:**

```bash
scp [options] <source> <user>@<remote_host>:<destination>
```

####  Upload

```bash
scp /home/alice/file.txt ubuntu@203.0.113.50:/home/ubuntu/
```

####  Download

```bash
scp ubuntu@203.0.113.50:/home/ubuntu/data.zip /home/alice/
```

####  Copy Directory

```bash
scp -r /home/alice/docs ubuntu@203.0.113.50:/home/ubuntu/backups/
```

**Options:**

| Option      | Description                    |
| ----------- | ------------------------------ |
| `-r`        | Recursive copy for directories |
| `-P <port>` | Specify non-default SSH port   |
| `-v`        | Verbose output for debugging   |

---

###  Method 2: `rsync` (Efficient Incremental Transfer)

Uses SSH for transport and syncs only changed data.

####  Upload

```bash
rsync -avz -e "ssh -p 22" /home/alice/project/ ubuntu@203.0.113.50:/home/ubuntu/project_backup/
```

####  Download

```bash
rsync -avz -e "ssh -p 22" ubuntu@203.0.113.50:/home/ubuntu/data/ /home/alice/local_data/
```

**Flags:**

| Flag     | Meaning                                         |
| -------- | ----------------------------------------------- |
| `-a`     | Archive mode (preserves permissions, ownership) |
| `-v`     | Verbose mode                                    |
| `-z`     | Compress data during transfer                   |
| `-e ssh` | Use SSH as transport layer                      |

**Advantages:**
- Transfers only changed parts
- Faster on repeated syncs
- Supports resume if interrupted

---

###  Method 3: `sftp` (SSH File Transfer Protocol)

Interactive shell for file management.

**Start Session:**

```bash
sftp ubuntu@203.0.113.50
```

**Example Commands:**

```
sftp> ls
sftp> cd /home/ubuntu
sftp> put /home/alice/file.txt
sftp> get remote_data.zip
sftp> exit
```

---

##  SSH Key-Based Authentication (Recommended)

### 1. Generate Key Pair

```bash
ssh-keygen -t rsa -b 4096 -C "alice@example.com"
```

### 2. Copy Key to Remote Host

```bash
ssh-copy-id ubuntu@203.0.113.50
```

Now connect without a password:

```bash
ssh ubuntu@203.0.113.50
```

---

##  Network Example

| Role   | Machine         | IP Address     | Username | Description   |
|--------|-----------------|----------------|----------|---------------|
| Client | Alice’s Laptop  | `192.168.1.20` | `alice`  | File sender   |
| Server | Ubuntu Cloud VM | `203.0.113.50` | `ubuntu` | File receiver |

**Command Example:**

```bash
scp /home/alice/report.pdf ubuntu@203.0.113.50:/home/ubuntu/reports/
```

Output:

```
report.pdf                              100%   10MB  12.5MB/s   00:00
```

---

##  Security Hardening Tips

1. **Change SSH Port**

```bash
sudo nano /etc/ssh/sshd_config
# Change: Port 22 → Port 2222
sudo systemctl restart ssh
```

2. **Disable Root Login**

```bash
PermitRootLogin no
```

3. **Install Fail2Ban**

```bash
sudo apt install fail2ban
```

4. **Use Key Authentication Only**

```bash
PasswordAuthentication no
```

---

##  Troubleshooting Guide

| Error                           | Cause               | Solution                                        |
|--------------------------------|---------------------|-------------------------------------------------|
| `Connection timed out`         | Port blocked        | `sudo ufw allow 22/tcp`                         |
| `Permission denied (publickey)`| Missing SSH key     | Run `ssh-copy-id`                               |
| `Host key verification failed` | Changed server key  | `ssh-keygen -R 203.0.113.50`                    |
| `Broken pipe`                  | Idle timeout        | Add `ServerAliveInterval 60` in `~/.ssh/config` |

---

##  Comparison Summary

| Method  | Encryption | Resume  | Interactive | Efficiency | Ideal Use              |
|---------|------------|---------|-------------|------------|------------------------|
| `scp`   | ✅          | ❌       | ❌           | Simple     | One-time transfers     |
| `rsync` | ✅          | ✅       | ❌           | Very high  | Incremental syncs      |
| `sftp`  | ✅          | Partial | ✅           | Medium     | Manual file management |

---

##  Visualization: Encrypted Data Flow

```
┌───────────────────────────────┐        ┌───────────────────────────────┐
│ Local Ubuntu (Client)         │        │ Remote Ubuntu (Server)        │
│  ┌────────────┐               │        │  ┌────────────┐               │
│  │ File.txt   │               │        │  │ SSH Daemon │               │
│  └────┬───────┘               │        │  └────┬───────┘               │
│  AES/ChaCha20 Encryption    │  TCP/22 │  Decryption  via Session Key│
│       │──────────────────────▶│────────▶│                              │
│       ▼                       │        │  Verified by Host Key & Hash  │
│  Data Sent Securely           │        │  Data Written to /home/ubuntu │
└───────────────────────────────┘        └───────────────────────────────┘
```

---

##  References

- [OpenSSH Documentation](https://www.openssh.com/manual.html)
- [Ubuntu SSH Server Guide](https
