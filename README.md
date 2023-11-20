# nginx.conf的参数如何配置
## STEP1
- 将 nginx.conf 移动至 /usr/local/nginx/conf/ 路径。  

## STEP2

- 修改nginx.conf的配置
 
  > nginx.conf 文件中的server中主要的参数设置如下：  
  >- listen：监听的端口号，默认80
  >- server_name：监听的IP或者域名
  >- charset：编码格式，可设为utf-8
  >- location / ：获取或设置窗口的 URL
  >- root：获取URL之后，进入root给定的路径，寻找网页
  >- index：默认首页
  >- proxy_pass ：设置代理的路径，格式为http://IP(域名)：端口
# STEP3
- 修改/etc/hosts文件，添加域名与IP之间的映射
```
sudo vim /etc/hosts
127.0.0.1     mynginx.com                  #添加域名与IP的映射
```
# STEP4
栗子：
```
  server{
      listen     83;                        #监测端口号
      server_name localhost;
      location / {
           root /home/zsy/code/mynginx/html83;
           index index.html index.htm;      #默认界面
        }
    }

  server {
  listen       80;                          #监听端口
  server_name  mynginx.com;                 #域名
        charset utf-8;              
        location / {
            proxy_pass http://mynginx.com;  #代理IP
        }
    }

upstream mynginx.com{
  server 172.16.60.187:81 weight=3;          #代理的IP以及端口，weight指明权值
  server 172.16.60.187:82 weight=2;
  server 172.16.60.187:83 weight=1;
  
  server 172.16.60.188:80 weight=1;
}
```
