# 循环
## 流程控制：if
```
#!/usr/bin/bash
###################################################
#         install apache                          #
#         v1.0 by kuibaobao                       #
###################################################
#if ping -c1 www.baidu.com &>/dev/null;then
ping -c1 www.baidu.com &>/dev/null
if [ $? -eq 0 ];then
	yum -y install httpd
	systemctl start httpd
	systemctl enable httpd
	firewall-cmd --permanent --add-service=http
	firewall-cmd --permanent --add-service=httpd
	firewall-cmd --reload
	sed -ri '/^SELINUX=/cSELINUX=disabled' /etc/selinux/config
	setenforce 0                #设置SELinux成为permissive模式 临时关闭selinux的  
	curl http://127.0.0.1 &>/dev/null
	if [ $? -eq 0 ];then
		echo "Apache ok..."
	fi
elif ping -c1 www.baidu.com &>/dev/null; then
	echo "check dns..."
else
	echo "check ip address!"
fi

```
## 多系统配置yum源
- 根据不同的系统配置yum源
```
#！/usr/bin/bash
###############################################
#            yum config                       # 
#           by kuibaobao                      #
###############################################
#获取版本号cat /etc/redhat-release
os_vsrsion=`cat /etc/redhat-release | awk '{print $4}' | awk -F"." '{print $1"."$2}'`
[ -d /etc/yum.repos.d ] || mkdir /etc/yum.repos.d/bak
mv /etc/yum.repos.d/{*.repos,bak}
if [ "$os_version"="7.3"]; then
elif [ "$os_version"="6.8"]; then
elif [ "$os_version"="6.3"]; then
else
	echo "No version"
fi
```
- ss -tnlp | grep vsftpd               //vsftpd 地址  端口 pid
- netstat -tnlp | grep vsftpd

## for 空行的秘密
- IFS $'\n'                          //循环次数的分割方式
```
#!/usr/bin/bash
#ping check
> ip.txt
for i in {2..254}
do
      ip=125.124.15.$i
      ping -c1 $ip &>/dev/null
      if [ $? -eq 0 ]; then
      	echo "$ip" | tee -a ip.txt
      fi
done
#wait                         #等待所有的后台程序进行完毕
echo "finishi...."

```
- 从文件中获取ip
```
#!/urs/bin/bash
for ip in `cat ip.txt`
do
	ping -c1 -W1 $ip &>/dev/null
	if [ $? -eq 0 ];then
		echo "$ip is up "
	fi
done
```
- 实现批量用户创建
```
#!/usr/bin/bash
#######################
#   useradd           #
#v2.0 by kuibaobao    #
#######################
while :
do
	read -p "please enter prefix & passwd & num :" prefix passwd number
	printf "user infomation
	----------------------------------------
	user prefix : $prefix
	user passwd : $passwd
	user number : $number
	----------------------------------------
	"
	read -p "Are you sure?[y/n]:" action
	if [ "$action" = "y" ]; then
		break
	fi
done

for i in `seq -w $number`   
do
	user=$prefix$i
	id $user &>/dev/null
	if [ $? -eq 0 ]; then
		echo "user $user already exists"
	else
		useradd $user
		echo "$passwd" | passwd --stdin $user &>/dev/null
		if [ $? -eq 0 ]; then
			echo "$user is created."
		fi
	fi
done


```
- 文件中批量用户的创建
```
#!/usr/bin/bash

if [ $# -eq 0 ]; then
        echo "usage:`basename $0` file"
        exit 1
fi

if [ ! -f $1 ]; then
        echo "error file"
        exit 2
fi
#IFS=$'\n'
IFS='
'
for line in `cat $1`
do
	if [ ${#line} -eq 0 ]; then
		continue
	fi
        user=`echo "$line"|awk '{print $1}'`
        pass=`echo "$line"|awk '{print $2}'`
        id $user &>/dev/null
        if [ $? -eq 0 ];then
                echo "user $user already exists"
        else
                useradd $user
                echo "$pass" | passwd --stdin $user &>/dev/null
                if [ $? -eq 0 ]; then
                        echo "$user is created."
                fi
        fi
done

```
### 批量远程主机的ssh配置
- 简易版
```
#!/usr/bin/bash
#v1.0 by kuibaobao
for ip in `cat ip.txt`
do      
        {
                ping -c1 -W1 $ip &>/dev/null
                if [ $? -eq 0 ]; then
                        ssh $ip "sed -ri '/^#UseDNS/cUseDNS no' /etc/ssh/ssh_config"
                fi
                
        }&
done    
wait
echo "all ok..."
```
##  while until循环
- while循环创建用户
```
#!/usr/bin/bash
# while-useradd
#v1.0 by kuibaobao
while read line
do
	if [ ${#line} -eq 0 }; then
		continue
	fi
        user=`echo "$line"|awk '{print $1}'`
        pass=`echo "$line"|awk '{print $2}'`
        id $user &>/dev/null
        if [ $? -eq 0 ]; then
                echo "$user already exists"
        else
                useradd $user
                echo "$pass"|passwd --stdin $user &>/dev/null
                if [ $? -eq 0 ]; then
                        echo "$user is created."
                fi
        fi
done < user.txt                         
```
- while 测试远程主机连接
```
#!/usr/bin/bash
# 
#v1.0 by kuibbaobao
ip=125.124.77.132
while ping -c1 -W1 $ip &>/dev/null    //条件为真循环
do
        sleep 1
done
echo "&ip is down"

```
- until测试远程主机连接
```
#!/usr/bin/bash
# 
#v1.0 by kuibbaobao
ip=192.168.133.75
until ping -c1 -W1 $ip &>/dev/null   //测试条件为假时循环
do
        sleep 1
done
echo "&ip is up"
```
- for while until比较
  - for脚本
    ```
    #!/usr/bin/bash
    for i in {2..54}
    do
  	  {
  		  ip=192.168.122.$i
  		  ping -c1 -W1 $ip &>/dev/null
  		  if [ $? -eq 0 ];then
  			  echo "$ip up."
  		  fi 
  	  }&
    done
    wait
    echo "all finish"
    ```
  - while脚本
  ```
  #!/usr/bin/bash
  i=2
  while [ $i -le 254 ]; then
  do
	  {
	  ip=192.168.122.$i
	  ping -c1 -W1 $ip &>/dev/null
	  if [ $? -eq 0 ];then
		  echo "$ip up"
	  fi
	  }&
  let i++
  done
  wait
  echo "all finsh..."
  ```
  - until脚本
  ```
  #!/usr/bin/bash
  i=2
  until [ $i -gt 254 ]; then
  do
	  {
	  ip=192.168.122.$i
	  ping -c1 -W1 $ip &>/dev/null
	  if [ $? -eq 0 ];then
		  echo "$ip up"
	  fi
	  }&
  let i++
  done
  wait
  echo "all finsh..."
  ```
