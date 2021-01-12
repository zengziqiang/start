# Linux版MySql 5.7安装配置
## 一、卸载原来配置
### 1、查看原来历史版本

```
rpm -qa | grep -i mysql
```
### 2、卸载原来安装
```
rpm -ev mysql-community-libs-5.7.27-1.el6.x86_64 --nodeps

#通用方式
/bin/rpm -e $(/bin/rpm -qa | grep mysql|xargs) --nodeps
/bin/rpm -e $(/bin/rpm -qa | grep mariadb|xargs) --nodeps
```
### 都删除成功之后，查找相关的mysql的文件

```
find / -name mysql
```
## 二、RPM安装mysql
### 1、下载上传文件包

```
 mysql-5.7.29-1.el7.x86_64.rpm-bundle.tar
# 或者wgt
wget http://ftp.ntu.edu.tw/MySQL/Downloads/MySQL-5.7/mysql-5.7.31-1.el7.x86_64.rpm-bundle.tar
```
### 2、解压
```
tar -xvf  mysql-5.7.29-1.el7.x86_64.rpm-bundle.tar
```
### 3、安装(依次执行（几个包有依赖关系，所以执行有先后）下面命令安装 )
```
# 添加依赖环境
yum install net-tools 
```
```
rpm -ivh mysql-community-common-5.7.29-1.el7.x86_64.rpm --force 
rpm -ivh mysql-community-libs-5.7.29-1.el7.x86_64.rpm --force 
rpm -ivh mysql-community-client-5.7.29-1.el7.x86_64.rpm --force
rpm -ivh mysql-community-server-5.7.29-1.el7.x86_64.rpm --force
```
### 4、文件目录授权

```
# 数据目录
cd /var/lib/mysql/
# 修改权限信息
chown -R mysql /var/lib/mysql
```

### 5、修改配置信息

```
vim /etc/my.cnf

#添加配置

skip-name-resolve
character_set_server=utf8
init_connect='SET NAMES utf8'
explicit_defaults_for_timestamp=true
max_allowed_packet=256M
innodb_rollback_on_timeout=on
log_timestamps=SYSTEM
lower_case_table_names=1
```
### 6、启动停止命令

```
systemctl status mysqld.service
systemctl stop mysqld.service
systemctl start mysqld.service
systemctl restart mysqld.service
```
### 7、添加开始启动

```
chkconfig mysqld on
```
### 8、查看初始密码2种方式

```
#方式一
grep 'temporary password' /var/log/mysqld.log 
#方式二
cat /root/.mysql_secret
```
### 9、登录后修改初始密码


```
#方式一
set password=password('123456');
flush privileges;
#方式二
alter user root@localhost identified by '123456';

#修改密码
mysql> set password for 'root'@'localhost' =password('newpassword');
```
### 10、创建用户

```
#只能本机登录
CREATE USER 'app'@'localhost' IDENTIFIED BY '123456';
#指定ip登录
CREATE USER 'app'@'192.168.1.101_' IDENDIFIED BY '123456';
#远程登录
CREATE USER 'app'@'%' IDENTIFIED BY '123456';

# 给mysql添加全部权限
GRANT privileges ON databasename.tablename TO 'username'@'host'
# 添加查询权限
GRANT select ON alu_dev.* TO 'spdtest'@'%';
```
