## nodejs 配置
- `wget https://nodejs.org/dist/v8.11.1/node-v8.11.1-linux-x64.tar.xz`
- 因为下去的文件是`*.xz`，所以不能直接使用 linux 命令的 tar 解压，需要下载对应格式的解压工具
- `yum -y install xz`
- `xz -d node-v8.11.1-linux-x64.tar.xz`
- `tar -xvf node-v8.11.1-linux-x64.tar`
- 为了操作方便可以将原文件夹重命名 `mv node-v8.11.1-linux-x64 node-v8.11.1`
- 配置为 nodejs 为全局使用，注意下面的路径，前段是自己解压文件所在路径
- `ln -s /root/dk/node-v8.11.1/bin/node /usr/local/bin/node`
- `ln -s /root/dk/node-v8.11.1/bin/npm /usr/local/bin/npm`
- 如果没有问题的话，现在可以 `node -v` 查看版本号，如果正常显示则表示安装成功。

## forever 
- `npm install forever -g`
- `ln -s /root/dk/node-v8.11.1/lib/node_modules/forever/bin/forever /usr/local/bin`
- `forever --version`