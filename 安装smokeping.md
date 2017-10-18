## 1. 安装相关依赖
yum install rrdtool fping wget dig curl bind-utils gcc make perl perl-Net-Telnet perl-Net-DNS perl-LDAP perl-libwww-perl perl-RadiusPerl perl-IO-Socket-SSL perl-Socket6 perl-FCGI perl-RRD-Simple  perl-CPAN rrdtool-perl perl-CGI-SpeedCGI perl-ExtUtils-MakeMaker

由于我是使有的nginx整合，需要安装：

yum -y install spawn-fcgi

如果使有apache，则需要安装如下包：
 yum install mod_fcgid httpd httpd-devel
 
 ## 2. 安装smokeping
 yum install smokeping
 
 ## 3. 修改配置文件
 
 ### smokeping配置文件
 vim /etc/smokeping/config
 将cgiurl 设置为
 cgiurl   = http://10.180.120.183/smokeping.fcgi
 
 ### 
