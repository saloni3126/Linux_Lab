## 1. ps aux :
### explanation:
* **a** : show process for all user
* **u** : show user/owner of process
*  **x** : show process not attached to terminal
---
* together it shows every process running on the system, in a detailed , user oriented format.

  ---
  ### typical output :
  ```
  USER       PID  %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMANDroot         1  0.0  0.1 167500  1100 ?        Ss   Sep25   0:05 /sbin/initvibhu     1234  1.2  1.5 274532 15632 ?        Sl   10:15   0:12 /usr/bin/python3 script.pymysql     2001  0.5  2.0 450000 20988 ?        Ssl  Sep25   1:02 /usr/sbin/mysqld
  ```
  ----
  #### column explanation:
  - user : who owns the process
  - pid :process ID
  - %cpu :cpu usage percentage
  - %MEM : memory usage percentage
  - VSZ: virtual memory size(in kb)
  - RSS: resident set size (actual physical memory used).
  - TTY: terminal linked to the process (?means none )
  -  STAT:process satus(e.g;s=sleeping,r=running; z= zombie)
  -  START: when process started
  -  TIME: total CPU time used
  -  COMMAND: command that launched the process
    ---
    ![image](<screenshot (16).jpg>)
  ***
## 2. process tree
#### command:
```bash
pstree -p
````
  ### example output:
  ```systemd(1)─┬─NetworkManager(778)
           ├─sshd(895)─┬─sshd(1023)───bash(1024)───pstree(1101)
           ├─mysqld(2001)
           └─python3(1234)
````
* shows parent-child process relationship
* it displays all running process in a tree format, showing which process started which (parent-child relationships)
* this makes it easier to visualize how processes are related, compared to **ps aux**
---
![image](<screenshot (17).jpg>)
```

## 3. real-time monitoring:
###  command:
```
top
```
### explanation:
* **top** : task manager for linux
* it shows a dynamic , real time view of :
  
  - running process
  - cpu usage
  - memory usage
  - load on the system
  -----
  ![image](<screenshot (18).jpg>)
  ```
 ### output:
 ```bash
top - 10:20:51 up 2 days,  3:12,  2 users,  load average: 0.22, 0.33, 0.45
Tasks: 197 total,   1 running, 196 sleeping,   0 stopped,   0 zombie
%Cpu(s): 12.3 us,  5.4 sy,  0.0 ni, 80.1 id,  2.2 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  8045632 total,  3564980 free,  1876324 used,  2604328 buff/cache
PID   USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
1234  vibhu     20   0  274532  15632   7892 R   45.0  1.5   0:12.34 python3
2001  mysql     20   0  450000  20988   7564 S   25.0  2.0   1:02.11 mysqld
```
* press ``q`` to quit
  ---

 ## 4. Adjust process priority :
 * start a process with the low priority:
 ```
 nice -n 10 sleep 300 &
 ````
 ### output :
 ```bash
 [1] 3188
 ```
 
## 4. Start a Process with Lower Priority
```bash
nice -n 10 sleep 300 &
````
- *Nice value* controls process scheduling priority.  
- Range: -20 (highest priority) → 19 (lowest).  
- sleep 300 & runs in background for 300 seconds.  

*Use case:* Run a heavy job without affecting system responsiveness.

---

## 5. Change Priority of a Running Process
```bash
renice 5 -p <PID>
````
- Changes the priority of a process after it starts.  
- Example:
 ``` bash
  renice 10 -p 1234
  ````
  lowers the priority of process 1234.

---

## 6. CPU Affinity (Bind Process to Specific CPU)
```bash
taskset -cp 0 <PID>
````
- Restricts a process to specific CPU cores.  
- Example:
 ``` bash
  taskset -cp 0,1 1234
  ````
Process 1234 runs only on CPUs 0 and 1.  
![image](<screenshot (20).jpg>)
```
* Why useful?* 
- Performance tuning  
- Prevents one process from hogging all CPUs.

---

## 7. I/O Scheduling Priority
```bash
ionice -c2 -n7 -p <PID>
````
- Controls how a process accesses disk I/O.  
- Classes:
  - 1 → Real-time (highest priority)  
  - 2 → Best-effort (default)  
  - 3 → Idle (runs only when system is idle)  
- -n sets priority within class (0 = highest, 7 = lowest).  



