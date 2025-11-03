
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
