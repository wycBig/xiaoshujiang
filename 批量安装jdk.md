#### 批量安装JDK

##### 机器背景

| IP         | 说明     |      |
| ---------- | -------- | ---- |
| 10.1.4.193 | 源机器   |      |
| 10.1.9.31  | 目标机器 |      |
| 10.1.9.33  | 目标机器 |      |

###### 1. 下载JDK

   ```shell
wget https://download.oracle.com/otn/java/jdk/8u202-b08/1961070e4c9b4e26a04e7f5a083f551e/jdk-8u202-linux-x64.tar.gz?AuthParam=1607736463_bce0bbcda3dffbcc224945edade310de
   ```

   > 下载链接有认证参数，需在官网自行获取链接,如果无外网，需将JDK包上传至源机器

###### 2. 在源机器生成免密登录所需的公钥

   ```shell
   ssh-keygen -t dsa
   ```

   > 一路回车即可

###### 3. 在源机器安装expect

   ```shell
   yum install expect
   ```

   > 该步骤依赖root用户权限，需申请权限安装此工具，否则无法完成免密登录

###### 4. 在源机器编写JDK安装脚本

   ```shell
   vim install_jdk.sh
   # 内容如下
   -----------------------------------------------------------------------------------------------------------------------------
   #!/bin/bash
   # 解压JDK压缩包
   tar -zxvf jdk-8u251-linux-x64.tar.gz
   # 配置环境变量
   cat >> ~/.bash_profile << EOF
   export JAVA_HOME=/home/kscd/jdk1.8.0_251
   export PATH=\$PATH:\$JAVA_HOME/bin
   EOF
   # 刷新环境变量
   source ~/.bash_profile
   # 验证结果
   java -version
   ```

   > 注意根据实际解压出的JDK路径替换其中的“JAVA_HOME”参数

###### 5. 在源机器编写boot脚本

   ```shell
   vim boot.sh
   # 内容如下
   -----------------------------------------------------------------------------------------------------------------------------
   #!/bin/bash
   
   # 需安装的目标机器
   SERVERS="10.1.9.31 10.1.9.33"
   # 目标机器账户（所有机器同一账户）
   USER=***
   # 目标机器密码（所有机器同一密码）
   PASSWORD=***
   # 要传出的文件，此处包括JDK和JDK安装脚本
   SOURCE_FILES="/home/kscd/deploy/jdk/jdk-8u251-linux-x64.tar.gz /home/kscd/deploy/install_jdk.sh"
   # 文件到目标机器的路径
   DEST_PATH=/home/kscd/
   
   # 自动设置ssh免密码登录
   auto_ssh_copy_id() {
       expect -c "set timeout -1;
           spawn ssh-copy-id $1; #$1表示传入函数的第一个参数：服务器ip地址
           expect {
               *(yes/no)* {send -- yes\r;exp_continue;}
               *assword:* {send -- $2\r;exp_continue;} #服务器登录密码
               eof        {exit 0;}
           }";
   }
   
   ssh_copy_id_to_all() {
       for SERVER in $SERVERS
       do
           auto_ssh_copy_id $SERVER $PASSWORD
       done
   }
   
   # 执行脚本
   ssh_copy_id_to_all
   
   
   for SERVER in $SERVERS
   do
       for FILE in $SOURCE_FILES
       do
           scp $FILE $USER@$SERVER:$DEST_PATH # 传输所需文件
       done
       # 执行安装脚本
       ssh $USER@$SERVER sh ${DEST_PATH}"install_jdk.sh"
   done
   ```

###### 6. 执行boot脚本开始传包并安装

   ```shell
   sh boot.sh
   ```



> 参考链接：https://www.cnblogs.com/at0x7c00/p/7966364.html

