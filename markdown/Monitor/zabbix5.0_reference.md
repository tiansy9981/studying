

# zabbix5.0 安装指南

##  一、安装repo源

修改epel源，关闭epel对zabbix包的支持

```
[epel]
...
excludepkgs=zabbix*
```

获取zabbix源

```
rpm -Uvh https://repo.zabbix.com/zabbix/5.0/rhel/7/x86_64/zabbix-release-5.0-1.el7.noarch.rpm
# yum clean all
```

## 二、安装zabbix相关包

```
yum install zabbix-server-mysql zabbix-agent

yum install centos-release-scl
```

修改repo文件开启前端包enable

```
[zabbix-frontend]
...
enabled=1
...
```

```
#安装前端相关包
yum install zabbix-web-mysql-scl zabbix-apache-conf-scl
```



## 三、安装mysql

> 注意zabbix对数据库版本是要求以及对加密方式的要求

```
# mysql -uroot -p
password
mysql> create database zabbix character set utf8mb4 collate utf8mb4_bin;
mysql> create user zabbix@localhost identified by 'password';
mysql> grant all privileges on zabbix.* to zabbix@localhost;
mysql> set global log_bin_trust_function_creators = 1;
mysql> quit;
```

## 四、导入数据

```
zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix
```

## 五、修改配置文件

修改 /etc/zabbix/zabbix_server.conf文件，设置对应的数据库密码

```
DBPassword=password
```

### 1、nginx版本

修改/etc/opt/rh/rh-nginx116/nginx/conf.d/zabbix.conf，取消注释并设置端口跟server_name

```
# listen 80;
# server_name example.com;
```

修改/etc/opt/rh/rh-nginx116/nginx/nginx.conf中的root目录与/etc/opt/rh/rh-nginx116/nginx/conf.d/zabbix.conf的root目录一致

修改修改/etc/opt/rh/rh-php72/php-fpm.d/zabbix.conf中的时区，取消注释，并修改acl的用户添加nginx

```
; php_value[date.timezone] = Europe/Riga
listen.acl_users = apache,nginx
```

> 注意：相关组与用户的创建以及对应文件的权限设置

### 2、apache版本

修改/etc/opt/rh/rh-php72/php-fpm.d/zabbix.conf中的时区，取消注释；

```
; php_value[date.timezone] = Europe/Riga
```

## 六、启动服务

```
systemctl restart zabbix-server zabbix-agent httpd php-fpm
systemctl enable zabbix-server zabbix-agent httpd rh-php72-php-fpm
```

