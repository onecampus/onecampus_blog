+++
Description = ""
date = "2015-09-29T15:34:00+08:00"
menu = "main"
title = "Centos下搭建DNS服务器 by yunjiang"

+++

### 1.yum -y install bind 安装bind

### 2.vim /etc/named.conf 修改named.conf文件的options如下:
```
options {
        listen-on port 53 { any; };
        //listen-on-v6 port 53 { ::1; };
        directory       "/var/named";
        dump-file       "/var/named/data/cache_dump.db";
        statistics-file "/var/named/data/named_stats.txt";
        memstatistics-file "/var/named/data/named_mem_stats.txt";
        allow-query     { any; };
        recursion yes;

        dnssec-enable yes;
        dnssec-validation yes;
        dnssec-lookaside auto;

        /* Path to ISC DLV key */
        bindkeys-file "/etc/named.iscdlv.key";

        managed-keys-directory "/var/named/dynamic";
};
```

### 3.vim /etc/named.rfc1912.zones，在named.rfc1912.zones最下面添加

```
zone "ags.com" IN {
        type master;
        file "named.ags.com";
        allow-update {none;};
};
```

### 4.vim /var/named/named.ags.com 添加named.ags.com文件，文件内容如下：
```
$TTL 1D
@     IN SOA ags.com rname.invalid. (
                         0    ;  serial 
                        1D    ;  refresh
                        1H    ;  retry
                        1W    ;  expire
                        3H )   ;  minimum
      NS   @
      A    127.0.0.1
      AAAA ::1
CYYDMKE   IN A 192.168.1.10  //这里的ip要改为本机ip
```
##### 如此设置之后，访问的域名就会固定为 CYYDMKE.ags.com

### 5.配置路由器DNS地址（找到DHCP服务器，修改首选DNS服务器）