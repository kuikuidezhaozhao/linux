# Linux-进程
## 进程
- 
- 进程是已经启动的可以执行的程序的运行实例，进程有一下组成部分
  - 已分配内存的地址空间;
  - 安全属性，包括所有权凭据和特权;
  - 程序代码的一个或多个执行线程;
  - 进程的状态;
  - 程序：二进制文件，静态/bin/date, /usr/sbin/httpd,/usr/sbin/sshd,/usr/local/nginx/sbin/nginx。
  - 进程：是程序运行的过程，动态，有生命周期及运行状态。
## 进程的生命周期
- 
- 父进程复制自己的地址空间（fork）创建一个新的（子）进程结构。每个新进程分配一个唯一的进程ID(PID)，满足跟踪安全性之需。PID和父进程ID(PPID)是子进程环境的元素，任何进程都可以创建子进程，所有进程都可以创建子进程，所有进程都是第一个系统进程的后代.
- 子进程继承父进程的安全性身份、过去和当前的文件描述符、端口和资源特权、环境变量，以及程序代码。随后，子进程可能exec自己的程序代码。通常，父进程在子进程运行期间处于睡眠（sleeping）状态。当子进程完成时发出（exit）信号请求，在退出时，子进程已经关闭或丢弃了其资源环境，剩余的部分称之为僵停（僵尸Zombie）。父进程在子进程退出时收到信号而被唤醒，清理剩余结构，然后继续执行自己的程序代码。
## 进程状态
- 
- 再多任务处理操作系统中，每个cup（或核心）在同一个时间点只能处理一个进程。在进程运行时，它对CUP时间和资源分配的要求会不断的变化，从而为进程分配一个状态，它随着环境要求而改变。
## 查看进程 snapshot--快照
- 静态查看进程ps *注*：ps -aux不同与ps aux
  - %MEM：内存占用率
  - VSZ：占用虚拟内存
  - `RSS`：占用实际内存 **驻留内存**
  - TTY：进程运行内存
  - STAT：进程状态 **man ps(/STATE)**
  - R  运行
  - S  可中断睡眠sleep
  - D  不可中断睡眠（usually IO）
  - T  停止的进程
  - Z  僵尸进程
  - X  死掉的进程
  - START：进程的启动时间
  - TIME：进程占CUP的时间
  - COMMAND：进程文件，进程名字
  - ps aux --sort %cup | less 升排列
  - ps aux --sort -%cup | less 降排列
  - ps auxf
  - ps -ef
  - ps axo user,pid,ppid,%mem,command 自定义显示字段
  - cat /run/sshd.pid   查看sshd的pid
  - ps aux | grep sshd  查看sshd的pid
  - pstree 进程树
- top   
  - top
  - top -d 1
  - top -d 1 -p 10126   查看指定进程的动态信息
  - top -d 1 -p 10126 1
  - top -d 1 -u apache  查看指定用户的进程
  
  - top -d 1 -b -n 2 >top.txt  将2次top信息写入到文件
## 信号控制进程
- 1---SIGHUP  重新加载配置  PID不变
- 2---SIGINT  键盘中断^C
- 3---SIGQUIT 键盘退出
- 9---SIGKILL 强制退出
- 15--SIGTERM 终止（正常结束），缺省信号
- 18--SIGCONT 继续
- 19--SIGSTOP 停止
- 20--SRGTSTP 暂停
- eg:  kill -1 PID号
- eg:  killall -1 服务名称
- pkill 
  - pattern---模式
  - pgrep -u root  -----c查看用户root的进程
  - daemon----守护进程
  - pkill -HUP syslogd
  - pkill -t pts/2         //终止终端tps/2上所有进程
  - pkill -9 -t pts/2      //终止终端tps/2上所有进程,并结束该pts/2
  - pdeudo---虚伪的

