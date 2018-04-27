## 安装 
查看是否有自带的 git
`git --version`

如果有自带的 git，版本通常会比较低，建议删除掉重新安装
`yum remove git`

如果没有 git，则按下面步骤完成安装

1. 下载 git 安装文件
`wget https://github.com/git/git/archive/v2.9.2.tar.gz`

2. 解压
`tar zxvf v2.9.2.tar.gz`
`cd git-2.9.2`

3. 编译安装
`make configure`
如果出现错误 make: command not found，则表示 make 需要手动安装
    - `yum -y install gcc automake autoconf libtool make`
    - `yum install gcc gcc-c++`

4. `./configure --prefix=/usr/local/git --with-iconv=/usr/local/libiconv`
5. `make all doc`

如果出现错误 make[1]: *** [perl.mak] Error 2
    - `yum install perl-ExtUtils-MakeMaker package`

如果出现错误 po/bg.msg make[1]: *** [po/bg.msg] 错误 127，则需要手动安装
    - `yum install tcl  build-essential tk gettext`

6. `make install install-doc install-html`

7. 修改环境变量 `vi /etc/profile`
8. 在最后一行添加 `export PATH=/usr/local/git/bin:$PATH`
9. 保存后使其立即生效 `source /etc/profile`
10. 查看是否安装成功 `git --version`

## git 配置
1. 配置用户名
    - `git config --global user.name "yourname"`
    - `git config --global user.email "youremail"`

2. git ssh key pair 配置

一路回车,不要输入任何 , 最后生成 ssh key pair  
`ssh-keygen -t rsa -C "youremail@example.com"`

3. `ssh -add ~/.ssh/id_rsa`
4. `cat ~/.ssh/id_rsa.pub `

到第 4 步后顺利生成了 key，然后把这段 key 复制到 github，就可以正常使用 github 提交代码了。 