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
- [Users and Groups](#users-and-groups)
- [Date & Time](#date--time)
- [Network](#network)
- [Miscellaneous](#miscellaneous)
- [Other Awesome Lists](#other-awesome-lists)

### Files and directories

#### List files sorted by extension

```sh
ls -l -X
# Or
ls -l --sort=extension
```

#### Count number of files within a directory

```sh
ls -1 ./my-directory | wc -l
```

_Note: use `-a` to include also dot files._

#### Create a symbolic link for one directory or file

```sh
ln -s /var/www/html ~/www
ln -s ~/my/large/path/file.txt ~/myfile.txt
```

#### Change permissions for a symbolic link only

```sh
chmod -h 770 ~/www
chown -h www-data:www-data ~/www
```

#### Find octal permissions of one file or directory

```sh
stat -c "%a %n" /var/www/html
# 770 /var/www/html
```

#### Add sticky permissions to one directory and subdirectories

```sh
find /var/www/html -type d -exec chmod g+s {} \
```

_Note: Any new file created will have the same permissions as the root folder_

#### Create one empty file in current directory and subdirectories

```sh
find ./my/current/directory -type d -exec touch {}/.gitignore \;
```

#### Delete one specific file of current directory and subdirectories

```sh
find ./my/current/directory -name ".gitignore" -type f -delete
```

#### Delete all content of one directory but except one in specific

```sh
find ./my-directory -mindepth 1 ! -regex '^./my-directory/data\(/.*\)?' -delete
```

#### Copy file content to clipboard
Copy shell command output to clipboard

```sh
cat myfile.txt | xclip -selection c
```

#### Copy entire directory to destination

```sh
cp -avr /my/current/directory /destination/directory
# Or
rsync -av /my/current/directory /destination/directory
```

#### Copy entire directory files only to destination with exclude option

```sh
rsync -har --progress --exclude .git /current/directory/. /destination/directory
```

#### Transfer an entire directory content to a remote destination

__Note:__ It's necessary to setup a SSH connection. You can use a [SSH Config file](https://www.cyberciti.biz/faq/create-ssh-config-file-on-linux-unix/).

```sh
rsync -hrtpluz --progress --stats my-current-directory/. my-remote-server:/var/data/my-remote-dir
```

Options used above:

```sh
-h,                  output numbers in a human-readable format 
-r                   recurse into directories
-t, --times          preserve modification times
-p, --perms          preserve permissions
-l, --links          copy symlinks as symlinks
-u, --update         skip files that are newer on the receiver
-z, --compress       compress file data during the transfer
--progress           show progress during transfer
--stats              give some file-transfer stats
```

#### Show the space usage of file or directory

Show the space usage of file or directory (recursive) in human readable format.

```sh
du -sh /var/log/dnf.librepo.log
# 4,1M	/var/log/dnf.librepo.log

du -sh /var/log
# 2,2G	/var/log
```

#### Show size of one symbolic link (file or directory)

```sh
du -Hsh $(which dart)
# 21M	/usr/local/bin/dart
```

#### Show space usage of directories and files ordered by size

```sh
du -sh /var/data/software/app/* | sort -rh
# 1.1G	/var/data/software/app/backups
# 223M	/var/data/software/app/logs
# 125M	/var/data/software/app/attachments
# 70M	/var/data/software/app/recovery
# 1.2M	/var/data/software/app/shared-home
# 592K	/var/data/software/app/viewfile
# 12K	/var/data/software/app/journal
# 4.0K	/var/data/software/app/bundled-plugins
# 0	    /var/data/software/app/lock
```

#### Show top ten of biggest directories

```sh
du -hS /home/user/some/directory | sort -rh | head -10
# 90G	/home/user/some/directory/big-dir
# 10G	/home/user/some/directory/subdir/another-big-dir
# ...
```

#### Delete all files in directory by pattern

```sh
find /usr/local/apache/logs/archive/ -name '*2017.gz' -delete
```

#### Move files by pattern

_This command move all `*.js` files into `*.ts` files (move equivalent)_

```sh
find src/ -type f -name "*.js" -exec bash -c 'mv {} `echo {} | sed -e "s/.js/.ts/g"`' \;
```

#### Compress files by pattern using tar and gzip

Command bellow compress a group of files by pattern using tar and gzip compression into a new file like `FILE_NAME.tar.gz`. 
No sorting guaranteed.

```sh
# Input files:
# src/file1.log.01
# src/file1.log.02
# src/file1.log.03

find src/ -type f -name "file1.log.*" -exec bash -c "tar cf - {} | gzip -9 > {}.tar.gz" \;

# Or using find -exec

find src/ -maxdepth 1 -type f -name "file1.log.*" -exec sh -c "tar cf - {} | gzip -9 > tarballs/{}.tar.gz && echo '{} (compressed)'" \;

# Output files:
# src/file1.log.01.tar.gz
# src/file1.log.02.tar.gz
# src/file1.log.03.tar.gz
```

#### Optimizing Tar/Gzip archive size by changing content files order

```sh
find directory/* -print | rev | sort | rev | \
      tar --create --no-recursion --files-from - | \
      gzip -c > directory.tar.gz
```

Motivated by https://news.ycombinator.com/item?id=24221498

**Alternative sorting in ascending way:**

```sh
find . -type f -name  "file1.log.*" | sort -n | xargs -I{} sh -c "tar cf - {} | gzip -9 > {}.tar.gz && echo '{} (compressed)'" \;
```

__Tip:__ Use `-maxdepth` flag in order to limit directory levels finding.

#### Compress a directory using tar and gzip

```sh
tar -zcvf my_dir.tar.gz my-dir/
```

#### List content of a tar and gzip file without extracting it

```sh
tar -tvf my_dir.tar.gz | less
```

#### Create a temporary directory with prefix

```sh
mktemp -dt "my-prefix"
```

#### Clean temporary directory

```sh
rm -rf /tmp/* /tmp/.*
```

#### Calculate gzip size of one no compressed file

```sh
gzip -c FILENAME.txt | wc -c | awk '{
if ($1 > 1000 ^ 3) {
  print($1 / (1000 ^ 3)"G")
} else if ($1 > 1000 ^ 2) {
  print($1 / (1000 ^ 2)"M")
} else if ($1 > 1000) {
  print($1 / 1000"K")
} else {
  print($1)"b"
}}'

# 560K
```

#### Split out a specific file into more small files by number of lines

```sh
split -d -l 200000 ./big_file.log small_file.log.

# Output files in current directory:
# small_file.log.01
# small_file.log.02
# small_file.log.03
# ....
```

_Notes:_

- `smallfile.log.` is a custom suffix for file names.
- `200000` is a custom number of lines per file.

#### Get only the number of lines of one specific file

```sh
wc -l big_file.log | sed -E "s/([a-z\-\_\.]|[[:space:]]*)//g"

# 9249133
```

### Paths

#### Show the full path of a command

```sh
which bash
# /usr/bin/bash

which git node
# /usr/bin/git
# /usr/bin/node
```

#### Show the resolved path of a symbolic link

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

#### Display file system disk space usage with total

Show the file system disk space usage in human readable format.

```sh
df -h --total
# Filesystem      Size  Used Avail Use% Mounted on
# devtmpfs        487M     0  487M   0% /dev
# tmpfs           497M     0  497M   0% /dev/shm
# tmpfs           497M  508K  496M   1% /run
# tmpfs           497M     0  497M   0% /sys/fs/cgroup
# /dev/vda1        30G  2.7G   26G  10% /
# tmpfs           100M     0  100M   0% /run/user/0
# total           2.2T  600G  100G  20% -
```

#### Display system memory information with total

```sh
free -h --total
#               total        used        free      shared  buff/cache   available
# Mem:           200G         60G        100G        262M         30G        180G
# Swap:            0B          0B          0B
# Total:         200G         60G        100G
```

or

```sh
cat /proc/meminfo
# MemTotal:       183815530 kB
# MemFree:        101918660 kB
# MemAvailable:   123712410 kB
# ....
```

_Tip: Pipe `grep` to filter your results. E.g `cat /proc/meminfo | grep MemTotal`_

#### Mount a FAT32 USB device

```sh
mount -t vfat /dev/sdb1 /media/usb
```

#### Increase temporary directory size

```sh
mount -o remount,size=5G /tmp/
```

#### Find out a block device by label

```sh
blkid --label MY_MICROSD
# /dev/sdc1

lsblk --output name,serial,uuid,partuuid,label,partlabel $(blkid --label MY_MICROSD)
# NAME SERIAL UUID          PARTUUID       LABEL        PARTLABEL
# sdc1        4BGE-7510     ec123nba-02    MY_MICROSD
```

### Users and Groups

#### Switch user and execute command immediately

```sh
sudo -Hu root fish
```

#### Add an existing user to existing group

```sh
usermod -a -G ftp john
```

### Date & Time

#### Show extended ISO format Date ([ISO 8601](http://en.wikipedia.org/wiki/ISO_8601))

```sh
date "+%Y-%m-%dT%H:%m:%S"
# 2018-09-13T10:09:26
```

### Network

#### Show current IP address

```sh
ifconfig | awk '/<UP,BROADCAST,RUNNING,MULTICAST>/ { getline; print $2 }'
# or
ifconfig | grep -E "([0-9]{1,3}\.){3}[0-9]{1,3}" | grep -v 127.0.0.1 | awk '{ print $2 }' | cut -f2 -d: | head -n1
```

### Miscellaneous

#### Show GNU/Linux distribution and operating system

a) Using `hostnamectl`:

```sh
hostnamectl | grep -E "(.+)(System|Kernel|Arch)(.+)"
#  Operating System: Arch Linux
#            Kernel: Linux 5.1.7-arch1-1-ARCH
#      Architecture: x86-64
```

b) Using `cat /etc/os-release`:

```sh
cat /etc/os-release
# NAME="Arch Linux"
# PRETTY_NAME="Arch Linux"
# ID=arch
# BUILD_ID=rolling
# ANSI_COLOR="0;36"
# HOME_URL="https://www.archlinux.org/"
# DOCUMENTATION_URL="https://wiki.archlinux.org/"
# SUPPORT_URL="https://bbs.archlinux.org/"
# BUG_REPORT_URL="https://bugs.archlinux.org/"
# LOGO=archlinux
```

#### Generate random numbers

a)

```sh
od -vAn -N64 < /dev/urandom | tr '\n' ' ' | sed "s/ //g" | head -c 32
# 03121617301002504516642404031105
```

b)

```sh
env LC_CTYPE=C tr -dc "0-9" < /dev/urandom | head -c 32 | xargs
# 50569696992247151969921987764342
```

_Change `head` value to truncate the result's length._

#### Generate random alphanumerics

a) Alphanumeric only

