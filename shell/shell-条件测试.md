## shell符号
- ()      子shell中运行
- (())    数值比较，运算 C语言风格
- $()     命令替换
- $(())   整数运算
- {}      集合
- ${}     变量的引用
- []      条件测试
- [[]]    条件测试，支持正则  =～
- $[]     整数运算
## 条件测试
- 方式一：test条件表达式
- 方式二：[条件表达式]
- 方式三：[[条件表达式]]
```
#!/usr/bin/bash
back_dir=/var/home
#if ! test -d $back_dir; then     //检测back_dir是不是目录
if [ ! -d $back_dir ]; then       //同上
	mkdir -p $back_dir        //创建目录
fi
echo "开始备份。。。"
```
## 文件测试
- [ -e dir / file ]         //测试文件/目录是否存在
- [ -d dir ]                //测试文件是否为目录
- [ -f file ]               //测试文件是否为文件
- [ -r file ]               //当前用户对该文件是否有读权限
- [ -x file ]
- [ -w file ]
- [ -L file ]               //测试文件是否为链接文件
- [ -b file ]               //测试文件是否为设备文件
- command -v /bin/date      //检测是不是命令
## 数值比较
- [a -gt b]                 //大于
- [a -lt b]                 //小于
- [a -eq b]                 //等于
- [a -ne b]                 //不等于
- [a -ge b]                 //大于等于
- [a -le b]                 //小于等于
- [ 1 -lg 2 -a 5 -gt 10 ]       //-o为或
### 添加用户
```
 #!/usr/bin/bash
 read -p "please input a username:" user
if  id $user &>/dev/null; then                      # id 用户名   检查用户是否存在
	echo "user $user already exists"
else
	useradd $user
	if [ $? -eq 0 ]; then
		echo "$user is created."
	fi
fi

```
### 内存预警
```
#!/usr/bin/bash
disk_use=`df -TH | grep '/$' | awk '{print $(NF-1)}' | awk -F"%" '{print $1}'`
if [ $disk_use -ge 20 ]; then
	echo "done"
#	echo "`date +%F-%H` disk: ${disk_use}%" | mail -s "disk war..." kuibaobao
fi
```
## 字符串比较
- 提示：使用双引号
- ["$USER"="root"]
- [ -z "$var" ]           //变量的长度为零  
- [ -n "$var" ]           //变量的长度不为零
- 变量为空或者未定义：长度都为零
- [ 1 -lg 2 -a 5 -gt 10 ]       //-a为并且
- [ 1 -lg 2 -a 5 -gt 10 ]       //-o为或
- [ 1 -lg 2 && 5 -gt 10 ]       
- [ 1 -lg 2 || 5 -gt 10 ]       
```
#!/usr/bin/bash
#############################################
# useradd                                   #
# v1.0 by kuibaobao                         #
#############################################

read -p "please input a number : " num
if [[ ! "$num" =~ ^[0-9]+$ ]];then     #^开头  +前面字符有1到多个 $结尾
	echo "error number!"
	exit
fi
read -p "please input a number : " prefix
if [ -z $prefix ]; then
	echo "error prefix!!"
	exit
fi
for i in `seq $num`
do
	user=$prefix$i
	useradd $user
	echo "123" | passwd --stdin $user &>/dev/null
	if [ $? -eq 0 ]; then
		echo "$user is created."
		fi
done

```
##  模式匹配case
```
read p
case "$p" in
"7.3")
	ls
	;;
"*")
	tree
	;;
esac	
```
### 删除用户的操作
```
#!/usr/bin/bash
#############################
#delete user                #
#v1.0 by kuibaobao          #
#############################

read -p "please input a username: " user
id $user &>/dev/null
if [ $? -ne 0 ]; then
	echo "no such $user"
        exit 1
fi


read  -p "Are you sure? [Y/N]" action
case "$action" in
y|Y|yes|YES|Yes)
	userdel -r $user
	;;
*)
	echo "good!!"
	;;
esac

#if [ "$action" = "Y" ]; then
#	userdel -r $user
#else
#	echo "good!!"
#fi

```
## 密钥登录
- 产生公钥密钥  ssh-keygen
- 拷贝到地址 ssh-copy-id 
### 脚本自动启动
- 1、./bash_profile      //在文件里面添加路径
- 2、
- 捕捉键盘信号
  - trap “” HUP INT OUIT TSTP
  - echo -en ""          //e加颜色  n不换行   
## 系统管理工具箱
```
#!/usr/bin/bash
menu(){
cat <<-EOF
+---------------------------------------
|	h.help                         |
|	f.disk partition               |
|	d.filesystem mount             |
|	m.memory                       |
|	u.system load                  |
|	q.exit                         | 
+---------------------------------------
EOF
}
menu
while :
do
	
	read -p "Please input [h for help]:" action
	case "$action" in
	h)
		clear
		menu
		;;
	f)
		fdisk -l
		;;
	d)
		df -TH
		;;
	m)
		free -m
		;;
	u)
		uptime
		;;
	q)
		#exit          //退出脚本
		break
		;;
       "")
		;;
	*)
		echo "error"
	esac
done

```
