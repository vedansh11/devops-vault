# Linux Cheatsheet (DevOps)

## File & Navigation
ls -la
cd /path
pwd
mkdir -p dir/subdir
rm -rf dir

## File Viewing
cat file
less file
head -n 20 file
tail -f file

## Search
grep -i error file
grep -r TODO .

## Permissions
chmod 755 file
chown user:group file
chgrp group file
umask

## Disk
df -h
du -sh *
lsblk

## Process
ps aux
top
kill PID
kill -9 PID

## Users
id user
groups user
whoami
who
