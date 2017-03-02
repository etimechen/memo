##群辉NAS搭建Git服务器

###使用管理员账号登录群辉web页面依次完成以下步骤

1. 在套件中心里搜索`Git Server`,安装,如果没有,可以通过ipkg安装git,跳到下面查看ipkg安装方法
2. 在控制面板里创建用户帐号 `git`,并指定分配群组`administrators` (_users分组为默认勾上的_)
3. 在控制面板里点击`用户帐号` - `高级设置`将`启动家目录服务`打上勾并点击`应用`
4. 在控制面板里点击`用户帐号`双击`git`用户,在权限里勾选git和homes`可读写`
5. 在控制面板里点击`终端机和SNMP`把`启动SSH功能`打上勾并点击`应用`
6. 在控制面板里点击`文件服务` - `FTP` - 开启FTP和SFTP功能

###免密码push和pull

1. 通过管理帐号通过ssh登录NAS
2. 使用`sudo -i`拿到root权限
3. 查看是否存在`/var/services/homes/git`目录,正常情况是有的,如果没有就创建
4. 在自己电脑上的`~/.ssh/`找到RSA公钥文件`id_rsa.pub`,如果没有就通过`ssh-keygen -t rsa -C “您的邮箱地址”`生成
5. 将自己电脑上的`id_rsa.pub`文件拷到NAS上,可以通过`scp ./id_rsa.pub git@IP地址:/var/services/homes/git/`
6. 将NAS里`id_rsa.pub`文件内容追加到`/var/services/homes/git/.ssh/authorized_keys`文件里,可以通过`cat ./id_rsa.pub >> ./.ssh/authorized_keys`命令追加

本地clone一个库进行push和pull看还需不需要输入密码

###群辉NAS里安装ipkg

> 群辉NAS默认是不可以使用apt-get或yum或ipkg安装linux软件的, 安装了ipkg软件管理器就可以随心所欲的安装软件了

1. 通过管理帐号通过ssh登录NAS
2. 使用`sudo -i`拿到root权限
3. 输入以下命令,下载和安装ipkg

```
wget http://ipkg.nslu2-linux.org/feeds/optware/cs08q1armel/cross/unstable/syno-mvkw-bootstrap_1.2-7_arm.xsh
chmod +x syno-mvkw-bootstrap_1.2-7_arm.xsh
./syno-mvkw-bootstrap_1.2-7_arm.xsh

```

如果提示 `Error: CPU not Marvell Kirkwood, probably wrong bootstrap.xsh`,请修改当前目录下的bootstrap.sh文件

输入`vim  bootstrap.sh`, 然后输入`:21,24d`后回车,在输入`:wq`回车.意思是删除21-24行的数据并保存退出

最后,输入`./bootstrap.sh`,ipkg就安装好了.
