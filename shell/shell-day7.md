# Expect实现scp非交互传输件
- expect-01
```
#!/usr/bin/expect
spawn ssh 125.124.15.132
expect {
	"yes/no" { send "yes\r";exp_continue }
	"password" {send "password"}
}
# interact
expect "#"
send "ls\r"
send "pwd\r"
send "exit\r"
expect eof
```
- spawn scp -r /etc root@125.124.15.132:/tmp   
- ssh-keygen -P "" -f ~/.ssh/id_rsa
## expect实现批量主机公钥推送
```
#!/usr/bin/bash
>ip.txt
password=centos

rpm -q expect &>/dev/null
if [ $? -ne 0 ];then
	echo "install expect"
	yum -y install expect &>/dev/null
fi

if [ ! -f ~/.ssh/id_rsa ]; then
	ssh-keygen -P "" -f ~/.ssh/id_rsa
fi

for i in {78..254}
do
	{
	ip=192.168.122.$i
	ping -c1 -W1 $ip &>/dev/null
	if [ $? -eq 0 ]; then
		echo "$ip" >> ip.txt
		/usr/bin/expect <<-EOF
		set timeout 10
		spawn ssh-copy-id $ip
		expect {
			"yes/no" { send "yes\r"; exp_continue }
			"password" { send "$password\r";exp_continue }
		}
		expect eof
		EOF
	fi
	}&
done
wait
echo "finish...."

```
