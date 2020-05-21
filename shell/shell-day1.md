# shell
- ping -c1 www.baidu.com && echo "成功" || echo "失败"
- ping -c1 www.baidu.com &>/dev/null && echo "成功" || echo "失败"
## 步骤
- 编写***.sh文件
- bash ***.sh 或 sh ***.sh
- ping -c1 www.baidu.com && echo "成功" || echo "失败"
- chmod +x ***.sh  //给***.sh加上执行权限，在绝对路径执行脚本的时候用
- #!/usr/bin/bash       //脚本用什么解释器解释
- 一定注意，不同的程序用不同的解释器
## bash中调用python Expect
- /usr/bin/python << -EOF
- EOF
- /usr/bin/expect << -EOF
- EOF
-               //本行忽略为了>,为了平衡markdown语法而用
## 执行方式
- ./bash.sh        //在子shell中执行     
- bash bash.sh
- . bash.sh        //在当前的shell中执行
- source bash.sh  
## shell特性
- login shell
  - su - alice
    - /etc/profile
    - /etc/bashrc
    - ~/.bash_profile
    - ~/.bashrc
- nologin shell
  - su alice
    - /etc/bashrc
  - ~/.bashrc
## bash shell特点
- 命令和文件自动补齐
- 命令历史记忆功能   上下键、！nummber、！string、！$、！！、^R
- 别名功能 alias、unalias
- 前后台作业控制 &、nohup、^C、^Z、bg%1、fg%1、kill%3、screen
- 输入输出重定向0,1,2 > >> 2> 2>> 2>&1 &> cat</etc/hosts  cat<<EOF cat>file1<<EOF cat</etc/1 >/etc/2
- 管道 | tee
  - ip addr | grep 'inet' | grep eth0
  - ip addr | grep 'inet' | tee test | grep eth0         /覆盖
  - ip addr | grep 'inet' | tee -a test | grep eth0      /-a追加
  - date > date.txt 
  - date | tee date.txt
- 命令排序
- ;
- &&     前一个命令成功后面的才进行
- ||     前一个命令失败后面的才进行
## shell的通配符（元字符）
- *匹配任意多个字符
- ？匹配一个字符
- []匹配括号中的任意任意一个[abc] [a-z] [0-9] [^a-z] [^a-z0-9]
- ()在子shell中执行(cd /boot;ls)
- {}集合touch file{1..9}
  - mkdir /home/{111,222}
  - mkdir -pv /home/{333/{aaa,bbb},444}
  - cp -rv /etc/apt/a /etc/apt/a.old
  - cp -rv /etc/apt/{a,a.old}
  - cp -rv /etc/apt/a{,.old}
- \转义字符
-
## 变量
## shell script脚本
## echo带颜色的输出
- echo -e "\e[1;31m This is a red text"
- echo -e "\e[1;31m This is a red text.\e[0m"
## printf输出
```
root@xuexi tmp]# cat >abc.sh<<eof  # 将下面的内容覆盖到abc.sh脚本中
> #!/bin/bash
> #文件名：abc.sh
> printf "%-5s %-10s %-4s\n" No Name Mark     # 三个%分别对应后面的三个参数
> printf "%-5s %-10s %-4.2f\n" 1 Sarath 80.34 # 减号“-”表示左对齐
> printf "%-5s %-10s %-4.2f\n" 2 James 90.998 # 5s表示第一个参数占用5个字符
> printf "%-5s %-10s %-4.2f\n" 3 Jeff 77.564
> eof
```
