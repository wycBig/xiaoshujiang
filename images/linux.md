## 相关命令
1.rpm -qa | grep xxxx：查询当前安装的所有xxx文件
2.rpm -e --nodeps xxxx：强制删除软件包

### mysql安装：
1.更新yum资源：
``` rpm -Uvh  http://dev.mysql.com/get/mysql57-community-release-el7-9.noarch.rpm
```
2.安装mysql

``` yum -y install mysql-community-server
```
3.查看mysql版本号(V要大写)

``` mysql -V
```
4.启动mysql

``` systemctl start mysqld
```
5.设置mysql开机自启

``` systemctl enable mysqld
```
6.查看/var/log/mysqld.log文件，获取并记录root用户的初始密码

``` grep 'temporary password' /var/log/mysqld.log
```
7.对MySQL进行安全性配置

``` mysql_secure_installation
```
	1. 1：重置root用户的密码
		Enter password for user root: #输入上一步获取的root用户初始密码
		The 'validate_password' plugin is installed on the server.
		The subsequent steps will run with the existing configuration of the plugin.
		Using existing password for root.
		Estimated strength of the password: 100 
		Change the password for root ? ((Press y|Y for Yes, any other key for No) : Y #是否更改root用户密码，输入Y
		New password: #输入新密码，长度为8至30个字符，必须同时包含大小写英文字母、数字和特殊符号。特殊符号可以是()` ~!@#$%^&*-+=|{}[]:;‘<>,.?/
		Re-enter new password: #再次输入新密码
		Estimated strength of the password: 100 
		Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : Y #是否继续操作，输入Y
	1.2：删除匿名用户账号
			``` 
			By default, a MySQL installation has an anonymous user, allowing anyone to log into MySQL without having to have a user account created for them. This is intended only for testing, and to make the installation go a bit smoother. You should remove them before moving into a production environment.
			Remove anonymous users? (Press y|Y for Yes, any other key for No) : Y  #是否删除匿名用户，输入Y
			Success.
```
	1. 3禁止root账号远程登录
			``` 
			Disallow root login remotely? (Press y|Y for Yes, any other key for No) : Y #禁止root远程登录，输入Y
			Success.
```
	1. 4删除test库以及对test库的访问权限。

		``` Remove test database and access to it? (Press y|Y for Yes, any other key for No) : Y #是否删除test库和对它的访问权限，输入Y
		- Dropping test database...
		Success.
		```
		1.5 重新加载授权表
			``` 
			Reload privilege tables now? (Press y|Y for Yes, any other key for No) : Y #是否重新加载授权表，输入Y
			Success.
			All done!
			```

## 安装教程：
``` https://help.aliyun.com/document_detail/116727.html
服务器mysql密码：0711Ch!@#
```

### 远程登录访问：
命令：
1.grant all on *.* to 'wyc'@'%' IDENTIFIED BY '0711Ch!@#'
2.flush privileges;
用户名：
密码：