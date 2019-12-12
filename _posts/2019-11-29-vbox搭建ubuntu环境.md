---
layout: post
---

- 修改时区,

```
1. tzselect
2. 选择亚洲
3. 选择china
4. 选择北京时间
5. cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
6. date -s hh:mm:ss 修改时间
7. date -s mm/dd/yyyy 修改日期
8. hwclock --systohc
```

- 网络配置

```
首先关闭ubuntu防火墙, 自己用, 可以不是使用, 
打开宿主机防火墙, ubuntu不可以ping宿主机
桥接模式, 让linux可以连接外网, 方便后续的下载更新操作,
1. 安装镜像ubuntu_19.04,
2. 配置网络, 添加桥接网卡,
3. 设置网卡, 通过netplan配置, 注意网关地址和ip地址段

vbox-host-only模式, 通过此模式, 可以让局域网没有连接外网的情况下可以互相ping通,包括宿主机和虚拟机的交互, 通过宿主机ssh连接linux,
1. 在vbox中添加网卡,配置ip, 取消dhcp,
2. 配置网络, 添加hostonly模式网卡,
3. linux中设置网卡地址, 通过netplan设置,
```

- 安装ssh

```
1. 国外软件源连接慢, 此处使用的阿里源,   /etc/apt/sources.list
2. 更换地址为http://mirrors.aliyun.com/ubuntu,
3. apt-get update 
4. 安装ssh,  apt-get install openssh-server
5. 安装客户端, 后续免密登录别的服务器需要,
6. apt-get install openssh-client
7. 配置免密登录
8. ssh-keygen -t rsa -P ""
9. cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
10. 需要将自己的公钥传到其他服务器并添加到authorized_keys中
10. ssh localhost 验证
```

- 根分区扩容

```
1. lvs 列出逻辑卷
2. pvcreate /dev/sda 创建物理卷
3. pvscan 查看创建的物理卷
4. pvdisplay 查看创建的物理卷的详细信息
5. vgdisplay 查看详细信息
6. df -h 查看详细信息
7. lvextend -L +xxxG /dev/....   扩容根分区
8. resize2fs 卷名    使扩容生效
```

- 安装jdk

```
1. 下载jdk的linux版本
2. 解压jdk,放到根目录下的某个文件夹,
3. 配置环境变量, 可以使用全局变量或者单个用户配置,
4. export JAVA_HOME=/comm_software/jdk/jdk1.8.0_201
5. export PATH=$JAVA_HOME/bin:$PATH
6. export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
```

- alien

```
转换rpm包和deb包
alien -k --scripts xxx.rpm    生成一个deb包
dpkg -I --instdir=/comm_packages/oracle/ xxx.deb 安装
```

- redis集群搭建

```
1. 使用四主四从
2. 安装gcc
3. make 编译redis
4. make PREFIX=/...... install安装
5. 配置cluster
6. bind 0.0.0.0
7. port 
8. daemonize yes 后台启动
9. cluster-enabled yes
10. cluster-config-file nods_7002.conf
11. cluster-node-timeout 15000
12. cluster-require-full-coverage no
13. logfile
14. dir 数据目录
15. dbfilename dump文件
16. redis-server .conf启动
17. redis-cli -p port cluster meet ip port 加入集群
18. redis-cli -p port1 cluster replicate id port1作为id的从机
19. 分配hash槽
20. redis-cli -p port cluster addslots $i

    #! /bin/bash
    start=$1
    end=$2

    for ((i=$start;i<=$end;i++));
    do
         /comm_software/redis/redis-5.0.7/bin/redis-cli -p $3 cluster addslots $i
    done
集群搭建完成, cluster命令自行查看

```

- mysql 单机安装卸载

```
1. sudo apt-get update
2. sudo apt-get install mysql-server
3. sudo apt-get autoremove --purge mysql-server
4. sudo apt-get remove mysql-server
5. sudo apt-get autoremove mysql-server
6. sudo apt-get remove mysql-common
7. dpkg -l| grep ^rc
```

- mysql集群安装

```
1. 使用三台机器host01, host02, host03
2. host01, host02 双主同步, 主备切换, host03作为另外一台
3. 更改mysql.conf.d/mysqld.cnf, 配置主从同步,
4. reset master
5. show master status
6. show slave status \G
7. change master to
8. master_host = '192.168.56.11',
9. master_port = 3306,
10. master_user = 'sync',
11. master_password = '123456',
12. master_log_file = 'host02.000001',
13. master_log_pos = 154;
14. start slave
15. myat是基于ali的cobar
16. mycat可以读写分离, 主备热切换, 分库分表,
17. mycat高可用集群以后再搭建, 目前来看, 一个mycat就够用
```

- zookeeper集群安装

```
2181  客户端连接端口
2888
2889  zk选举端口
1. zookeeper解压,
2. 添加日志目录, 
    /var/log/zookeeper/
    /var/log/zookeeper/datalog
    /var/log/zookeeper/trace
3. 修改主机名, /etc/hostname
4. 修改hosts,
5. 添加myid(zoo配置文件中的server.x中的x), 只需配置一个数字就可以,
6. 配置zoo.cfg, 添加
    dataDir=
    dataLogDir=
    server.myid=ip:port:port
```

- kafka集群

```
1. 修改配置文件config/server.properties,
2. brokenid,
3. listens地址
2. zookeeper集群地址
3. 启动kafka-server-start.sh config/server.properties
```

- hadoop

```
1. 配置hosts文件
2. 配置免密登录
3. 配置hadoop
4. 分发到其他节点
5. hadoop namenode -format, 在启动之前需要格式化namenode
6. start-dfs.sh
7. start-yarn.sh
8. 后续需要使用start-all.sh
9. stop-all.sh

```

- 安装docker(此处之后再说)

```
1. 
```

- 不同机器shell脚本互通

```
ssh cosy-sun@host02 > /dev/null 2>&1  <<remote
cd ~
touch txt1.txt
exit
remote 
echo done!
```