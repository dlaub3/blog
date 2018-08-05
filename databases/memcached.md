---
title: "Memcashed"
date: 2018-07-29
tags: ["memcached", "jot"]	
---

https://memcached.org


**Memcached** 

```bash
ps -aux | grep -i memcached
/etc/init.d/memcached restart
ps -aux | grep -i memcached
telnet localhost 11211
stats
	
get key
set key
add key
delete key

flush_all 10
quit
```

Supervisord config

```bash
[program:memcached]
command=/usr/bin/memcached -m 64 -p 11211 -u memcache -l 127.0.0.1 -DFOREGROUND
autostart = true
autorestart = true
```