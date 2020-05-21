# 周期性任务
## 循环调度执行cron用户级
- systemctl status cron
- cron进程每分钟会处理一次计划任务
### 存储位置
- ls /var/spool/cron/

## 用户级的计划任务
### 管理方式
- crontab -l        //列出当前的计划任务
- crontab -r        //删除当前用户的所有计划任务
- crontab -e        //编辑
- 管理员可以使用-u username，去管理其他用户的计划任务
- /etc/cron.deny    在此文件一行写一个用户名，被写上的用户不能使用cron 
- echo "jack">/etc/cron.deny
- mail        d 1-50
- >/var/spool/mail/root
### 语法格式
Minutes Hours Day-of-Month Month Day-of-Week Command
星期和日期是或的关系
- 0 2 * * * /mysql_back.sh   //每天的两点执行
- 0 2 4 5 5 /my.sh           //5月4日或每周五两点整执行
- */5 * * * * /my.sh         //每隔5分钟执行
- 0 2 1,2,3 * * /my.sh       //每月的1号2号3号两点执行
- 0 2 1-9 * * /my.sh         //每月的1到9号两点执行

## 系统级的计划任务
- 临时文件的清理 /tmp /var/tmp
- 系统信息的采集sar
- 日志的轮转（切割）logrotate
- 通常不是由用户定义
- /etc/crontab
- 01 * * * * root run-parts /etc/cron.hourly     

## anacron
- 错过了重新执行
- /etc/anacrontab
- cat /var/spool/anacron/cron.daily
