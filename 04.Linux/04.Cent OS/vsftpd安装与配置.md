# vsftpd安装与配置



## 一、环境
- OS：Cent OS 7


## 二、安装
### 1.安装Vsftpd服务相关部件：
```
yum install vsftpd*
```

### 2.确认安装PAM服务相关部件：
```
yum install pam*
```
开发包，其实不装也没有关系，主要的目的是确认PAM。
### 3.安装DB4部件包
这里要特别安装一个db4的包，用来支持文件数据库。
```
yum install db4*
```
## 三、系统账户
### 1.建立Vsftpd服务的宿主用户：
```
useradd vsftpd -s /sbin/nologin
```
默认的Vsftpd的服务宿主用户是root，但是这不符合安全性的需要。这里建立名字为vsftpd的用户，用他来作为支持Vsftpd的服务宿主用户。由于该用户仅用来支持Vsftpd服务用，因此没有许可他登陆系统的必要，并设定他为不能登陆系统的用户。
### 2.建立Vsftpd虚拟宿主用户：
```
useradd overlord -s /sbin/nologin
```
本篇主要是介绍Vsftp的虚拟用户，虚拟用户并不是系统用户，也就是说这些FTP的用户在系统中是不存在的。他们的总体权限其实是集中寄托在一个在系统中的某一个用户身上的，所谓Vsftpd的虚拟宿主用户，就是这样一个支持着所有虚拟用户的宿主用户。由于他支撑了FTP的所有虚拟的用户，那么他本身的权限将会影响着这些虚拟的用户，因此，处于安全性的考虑，也要非分注意对该用户的权限的控制，该用户也绝对没有登陆系统的必要，这里也设定他为不能登陆系统的用户。（这里插一句：原本在建立上面两个用户的时候，想连用户主路径也不打算给的。本来想加上 -d /home/nowhere 的，据man useradd手册上讲述：“

>-d, --home HOME_DIR
The new user will be created using HOME_DIR as the value for the user鈙 login directory. The default is to append the LOGIN name to
BASE_DIR and use that as the login directory name. The directory
HOME_DIR does not have to exist but will not be created if it is
missing.

使用-d参数指定用户的主目录，用户主目录并不是必须存在的。如果没有存在指定的目录的话，那么它将不会被建立”。
### 3.调整Vsftpd的配置文件：
#### 3.1. 编辑配置文件前先备份
