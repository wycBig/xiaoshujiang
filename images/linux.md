## 相关命令
1.rpm -qa | grep xxxx：查询当前安装的所有xxx文件
2.rpm -e --nodeps xxxx：强制删除软件包

### mysql安装：
1.cat /etc/group | grep mysql：检查mysql用户组和用户是否存在
2.cat /etc/passwd | grep mysql
3.groupadd mysql
4.useradd -r -g  mysql mysql  （2-4：创建用户组及用户）

## 安装教程：
https://help.aliyun.com/document_detail/116727.html
服务器mysql密码：0711Ch!@#