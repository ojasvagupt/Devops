# Linux Notes for DevOps - Key Commands with Examples

## Basics

- Linux is an operating system kernel (not full OS; full distros = kernel + GNU tools + etc.).
- Key features: Kernel-based OS, multi-user, multi-tasking, open-source, Unix-like.
- Distros: Linux OS variations (flavours) with different package managers & uses. Popular: Ubuntu, CentOS/RHEL, Debian, Fedora.

## File System

- `/` : root directory (all files start here)
- `/home` : user home directories (personal files)
- `/etc` : configuration files (system configs like `/etc/passwd`)
- `/var` : variable data like logs (`/var/log`)
- `/tmp` : temporary files (cleared on reboot)
- `/usr` : user binaries (`/usr/bin` apps), libraries (`/usr/lib`)
- `/bin` : essential binaries (`/bin` user cmds)

## Basic Navigation & Info Commands

- **pwd** : Print working directory (shows current path).
  ```
  $ pwd
  ```
  **Output:**
  ```
  /home/user
  ```
- **ls** : List directory contents (files/folders).
  ```
  $ ls
  ```
  **Output:**
  ```
  dir1 file.txt README
  ```
- **ls -l** : Long format (detailed: perms, owner, size, date).
  ```
  $ ls -l
  ```
  **Output:**
  ```
  -rw-r--r-- 1 user group 1024 Oct10 file.txt
  ```
- **ls -a** : Show hidden files (starting with .).
  ```
  $ ls -a
  ```
  **Output:**
  ```
  . .. .hidden .bash_history file.txt
  ```
- **ls -la** : Long + all (detailed hidden).
  ```
  $ ls -la
  ```
  **Output:**
  ```
  drwxr-xr-x 2 user group 4096 Oct10 .
  ```
- **ls -ltr** : Long, time reverse (oldest first).
  ```
  $ ls -ltr
  ```
  **Output:**
  ```
  oldest files to newest
  ```
- **ls -lrt** : Long, reverse time (newest first).
  ```
  $ ls -lrt
  ```
  **Output:**
  ```
  newest files to oldest
  ```
- **cd /path** : Change directory.
  ```
  $ cd /home
  ```
  **Effect:** changes to `/home` dir
  ```
  $ cd ..
  ```
  **Effect:** goes to parent dir
  ```
  $ cd ~
  ```
  **Effect:** goes to home dir
- **clear** : Clear terminal screen (scroll history gone).
  ```
  $ clear
  ```
  **Effect:** screen cleared
- **whoami** : Current logged user.
  ```
  $ whoami
  ```
  **Output:**
  ```
  ojasva
  ```
- **uname** : Kernel/OS name.
  ```
  $ uname
  ```
  **Output:**
  ```
  Linux
  ```
- **uname -r** : Kernel version/release.
  ```
  $ uname -r
  ```
  **Output:**
  ```
  5.15.0-73-generic
  ```
- **uname -a** : All kernel info.
  ```
  $ uname -a
  ```
  **Output:**
  ```
  Linux host 5.15.0-73-generic #80-Ubuntu x86_64 GNU/Linux ...
  ```
- **history** : Command history list.
  ```
  $ history
  ```
  **Output:**
  ```
    1  ls   
    2  pwd   
    3  cd
  ```
  ```
  $ history | grep ls
  ```
  **Output:**
  ```
    1  ls
  ```

## File & Directory Management

- **mkdir abc** : Create single directory.
  ```
  $ ls (before: no abc)
  $ mkdir abc
  $ ls (after: abc dir created)
  ```
- **mkdir abc xyz pqr** : Create multiple directories.
  ```
  $ ls (before: no dirs)
  $ mkdir abc xyz pqr
  $ ls (after: abc xyz pqr created)
  ```
- **mkdir -p /abc/pqr/xyz** : Create with parents, no error if exists.
  ```
  $ mkdir -p /abc/pqr/xyz
  ```
  **Effect:** creates nested dirs abc/pqr/xyz
- **touch file.txt** : Create empty file or update timestamp.
  ```
  $ ls (before: no file)
  $ touch file.txt
  $ ls (after: file.txt 0 bytes)
  ```
- **rm -rvf abc** : Remove dir recursive verbose force.
  ```
  $ mkdir test; ls (before: test dir)
  $ rm -rvf test (r=recursive v=verbose f=force)
  $ ls (after: test gone)
  ```
