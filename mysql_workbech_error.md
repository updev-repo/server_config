## Access Denied for User 'root'@'localhost' (using password: YES) - No Privileges?

```angular2html
sudo mysql -u root -p
use mysql;
select user, host, plugin from mysql.user;
update user set plugin='mysql_native_password' where user='root';
select user, host, plugin from mysql.user;
flush privileges;
```
