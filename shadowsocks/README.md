### shadowsocks
shadowsocks 是一款自定义协议的代理软件，由于其流量特征不明显，一直可以稳定提供上网代理。

shadowsocks 客户端会在本地开启一个 socks5 代理，通过此代理的网络访问请求由客户端发送至服务端，服务端发出请求，收到响应数据后再发回客户端。

因此使用 shadowsocks 需要一台墙外的服务器来部署 shadowsocks 服务端。

### 服务端配置
1. `yum install epel-release`
2. `yum update`
3. `yum install python-setuptools && easy_install pip`
4. `pip -V` 如果不能正常显示版本号则表示 pip 没有安装成功，然后可以根据分支的步骤手动安装
    - `wget --no-check-certificate https://github.com/pypa/pip/archive/1.5.5.tar.gz `
    - `mv 1.5.5.tar.gz pip-1.5.5.tar.gz`
    - `tar -xzvf pip-1.5.5.tar.gz`
    - `cd pip-1.5.5`
    - `python setup.py install`
    - `pip -V` 如果能正常显示版本号则表示 pip 安装成功。如果还是提示没有 pip 命令，则需要再添加环境变量。
        - `vim /etc/profile`
        - 在文档最后，添加: `export PATH="/usr/local/python2.7/bin:$PATH"`，然后保存退出。
        - `source /etc/profile`
5. `pip install shadowsocks`
6. `vi /etc/shadowsocks.json`
7. 将下面内容复制进去然后保存退出。
```javascript
{
    "server": "your_server_ip",
    "server_port": 443,
    "local_address": "127.0.0.1",
    "local_port": 1080,
    "password": "your_password",
    "timeout": 300,
    "method": "aes-256-cfb",
    "fast_open": false,
    "workers": 1
}
```

- server: 服务器ip地址
- server_port: 绑定的端口，注意不要设置已经使用了的端口
- possword: 客户端登陆时密码，自己设定
- timeout: 超时时间
- method: 加密方法
- fast_open: 如果你的服务器 Linux 内核在3.7+，可以开启 fast_open 以降低延迟
- workers: 最大连接数量，默认为 1
- 如果需要配置多个SS账号，可以按照如下案例进行配置：
```javascript
{
    "server": "your_server_ip",
    "port_password": {
        "8381": "password1",
        "8382": "password2",
        "8383": "password3",
        "8384": "password4"
        },
    "timeout": 300,
    "method": "aes-256-cfb",
    "fast_open": false,
    "workers": 1
}
```
8. 添加到进程 `vi /etc/supervisord.conf`
9. 将下面内容复制进去然后保存退出。
```javascript
[program:shadowsocks]
command=ssserver -c /etc/shadowsocks.json
autostart=true
autorestart=true
user=root
log_stderr=true
logfile=/var/log/shadowsocks.log
```
10. 设置开机启动 `vi /etc/rc.local`
11. 将下面内容复制进去然后保存退出 `service supervisord start`
12. 重启服务器 `reboot`

##### shadowsocks 操作
- 启动 `ssserver -c /etc/shadowsocks.json -d start`
- 停止 `ssserver -c /etc/shadowsocks.json -d stop`
- 重启 `ssserver -c /etc/shadowsocks.json -d restart`

### 客户端配置
- 下载对应平台的 shadowsocks 客户端。
- 下载地址：`https://github.com/shadowsocks/shadowsocks/wiki/Ports-and-Clients`
- 下载地址：`https://cdnfile.fyvps.com/ShadowsocksX-NG-R8.dmg`
- 安装客户端
- 填写对应服务端配置
- 配置成功后可以看墙外的风景了。