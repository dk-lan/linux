## CentOS 常用命令
### yum
Yum:  Update Modifier，是一种基于rpm的包管理工具.
- yum repolist all -> 显示所有仓库
- yum repolist enabled -> 显示可用的仓库
- yum list all -> 显示所有的程序包
- yum list recent -> 显示仓库中最近增加的程序包 
- yum install [程序包] -> 用于安装程序包 -> 例如：yum install wget
- yum remove [程序包] -> 用于卸载程序包 -> 例如：yum remove wget
- yum clean all -> 清理本地缓存
- yum clean plugins -> 清楚插件缓存
- yum search [程序包] -> 搜索程序包

### wget
- wget [url] -> 用于下载文件

### curl
由于安装linux的时候很多时候是没有安装桌面的，也意味着没有浏览器，因此这个方法也经常用于测试一台服务器是否可以到达一个网站

语法 curl [option] [url]

- 基本用法 -> curl http://www.linux.com
- 保存网页 -> curl -o linux.html http://www.linux.com 
- 测试网页返回值 -> curl -o /dev/null -s -w %{http_code} www.linux.com

### tar
对文件压缩和解压

#### 解压
- *.tar -> tar –xvf file.tar
- *.tar.gz -> tar -xzvf file.tar.gz
- *.gz ->  gzip -d 或者 gunzip
- *.bz2 -> bzip2 -d 或者 bunzip2 
- *.tar.bz2 -> tar –xjf
- *.tar.Z -> tar –xZf 

#### 压缩
- tar –cvf jpg.tar *.jpg -> 将目录里所有jpg文件打包成tar.jpg
- tar –czf jpg.tar.gz *.jpg -> 将目录里所有jpg文件打包成jpg.tar后，并且将其用gzip压缩，生成一个gzip压缩过的包，命名为jpg.tar.gz
- tar –cjf jpg.tar.bz2 *.jpg -> 将目录里所有jpg文件打包成jpg.tar后，并且将其用bzip2压缩，生成一个bzip2压缩过的包，命名为jpg.tar.bz2
- tar –cZf jpg.tar.Z *.jpg -> 将目录里所有jpg文件打包成jpg.tar后，并且将其用compress压缩，生成一个umcompress压缩过的包，命名为jpg.tar.Z