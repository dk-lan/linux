## nodejs 配置
- `wget https://nodejs.org/dist/v10.15.0/node-v10.15.0-linux-x64.tar.xz`
- 因为下去的文件是`*.xz`，所以不能直接使用 linux 命令的 tar 解压，需要下载对应格式的解压工具
- `xz -d node-v10.15.0-linux-x64.tar.xz`
- `tar -xvf node-v10.15.0-linux-x64.tar`
- 为了操作方便可以将原文件夹重命名 `mv node-v10.15.0-linux-x64 node-v10.15.0`
- 配置为 nodejs 为全局使用，注意下面的路径，前段是自己解压文件所在路径
- `sudo ln -s /home/ubuntu/downloads/node-v10.15.0/bin/node /usr/local/bin/node`
- `sudo ln -s /home/ubuntu/downloads/node-v10.15.0/bin/npm /usr/local/bin/npm`
- 如果没有问题的话，现在可以 `node -v` 查看版本号，如果正常显示则表示安装成功。

## forever 
- `npm install forever -g`
- `sudo ln -s /home/ubuntu/downloads/node-v10.15.0/lib/node_modules/forever/bin/forever /usr/local/bin`
- `forever --version`

## 坑
如果有遇到类似 scrypt.node 等第三方不是 js 的库，那要在目录下用 npm 来安装，因为这个安装包会根据当前系统版本自动下载，本地上传上去的很多时候会因为系统版本对不上而报错

## npm
npm 安装时错误 
`gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.`

需要安装 python 2.7
`sudo apt-get install python2.7`
`sudo ln -s /usr/bin/python2.7 /usr/bin/python`

需要安装 g++ 环境，如果安装了 nginx 对应的 c++ 环境，这个则可跳过
`sudo apt-get install build-essential`