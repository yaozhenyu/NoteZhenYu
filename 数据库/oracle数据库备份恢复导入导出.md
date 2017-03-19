#### 使用EXPDP和IMPDP时应该注意的事项：

EXP和IMP是客户端工具程序，它们既可以在客户端使用，也可以在服务端使用。

EXPDP和IMPDP是服务端的工具程序，他们只能在ORACLE服务端使用，不能在客户端使用。

IMP只适用于EXP导出的文件，不适用于EXPDP导出文件；IMPDP只适用于EXPDP导出的文件，而不适用于EXP导出文件。

expdp或impdp命令时，可暂不指出用户名/密码@实例名 as 身份，然后根据提示再输入，如：

expdp schemas=scott dumpfile=expdp.dmp DIRECTORY=dpdata1;

一、创建逻辑目录，该命令不会在操作系统创建真正的目录，最好以system等管理员创建。

create directory dpdata1 as 'd:\test\dump';

二、查看管理理员目录（同时查看操作系统是否存在，因为Oracle并不关心该目录是否存在，如果不存在，则出错）

select * from dba_directories;

三、给scott用户赋予在指定目录的操作权限，最好以system等管理员赋予。

grant read,write on directory dpdata1 to scott;

四、导出数据

1)按用户导

expdp scott/tiger@orcl schemas=scott dumpfile=expdp.dmp DIRECTORY=dpdata1;

2)并行进程parallel

expdp scott/tiger@orcl directory=dpdata1 dumpfile=scott3.dmp parallel=40 job_name=scott3

3)按表名导

expdp scott/tiger@orcl TABLES=emp,dept dumpfile=expdp.dmp DIRECTORY=dpdata1;

4)按查询条件导

expdp scott/tiger@orcl directory=dpdata1 dumpfile=expdp.dmp Tables=emp query='WHERE deptno=20';

5)按表空间导

expdp system/manager DIRECTORY=dpdata1 DUMPFILE=tablespace.dmp TABLESPACES=temp,example;

6)导整个数据库

expdp system/manager DIRECTORY=dpdata1 DUMPFILE=full.dmp FULL=y;

五、还原数据

1)导到指定用户下

impdp scott/tiger DIRECTORY=dpdata1 DUMPFILE=expdp.dmp SCHEMAS=scott;

2)改变表的owner

impdp system/manager DIRECTORY=dpdata1 DUMPFILE=expdp.dmp TABLES=scott.dept REMAP_SCHEMA=scott:system;

3)导入表空间

impdp system/manager DIRECTORY=dpdata1 DUMPFILE=tablespace.dmp TABLESPACES=example;

4)导入数据库

impdb system/manager DIRECTORY=dump_dir DUMPFILE=full.dmp FULL=y;

5)追加数据

impdp system/manager DIRECTORY=dpdata1 DUMPFILE=expdp.dmp SCHEMAS=system TABLE_EXISTS_ACTION