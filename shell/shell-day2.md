## shell变量
```
#!/usr/bin/bash
ip=125.124.15.132

ping -c1 $ip &>/dev/null
if [ $? -eq 0 ]; then
	echo "$ip is up."
else 
	echo "$ip is down."
fi
```
- read 从键盘读入
```
#!/usr/bin/bash

read -p "Please input a ip: " ip
ping -c1 ${ip} &>/dev/null
if [ $? -eq 0 ]; then
	echo "${ip} is up"
else
	echo "${ip} is down"
fi
```
## 环境及自定义变量
```
#!/usr/bin/bash
ping -c1 $1 &>/dev/null
if [ $? -eq 0 ]; then
	echo "$1 is up"
else
	echo "$1 is down"
fi
```
## 1、自定义变量
- 定义变量
- 引用变量：$变量名或${变量名}
- 查看变量：echo $变量名 set(所有变量：包括自定义变量和环境变量)
- 取消变量：unset变量名
- 作用范围：仅在当前shell中有效
## 2、环境变量
- 定义环境变量：
  - 1、export back_dir2=/home/backup
  - 2、export back_dir1将自定义变量转换成环境变量
- 查看环境变量：echo $变量名   env 例如 env |grep back_dir2
- 变量作用范围：在当前shell和子shell有效
- eg：PATH=$PATH:/new/bin       export PATH
## 位置变量
- $1 $2 $3 $4 $5 $6 $7 $8 $9 ${10}
## 预定义变量
- $0  带路经的脚本名
- $*  所有的参数
- $@  所有的参数
- $#  参数的个数
- $$  当前进程的PID
- $!  上一个后台进程的PID
- $?  上一个命令的返回值0表示成功

```
#!/usr/bin/bash
#如果入户没有加入参数
if [ $# -eq 0 ]; then
	echo "usage:`basename $0` file"
	exit
fi

if [ ! -f S1 ]; then
	echo "error file!"
	exit
fi

for ip in `cat $1`
do
	ping -c1 $ip &>/dev/null
	if [ $? -eq 0 ]: then
		echo "$ip is up"
	else
		echo "$ip is down"
	fi
done

```
