<!-- MarkdownTOC -->
- [HTTP服务器](#一-HTTP服务器)
- [反向代理 ](#二-反向代理)
  - [1.功能作用](#功能作用)
  - [2.配置](#文件配置)
- [负载均衡 ](#三-负载均衡)
  - [1.功能作用](#功能作用)
  - [2.配置](#文件配置)
  - [3.服务器宕机超时](#服务器宕机检查)
- [防盗链](#四-防盗链)
  - [1.功能作用](#功能作用)
  - [2.配置](#文件配置)

# nginx 功能
##一 HTTP服务器
##二 反向代理
### 功能作用
   1、将外网请求转发内网服务器
   2、隐藏内部服务器IP

### 文件配置
   1、配置拦截服务名称 server_name 
   2、配置反向代理服务器地址proxy_name
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
     
##三 负载均衡服务器
### 功能作用
   1、减轻单台服务器压力，提供系统服务能力
   2、高并发的最基本解决方案
### 文件配置
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
###服务器宕机检查
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

##四 防盗链
```
location ~ .*\.(jpg|jpeg|JPG|png|gif|icon)$ {
        valid_referers blocked http://www.itmayiedu.com www.itmayiedu.com;
        if ($invalid_referer) {
            return 403;
        }
		}

```
