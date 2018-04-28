## 安装 
查看是否有自带的 git
`git --version`

如果有自带的 git，版本通常会比较低，建议删除掉重新安装
`yum remove git`

如果没有 git，则按下面步骤完成安装

1. 下载 git 安装文件 `wget https://github.com/git/git/archive/v2.9.2.tar.gz`
2. 解压 `tar zxvf v2.9.2.tar.gz` 
3. `cd git-2.9.2`
4. 编译安装`make configure`
    - 如果出现错误 make: command not found，则表示 make 需要手动安装
        - `yum -y install gcc automake autoconf libtool make`
        - `yum -y install gcc gcc-c++`
        - 重装运行第 4 步
5. `./configure --prefix=/usr/local/git --with-iconv=/usr/local/libiconv`
6. `make all doc`
    - 如果出现错误 make: *** [credential-store.o] 错误 1，是由于缺少 zlib的头文件， 开发包没装
        - `yum install zlib`
        - `yum -y install zlib-devel `
    - 如果出现错误 make[1]: *** [perl.mak] Error 2
        - `yum -y install perl-ExtUtils-MakeMaker package`
    - 如果出现错误 po/bg.msg make[1]: *** [po/bg.msg] 错误 127，则需要手动安装
        - `yum -y install tcl  build-essential tk gettext`
    - 如果出现错误 asciidoc: command not found
        - `yum -y install asciidoc`
    - 如果出现错误 xmlto: command not found
        - `yum -y install xmlto`
    - 如果上述问题解决的话重新运行第 6 步
7. `make install install-doc install-html`
8. 修改环境变量 `vi /etc/profile`
9. 在最后一行添加 `export PATH=/usr/local/git/bin:$PATH`
10. 保存后使其立即生效 `source /etc/profile`
11. 查看是否安装成功 `git --version`

## git 配置
1. 配置用户名
    - `git config --global user.name "yourname"`
    - `git config --global user.email "youremail"`

2. git ssh key pair 配置
    - 一路回车,不要输入任何 , 最后生成 ssh key pair  
    - `ssh-keygen -t rsa -C "youremail@example.com"`

3. `ssh -add ~/.ssh/id_rsa`
4. `cat ~/.ssh/id_rsa.pub`

到第 4 步后顺利生成了 key，然后把这段 key 复制到 github，就可以正常使用 github 提交代码了。 