## nginx 下载并安装
- `wget http://nginx.org/download/nginx-1.10.2.tar.gz`
- `tar -zxvf nginx-1.10.2.tar.gz`
- `cd nginx-1.10.2`
- `./configure --prefix=/usr/local/nginx`
    - 如果出现错误 `./configure: error: the HTTP rewrite module requires the PCRE library.`
    - `yum -y install pcre-devel`
    - 重新执行上面的步骤
- `make`
- `make install` 
- 到这一步就安装完成，下面的步骤为配置环境变量
- `cd ~`
- `vi .bashrc`
- `export NGINX_HOME=/usr/local/nginx`
- `export PATH=$PATH:$NGINX_HOME/sbin`
- 保存退出
- `source .bashrc`
- `nginx`
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

## 配置 nginx 反向代理
- `cd /usr/local/nginx/conf/`
- `vi nginx.node.conf`
- 将下面配置修改成自己信息后复制进去，保存退出
```javascript
server{  
    listen 80;  
    server_name 域名 || ip;  
    index index.html index.htm index.php default.html default.htm default.php;

    location / {  
        proxy_set_header X-Real-IP $remote_addr;  
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;  
        proxy_set_header Host $http_host;  
        proxy_set_header X-NginX-Proxy true;  
        proxy_pass http://127.0.0.1:1337/;  
        proxy_redirect off;  
    }
}
```
- 重启 nginx。