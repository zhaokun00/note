### 										Jenkins环境搭建

#### 1.Ubuntu14.04安装Jenkins

> 参考官网 https://pkg.jenkins.io/debian-stable/

这里我们使用Jenkins的官方提供的软件仓库，要使用官方的软件仓库之前必须将软件仓库的秘钥添加到本地

```shell
#添加官方软件仓库的秘钥到本地的apt秘钥中
wget -q -O - http://pkg.jenkins-ci.org/debian/jenkins-ci.org.key | sudo apt-key add - 
```

将官方提供的软件仓库地址加入到本地的apt软件源中，本地用于存放软件源的文件在/etc/apt/sources.list

```shell
#将地址添加进本地的软件源列表
sudo sh -c 'echo deb http://pkg.jenkins-ci.org/debian binary/ > /etc/apt/sources.list.d/jenkins.list' 
```

更新我们本地的软件源缓存，然后直接安装jenkins

```shell
$ sudo apt-get update
$ sudo apt-get install jenkins
```

### jenkins常用命令

```shell
sudo /etc/init.d/jenkins [start|restart|stop|status]
```



ubuntu查看开机项

https://www.cnblogs.com/21207-iHome/p/7472316.html



安装目录：/var/lib/jenkins 日志目录：/var/log/jenkins/jenkins.log

http:*//192.168.1.100:8080/*

修改端口

/etc/default/jenkins:63

https://www.cnblogs.com/xianhan/p/10082543.html

https://www.cnblogs.com/duoban/p/11342929.html



1. https://www.deathearth.com/256.html //jenkins-修改jdk
2. https://www.cnblogs.com/jwrwst/p/6520114.html 使用jenkins

#### 2.Jenkins从github下载代码

https://www.jianshu.com/p/5f671aca2b5a

#### 3.Jenkins执行脚本

https://www.cnblogs.com/yitianyouyitian/p/9255098.html



虚拟机域名

https://blog.csdn.net/xyh930929/article/details/84109621

构建参数

https://blog.csdn.net/qq_21047625/article/details/99643894

# [jenkins定位GitLab推送的最新Webhook中push event来自哪一个分支](https://www.cnblogs.com/zblade/p/9897604.html)

https://www.cnblogs.com/blog-ws/p/9306375.html



# 通过jenkins持续集成 github中的代码到服务器

https://www.jianshu.com/p/9db976b675b2



sh testsh.sh

sh -x testsh.sh



0f48da6  c21578e254e3c3a24df0cbb037d6ab0d7

ea394f1 b4485927cf680198c396bbcaef55e7e68



gitlab中打标签

gitlab中比较两次提交的不同





# GitLab提交一个自己分支的完整流程

http://www.xusiwei.cn/article/266.html

gitlab中提交merge request

https://blog.csdn.net/qq_33829154/article/details/81364047



sudo apt-get install  ca-certificates postfix



# dpkg和apt-get区别

https://blog.csdn.net/liuxiaodong400/article/details/81057892

dpkg命令

https://blog.csdn.net/weixin_34090562/article/details/86061949

# apt-get命令详解

https://www.fujieace.com/linux/man/apt-get-2.html

# [ubuntu 14.04 gitlab 的搭建]

https://www.cnblogs.com/chenfulin5/p/8401212.html

126邮箱第3方授权码

**abc15301070551**



gitlab首次登录时需要改变密码

root

**abc15301070551**

gitlab-ctl reconfigure

gitlab-ctl start



https://www.cnblogs.com/zblade/p/9897604.html

http://192.168.110.129:8888/



jenkins与gitlab

https://www.jianshu.com/p/63b012ee52ea

https://www.jianshu.com/p/eeb15a408d88

https://www.cnblogs.com/zblade/p/9480366.html

https://baijiahao.baidu.com/s?id=1630678692475452408&wfr=spider&for=pc