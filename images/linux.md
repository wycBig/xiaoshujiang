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
8.远程访问MySQL数据库
	``` 
	grant all on *.* to 'wyc'@'%' IDENTIFIED BY '0711Ch!@#';
	flush privileges;

	```

## 安装教程：
``` https://help.aliyun.com/document_detail/116727.html
服务器mysql密码：0711Ch!@#
```

### 远程登录访问：
命令：
1.grant all on *.* to 'wyc'@'%' IDENTIFIED BY '0711Ch!@#'
2.flush privileges;
用户名：root
密码：0711Ch!@#

## Navicat链接sql报错：

``` ERROR 1045 (28000): Access denied for user 'root'@'8.131.103.29' (using password: YES)
```

### 1. 先用localhost登录

``` mysql -u root -p
		Enter password: 正确密码
```
## 2. 执行授权命令

``` mysql> grant all privileges on *.* to root@'%' identified by '密码';
```

### 3. 退出再试
退出mysql，重启mysql，试修改的数据生效。然后在使用 mysql -uroot -h xxx.xxx.xxx.xxx. -p  登录

## nacos安装：

### 准备条件：

1.下载nacos安装包
2.使用文件传输工具，将安装包传输到linux
	1. 1，下载文件传输工具
			```
				yum install lrzsz -y
			```
	1.2，查看文件包
			```
			rpm -qa |grep lrzsz
			```
	1.3，输入命令，拉取要传输的文件
		``` 
		rz
		```
			
一、解压nacos安装包（zip包要用unzip命令解压）
``` 
	unzip nacos-server-1.2.1.zip
```
二、启动nacos（需要安装jdk环境）

``` 
	sh startup.sh -m standalone
```
三、浏览器登录nacos，确认nacos是否能被访问（云服务器配置安全组，开放相关端口）

``` 
http://8.131.103.29:8848/nacos/
```

## 安装jdk
1.下载安装包

``` 
https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html
```
2.使用文件传输工具，将安装包传输到linux中，然后解压文件

``` 
rz
tar -zxvf jdk-8u251-linux-x64.tar.gz
```
3.配置环境

``` 
vim /etc/profile
```
4.修改配置文件

``` 
export JAVA_HOME=/xxx/xxx/jdk1.8.0_251
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib:$CLASSPATH
export JAVA_PATH=${JAVA_HOME}/bin:${JRE_HOME}/bin
export PATH=$PATH:${JAVA_PATH}
```
5.重新加载配置文件

``` 
source /etc/profile
```
## 安装tomcat
1.下载相关安装包
	选择core：tar.gz(pgp,sha512)文件下载
	
2.解压tomcat压缩包

``` 
tar -zxvf xxxxx.tar.gz
```
3. 进入tomcat -> config 文件夹下，打开service.xml文件 

```
# 修改默认的HTTP端口，由8080更改为自定义的(取值范围1-65535)只要没被占用即可。这里改为8030
<Connector port="8030" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
# <Server port="8005" shutdown="SHUTDOWN"> 
SHUTDOWN 端口，你可以用 telnet 命令玩一下：telnet 8005 进去后，输入 SHUTDOWN 后，tomcat 就被关闭了。无需更改这个端口。
# <Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />
AJP端口，是用于与 Apache httpd 服务器进行 AJP 通信用的，是给 mod_jk 库用的，如果不用 Apache httpd 服务器那这个端口可以不用理会。
```
4. 修改阿里云服务器安全组配置，将服务器端口暴露出来

5.启动tomcat服务
	进入tomcat -> conf 文件夹中
``` 
./startup.sh 
```
6.tomcat端口号8075
  浏览器输入：地址：端口号