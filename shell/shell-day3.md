# shell可以用 bash -vx **.sh进行调试
## 变量的赋值
- 1、显示赋值
  - 变量名=变量值
    - ip1=192.168.1.251
    - school="BeiJing 1000phone"
    - today1=`date +%F`
    - today2=$(date+%F)
- 2、read从键盘读入变量值
  - read 变量名
  - read -p "提示信息" 变量名
  - read -t 5 -p "提示信息" 变量名     //5秒内输入
  - read -n 2 变量名     //只要前两个字符
    - eg：
    ```
    #!/usr/bin/bash
    read -p " please input a b c " a b c 
    ```
    - shengyu=`df -Ph |grep '/$' | awk '{print $4}'`
    - shengyu=$(df -Ph |grep '/$' | awk '{print $4}')
## 变量运算
- 1、整数运算 + - * / %
  - 方法一：expr
    - expr 1+2
    - expr $num1 + $num2
    - sum `expr $num1 + $num2`
  - 方法二：$(())
    - echo $(($num+$num2))
    - echo $((num1+num2))
    - echo $((2**10))             //2的10次方
  - 方法三 $[]
    - 同方法二
  - 方法四：let
    - let sum=1+1
    - let i++
```
#!/usr/bin/bash
# mem_used=`free -m |grep '^Mem'| awk '{print $3}'`
# mem_total=`free -m |grep '^Mem'| awk '{print $2}'`
# mem_percent=$((mem_used*100/mem_total))
# echo $mem_percent


#重新 开始
ip=125.124.15.132
i=1
while [ $i -le 5 ]
do
	ping -c1 $ip &>/dev/null
	if [ $? -eq 0 ]; then
		echo "$ip is up..."
	fi
	let i++
done

```
## 小数运算
- echo "2*4" |bc
- echo "2**4" |bc
- echo "scale=2;6/4" |bc
- echo 'BEGIN{print 1/2}'
- echo "print 5.0/2" | python
