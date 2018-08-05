---
title: "Bash"
date: 2018-07-29
tags: ["bash", "jot"]			
---



Rename files to lowercase: 

* ` rename -f 'y/A-Z/a-z/' * `



Find file size:

* `ls -lhS`
* `du -sh *`
* `df -h .`
* `du -h --max-depth=1 | sort -hr`



Monitoring:

* `iostate` 
* `iotop -u www-data`
* `top -u www-data`
* `htop -u www-data`



Manage Processes 

* `fg jobid`
* `bg jobid`
* `jobs`
* `pkill -9 php`



Make a big file: 

* `fallocate -l 100M bigfile`



Manage directories 

* `pushd /home/Downloads`
* `dirs -v`
* `cd ~0`



Rsync: 

```
rsync -aP --exclude="*" --include="201[1-8]/" --include="201[1-8]/*" --include="201[1-8]/*/*" --omit-dir-times  /source /dest
```



NSLoookup: 

```
nsloopup
>  https://example.com
```