## 8. Trace System Calls of a Process
```bash
strace -p <PID>
````
- Attaches to a process and logs all *system calls* it makes.  
- Useful for debugging issues like:
  - File not found  
  - Permission denied  
  - Network calls  

*Example:*  
```bash
strace ls
````
Shows all syscalls used by ls.

---

## 9. Find Process Using a Port
```bash
sudo lsof -i :8080
````
- Lists processes using TCP/UDP port 8080.  

Alternative:
```bash
sudo netstat -tulnp | grep 8080
````

*Use case:* Troubleshoot why a server port is busy.
![image](<screenshot (19).jpg>)
---

## 10. Per-Process Statistics
```bash
pidstat -p <PID> 1
````
- Reports CPU, memory, and I/O usage per process.  
- 1 → update every second.  


## 11. Control Resources with cgroups
Linux Control Groups (cgroups) allow limiting CPU, memory, and I/O usage of processes.
### Create a cgroup
```bash
sudo cgcreate -g cpu,memory:/mygroup
````

### Limit CPU usage
```bash
echo 50000 | sudo tee /sys/fs/cgroup/cpu/mygroup/cpu.cfs_quota_us
```
- 50000 microseconds out of 100000 → limits to *50% of one CPU*.

### Add a process to the cgroup
```bash
echo <PID> | sudo tee /sys/fs/cgroup/cpu/mygroup/cgroup.procs
```

*Use case:* Prevent background jobs from consuming too many resources.

---

# 🔑 Summary

- *ps, pstree, top* → view running processes  
- *nice, renice* → control CPU scheduling priority  
- *taskset, ionice* → manage CPU/I/O priorities  
- *strace, lsof* → debug process behavior and network usage  
- *pidstat, cgroups* → advanced resource monitoring and limitation.
---

### Alternatives to nice and renice
1. **chrt**(Real-Time Scheduling)
-Allows you to set real-time scheduling policies instead of just a "niceness" value.
-Supports FIFO and Round Robin scheduling.
-Useful for processes that need predictable latency.
#### example:
```bash
# Start a process with FIFO scheduling and priority 50
sudo chrt -f 50 sleep 1000
```
#### Check scheduling:
```bash
chrt -p <pid>
```
2. **ionice** (I/O Priority Control)
-Instead of CPU priority, it sets I/O scheduling priority.
-Useful when you don’t want a background process (like backups) to slow down disk-heavy apps.
#### Example:
```bash
# Run with best-effort class, priority 7
ionice -c 2 -n 7 tar -czf backup.tar.gz /home
```
3. **taskset**(CPU Affinity)
-Controls which CPU cores a process can run on.
-Doesn’t change priority, but limits CPU scheduling scope.
#### Example:
```bash
# Run process only on CPU core 1
taskset -c 1 firefox
```
4. **Control Groups** (cgroups)
-The modern alternative for process resource management.
-Allows setting CPU shares, memory limits, and I/O throttling.
-Much more powerful than *nice*.
#### Example:
```bash
# Create a cgroup
sudo cgcreate -g cpu,memory:/lowprio

# Limit CPU to 20% and memory to 200MB
echo 20000 | sudo tee /sys/fs/cgroup/cpu/lowprio/cpu.cfs_quota_us
echo 200M   | sudo tee /sys/fs/cgroup/memory/lowprio/memory.limit_in_bytes

# Add process (PID 1234) to the group
echo 1234 | sudo tee /sys/fs/cgroup/cpu/lowprio/cgroup.procs
```
5. **systemd-run**
-If your system uses systemd, you can start processes with custom resource controls directly.
-Integrates with cgroups under the hood.
#### Example:
```bash
# Run a process with limited CPU weight
systemd-run --scope -p CPUWeight=200 stress --cpu 4
```
6. **schedtool** (Less Common)
-Utility for setting process scheduling policies (like *chrt*).
-More granular options, but not installed by default on all distros.
#### Example:
```bash
# Run with Round Robin scheduling, priority 10
sudo schedtool -R -p 10 <pid>
```
## Summary Table :
|Tool|	Focus|	Alternative to|
-----|------|---------------|
|chrt |Real-time scheduling policies|	nice|
|ionice|	I/O priority control|	(complement)|
|taskset|	CPU affinity	|(complement)
|cgroups|	Fine-grained resource limits	|nice (more powerful)|
|systemd-run|	systemd + cgroups |resource mgmt	|nice
|schedtool|	Custom scheduling policies|	nice