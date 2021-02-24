## 1.本机电脑秘钥关联到github上
	1.github：setting->ssh and gpg keys->new ssh key
	2.本机：c盘->user->.ssh->id_rsa_pub文件
	3.输入“ssh -T git@github.com” 若为“your successful authenticated xxxxxx”

## 2.遇到“git@github.com: Permission denied (publickey).”错误
   原因：没有绑定秘钥没绑定
   解决方案，绑定相关信息
