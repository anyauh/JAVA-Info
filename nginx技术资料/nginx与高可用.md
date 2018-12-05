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
#### keepalived conf 配置文件
```
! Configuration File for keepalived

global_defs {
     router_id localhost
}


vrrp_script chk_mysql_port {     #检测mysql服务是否在运行。有很多方式，比如进程，用脚本检测等等
    script "/opt/nginx_check.sh"   #这里通过脚本监测
    interval 2                   #脚本执行间隔，每2s检测一次
    weight -5                    #脚本结果导致的优先级变更，检测失败（脚本返回非0）则优先级 -5
#    fall 2                    #检测连续2次失败才算确定是真失败。会用weight减少优先级（1-255之间）
#   rise 1                    #检测1次成功就算成功。但不修改优先级
}

vrrp_instance VI_1 {
    state MASTER
    interface ens33      #指定虚拟ip的网卡接口 eth0需要更换为自己的
    mcast_src_ip 192.168.200.128   #自己的ip
    virtual_router_id 121  #路由器标识，MASTER和BACKUP必须是一致的
    priority 101            #定义优先级，数字越大，优先级越高，在同一个vrrp_instance下，MASTER的优先级必须大于BACKUP的优先级。这样MASTER故障恢复后，就可以将VIP资源再次抢回来
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        192.168.200.111    #vip ip,虚拟的
    }

track_script {
   chk_mysql_port
  }
}
```

#### nginx检测脚本
```
#!/bin/bash
echo "ok" >> /tmp/echo.log
A=`ps -C nginx --no-header |wc -l`
if [ $A -eq 0 ];then
    /usr/local/nginx/sbin/nginx
    sleep 2
    if [ `ps -C nginx --no-header |wc -l` -eq 0 ];then
        killall keepalived
    fi
fi
```
**nginx脚本赋予执行权限**
```
 [root@localhost opt]# chmod +755 nginx_check.sh 
```

