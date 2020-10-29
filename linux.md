# __Linux Command Line (BASH ─ Bourne Against Shell)__

---

Author: Danilo Matrangolo Marano (danilo.m.marano@gmail.com)

---

## 1.Index

- [1. Index](#1-index)
- [2. Basics](#2-basics)
  - [2.1 `whoami`](#21-whoami)
  - [2.2 `pwd`](#22-pwd)
  - [2.2 `pwd`](#Archivingfileswith`tar`)

  [Custom foo description](#2foo)




## 2. Getting information

### 2.1 `whoami`

It's pretty obvious what it does, indicates the curently user logged in. It's basicaly you asking the system: _Who am I?_ or _What user am I_?

```console
dpc@danilo:~$ whoami
dpc

root@danilo:~$ whoami
root
```

### 2.2 `pwd`

This command indicates the curently open path. It is the acronym of '_print the working directory_'

```console
dpc@danilo:~$ pwd
/home/dpc
```

### 2.3 `hostname`

You can print the system hostname.

```console
dpc@danilo:~$ hostname
danilo
```

### 2.4 `man` e `--help`

Both of them results in the information about the command that is next to them. The difference between both is that `man` is a command, offering the complete and extense documentation of that other command, and `--help` is an atribute, wich results on a quick tutorial and some information about the command.

```console
dpc@danilo:~$ whoami --help
Usage: whoami [OPTION]...
Print the user name associated with the current effective user ID.
Same as id -un.

      --help     display this help and exit
      --version  output version information and exit

GNU coreutils online help: <https://www.gnu.org/software/coreutils/>
Full documentation at: <https://www.gnu.org/software/coreutils/whoami>
or available locally via: info '(coreutils) whoami invocation'

```

```console
dpc@danilo:~$ man whoami

WHOAMI(1)                        User Commands                       WHOAMI(1)

NAME
       whoami - print effective userid

SYNOPSIS
       whoami [OPTION]...

DESCRIPTION
       Print  the  user  name  associated  with the current effective user ID.
       Same as id -un.

       --help display this help and exit

       --version
              output version information and exit

AUTHOR
       Written by Richard Mlynarik.

REPORTING BUGS
       GNU coreutils online help: <https://www.gnu.org/software/coreutils/>
       Report   whoami   translation    bugs    to    <https://translationproject.org/team/>

COPYRIGHT
       Copyright  ©  2018  Free Software Foundation, Inc.  License GPLv3+: GNU
       GPL version 3 or later <https://gnu.org/licenses/gpl.html>.
       This is free software: you are free  to  change  and  redistribute  it.
       There is NO WARRANTY, to the extent permitted by law.

SEE ALSO
       Full documentation at: <https://www.gnu.org/software/coreutils/whoami>
       or available locally via: info '(coreutils) whoami invocation'

GNU coreutils 8.30              September 2019                       WHOAMI(1)
```



### 2.5 `passwd`

Change the password of a user. On Ubuntu OS the root doesn't have passowrd.

```console
dpc@danilo:~$ passwd
Changing password for dpc.
Current password:
```

### 2.6 `last`

Informs the last users logged on the system

```console
dpc@danilo:~$ last
dpc      :1           :1               Wed Oct 28 23:19   still logged in
reboot   system boot  5.4.0-7642-gener Wed Oct 28 23:19   still running
dpc      :1           :1               Wed Oct 28 20:29 - down   (02:49)
reboot   system boot  5.4.0-7642-gener Wed Oct 28 20:29 - 23:19  (02:49)
dpc      :1           :1               Wed Oct 28 15:34 - down   (04:54)
reboot   system boot  5.4.0-7642-gener Wed Oct 28 14:31 - 20:28  (05:57)
```

### 2.7 `apropos`

This command is used when you want to find a command that does a specific thing or you forget the name of one. It will search all that have some correlation with the word chosen.

```console
dpc@danilo:~$ apropos empty
git-init (1)         - Create an empty Git repository or reinitialize an exis...
git-init-db (1)      - Creates an empty Git repository
LIST_EMPTY (3)       - implementations of singly-linked lists, singly-linked ...
rmdir (1)            - remove empty directories
SDL_CreateRGBSurface (3) - Create an empty SDL_Surface
sigemptyset (3)      - POSIX signal set operations
sigisemptyset (3)    - POSIX signal set operations
SLIST_EMPTY (3)      - implementations of singly-linked lists, singly-linked ...
STAILQ_EMPTY (3)     - implementations of singly-linked lists, singly-linked ...
TAILQ_EMPTY (3)      - implementations of singly-linked lists, singly-linked ...
```


### Path Links

A link is a short-cut, in linux we have a hard link and a simbolic link. To understand it we need to know about __inode__: its store all the administration of a file, its metadata. To access the inode we use names, they are links that points directly to the inode's file and are called __hard links__. Now if a link points to the a hard link or the file name instead to it's inode it becomes a __symbolic link__.

<img alt="hard and symbolic links diagram" src="imgs/hard_symbolic_link.png" width=80%>

The vantage of symbolic links comparing to hard ones are that they can refer to files on another partition and make links of directories.

To visualize some of the information stored in the inode:

```
$ ls -l ../myfile
-rw-rw-r-- 1 dpc dpc 2 Oct 28 19:03 ../myfile
```

The number `1` is the link counter, it counts how many links this file has and the number `2` indicates the size in bytes.

To create a hard link we use the command `ln`:

```
$ ln ../myfile myhardlink
$ ls -l myhardlink
-rw-rw-r-- 2 dpc dpc 2 Oct 28 19:03 myhardlink
```

And to create a symbolic link we use the same previous command with the parameter `ln -s`:

```
$ ln -s ../myfile mysymboliclink
$ ls -l mysymboliclink
lrwxrwxrwx 1 dpc dpc 9 Oct 28 19:09 mysymboliclink -> ../myfile
```

If we move the hard link to another directory nothing happends, but if we made the same thing to a symbolic it will stop working. This is because the hard link shares the same inode from the file and symbolic link is just a path that points to the file. In the code bellow you can notice that the hard link has the same inode index (`11807072`) and size (`2`) of the original file, but the symbolic has a different index (`11807151`) and size (`9`, the length of the path that is pointing to, `../myfile`).

```
$ ls -li ..
11807072 -rw-rw-r-- 2 dpc dpc    2 Oct 28 19:03 myfile

$ ls -li
total 3
11807072 -rw-rw-r-- 2 dpc dpc    2 Oct 28 19:03 myhardlink
11807151 lrwxrwxrwx 1 dpc dpc    9 Oct 28 19:09 mysymboliclink -> ../myfile
```

### Archiving files with `tar`

# 2.Foo
