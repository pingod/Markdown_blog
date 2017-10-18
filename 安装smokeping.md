## 1. 安装相关依赖
yum install rrdtool fping wget dig curl bind-utils  perl perl-Net-Telnet perl-Net-DNS perl-LDAP perl-libwww-perl perl-RadiusPerl perl-IO-Socket-SSL perl-Socket6 perl-FCGI perl-RRD-Simple  perl-CPAN rrdtool-perl perl-CGI-SpeedCGI perl-ExtUtils-MakeMaker

由于我是使有的nginx整合，需要安装：

yum -y install spawn-fcgi nginx

安装PHP、nginx、spawn-fcgi等基础支持软件
yum install -y openssh-clients telnet wget nginx php-cgi php-cli spawn-fcgi mysql-server mysql rrdtool net-snmp net-snmp-utils net-snmp-devel php-mysql php-snmp


如果使有apache，则需要安装如下包：
 yum install mod_fcgid httpd httpd-devel
 
 ## 2. 安装smokeping
 yum install smokeping
 
 ## 3. 修改配置文件
 
 ### 配置PHP管理器 
vim /etc/sysconfig/spawn-fcgi
SOCKET=/var/run/php-fcgi.sock
#OPTIONS="-u apache -g apache -s $SOCKET -S -M 0600 -C 32 -F 1 -P /var/run/spawn-fcgi.pid -- /usr/bin/php-cgi"
OPTIONS="-u nginx -g nginx -s $SOCKET -S -M 0600 -C 32 -F 1 -P /var/run/spawn-fcgi.pid -- /usr/bin/php-cgi"


 ### smokeping配置文件
 vim /etc/smokeping/config
 将cgiurl 设置为
 cgiurl   = http://10.180.120.183/smokeping.fcgi
 
 ### 配置nginx虚拟主机
 ```
  server {
        listen       80;
        server_name  localhost;
        location / {
            root   /usr/share/smokeping/htdocs/;
            index  index.html index.htm index.cgi;
        }
        location ~ .*\.fcgi$ {
            root  /usr/share/smokeping/htdocs/;
            fastcgi_pass   127.0.0.1:9007;
            include /opt/nginx/conf/fastcgi_params;
        }
}
```
## 启动相关服务
nginx 
nginx -t && nginx -s start

启动php管理器及设置开机启动服务 
/etc/init.d/spawn-fcgi restart
chkconfig spawn-fcgi on 

## 验证服务是否启动
