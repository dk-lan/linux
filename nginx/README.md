## nginx 下载并安装
- `wget http://nginx.org/download/nginx-1.15.6.tar.gz`
- `tar -zxvf nginx-1.15.6.tar.gz`
- `cd nginx-1.15.6`
- `sudo apt-get update`
- `./configure --prefix=/usr/local/nginx`  `./configure --prefix=/usr/local/nginx --with-http_ssl_module`
    - 如果出现错误 `./configure: error: the HTTP rewrite module requires the PCRE library.`
    - `yum -y install pcre-devel` centOS
    - `sudo apt-get install libpcre3 libpcre3-dev openssl libssl-dev zlib1g zlib1g-dev libssl-dev` ubuntu
    - 重新执行上面的步骤
    - 如果出现错误 `./configure: error: C compiler cc is not found`
    - `whereis gcc` 如果结果为 `gcc: /usr/lib/gcc`
    - `sudo apt-get install build-essential`
- `make`
- `sudo make install` 
- 到这一步就安装完成，下面的步骤为配置环境变量 centOS
- `cd ~`
- `vi .bashrc`
- `export NGINX_HOME=/usr/local/nginx`
- `export PATH=$PATH:$NGINX_HOME/sbin`
- 保存退出
- `source .bashrc`
- `nginx`
- ubuntu 在 root 下操作
- `vi /etc/profile`
- `export NGINX_HOME=/usr/local/nginx`
- `export PATH=$PATH:$NGINX_HOME/sbin`
- 保存退出
- `source /etc/profile`
- 到这一步，在浏览器中输入域名或 ip，便可正常访问 nginx 的欢迎界面。

## nginx 操作
- 使用下面命令的前提是已配置了环境变量
- 开启服务 `nginx`
- 停止服务 `nginx -s stop`
- 重启服务 `nginx -s reload`
- 如果没有配置环境变量的使用下面命令
- 开启服务 `./usr/local/nginx-1.11.2/sbin/nginx`
- 停止服务 `./usr/local/nginx-1.11.2/sbin/nginx -s stop`
- 重启服务 `./usr/local/nginx-1.11.2/sbin/nginx -s reload`

## nginx 修改防火墙配置
- `vi + /etc/sysconfig/iptables` -> 修改防火墙配置
- `-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT` -> 保存退出
    - 如果出现错误 `iptables: Applying firewall rules: iptables-restore: line 1 failed`
    - `iptables -N RH-Firewall-1-INPUT`
    - `service iptables save`
    - 重新执行上面的步骤

## 干净卸载nginx
- `sudo apt-get --purge autoremove nginx`
- `which nginx` 如果没有提示，证明卸载成功

## 配置 nginx 反向代理
- `vi /usr/local/nginx/conf/nginx.conf`
- 将下面配置修改成自己信息后复制进去，保存退出
```javascript
worker_processes  4;
events {
    worker_connections  10240;
}    
http{
    client_max_body_size 50m;
    include       mime.types;
    default_type  application/octet-stream;    
    sendfile        on;
    keepalive_timeout  65;

    # 待选服务器列表
    upstream myproject{
        server xx.xx.xx.xx;
        server xx.xx.xx.xx;
    }

    server {
        listen 80;
        server_name localhost;

        location / {
                proxy_pass http://x.x.x.x:xx; #代理
        }

        location /xx {
                alias /home/ubuntu/api/; #本地
        }  
        
        location /oo {
		    proxy_pass http://myproject;#负载均衡的一种配置
        }        
    }  
}
```

- 重启 nginx。

## Nginx如何安装https-ssl证书
### 在对应服务器生成私钥
`openssl req -new -newkey rsa:2048 -nodes -keyout xxoo.key -out xxoo.csr`
- xxoo 为域名
- 在生成过程如果不清楚如何填，就全部写域名，如 xxoo.com

`unknown directive "ssl"`

出现这个错误说明没有将ssl模块编译进nginx，在configure的时候加上“–with-http_ssl_module”即可
1. `./configure --with-http_ssl_module` 下载你当前版本的nginx包，并且解压 进到目录 
2. `make` 切记千万不要make install 那样就覆盖安装了 
3. `sudo cp /usr/local/nginx/sbin/nginx /usr/local/nginx/sbin/nginx.bak` 将原来的nginx备份 备份之前先kill当前正在启动的nginx
4. `sudo cp objs/nginx /usr/local/nginx/sbin/nginx`  make之后会在当前目录生成 objs 目录
5. `/usr/local/nginx/sbin/nginx`  然后重新启动nginx

### 配置文件
```javascript
    server {
        listen 80;
        server_name localhost;

        location / {
                proxy_pass https://www.xxoo.com;
        }
    }

    server {
        listen 443 ssl;
        server_name www.xxoo.com;
        ssl_certificate /home/ubuntu/ssl/server.crt;
        ssl_certificate_key /home/ubuntu/ssl/server.key;

        location / {
                proxy_pass http://0.0.0.0;
        }

        location /xx/ {
                alias /home/ubuntu/xxoo/;
        }
    }
```