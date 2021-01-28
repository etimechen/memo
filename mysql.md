##mysql开启远程访问

```mysql
GRANT ALL PRIVILEGES ON *.* TO 'username'@'%'IDENTIFIED BY 'password' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```
（username：你的用户名，password：你的密码）

##临时开启批量update操作

```mysql
SET SQL_SAFE_UPDATES=0;
```

##永久开启批量update操作

* 打开`Workbench`
* 点击菜单`Preferences` 
* 点击`SQL Editor`
* 取消勾选`"Safe Updates".Forbid UPDATES and DELETEs with no key in WHERE clause or no LIMIT clause.Requires a reconnection.` 

![图片][img1]

##Mac下mysql数据库改为UTF8编码

* 将文件拷到桌面编辑 `/Library/LaunchDaemons/com.oracle.oss.mysql.mysqld.plist`
* 覆盖回去,更改拥有者为root
* 在mysqld下加入 `--character_set_server=utf8`

##查看编码命令

```mysql
show variables like 'character%';
```


update opt left join (select optid,sum(rmbprice) as price from optproduct group by optid) op on opt.id=op.optid set opt.totalprice = op.price













[img1]: https://github.com/etimechen/memo/blob/master/images/BAF0DB76-2091-4FB8-94E7-A7F608644813.png
