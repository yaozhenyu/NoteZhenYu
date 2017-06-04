**安装**

1、下载yum repository

`mysql57-community-release-el7-11.noarch.rpm`

安装` rpm -ivh mysql57-community-release-el7-11.noarch.rpm`

```
yum list mysql-server
yum install mysql-server
service mysqld start
```

查看初始密码

`sudo grep 'temporary password' /var/log/mysqld.log`

修改密码

`ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';`

登陆

`mysql -u root -p`

创建用户并设置密码和权限

```
CREATE USER 'username'@'host' IDENTIFIED BY 'password'; // 创建用户并设置密码
// 设置权限例子：
GRANT SELECT, INSERT ON test.user TO 'pig'@'%'; // 查，增。数据库test的user表;
GRANT ALL ON *.* TO 'username'@'%'; // 设置权限;增删改查，所有数据库，所有表
REVOKE privilege ON databasename.tablename FROM username; // 撤销权限
SHOW GRANTS FOR 'pig'@'%';//查看权限 
SET PASSWORD FOR 'username'@'host' = PASSWORD('newpassword');// 修改密码
DROP USER username; // 删除用户
Create database dbname;
```



**设置Mysql开机启动**

`chkconfig mysqld on`

打开3306端口

`firewall-cmd --add-port=3306/tcp --permanent`