```sh
base64 /dev/urandom | tr -d '/+' | head -c 32 | xargs
# 3udiq6F74alwcPwXzIDWSnjRYQXcxiyl
```

b) Alphanumeric with a custom chars set

```sh
env LC_CTYPE=C tr -dc "A-Za-z0-9_!@#\$%^&*()-+=" < /dev/urandom | head -c 32 | xargs
# yiMg^Cha=Zh$6Xh%zDQAyBH1SI6Po(&P
```

_Change `tr -dc` char set to get a custom result._

#### Generate a random hash

```sh
od -vAn -N64 < /dev/urandom | tr '\n' ' ' | sed "s/ //g" | openssl dgst -sha256 | sed "s/-//g"
# 7adf57e0a90b32ce0e1f446268dbd62b583c649a2e71a426519c6e9c0006b143
```

_Openssl digest algorithms supported: `md5`, `md4`, `md2`, `sha1`, `sha`, `sha224`, `sha256`, `sha384`, `sha512`, `mdc2` and `ripemd160`_

#### Generate a random UUID

```sh
uuidgen | tr "[:lower:]" "[:upper:]"
# D2DA7D0C-ABAA-4866-9C97-61791C9FEC89
```

#### Generate 1 million of unique random phone numbers
This command generate one million of unique random phone numbers (random permutations) fast using GNU/Linux [shuf](https://www.gnu.org/software/coreutils/manual/html_node/shuf-invocation.html) command.
Use `sed` command for customize each number format. For example for add some prefix or suffix. Remember `shuf` is not limited to numbers only.

```sh
shuf -i 100000000-999999999 -n 1000000 | sed -e 's/^/51/' > gsm.txt
```

#### Download a file using Curl and get its stats

These commands below downloads a file using Curl and get its download stats into a `curl-download-stats.log` file.
Place the script content into a custom file `.sh` and run just it.

```sh
#!/bin/bash

curl -Skw "\
CURL download testing\n\
=====================\n\
URL:                    %{url_effective}\n\
Response code:          %{response_code}\n\
Download size:          %{size_download}B\n\
Download speed:         %{speed_download}B/s\n\
Time connect:           %{time_connect}s\n\
Time name lookup:       %{time_namelookup}s\n\
Time pretransfer:       %{time_pretransfer}\n\
Time start transfer:    %{time_starttransfer}s\n\
Time redirect:          %{time_redirect}s\n\
Time total:             %{time_total}s\n" \
-Lo /dev/null --url \
https://ger.mirror.pkgbuild.com/iso/2020.02.01/archlinux-2020.02.01-x86_64.iso \
> curl-download-stats.log
```

__Notes:__

- For get the file into a location just replace `/dev/null` with the corresponding file path.
- For customize the write-out format take a look at https://ec.haxx.se/usingcurl/usingcurl-verbose/usingcurl-writeout

### Other Awesome Lists
- [awesome-shell](https://github.com/alebcay/awesome-shell)
- [awesome-fish](https://github.com/jbucaran/awesome-fish)
- [awesome-zsh](https://github.com/unixorn/awesome-zsh-plugins)

## Contributions
Please check out [the contribution file](contributing.md).

## License

[![CC0](http://i.creativecommons.org/p/zero/1.0/88x31.png)](http://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, [José Luis Quintana](http://git.io/joseluisq) has waived all copyright and related or neighboring rights to this work.
