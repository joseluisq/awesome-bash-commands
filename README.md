```sh
┌──────────────────────────────────────────────┐
│ ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │
│ ░░░░ ╔═══╗ ░░░░░ AWESOME BASH COMMANDS ░░░░░ │
│ ░░░░ ╚═╦═╝ ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │
│ ░░░ ╒══╩══╕ ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │
│ ░░░ └────°┘ ░░ A curated list of awesome ░░░ │
│ ░░░░░░░░░░░░░░░░░ Bash useful commands ░░░░░ │
│ ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │
└──────────────────────────────────────────────┘
```

# Awesome Bash Commands [![](https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://github.com/sindresorhus/awesome)

> A curated list of awesome [Bash](https://www.gnu.org/software/bash/) useful commands. Inspired by [awesome-shell](https://github.com/alebcay/awesome-shell) and [bash-handbook](https://github.com/denysdovhan/bash-handbook).

## Table of Contents
- [Files and directories](#files-and-directories)
- [Paths](#paths)
- [Devices](#devices)
- [Users and Groups](#user-and-group)
- [Network](#network)
- [Miscellaneous](#miscellaneous)
- [Other Awesome Lists](#other-awesome-lists)

### Files and directories

#### List files sorted by extension

```sh
$ ls -l -X
# Or
$ ls -l --sort=extension
```

#### Create a symbolic link for one directory or file

```sh
ln -s /var/www/html ~/www
ln -s ~/my/large/path/file.txt ~/myfile.txt
```

#### Create one empty file in current directory and subdirectories

```sh
find ./my/current/directory -type d -exec touch {}/.gitignore \;
```

#### Delete one specific file of current directory and subdirectories

```sh
find ./my/current/directory -name ".gitignore" -type f -delete
```

#### Copy file content to clipboard
Copy shell command output to cilpboard

```sh
$ cat myfile.txt | xclip -selection c
```

#### Copy entire directory to destination

```sh
$ cp -avr /my/current/directory /destination/directory
# Or
$ rsync -av /my/current/directory /destination/directory
```

#### Show the space usage of file or directory

Show the space usage of file or directory (recursive) in human readable format.

```sh
du -sh /var/log/dnf.librepo.log
# 4,1M	/var/log/dnf.librepo.log

du -sh /var/log
# 2,2G	/var/log
```

#### Delete all files in directory by pattern

```sh
find /usr/local/apache/logs/archive/ -name '*2017.gz' -delete
```

#### Move files using a RegExp

_This command move all `*.js` files into `*.ts` files (move equivalent)_

```sh
find src/ -type f -name "*.js" -exec bash -c 'mv {} `echo {} | sed -e "s/.js/.ts/g"`' \;
```

### Paths

#### Show the full path of (shell) commands

```sh
which bash
# /usr/bin/bash

which git node
# /usr/bin/git
# /usr/bin/node
```

#### Show the resolved path of symbolic link

```sh
realpath ~/www
# /usr/share/nginx/html
```

#### Determine the current directory

```sh
pwd
# /home/my/current/directory
```

### Devices

#### Display file system disk space usage

Show the file system disk space usage in human readable format.

```sh
df -h
# Filesystem      Size  Used Avail Use% Mounted on
# devtmpfs        487M     0  487M   0% /dev
# tmpfs           497M     0  497M   0% /dev/shm
# tmpfs           497M  508K  496M   1% /run
# tmpfs           497M     0  497M   0% /sys/fs/cgroup
# /dev/vda1        30G  2.7G   26G  10% /
# tmpfs           100M     0  100M   0% /run/user/0
```

#### Mount a FAT32 USB device

```sh
$ mount -t vfat /dev/sdb1 /media/usb
```

### Users and Groups

#### Add an existing user to existing group

```sh
usermod -a -G ftp john
```

### Network

#### Show current IP address

```sh
ifconfig | awk '/<UP,BROADCAST,RUNNING,MULTICAST>/ { getline; print $2 }'
# or
ifconfig | grep -E "([0-9]{1,3}\.){3}[0-9]{1,3}" | grep -v 127.0.0.1 | awk '{ print $2 }' | cut -f2 -d: | head -n1
```

### Miscellaneous

#### Generate 1 million of unique random phone numbers
This command generate one million of unique random phone numbers (random permutations) fast using GNU/Linux [shuf](https://www.gnu.org/software/coreutils/manual/html_node/shuf-invocation.html) command.
Use `sed` command for customize each number format. For example for add some prefix or suffix. Remember `shuf` is not limited to numbers only.

```sh
shuf -i 100000000-999999999 -n 1000000 | sed -e 's/^/51/' > gsm.txt
```

### Other Awesome Lists
- [awesome-shell](https://github.com/alebcay/awesome-shell)
- [awesome-fish](https://github.com/jbucaran/awesome-fish)
- [awesome-zsh](https://github.com/unixorn/awesome-zsh-plugins)

## Contributions
Please check out [the contribution file](contributing.md).

## License

[![CC0](http://i.creativecommons.org/p/zero/1.0/88x31.png)](http://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, [José Luis Quintana](http://git.io/joseluisq) has waived all copyright and related or neighboring rights to this work.
