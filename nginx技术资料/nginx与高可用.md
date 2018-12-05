## linux安装Nginx及相关组件

### openssl安装
```
[root@localhost src]# tar -zxvf openssl-fips-2.0.1.tar.gz
[root@localhost openssl-fips-2.0.1]# ./config && make && install
```

### pcre安装
```
[root@localhost src]# tar -zxvf pcre-8.42.tar.gz
[root@localhost src]# cd pcre-8.42
[root@localhost pcre-8.42]# ./configure && make && make install

```
### zlib安装
```
[root@localhost src]# tar -zxvf zlib-1.2.11.tar.gz
[root@localhost zlib-1.2.11]# ./configure && make && install
```
### nginx安装
```
[root@localhost nginx-1.15.7]# ./configure --prefix=/usr/local/nginx
[root@localhost nginx-1.15.7]# make && make install

 ```

### 开启防火墙端口
centos 7
```
[root@localhost local]# firewall-cmd --zone=public --add-port=8088/tcp --permanent
[root@localhost local]# firewall-cmd --reload

```

### keepalived高可用
```
[root@localhost src]# tar -zxvf keepalived-2.0.10.tar.gz 
[root@localhost keepalived-2.0.10]# ./configure && make && make install
[root@localhost keepalived-2.0.10]# mkdir /etc/keepalived
[root@localhost keepalived]# cp /usr/local/keepalived/etc/keepalived/keepalived.conf /etc/keepalived/keepalived.conf
[root@localhost keepalived]# cp /usr/local/keepalived/etc/sysconfig/keepalived /etc/keepalived/sysconfig/keepalived

```
