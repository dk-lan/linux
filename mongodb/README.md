## Mongodb
- `wget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-rhel62-3.4.4.tgz`
- `tar -xzvf mongodb-linux-x86_64-rhel62-3.4.4.tgz`
- `mv mongodb-linux-x86_64-rhel62-3.4.4 mongodb`
- `mv mongodb /usr/local`
- `cd /usr/local/mongodb`
- `mkdir db`
- `mkdir logs`
- `cd bin`
- `vi mongodb.conf`
- 将下面配置添加到文件当中并保存退出
```javascript
port=27017
dbpath=/usr/local/mongodb/db
logappend=true
fork=true
logpath=/usr/local/mongodb/logs/mongpdb.log
nohttpinterface=true
```
- 启动 mongodb
- `cd /usr/local/mongodb/bin`
- `./mongod -f mongodb.conf`
- 到这一步在浏览器输入 ip:27017 应该能正常访问，说明 mongodb 安装完成。
- 设置开机启动
- `vi /etc/rc.d/rc.local`
- `/usr/local/mongodb/bin/mongod --config /usr/local/mongodb/bin/mongodb.conf`