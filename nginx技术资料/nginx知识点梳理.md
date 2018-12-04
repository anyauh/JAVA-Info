<!-- MarkdownTOC -->
- [HTTP服务器](#一-HTTP服务器)
- [反向代理 ](#二-反向代理)
  - [1.功能作用](#功能作用)
  - [2.配置](#文件配置)
- [负载均衡 ](#三-负载均衡)
  - [1.功能作用](#功能作用)
  - [2.配置](#文件配置)
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
   ```server {
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

##四 防盗链

