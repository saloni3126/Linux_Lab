## what is secure shell scripting?
---
**ssh(secure shell)**
it is a cryptographical network protocol used to scurely access the network services over an unsecured network .it provides a secure channel between a client and a server, enabling encrypted communication for remote login and other secure network services.
***
### key features:
* ***Encryption**:all dat atransferred is encrypted ,preventing eavesdroping.
* **Authentication**: Supports password and public key authentication.
- **Integrity**: Ensures data is not tampered with during transmission.
- **Port Forwarding**: Allows tunneling of other protocols through SSH.

### purpose:
* secure remote login to servers
*  encrypted communication ensures confidentiality
*  supports secure file transfers and tunneling
*  widely supported and standardized
---
## SSH Protocol Versions

- *SSH-1:* The first version of SSH, now obsolete and insecure.
- *SSH-2:* Current, secure version with stronger encryption and better features.

***ssh** replaces insecure protocols like *telnett*  and *rlogin*,which transmit data (including passwords) in plaintext.
  ***
  ## Why Do We Need an Open SSH Client?

An *SSH client* is the tool on your computer that lets you securely connect to and control a remote machine over the network.

### Key reasons we need it:

- *Secure remote access:* Connect safely to servers with encrypted communication.  
- *Run remote commands:* Manage servers as if you are physically present.  
- *File transfer:* Copy files securely using scp or sftp.  
- *Port forwarding:* Create secure tunnels to access services behind firewalls.  
- *Automation:* Run remote tasks via scripts without manual login.

The *OpenSSH Client* is a popular, open-source SSH tool available on most Linux/Unix systems, ensuring secure and compatible connections.

---
Without an SSH client, you cannot start a secure remote session, making it essential for remote administration and secure communication.


  ---
## SSH Client : Remote Login Syntax
**to access the remote linux system securely , use the SSH client command :**
````bash
ssh[user]@[host] [-p port]
````
```user```: the username on the remote machine

```host```: the IP address or fully qualified domain name (FQDN) of the remote machine

```-p port```: optional flag to specify a custom SSH port *(default is 22)*

---
### example:
```bash
ssh saloni@10.0.2.15
```
**this command initiates an encrypted connection to the remote host as user ```saloni```

## 2. Installing and Configuring OpenSSH server on Ubuntu
 To accept SSH connections,the remote host must run the ssh server daemon

 ### Install OpenSSh server package:
 ```bash
 sudo apt update
 sudo apt install -y openssh-server
 ```
### Enable and start the SSH service:
```bash
suo systemctl enable ssh
sudo systemctl start ssh
```
### Verify that the SSh is running :
```bash
sudo systeemctl status ssh 
```
*you should see ```active(running)``` status*

## 3. SSH Key-Based authentication (Recommended):
using passwords is less secure and more error prone 
. SSHkey-based authentication uses cryptographic key pairs to authenticate without sending passwwords.
## Generating public key to the remote host:
```bash
ssh-copy-id [user]@[host]
```
*or manually*
```bash
cat ~/ .ssh/id_rsa.pub | ssh [user]@[host] 'mkdir -p ~/.ssh&&cat>> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys'
```

### Why Key-Based authentication?
* it prevents brute-force passwords attacks
* allows automated and passwordless login (useful for scripts)
8more secure and convenient.
***
### test Key-Based login:
```bash
ssh[user]@[host]
```
## Quick summary of available access methods

1. *Direct SSH login as that user* (recommended when user account exists and SSH keys/passwords are set):  
   ssh USERNAME@SERVER_IP

2. *SSH in as a different user, then switch to that user* (requires sudo privileges):  
   - ssh admin@SERVER_IP  
   - sudo su - USERNAME  or sudo -i -u USERNAME

3. *Key-based setup if keys are not yet present* (server-side or client-side options).

4. **Using ssh-copy-id from client** to install a public key into the target user’s ~/.ssh/authorized_keys.

---
## 4. File Transfer with SCP and SFTP
SSH also supports secure file transfer protocols:
## SCP(secure copy)
### What is scp?

