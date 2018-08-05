---
title: "Mysql Primer"
date: 2018-07-29T16:53:34-04:00
draft: true
---



Unsigned for int support more space since it is not reserved for negative numbers. 



To see mysql queries connect to mysql and find the log location. 

```bash
mysql -p 
mysql> SHOW VARIABLES LIKE "general_log%";
mysql> SET GLOBAL general_log = 'ON';
mysql> SHOW VARIABLES LIKE "general_log%";


tail -f /var/run/mysqld/mysqld.log | grep 'insert into mytable'
```

