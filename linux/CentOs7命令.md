# centos 7用ss命令来查看端口占用和对应进程

mysqld进程在监听4567端口，进程id是2593：

```
# ss -lnp|grep 4567
```

> tcp LISTEN 0 128 *:4567 *:* users:(("mysqld",2593,11))

2593的父进程是2592：

```
# ps -ef|grep 2593
```

> mysql 2593 2592 0 04:46 ? 00:00:57 /usr/libexec/mysqld --wsrep-cluster-address=gcomm://

查找文件命令

```
$ find . -name 'my*' -ls
```







