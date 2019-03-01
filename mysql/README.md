## 安装mysql
`sudo apt-get install mysql-server mysql-client` 
安装过程会弹出输入数据库密码的确认对话框，在这一步设置好密码

### 安装时没有弹框输入密码
- `sudo vim /etc/mysql/debian.cnf`
- `mysql -u debian-sys-maint -p 此长密码`
- `update mysql.user set authentication_string=password('新密码') where user='root'and Host = 'localhost';`
- `update mysql.user set plugin="mysql_native_password";`
- `flush privileges;`
- `quit;`
- `sudo service mysql restart`
- `mysql -u root -p` 登录

## 测试是否安装成功
`sudo netstat -tap | grep mysql` 出现以下信息表示安装成功  
tcp        0      0 localhost:mysql         *:*                     LISTEN       5341/mysqld 

## 相关操作
- `netstat -nlt|grep 3306` 检查MySQL服务器占用端口
- `ps -aux|grep mysql` 检查MySQL服务器系统进程
- `show variables like '%char%';` 查看数据库的字符集编码
- `mysql -u root -p` 登录
- `sudo service mysql start`
- `sudo service mysql stop`
- `sudo service mysql restart`
- `systemctl status mysql.service` 查看详细信息

## 配置
### 设置mysql允许远程访问
- `sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf`
- 注释掉 `bind-address = 127.0.0.1`
- 保存退出，然后进入mysql服务，执行授权命令
- `grant all on *.* to root@'%' identified by '你的密码' with grant option;`
- `flush privileges;`
- 然后执行quit命令退出mysql服务，执行如下命令重启mysql
- `sudo service mysql restart`
- 现在在windows下可以使用navicat远程连接ubuntu下的mysql服务

### 将字符编码设置为UTF-8
默认情况下，MySQL的字符集是latin1，因此在存储中文的时候，会出现乱码的情况，所以我们需要把字符集统一改成UTF-8。
- `sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf`
在[mysqld]下 添加 `character-set-server=utf8` 就可以了，如果原来就有 `default-character-set=utf8`，注释掉，这是老版本的写法。

## 卸载
- `sudo apt purge mysql-*`
- `sudo rm -rf /etc/mysql/ /var/lib/mysql`
- `sudo apt autoremove` 