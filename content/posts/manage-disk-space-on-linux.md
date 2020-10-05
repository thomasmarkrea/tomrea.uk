+++
title = "Manage Disk Space on Linux"
description = "Two simple commands for managing disk space on Linux systems"
categories = []
tags = ['Linux']
date = 2020-10-04T11:36:39+01:00
draft = true
+++

Two simple commands for managing disk space on Linux systems.

## df

`df` can be used to check the total available disk space on a system.

Add the `--human-readable` (`-h`) option to display sizes in kB, MB, etc. and `--total` to show the system total.

```bash
df -h --total
```

```
Filesystem             Size  Used Avail Use% Mounted on
udev                   7.7G     0  7.7G   0% /dev
tmpfs                  1.6G  2.3M  1.6G   1% /run
/dev/mapper/data-root  461G   26G  411G   6% /
tmpfs                  7.7G  269M  7.5G   4% /dev/shm
tmpfs                  5.0M     0  5.0M   0% /run/lock
tmpfs                  7.7G     0  7.7G   0% /sys/fs/cgroup
/dev/nvme0n1p2         4.0G  2.1G  2.0G  52% /recovery
/dev/nvme0n1p1         498M  281M  217M  57% /boot/efi
tmpfs                  1.6G   20K  1.6G   1% /run/user/110
tmpfs                  1.6G   56K  1.6G   1% /run/user/1000
total                  493G   29G  441G   7% -
```

## du

`du` can be used to find which directories are taking up the most space.

As with `df`, `-h` makes the results 'human readable'.

Using `--max-depth=1` (`d 1`) can make the results more manageable by limiting them to the current directory. This allows you to step through the directories checking where most space is being used.

Piping (`|`) the results to `sort` with the `-h` option list them in ascending size order.

```bash
sudo du -hd 1 | sort -h
```

```
0	./dev
0	./proc
0	./sys
4.0K	./mnt
4.0K	./srv
8.0K	./media
16K	./lost+found
100K	./root
152K	./tmp
2.4M	./run
12M	./etc
237M	./opt
874M	./boot
2.1G	./recovery
6.7G	./home
7.9G	./var
11G	./usr
28G	.
```