# 文件打包及压缩
## 打包，压缩
- tar -czf etc1.tar.gz /etc   //-z调用gzip    : tar removeing 非报错
- tar -cjf etc2.tar.bz2 /etc  //-j调用bzip2
- tar -cJf etc3.tar.xz /etc   //-J调用xz
- ll -h etc*
## 解压，解包
- tar -tf                              //查看包
- tar -xf                              //自动判断压缩格式进行解压
- tar -xzf                             //解压gz格式进行解压
- tar -xvf                             //自动判断压缩格式进行解压
- tar -xvf 压缩文件 -C 自定义目录      
- zip解压
  - upzip xxx.zip
## 例子
- date +%F
- mysql物 理 备 份 及 恢 复
  - tar -cJf /backup/mysql.tar.xz /var/lib/mysql
  - rm -rf /var/lib/mysql/*
  - tar -xf /backup/mysql.tar.xz -C /
- mysql物 理 备 份 及 恢 复
  - cd /var/lib/mysql
  - tar -cJf /backup/mysql.tar.xz *
  - tar -xf /backup/mysql.tar.xz -C /var/lib/mysql
- 海量小文件
  - 本地
    - time tar -czf - /etc - |tar -xzf - -C /temp    //czf后面的-意思是不写到硬盘，只在内存中,表示是内存中
  - 远程：
    - scp -r /etc 176.16.20.21:/tmp     //不建议用，是散文件，传输的很慢
    - 建议的方式
      - host B 监听端口
        - 1、systemctl stop firewalld.service       //关闭防火墙，如果因为防火墙缘故出错建议进行这个命令
        - 2、firewall-cmd --permanent --add-port=8888/tcp   //1或（2和3）随便选择一种   
        - 3、firewall-cmd --reload
        - nc -l 8888 |tar -xzf - -C /tmp
      - host A
        - tar -caf - /etc | nc 172.16.20.21 8888

## 注：
- 再传属的时候尽量不要在磁盘上面，磁盘比较慢，尽量在内存中进行（也就是在打包解压中使用 - ）
