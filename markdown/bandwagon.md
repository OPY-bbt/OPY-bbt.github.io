# bandwagon  使用简介

## ssh 链接
- [Link](http://www.ruanyifeng.com/blog/2011/12/ssh_remote_login.html)
- 如果你本来已经有公钥， 并且还设置过密码，那么你登陆的时候还是需要设置密码，所以应该把 passphrase 删除， `$ ssh-keygen -f id_rsa -p` 不输入任何字符敲击回车，就可以删除密码。这个时候就可以使用共钥无密码登陆。
- 如何你觉得每次都输入 ssh root@XXXX 这个也很麻烦的话，推荐你下个 iterm 2. 这个可以配置新建窗口的初始化命令，如果你设置了无密码登陆的话，只需要将连接命令写入初始化命令中，就可以了。
- [端口转发](http://www.ruanyifeng.com/blog/2011/12/ssh_port_forwarding.html)

## 性能测试
- [bench.sh](https://teddysun.com/444.html) 查看机器信息
- [路由跟踪](http://tool.chinaz.com/Tracert/) 我的竟然跑到欧洲去了。。。
- [全国ping](http://ping.chinaz.com/) 一般日本在70ms - 100ms，美国在 130ms-250ms 之间.

