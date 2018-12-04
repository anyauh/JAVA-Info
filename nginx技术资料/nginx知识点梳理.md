<!-- MarkdownTOC -->
- [HTTP服务器](#HTTP服务器)
- [反向代理](#反向代理)
    - [1.什么是反向代理](#什么是反向代理)
    - [2.nginx.conf文件配置](#nginx.conf文件配置)
- [负载均衡](#负载均衡)
    - [1.什么是负载均衡](#什么是负载均衡)
    - [2.负载代码配置](#负载代码配置)
    - [3.服务器宕机检查](#服务器宕机检查)
- [防盗链](#防盗链)
<!-- MarkdownTOC -->

# nginx 功能
## HTTP服务器
## 反向代理
### 什么是反向代理
   1、将外网请求转发内网服务器,隐藏内部服务器IP

### nginx.conf文件配置
   1、配置拦截服务名称 server_name,配置反向代理服务器地址proxy_name
   ```
   server {
        listen       80;
        server_name www.anyauh.com;# nginx拦截服务名称

        location / {
            #root   html;
			proxy_pass http://127.0.0.1:8088;#代理服务器地址
            index  index.html index.htm;
	
         }
       }
   ```
     
## 负载均衡
### 什么是负载均衡
   1、减轻单台服务器压力，提供系统服务能力,是高并发的最基本解决方案
### 负载代码配置
   1、配置拦截服务名称 server_name 
```
      server {
           listen       80;
           server_name www.anyauh.com;
           location / {
               #root   html;
   					proxy_pass http://anyauh;
               index  index.html index.htm;
   			proxy_connect_timeout 1;
   			proxy_send_timeout 1;
   			proxy_read_timeout 1;
            }
       }
```
### 服务器宕机检查
```
 server {
        listen       80;
        server_name www.anyauh.com;

        location / {
            #root   html;
					proxy_pass http://anyauh;
            index  index.html index.htm;
			proxy_connect_timeout 1;# 表示1s 超时
			proxy_send_timeout 1;
			proxy_read_timeout 1;
        }
    }
```

## 防盗链
```
location ~ .*\.(jpg|jpeg|JPG|png|gif|icon)$ {
        valid_referers blocked http://www.itmayiedu.com www.itmayiedu.com;
        if ($invalid_referer) {
            return 403;
        }
		}

```
