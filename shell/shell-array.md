# 数组
### Sort (分类)
- 普通数组 (ordinary array)
- 定义数组 (define array)
    - 方法一：一次赋一个值
    - 数字名[下标]=变量值
    - array[0]=pear	
    - array[1]=peach

    - 方法二：一次赋值多个值
    - array=(tom jack alxe)
    - array= (`cat /etc/passwad`)           //分隔符控制
    - array=($blue $red)
    - array=(1 2 3 4 5 "linux shell" [20]=bash)
    - 查看数组 (vive array)
    - declare -a
  - 访问数组元素 
    - echo ${!array[@]}       //Get index of array element 获得数组元素的索引
    - echo ${array[@]}        //访问数组中的所有元素，等同于echo ${array[*]}
    - echo ${#array[@]}       //访问数组元素的个数
    - echo ${array[0]}        //访问数组的第一个元素
    - echo ${array[@]:1}      //从数字下标1开始
    - echo ${array[@]:1:2}    //从数组下标1开始访问两个元素

- 关联数组
- 定义数组
  - 申明关联数组变量
  - declare -A array1
  - declare -A array2

  - 方法一：一次赋一个值
  - array[index1]=pear
  - array[indexx]=orange

  - 方法二：一次赋多个值
  - array=([index1]=tom [index2]=jiack)

- 查看数组
  - declare -A

- 访问数组
  - echo ${array[index2]}


## 数组的赋值和遍历
- 数组的赋值(array-valued)
```
#!/bin/bash
while read line
do
	hosts[++i]=$line
done </etc/hosts

echo "hosts first: ${hosts[1]}"
echo

for i in ${!hosts[@]}
do
	echo "$i : ${hosts[$i]}"
done
```
- for赋值遍历（赋值assignment）
```
#!/bin/bash
OLD_IFS=$IFS
IFS=$'\n'
for line in `cat /etc/hosts`
do
	hosts[++i]=$line
done

echo "hosts "

for i in ${!arrar[@]}
do
	echo "$i : ${array[$i]}"
done
IFS=$OLD_IFS
```
- awk '{print $2}' sex.txt |sort |uniq -c
## 项目
- 区别array[m]++和array+=([m]=1),前者是给数组的m的值加一，后者为给数组加一个为m的新元素
```
#!/bin/bash
# count sex
# v1.0 by tianyun
declare -A sex

while read line
do
	type=`echo $line |awk '{print $2}'`
	let sex[type]++
done < sex.txt

```
- 统计不同类型杀的shell数量
- awk -F":" '{print $NF}' /etc/paaawd | sort |uniq -c
```
#!/bin/bash
# count shells
declare -A shells
while read line
do
	type=`echo $line | awk -F":" '{print $NF}' `
	let shells[$type]++
done</etc/passwd

for i in ${!shells[@]}
do
	echo "$i: ${shells[$i]}"
done

```

- 统计ttcp链接状态数量

