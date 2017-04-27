---
layout: post
title:  "Terminal Cheat Sheet"
categories: cheat_sheet
---

## System

* View disk space usage

```
df -h # useful to check the free space in your VPS
```

* Kill the process using the specific port

```
lsof -i :<port> # check the process running on port
kill -9 <PID>   # kill the process
```

## Remote server (ubuntu)

* Copy files to a remote server

```
scp <file> <username>@<domain>:<destination>
scp sites-available/* ducktyper@ducktyper.co.nz:/etc/apache2/sites-available
```

* Copy files to a remote server as one archived file

```
tar cf - <folder/files> | ssh <username>@<domain> "tar xf - -C <destination>"
tar cf - app | ssh ducktyper@ducktyper.co.nz "tar xf - -C /var/www"
# Add --exclude=*.DS_Store at the end for MAC
```

* Copy git archive to a remote server as one archived file

```
git archive --format=tar --prefix=<parent_directory> <branch> | ssh <username>@<domain> "tar xf - -C <destination>"
git archive --format=tar --prefix=app/ master | ssh ducktyper@ducktyper.co.nz "tar xf - -C /var/www"
```

* Copy files from a remote server

```
ssh <username>@<domain> "cd <folder> && tar cf - <folder/files>" | tar xf - -C <destination>
ssh ducktyper@ducktyper.co.nz "cd ~/projects && tar cf - app" | tar xf - -C .
```


* Execute a command to a remote server on bash (which runs .bashrc)

```
ssh <username>@<domain> "bash --login -c '<command>'"
ssh ducktyper@ducktyper.co.nz "bash --login -c 'cd /var/www/app && bundle install'"
```

* Work with log files

```
tail -f production.log   # watch file
tail -100 production.log # view last 100 lines
```
