## Mongodb
- `wget wget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-ubuntu1604-4.0.5.tgz`
- `tar -xzvf mongodb-linux-x86_64-ubuntu1604-4.0.5.tgz`
- `mv mongodb-linux-x86_64-ubuntu1604-4.0.5/ mongodb`
- `mv mongodb /usr/local`
- `cd /usr/local/mongodb`
- `mkdir db`
- `mkdir logs`
- `cd bin`
- `sudo vim mongodb.conf`
- 将下面配置添加到文件当中并保存退出
```javascript
dbpath=/usr/local/mongodb/db
logappend=true
fork=true
logpath=/usr/local/mongodb/logs/mongpdb.log
auth=true //第一次没有设置用户名密码前为 false
bind_ip = 0.0.0.0
port = 27017
```

## error
1. ./mongod: error while loading shared libraries: libcurl.so.4: cannot open shared object file: No such file or directory
apt-get install libcurl4-openssl-dev
2. ./mongod: error while loading shared libraries: libnetsnmpmibs.so.30: cannot open shared object file: No such file or directory
apt-get install snmpd snmp snmp-mibs-downloader

- 启动 mongodb
- `cd /usr/local/mongodb/bin`
- `./mongod -f mongodb.conf`
- 到这一步在浏览器输入 ip:27017 应该能正常访问，说明 mongodb 安装完成。
- 设置开机启动
- `vi /etc/rc.d/rc.local`
- `/usr/local/mongodb/bin/mongod --config /usr/local/mongodb/bin/mongodb.conf`

## 权限
https://blog.csdn.net/fofabu2/article/details/78983741

## 操作 停止
use admin;
db.shutdownServer();

## 权限
1. use admin
2. 创建管理员账户
db.createUser({ user: "useradmin", pwd: "adminpassword", roles: [{ role: "userAdminAnyDatabase", db: "admin" }] })
mongodb中的用户是基于身份role的，该管理员账户的 role是 userAdminAnyDatabase。 ‘userAdmin’代表用户管理身份，’AnyDatabase’ 代表可以管理任何数据库。
3. 验证第3步用户添加是否成功 db.auth("useradmin", "adminpassword") 如果返回1，则表示成功。
4. 重启 use admin    db.auth("useradmin", "adminpassword")
5. 新建你需要管理的mongodb 数据的账号密码。
use yourdatabase
db.createUser({ user: "youruser", pwd: "yourpassword", roles: [{ role: "dbOwner", db: "yourdatabase" }] })
rote:dbOwner 代表数据库所有者角色，拥有最高该数据库最高权限。比如新建索引等
6. 新建数据库读写账户
use yourdatabase
db.createUser({ user: "youruser2", pwd: "yourpassword2", roles: [{ role: "readWrite", db: "yourdatabase" }] })
该用户用于该数据的读写，只拥有读写权限。
7. 现在数据的用户名和密码就建好了。
可以使用：mongodb://youruser2:yourpassword2@localhost/yourdatabase来链接