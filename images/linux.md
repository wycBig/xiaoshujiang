## 相关命令
1.rpm -qa | grep xxxx：查询当前安装的所有xxx文件
2.rpm -e --nodeps xxxx：强制删除软件包

### mysql安装：
1.cat /etc/group | grep mysql：检查mysql用户组和用户是否存在
2.cat /etc/passwd | grep mysql
3.groupadd mysql
4.useradd -r -g  mysql mysql  （2-4：创建用户组及用户）
一、下载相关的安装包

``` wget https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.24-linux-glibc2.12-x86_64.tar.gz
```
二、解压文件，并将文件改名

``` tar xzvf mysql-5.7.24-linux-glibc2.12-x86_64.tar.gz  ：解压文件
       mv mysql-5.7.24-linux-glibc2.12-x86_64/ mysql ：重命名
```
三、创建data文件夹，在mysql目录中

``` mkdir data   服务器密码：0711Ch!@#
```