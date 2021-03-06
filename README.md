  
## 安装OTRS工单管理系统  
Open-source Ticket Request System

**1）OTRS简介**  
1.OTRS的名字是由Open-source Ticket Request System首字母縮略字而来，是一个开源的缺陷跟踪管理系统软件。  
2.OTRS将电话，邮件等各种渠道提交进来的服务请求归类为不同的队列，服务级别，服务人员通过OTRS系统来跟踪和回复客户，相对传统的处理流程来而言，OTRS提供了一个部门或团队的协调环境，以更有效率的方式处理，查询和跟踪。  
3.OTRS是Lisog德国开源非盈利性发展协会创始成中之一。在2010 年被评选为infoworld年度十佳开源网络软件。  
4.OTRS的ITSM 第一个符合ITIL的IT服务管理解决方案，是建立在开放源代码的基础上。这是一个兼容的开源ITIL的IT服务管理ITSM 解决方案。  
  
  
**2）OTRS包括以下几个特点**  
1.能支持平台非常广。操作系统有Linux、Unix还有Windows；数据库有MySQL，PostgreSQL，Oracle和SQL Server。    
2.安装和配置是相当的简单。我使用过Centos Linux和Windows 7 ，整个安装配置过程只需要10分钟。  
3.支持多语言，目前能支持的语言有10几种，包括简繁体中文。  
4.纯Web操作界面，Web界面可以定制；很好的邮件系统集成。有问题单生成接口，能够将第三方网络系统监控的故障告警变成问题单，再自动分配到相关的维护组。  


**3）环境**  
```shell  
[root@iZ8vb6tda6e8mxu62r0okfZ ~]# sestatus 
SELinux status:                 disabled
[root@iZ8vb6tda6e8mxu62r0okfZ ~]# systemctl stop firewalld
[root@iZ8vb6tda6e8mxu62r0okfZ ~]# systemctl disable firewalld
[root@iZ8vb6tda6e8mxu62r0okfZ ~]# cat /etc/redhat-release 
CentOS Linux release 7.5.1804 (Core)
```

**4）安装数据库**
```shell
[root@iZ8vb6tda6e8mxu62r0okfZ ~]# yum -y install mariadb-server
[root@iZ8vb6tda6e8mxu62r0okfZ ~]# cat /etc/my.cnf.d/zotrs.cnf 
[mysqld]
max_allowed_packet   = 20M
query_cache_size     = 32M
innodb_log_file_size = 256M
[root@iZ8vb6tda6e8mxu62r0okfZ ~]# systemctl start mariadb
[root@iZ8vb6tda6e8mxu62r0okfZ ~]# netstat -lntp|egrep 3306
tcp        0      0 0.0.0.0:3306            0.0.0.0:*               LISTEN      2032/mysqld
[root@iZ8vb6tda6e8mxu62r0okfZ ~]# systemctl enable mariadb
Created symlink from /etc/systemd/system/multi-user.target.wants/mariadb.service to /usr/lib/systemd/system/mariadb.service.
```

**5）安装OTRS**
```shell
[root@iZ8vb6tda6e8mxu62r0okfZ ~]# wget https://ftp.otrs.org/pub/otrs/RPMS/rhel/7/otrs-6.0.27-02.noarch.rpm
[root@iZ8vb6tda6e8mxu62r0okfZ ~]# yum localinstall otrs-6.0.27-02.noarch.rpm -y  #130多个依赖包，里面包含httpd和大量perl模块
[root@iZ8vb6tda6e8mxu62r0okfZ ~]# systemctl start httpd.service
[root@iZ8vb6tda6e8mxu62r0okfZ ~]# systemctl enable httpd
Created symlink from /etc/systemd/system/multi-user.target.wants/httpd.service to /usr/lib/systemd/system/httpd.service.
[root@iZ8vb6tda6e8mxu62r0okfZ ~]# /opt/otrs/bin/otrs.CheckModules.pl #检查缺失的perl模块
[root@iZ8vb6tda6e8mxu62r0okfZ ~]# /opt/otrs/bin/otrs.CheckModules.pl &>check.txt #提示Not installed!的 使用提示的yum install "perl(Text::CSV_XS)"去安装。
[root@iZ8vb6tda6e8mxu62r0okfZ ~]# grep -o 'yum.*"' check.txt|sed 's#.*#& -y#g'|bash
[root@iZ8vb6tda6e8mxu62r0okfZ ~]# su - otrs -c "/opt/otrs/bin/otrs.Daemon.pl start"
[root@iZ8vb6tda6e8mxu62r0okfZ ~]# su - otrs -c "/opt/otrs/bin/Cron.sh start"
https://127.0.0.1/otrs/installer.pl
```






