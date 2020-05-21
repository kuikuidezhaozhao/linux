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
## 进程优先级nice
- linux进程调度及多任务
  - 每个CPU（或CPU核心）在一个时间节点上只能处理一个进程，通过时间片技术，linux实际能够运行的进程（和线程数）可以超出是实际可用的CPU及核心数量。linux内核进程调度程序将多个进程在CPU核心上快速切换，从而给用户多个进程在同时运行的印象。
- 相对优先级nice
  - 由于不是每个进程都与其他进程同样重要，可告知进程调度程序为不同的进程使用不同的调度策略。常规系统上运行的大多数进程所使用的调度策略运行的进程为SCHED_OTHER(也称为SCHED_NORMAL)，但还有其他的一些调度策略用于不同的目的。
  - SCHED_OTHER调度策略运行的进程的相对优先级称为进程的nice值，可以有40种不同的级别的nice值。
    - nice值越高：表示优先级越低
    - nice值越低：表示优先级越高
- 查看进程的nice级别
  - 1、使用top查看nice的级别
    - NI：实际nice级别
    - PR：将nice级别显示为映射到更大优先级队列，-20映射到0,+19映射到39.
  - 2、使用ps查看nice级别
    - ps axo pid,command,nice --sort=-nice  降序
    - ps axo pid,command,nice,cls --sort=-nice
    - TS表示该进程使用的的调度策略为SCHED_OTHER
    - nice -n -20 sleep 8000 &  启动服务时设置进程的nice为-20
    - renice -20 pid号  修改进程的nice值
## 作业控制jobs
- foreground：前台进程是在终端中运行的命令，该终端为进程的控制终端。前台进程接受键盘产生的输入和信号，并允许从终端读取或写入到终端。
- background：后台进程的没有控制终端，它不需要终端的交互
  - sleep 3000 &   ------运行程序时，让其在后台执行
  - sleep 4000     ------^Z将前台的程序挂起（暂停）到后台
  - jobs           ------查看后台作业
  - bg %2          ------让作业2在后台运行
  - fg %1          ------将作业1调回到前台
  - kill %1        ------kill 1,终止PID是1的进程
- screen 
  - screen  --list
  - screen -r  
  - screen -S  自己设置的名字
