# 日志管理
## 日志管理基础
rsyslog 日志管理
logrotate 日志轮转
采集-------->分析

### 一、处理日志进程
- rsyslogd: 绝大部分日志记录，和系统操作有关，安全，认证sshd，速、计划任务at,cron...
- 日志可以存放在本地
- 日志可以存放在远程服务器
### 二、常见的日志文件（系统、进程、应用程序）
- tail /var/log/messages        //系统主日志文件
- tail -20 /var/log/messages     
- tail -f /var/log/messages     //动态查看日志文件的尾部
- tail /var/log/secure          //认证、安全
- tail /var/log/maillog         //跟邮件postfix相关
- tail /var/log/cron            //cron、at进程产生的日志
- tail /var/log/dmesg           //和系统启动相关
- tail /var/log/audit/audit.log //系统审计日志
- tail /var/log/apt.log         //yum
- tail /var/log/mysqld.log      //MySql
- tail /var/log/xferlog         //和访问FTP服务器相关
- w                             //当前登录的用户 /var/log/wtmp
- last                          //最近登录的用户 /var/log/btmp
- lastlog                       //所有用户的登录情况/var/log/lastlog

- eg 
  - grep 'fail' /var/log/secure | awk '{print $11}' | sort | unip -C| sort -k1 -n -r | hear -5
  - grep 'Accepted' /var/log/srcure | awk '{print $(NF-3)}' | sort | unip -C
  - grep -i 'mysql'            //忽略大小写
  - grep -i eth /var/log/dmesg //查看网卡是否被驱动
- tail /var/log/messages      //系统主日志文件