scp stands for *Secure Copy Protocol* (or Secure Copy Program).
It’s a *command-line tool* used to *copy files and directories* between systems *securely over a network, using **SSH (Secure Shell)* for authentication and data transfer.
* So in essence: scp = cp (copy) + ssh (security l
* command-line tool for copying files securely between hosts over SSH.
* uses the same encryption and authentication as SSH

### How scp Works (Step-by-Step)

1. **You run an scp command** specifying:

   * The *source* (file/folder to copy)
   * The *destination* (where to send it)
   * Optionally, the *remote username* and *host (IP or domain)*

2. **scp connects via SSH** to the remote host:

   * It uses port *22* (default SSH port).
   * It authenticates the user using a *password* or *SSH key*.

3. **Data transfer begins securely**:

   * All data (file contents, filenames, and credentials) is *encrypted*.
   * Transfer integrity is maintained — the data cannot be intercepted or tampered with easily.
   4. **Transfer completes**:

   * The file ends up on the remote system (or locally, if copying from remote).

---


### syntax:
```bash
scp [option] source_file [user]@[host]:destination_path
```
Both <source> and <destination> can be *local paths* or *remote locations* of the form:
```bash
user@host:/path/to/file
```

### example:
copy a file from local machine to remote :
```bash
scp ~/documents/report.pdf saloni@10.0.2.15:/home/saloni/
```
This:

* Connects to host 192.168.1.20 as user user
* Copies myfile.txt from your local machine
* Saves it in /home/user/ on the remote host

---

copy a file from remote to local:
```bash
scp saloni@10.0.2.15:/home/saloni/log.txt ~/downloads/
```
This copies the file from the remote machine to your local Documents folder.

---
Copy directory recursively*

bash
scp -r ~/Projects user@192.168.1.20:/home/user/


The -r flag means “recursive” — it copies all files and subdirectories.

---
Specify a different SSH port*

If the remote host uses a custom port (not 22):

bash
scp -P 2222 file.txt user@192.168.1.20:/tmp/


> Note: -P (uppercase) — not -p — sets the port.

---
Copy between two remote hosts*

bash
scp user1@host1:/path/file user2@host2:/path/


This copies directly from host1 to host2 via your local machine (data may pass through your client).

---
### Commonly Used Options

| Option             | Meaning                                              |
| ------------------ | ---------------------------------------------------- |
| -r               | Copy directories recursively                         |
| -P port          | Use a different SSH port                             |
| -p               | Preserve modification times, modes, and access times |
| -v               | Verbose mode (debugging info)                        |
| -C               | Enable compression (faster for large text files)     |
| -i identity_file | Specify an SSH private key file                      |
| -q               | Quiet mode (no progress or error messages)           |

---
## Security Features

* *Encryption:* All traffic (including passwords and file data) is encrypted via SSH.
* *Authentication:* Uses SSH’s authentication — password, public/private key, or host-based.
* *Integrity:* Ensures data is not corrupted during transfer.
* *No need for extra configuration:* If SSH works, scp works.

---

## Under the Hood (Technical Explanation)

When you run:
```bash
scp file.txt user@remote:/path/
```
* Your local scp program starts an *SSH session*:

  * Equivalent to running: ssh user@remote scp -t /path/
  * The -t (to) or -f (from) flags are used internally by scp on the remote side to indicate direction.
* SSH establishes a *secure channel*.
* Data is sent over that encrypted channel using a simple *file transfer protocol* built into scp.
* The remote scp process writes the incoming data to disk.

This makes it *simple, fast, and secure*, but less flexible than tools like rsync.

----
## scp vs rsync

| Feature                 | scp               | rsync                   |
| ----------------------- | ------------------- | ------------------------- |
| Security                | Encrypted (via SSH) | Encrypted (via SSH)       |
| Resume transfers        | ❌ No                | ✅ Yes                     |
| Sync only changed files | ❌ No                | ✅ Yes                     |
| Preserve timestamps     | ✅ (with -p)       | ✅                         |
| Compression             | ✅ (with -C)       | ✅                         |
| Speed                   | Slightly slower     | Faster for repeated syncs |

> 🔹 If you’re copying files once — scp is perfect.
> 🔹 If you’re syncing large directories often — rsync is better.

---
## Troubleshooting Tips

| Problem                     | Cause                            | Solution                                        |
| --------------------------- | -------------------------------- | ----------------------------------------------- |
| Permission denied         | Wrong credentials or permissions | Check SSH login and file permissions            |
| Connection refused        | SSH not running on remote        | sudo systemctl start ssh                      |
| No such file or directory | Wrong path on destination        | Check full path, quotes if spaces exist         |
| Stuck transfers             | Network lag or large files       | Use -C for compression, or rsync for resume |

---
## Summary

| Feature        | Description                           |
| -------------- | ------------------------------------- |
| Protocol       | Uses SSH for secure copy              |
| Default Port   | 22                                    |
| Key Benefit    | Simple + Secure file transfer         |
| Authentication | Password or SSH key                   |
| Supports       | File and directory transfers          |
| Typical Use    | Copy files between Linux/Unix systems |
| Alternative    | rsync, sftp, scp2               |

---

## STFP (SSH File Transfer Protocol)
What is sftp?

sftp stands for Secure File Transfer Protocol (or SSH File Transfer Protocol).
It’s a command-line utility and protocol used to transfer files securely between local and remote systems over an encrypted SSH connection.
So in essence:
 sftp = ftp (file transfer) + ssh (security layer)

* interactive file transfer pogram similar to FTP but secured over SSH  
* supports commands like ```ls```,```cd```,```put```,```get```.

## How SFTP works (step-by-step)
### 1. start SFTP session:
```bash
sftp saloni@10.0.2.15
```
or
```bash
sftp -p 2222 user@hostname
```
#### Example:

```bash
sftp saloni@192.168.1.10
```
### 2. SFTP connects via SSH:

* Uses the same port (default 22) and authentication (password or SSH keys).

* Establishes an encrypted channel.

### 3. You enter an interactive SFTP shell:

It looks like this:
```bash
sftp>
```

* From here, you can upload, download, list, rename, or delete files securely.

### 4. Data is transferred securely:

* Both commands and data are encrypted.

* File integrity and privacy are guaranteed.
---

## Syntax of sftp:
```bash
sftp [options] [user@]host
```
or to sepcify a remote path immediately:
```bash
sftp [user]@host:/path/to/file
```
once connected ,you use **sftp** commands inside the session to navigate and tranfer the files.

## Examples
### 1. start a simple sftp
 *prompts a password (or uses your SSH key) ,then drops you into the sftp prompt:
 ```bash
 sftp>
 ```
 ### 2.download file from remote to local
 ```bash
 sftp> get /home/saloni/report.txt ~/Documents/
```
This copies ```report.txt``` from the remote machine to your local ```Documents``` folder.
### 3. upload fie from local to remote
```bash
sftp> put ~/downloads/config.yaml /etc/app/
```
Uploads the local file to ```/etc/app/``` on the remote host.
### 4. transfer entire dierectories(recursively)
```bash
sftp> get -r /home/saloni/Projects ~/Backups/
```
or 
```bash
sftp> put -r ~/Projects /home/saloni/
```
the ```r`` flag means *recursive* - includes all subdierectories and files.
### 5. Run a single SFTP command (non-interactive)

Instead of entering the prompt, you can run a single command directly:

```bash
sftp saloni@192.168.1.20:/home/saloni/report.txt .
```
Or:
```bash
sftp -b batch_commands.txt saloni@192.168.1.20
```
Where batch_commands.txt contains:
```bash
cd /home/saloni
get report.txt
bye
```
---
## Commonly Used SFTP Commands

| Command         | Description                    |
| --------------- | ------------------------------ |
| ls            | List files on remote server    |
| lls           | List files on local machine    |
| cd <path>     | Change remote directory        |
| lcd <path>    | Change local directory         |
| get <file>    | Download remote file           |
| put <file>    | Upload local file              |
| mget *.txt    | Download multiple files        |
| mput *.log    | Upload multiple files          |
| mkdir <dir>   | Create remote directory        |
| rmdir <dir>   | Remove remote directory        |
| rm <file>     | Delete remote file             |
| pwd           | Print remote working directory |
| lpwd          | Print local working directory  |
| exit or bye | Close SFTP session             |

---
## Security Features

* *Encryption:* Uses SSH encryption for both commands and file data.
* *Authentication:* Supports passwords, public/private key pairs, and host-based authentication.
* *Integrity:* Ensures that data isn’t corrupted or intercepted.
* *Firewall-friendly:* Works over SSH port (22) — no need for multiple ports like FTP.

---

## Under the Hood (Technical Explanation)

When you run:

bash
sftp user@remote:/path/


Here’s what happens behind the scenes:

1. sftp client starts an *SSH session* with the remote host.
2. The SSH server launches its *SFTP subsystem* (usually defined in /etc/ssh/sshd_config as Subsystem sftp /usr/lib/openssh/sftp-server).
3. The two sides exchange *SFTP packets* that handle:

   * File metadata (permissions, timestamps)
   * Read/write requests
   * Directory listings
4. Files are transferred in *binary chunks*, fully encrypted, and acknowledged for reliability.

Essentially, SFTP is a *protocol inside SSH*, unlike SCP, which uses SSH for tunneling raw data streams.

## sftp vs scp

| Feature                 | sftp                          | scp                          |
| ----------------------- | ----------------------------- | ---------------------------- |
| Security                | Encrypted via SSH             | Encrypted via SSH            |
| Interactive mode        | ✅ Yes                         | ❌ No                         |
| Resume transfers        | ✅ (supported in some clients) | ❌ No                         |
| Directory listing       | ✅ Built-in (ls)             | ❌ No                         |
| Automation (batch mode) | ✅ Yes (-b)                  | ⚠ Limited (expect scripts) |
| Default Port            | 22                            | 22                           |
| Transfer protocol       | Structured SFTP protocol      | Raw data over SSH tunnel     |
| Compression             | Via SSH options               | Via -C flag                |

> 🔹 Use *SFTP* for interactive or scripted file management.
> 🔹 Use *SCP* for quick one-off file copies.

---
## Troubleshooting Tips

| Problem               | Cause                                | Solution                                    |
| --------------------- | ------------------------------------ | ------------------------------------------- |
| Connection refused  | SSH server not running               | sudo systemctl start ssh                  |
| Permission denied   | Wrong credentials or directory perms | Check username/password and file ownership  |
| No such file        | Wrong path                           | Use ls to verify remote directory         |
| Timeout or hang     | Network/firewall issues              | Check connectivity and port 22 availability |
| Cannot open display | GUI command in SFTP shell            | Not supported — SFTP is text-only           |

---
## Summary

| Feature        | Description                               |
| -------------- | ----------------------------------------- |
| Protocol       | SSH-based file transfer                   |
| Default Port   | 22                                        |
| Key Benefit    | Secure, interactive file management       |
| Authentication | SSH password or key-based                 |
| Supports       | File/directory transfers, listings, edits |
| Typical Use    | Upload/download between systems           |
| Alternative    | scp, rsync, ftp (insecure)          |

----
### inside SFTP prompt,upload a file :
```bash
put localfile.txt
```
### download a file:
``` bash 
get remotefile.txt
```
## 5. Harden SSH Configuration
editing the SSH server configure improves security.
edit ```/etc/ssh/sshd_config```:
```bash
sudo nano /etc/ssh/sshd_config
```
### recommended settings :
```bash
Port 22                  # Change to a non-standard port to reduce automated attacks
PermitRootLogin no       # Disable direct root login for security
PasswordAuthentication no # Force use of keys instead of passwords
AllowUsers alice bob     # Restrict SSH access to specific users only
```
### restart SSH server to apply changes:
```bash
sudo systemctl restart ssh
```
## 6. Verify SSH Daemon Listening
check which port SSH listens on :
```bash
sudo -tuln |grep ssh
```
*or*
```bash
sudo netstat -tulp |grep ssh
```
*ensure SSH is bound to correct interfaces and port*
## 7. Configure Firewall (UFW)
if UFW (uncomplicated firewall) is enabled ,allow SSH traffic
*for default port 22*:
```bash
sudo ufw allow ssh
```
*for custom port(e.g.,2222):
```bash
sudo ufw allow 2222/tcp
```
*enable firewall if not active*:
```bash
sudo ufw enable
```
*check firewall status*:
```bash
sudo ufw satus
```
---
## 8. Testing SSH Login and File Transfer
*from client, test SSH login*:
```bash
ssh -i ~/.ssh/id_rsa [user]@[host] -p [port]
```
*test file transfer with scp*:
```bash
scp -P [port] /path/to/localfile [user]@[host]:/path/to/remote/directory/
```
## Bonus: Automate SSH Tasks and Key Rotation

* Use Ansible for automated SSH configuration and management.

* Schedule periodic SSH key rotation for security compliance.

* Integrate SSH keys with Git/GitHub for seamless repository access.

# Summary

SSH provides a secure, encrypted channel to remotely access Linux systems and transfer files. Using SSH keys improves security and enables automation. Harden SSH by disabling password login, restricting users, and configuring firewall rules.

