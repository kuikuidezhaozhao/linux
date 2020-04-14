# 重定向
```
- ulimit -a 
- ls /proc/$$/fd -l  //查看文件打看的文件
- 每创建一个进程都会有一个pid 和ppid，每打开一个文件都会有一个文件描述符
- date 1>date.txt  //对标准输出的重定向  1==stdout
- date 2>date.txt  //对标准错误的重定向  2==stderr
- cat 0< /etc/hosts  //对标准输入的重定向 0==stdin
```
- rm /dev/null     删除/dev/null
- mknod -m 666 /dev/null c 1 3    创建/dev/null
- 重启创建/dev/null
- ping c1 192.168.1.1 & > /dev/null
- >重定向
- >>追加
## 重定向举例
### 设备文件
- 主设备号相同：表示为同一种设备类型，也可以认为keme使用的是下共同的驱动
- 从设备号：在同一设备类型中的一个序号
-快设备和字符设备的区别：块设备可以有缓存，字符设备没有缓存

### 用重定向建立多行的文件
- echo "111">man.txt
- cat > man.txt   //把cat输出重定向
- cat >> man.txt  //如果原有man.txt文件追加在内容后面，如果原本没有man.txt文件>>与>的效果相同
- cat > man.txt <<EOF   //输入EOF结束

### 脚本中利用重定向打印消息
```
cat <<-EOF
利用脚本输出本句话
EOF
```
- (ls;date) &>/dev/null
- tailf 文件名    //刷新看文件
- subshell
  - (ls)与ls区别是(ls)是在subshell（子shell）中进行
  - 如不希望某些命令的执行对当前shell环境产生影响，请在subshell中执行，（eg：umask 777：touch man.txt)

