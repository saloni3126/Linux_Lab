# basic linux comands
## "pwd" comand

```bash
>>pwd
```
output example
```bash
/c/Users/VICTUS/Desktop/Upes
```

## ls-l
```bash
>>>ls-l
```

output example

```bash
$ ls -l
total 4
drwxr-xr-x 1 VICTUS 197121 0 Aug 13 23:20 data/
drwxr-xr-x 1 VICTUS 197121 0 Aug 13 23:39 Linux_Lab/
```
## ls-a
```bash
>> ls-a
```
output example
```bash
./  ../  .git/  data/  Linux_Lab/
```

## ls-la
```bash
>>ls-la
```
output example

```bash
total 12
drwxr-xr-x 1 VICTUS 197121 0 Aug 13 23:20 ./
drwxr-xr-x 1 VICTUS 197121 0 Aug 12 22:54 ../
drwxr-xr-x 1 VICTUS 197121 0 Aug 12 23:05 .git/
drwxr-xr-x 1 VICTUS 197121 0 Aug 13 23:20 data/
drwxr-xr-x 1 VICTUS 197121 0 Aug 13 23:39 Linux_Lab/
```
# file and dierectory management

## "mkdir" comand:
this command is used to make new dierectory in file system

### output
```bash
VICTUS@LAPTOP-D8G2JK9F MINGW64 ~/Desktop/Upes (master)
$ mkdir file.txt
```
## "cd" command:
it is used to move between dierectories (folders)in the terminant.
### output
```bash
VICTUS@LAPTOP-D8G2JK9F MINGW64 ~/Desktop/Upes (master)
$ cd
```
## touch command:
this command is used for creating empty files and updating file timestamps.
### ouput 
```bash
VICTUS@LAPTOP-D8G2JK9F MINGW64 ~
$ touch file1.txt 
```
## "touch" command:
this command is also used for creating multiple files.
### output
```bash
VICTUS@LAPTOP-D8G2JK9F MINGW64 ~
$ touch file 1 .txt file2.txt  file3.txt
```
## "cp" command:
this command copies files or dierectories.
### output
```bash
VICTUS@LAPTOP-D8G2JK9F MINGW64 ~
$ cp file file.txt
```
## "cp -r" command :
### output
```bash
VICTUS@LAPTOP-D8G2JK9F MINGW64 ~
$ cp -r  folder.txt
```
## "mv" command:
this command move or rename files
### output
```bash
VICTUS@LAPTOP-D8G2JK9F MINGW64 ~
$ mv file file.txt
```
## "rm" command :
this command used to remove files
### output
```bash
VICTUS@LAPTOP-D8G2JK9F MINGW64 ~
$ rm file.txt
```
# system commands
## " echo" command":
this command is used to print text.
### output
```bash
VICTUS@LAPTOP-D8G2JK9F MINGW64 ~
$ echo "Hello, World!"
Hello, World!
```
## "whoami" command:
this command is used to show the current user.
### output
```bash
VICTUS@LAPTOP-D8G2JK9F MINGW64 ~
$ whoami
VICTUS
```
## "man" command:
this command is used to manual for any command.
### output
```bash


```
# searching and finding
## "find" command :
this command is used to locate the files.
### output
```bash
VICTUS@LAPTOP-D8G2JK9F MINGW64 ~
./file1.txt
./file2.txt
./file3.txt
./files.txt
```
## "grep" command:
this is used to search inside the files.
### output
```bash
grep "hello" file.txt
```
# helpful shortcuts
Shortcut  |	Action
--------- |---------------------
 Tab      |	Auto-complete files/folders
 ↑ / ↓    |	Browse command history
 CTRL + C |	Stop a running command
 CTRL + L |	Clear screen
 
--- 

# bonus : chaining comands
Run multiple commands:
### output
```bash
VICTUS@LAPTOP-D8G2JK9F MINGW64 ~
$ mkdir test && cd test && touch hello.txt
```
![image](<Screenshot (10)-1.png>)
![image](<Screenshot (11).png>)
![image](<Screenshot (12).png>)