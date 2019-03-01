### mac 连接 linux
1. 在 Mac 打开终端
2. 输入命令 ssh root@ip -p 端口号
    - 如果是第一次连接会提示生成 RSA key，直接输入 `yes` 就好。
    - 如果出现错误 `RSA host key for mysharebook.cn has changed and you have requested strict checking.Host key verification failed.`，通常是 Linux 重装或者 openssh-server 重装引起的
    - 可以尝试 `ssh-keygen -R 服务器ip`，如果不行的话就手动清除。
    - 手动清除：
        - `cd .ssh`
        - `vi known_hosts`
        - 找到对应你主机 ip 的 key，全部删除并保存退出。
        - 重新运行第 2 步连接就可以了。
 
### yum
Yum:  Update Modifier，是一种基于rpm的包管理工具.
- `yum repolist all` -> 显示所有仓库
- `yum repolist enabled` -> 显示可用的仓库
- `yum list all` -> 显示所有的程序包
- `yum list recent` -> 显示仓库中最近增加的程序包 
- `yum install [程序包]` -> 用于安装程序包 -> 例如：yum install wget
- `yum remove [程序包]` -> 用于卸载程序包 -> 例如：yum remove wget
- `yum clean all` -> 清理本地缓存
- `yum clean plugins` -> 清楚插件缓存
- `yum search [程序包]` -> 搜索程序包

### apt-get
- `sudo mv /etc/apt/sources.list /etc/apt/source.list.bak` 原文件重命名备份
- `sudo vim /etc/apt/sources.list` 编辑源列表文件
- ubuntu 16.04 阿里云源
`
deb-src http://archive.ubuntu.com/ubuntu xenial main restricted #Added by software-properties
deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted
deb-src http://mirrors.aliyun.com/ubuntu/ xenial main restricted multiverse universe #Added by software-properties
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted multiverse universe #Added by software-properties
deb http://mirrors.aliyun.com/ubuntu/ xenial universe
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates universe
deb http://mirrors.aliyun.com/ubuntu/ xenial multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse #Added by software-properties
deb http://archive.canonical.com/ubuntu xenial partner
deb-src http://archive.canonical.com/ubuntu xenial partner
deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted multiverse universe #Added by software-properties
deb http://mirrors.aliyun.com/ubuntu/ xenial-security universe
deb http://mirrors.aliyun.com/ubuntu/ xenial-security multiverse
`
- ubuntu 16.04 清华大学
`
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial main restricted
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates main restricted
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial universe
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates universe
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security main restricted
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security universe
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security multiverse

`

- `sudo apt-get update` `sudo apt-get upgrade` 更新

### wget
- `yum install wget`
- `wget [url]` -> 用于下载文件

### mv
- `mv oldname newname` -> 重命名
- `mv filename pathname` -> 移动文件到对应的目录

### 端口
- `lsof -i:端口号` 用于查看某一端口的占用情况，比如查看8000端口使用情况，lsof -i:8000
- `netstat -tunlp |grep 端口号`，用于查看指定的端口号的进程情况，如查看8000端口的情况，netstat -tunlp |grep 8000

### curl
由于安装linux的时候很多时候是没有安装桌面的，也意味着没有浏览器，因此这个方法也经常用于测试一台服务器是否可以到达一个网站

语法 `curl [option] [url]`

- 基本用法 -> `curl http://www.linux.com`
- 保存网页 -> `curl -o linux.html http://www.linux.com`
- 测试网页返回值 -> `curl -o /dev/null -s -w %{http_code} www.linux.com`

### tar
对文件压缩和解压

#### 解压
- *.tar -> `tar –xvf file.tar`
- *.tar.gz -> `tar -xzvf file.tar.gz`
- *.gz ->  `gzip -d` 或者 `gunzip`
- *.bz2 -> `bzip2 -d` 或者 `bunzip2 `
- *.tar.bz2 -> `tar –xjf`
- *.tar.Z -> `tar –xZf`

#### 压缩
- `tar –cvf jpg.tar *.jpg` -> 将目录里所有jpg文件打包成tar.jpg
- `tar –czf jpg.tar.gz *.jpg` -> 将目录里所有jpg文件打包成jpg.tar后，并且将其用gzip压缩，生成一个gzip压缩过的包，命名为jpg.tar.gz
- `tar –cjf jpg.tar.bz2 *.jpg` -> 将目录里所有jpg文件打包成jpg.tar后，并且将其用bzip2压缩，生成一个bzip2压缩过的包，命名为jpg.tar.bz2
- `tar –cZf jpg.tar.Z *.jpg` -> 将目录里所有jpg文件打包成jpg.tar后，并且将其用compress压缩，生成一个umcompress压缩过的包，命名为jpg.tar.Z

#### 查看端口
- `netstat –apn | grep 8080`
- `kill -9 [PID]` -9 表示强迫进程立即停止