### Fd和命名管道实现shell并发控制
- 并发ping
```
#!/usr/bin/bash
#ping01
for i in {1..254}
do
	{
		ip=125.124.15.$i
		ping -c1 -W1 $ip &>/dev/null
		if [ $? -eq 0 ]; then
			echo "$ip is up"
		else
			echo "$ip is down"
		fi
	}&
done
wait
echo "all finish..."
```
- 并发创建用户
```
#!/usr/bin/bash
for i in {1..5}
do
        {
        user=qq$i
        id $user &>/dev/null
        if [ $? -ne 0 ]; then
                echo "$user is exist"
                continue
        else
                useradd $user
                if [ $? -eq 0 ]; then
                        echo "123"|passwd --stdin $user &>/dev/null
                        if [ $? -eq 0 ];then
                                echo "$user is creadted!!!"
                        fi
                fi
        fi
        }&
done
wait
echo "finish....."
```
### shell并发控制
- ll /proc/$$/fd         //文件描述符
- 系统修改的是文件描述符
- 删除的文件在文件句柄释放之前从/proc/$$/fd/中拷贝出来就可以
- exec 6<> /file1        //打开文件
- exec 6<&-              //关闭文件
- mkfifo /tmp/fifo1      //命名管道  管道先进先出
- grep 'sda' /tmp/fifo1  //在管道中获取
- ll /dev > /tmp/fifo1   //重定向到管道 
- echo >&6               // &6文件描述符6
- read -u 6              //从文件描述符6读取
```
#!/usr/bin/bash
#ping01
thread=8
mkfifo /tmp/$$.fifo
exec 8<> /tmp/$$.fifo
rm /tmp/$$.fifo

for i in `seq $thread`
do
        echo >&8
done

for i in {1..254}
do
        read -u 8          //从文件描述符读取
        {
                ip=125.124.15.$i
                ping -c1 -W1 $ip &>/dev/null
                if [ $? -eq 0 ]; then
                        echo "$ip is up"
                else
                        echo "$ip is down"
                fi
                echo >&8
        }&
done
wait
exec 8>&-
echo "all finish..."
```
### 批量改密码
```
#!/usr/bin/bash
>ok.txt
>fail.txt
read -p "please enter a new passwd: " pass

for ip in $(cat ip.txt)
do
	{	
	ping -c1 -w1 $ip &>/dev/null
	if [ $? -eq 0 ]; then
		ssh $ip "echo $pass |passwd --stdin root"
		if [ $? -eq 0 ]; then
			echo $ip >> ok.txt
		fi
	else
		echo $ip >> fail.txt
	fi
	}&
done

```