- **cp -rvf src dest** : Copy recursive verbose force.
  ```
  $ mkdir src; echo "hi" > src/file.txt; ls src (before)
  $ cp -rvf src dest
  $ ls dest (after: dest/src/file.txt)
  ```
- **mv src dest** : Move or rename file/dir.
  ```
  $ mkdir old; ls (before: old)
  $ mv old new
  $ ls (after: new, old gone)
  ```
- **cat file** : Display file content.
  ```
  $ echo "hello" > test.txt
  $ cat test.txt
  ```
  **Output:**
  ```
  hello
  ```
- **echo "text" > file** : Write/overwrite file.
  ```
  $ echo "hello" > test.txt
  $ cat test.txt (after: hello)
  ```
- **head -n 5 file** : Show first 5 lines.
  ```
  $ seq 10 > lines.txt
  $ head -5 lines.txt
  ```
  **Output:**
  ```
  1
  2
  3
  4
  5
  ```
- **tail -f file** : Show last lines, follow live.
  ```
  $ tail -f /var/log/syslog
  ```
  **Effect:** follows log updates in real-time

## Search & Text Processing

- **grep "pattern" file** : Grep for pattern.
  ```
  $ grep "error" /var/log/syslog
  ```
  **Output:** lines containing "error"
- **find /path -name "\*.txt"** : Find files matching.
  ```
  $ mkdir test; touch test/file.txt; find test -name "*.txt"
  ```
  **Output:** test/file.txt
- **nslookup host** : DNS lookup.
  ```
  $ nslookup google.com
  ```
  **Output:** IP addresses for google.com

## Disk & Memory

- **du -sh /dir** : Dir size summary human-readable.
  ```
  $ du -sh /home
  ```
  **Output:** 1.2G /home
- **df -h** : Filesystem disk usage human.
  ```
  $ df -h
  ```
  **Output:**
  ```
  Filesystem      Size  Used Avail Use% Mounted on
  /dev/sda1        50G   20G   30G  40% /
  ```
- **free -h** : Memory usage human.
  ```
  $ free -h
  ```
  **Output:**
  ```
                total        used        free      shared  buff/cache   available
  Mem:           7.7Gi       2.1Gi       3.2Gi       300Mi       2.4Gi       5.0Gi
  Swap:          2.0Gi          0B       2.0Gi
  ```

## Editors

- **nano file** : Simple editor.
  ```
  $ nano config.txt
  ```
  **Keys:** Ctrl+O (save), Ctrl+X (exit)
- **vi/vim file** : Vim editor.
  ```
  $ vim file
  ```
  **Modes:** i (insert), Esc :wq (save quit), :q! (quit no save)

## Users & Permissions

- **useradd username** : Add user.
  ```
  $ sudo useradd devops
  ```
  **Effect:** creates devops user
- **passwd user** : Set password.
  ```
  $ sudo passwd devops
  ```
  **Effect:** prompts for password
- **sudo usermod -aG group user** : Add to group.
  ```
  $ sudo usermod -aG sudo devops
  ```
  **Effect:** adds devops to sudo group (log out/in)
- **ls -ld dir** : Dir permissions.
  ```
  $ ls -ld /home
  ```
  **Output:** drwxr-xr-x 2 user group 4096 date /home
- **Permissions**:
  Symbolic: `drwx rwx rwx` (d=dir, owner group other; r=read w=write x=exec)
  Numeric: 4=r 2=w 1=x → 755 = rwxr-xr-x
- **chmod 755 file** : Numeric perms.
  ```
  $ ls -l script.sh (before: 644)
  $ chmod 755 script.sh
  $ ls -l (after: rwxr-xr-x)
  ```
- **chown user:group file** : Owner change.
  ```
  $ ls -l app.py (before: root:root)
  $ sudo chown devops:devops app.py
  $ ls -l (after: devops:devops)
  ```
- **sudo** : Run as superuser.
  ```
  $ sudo apt update
  ```
  **Effect:** elevates privileges

## Package Management

- **apt (Ubuntu/Debian)**:
  ```
  $ sudo apt update
  $ sudo apt install nginx
  $ sudo apt search nginx
  ```
- **yum/dnf (RHEL)**:
  ```
  $ sudo dnf install nginx
  ```
- **apt-get (legacy)**:
  ```
  $ sudo apt-get update
  ```

