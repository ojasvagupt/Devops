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
  user@host:~$ ls
  (no output - empty dir)
  user@host:~$ mkdir abc
  user@host:~$ ls
  abc
  user@host:~$
  ```

- **mkdir abc xyz pqr** : Create multiple directories.

  ```
  user@host:~$ ls
  (no output - empty dir)
  user@host:~$ mkdir abc xyz pqr
  user@host:~$ ls
  abc  pqr  xyz
  user@host:~$
  ```

- **mkdir -p /abc/pqr/xyz** : Create with parents, no error if exists.

  ```
  user@host:~$ mkdir -p /tmp/abc/pqr/xyz
  user@host:~$ ls /tmp/abc
  pqr
  user@host:~$ ls /tmp/abc/pqr
  xyz
  user@host:~$
  ```

  **Effect:** creates all parent directories automatically

- **touch file.txt** : Create empty file or update timestamp.

  ```
  user@host:~$ ls
  (no output)
  user@host:~$ touch file.txt
  user@host:~$ ls
  file.txt
  user@host:~$ ls -l file.txt
  -rw-r--r-- 1 user group 0 date file.txt
  user@host:~$
  ```

- **rm -rvf abc** : Remove dir recursive verbose force.

  ```
  user@host:~$ mkdir test
  user@host:~$ touch test/file.txt
  user@host:~$ ls test
  file.txt
  user@host:~$ rm -rvf test
  removed 'test/file.txt'
  removed directory 'test'
  user@host:~$ ls test
  ls: cannot access 'test': No such file or directory
  user@host:~$
  ```

  **Flags:** -r (recursive), -v (verbose), -f (force, no prompt)

- **cp -rvf src dest** : Copy recursive verbose force.

  ```
  user@host:~$ mkdir src
  user@host:~$ echo "source content" > src/file.txt
  user@host:~$ ls src
  file.txt
  user@host:~$ cp -rvf src dest
  'src/' -> 'dest/src'
  'src/file.txt' -> 'dest/src/file.txt'
  user@host:~$ ls -R dest
  dest:
  src

  dest/src:
  file.txt
  user@host:~$
  ```

- **mv src dest** : Move or rename file/dir.

  ```
  user@host:~$ mkdir old
  user@host:~$ ls
  old
  user@host:~$ mv old new
  user@host:~$ ls
  new
  user@host:~$
  ```

- **cat file** : Display file content.

  ```
  user@host:~$ echo "hello world" > test.txt
  user@host:~$ cat test.txt
  hello world
  user@host:~$
  ```

- **echo "text" > file** : Write/overwrite file.

  ```
  user@host:~$ echo "hello" > test.txt
  user@host:~$ cat test.txt
  hello
  user@host:~$
  ```

- **head -n 5 file** : Show first 5 lines.

  ```
  user@host:~$ seq 10 > lines.txt
  user@host:~$ head -5 lines.txt
  1
  2
  3
  4
  5
  user@host:~$
  ```

- **tail -f file** : Show last lines, follow live.
  ```
  user@host:~$ tail -f /var/log/syslog
  Oct 10 12:00:01 host systemd[1]: Started My Service.
  (press Ctrl+C to stop following)
  user@host:~$
  ```
  **Effect:** shows last 10 lines and follows new log entries in real-time

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

| Number | Permission Type        | Symbol |
| ------ | ---------------------- | ------ |
| 0      | No permission          | ---    |
| 1      | Execute only           | --x    |
| 2      | Write only             | -w-    |
| 3      | Write + Execute        | -wx    |
| 4      | Read only              | r--    |
| 5      | Read + Execute         | r-x    |
| 6      | Read + Write           | rw-    |
| 7      | Read + Write + Execute | rwx    |

- **useradd username** : Add user.
  ```
  user@host:~$ sudo useradd devops
  user@host:~$
  ```
  **Effect:** creates devops user (check with `id devops`)
- **passwd user** : Set password.
  ```
  user@host:~$ sudo passwd devops
  Enter new UNIX password:
  Retype new UNIX password:
  passwd: password updated successfully
  user@host:~$
  ```
  **Effect:** prompts for password twice
- **sudo usermod -aG group user** : Add to group.
  ```
  user@host:~$ sudo usermod -aG sudo devops
  user@host:~$ groups devops
  devops : devops sudo
  user@host:~$
  ```
  **Effect:** adds to sudo group (log out/in to take effect)
- **ls -ld dir** : Dir permissions.
  ```
  user@host:~$ ls -ld /home
  drwxr-xr-x 2 user group 4096 Oct 10 12:00 /home
  user@host:~$
  ```
- **chmod 755 file** : Numeric perms.
  ```
  user@host:~$ ls -l script.sh
  -rw-r--r-- 1 user group 1024 Oct 10 script.sh
  user@host:~$ chmod 755 script.sh
  user@host:~$ ls -l script.sh
  -rwxr-xr-x 1 user group 1024 Oct 10 script.sh
  user@host:~$
  ```
- **chown user:group file** : Owner change.
  ```
  user@host:~$ ls -l app.py
  -rw-r--r-- 1 root root 2048 Oct 10 app.py
  user@host:~$ sudo chown devops:devops app.py
  user@host:~$ ls -l app.py
  -rw-r--r-- 1 devops devops 2048 Oct 10 app.py
  user@host:~$
  ```
- **sudo** : Run as superuser.
  ```
  user@host:~$ sudo apt update
  [sudo] password for user:
  Hit:1 http://archive.ubuntu.com/ubuntu jammy InRelease
  ...
  user@host:~$
  ```
  **Effect:** elevates privileges (password prompt first time)

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
