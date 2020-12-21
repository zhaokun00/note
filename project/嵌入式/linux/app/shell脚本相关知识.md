#### 1.shell脚本的调用与调试

```shell
#一、shell脚本的调用
方式一:直接运行脚本，例如./export.sh

#在shell窗口中直接赋值一个变量
zhaokun@linux:shell$ CC=gcc
#直接打印变量,可以打印
zhaokun@linux:shell$ echo $CC
gcc

#在shell脚本中调用，是不能够使用的
zhaokun@linux:shell$ ./export.sh
CC:

#shell脚本内容如下:
 #! /bin/sh
 echo CC:${CC}
 
#原因:从shell脚本内容,可以看到打印的变量为空，子shell中不能直接使用父进程中定义的普通变量

 
#要想在子shell中使用，必须在父shell中用export导出环境变量，export主要用户父进程向子进程中导出变量,而不能够用于子进程向父进程传递数据

zhaokun@linux:shell$ export CC=gcc
zhaokun@linux:shell$ ./export.sh  
CC:gcc

方式二:使用exec函数,该种方式当执行exec命令后，后面的脚本将不再运行

#shell脚本内容如下:

 #! /bin/sh
 echo CC:${CC}

 #在这里可以调用其他的脚本
 exec echo "exec start"                                                                   

 #使用exec执行脚本,该语句将不再执行
 echo "exec end"

#输出内容如下:
zhaokun@linux:shell$ ./export.sh 
CC:gcc
exec start

方式三:使用source命令,source主要用于父进程可以使用子进程中定义的变量或者函数，例如在Android中，source build/envsetup.sh后，就可以使用mm等命令，这些命令是定义在子shell脚本中，通过source命令让在父进程中也可以使用。source命令类型include性质把其脚本包含进来

#子shell中定义的变量和函数
 #! /bin/sh
name=zhaokun
echo ${name}

test() {
    echo "test"                                                                           
}

#使用source命令执行,也可以用.替代source
zhaokun@linux:shell$ source export.sh 
zhaokun

#可以在终端中直接执行函数的名字
zhaokun@linux:shell$ test
test

#二、shell脚本调试(可以输出执行命令)

方法一:使用sh -x
#脚本内容
 #! /bin/sh
echo "test set -x start"

//执行的命令
zhaokun@linux:shell$ sh -x export.sh 
+ echo test set -x start
test set -x start

方式二:使用set -x
//脚本内容
 #! /bin/sh

#开启调试
set -x
echo "test set -x start"
#关闭调试
set +x

//执行的命令
zhaokun@linux:shell$ ./export.sh 
++ echo 'test set -x start' //能够输出命令
test set -x start //输出的打印结果
++ set +x

参考文档:
#shell脚本的调用方式
https://www.cnblogs.com/insane-Mr-Li/p/9095668.html

#linux下export命令添加、删除环境变量
https://www.cnblogs.com/zhangwuji/p/7899075.html

```

#### 2. CFLAGS、LDFLAGS、LD_LIBRARY_PATH区别

```shell
#CFLAGS与LDFLAGS是在编译时指定头文件与库文件路径,该变量会在makefile中使用

#例如在shell中导出该环境变量，实际即是给该变量赋值:
export LDFLAGS="-L/var/xxx/lib -L/opt/mysql/lib -Wl,R/var/xxx/lib -Wl,R/opt/mysql/lib"

#LD_LIBRARY_PATH在程序运行时指定依赖的库路径
export LD_LIBRARY_PATH=${rootDir}lib:$LD_LIBRARY_PATH

参考链接:
https://www.cnblogs.com/traxex/archive/2013/04/02/5850899.html
```

#### 3. shell脚本中的[]

```shell
 中括号[]:用于条件判断
 
 #脚本内容如下:
 
 #! /bin/sh

[ $1 = "zk" ]

echo "test 1"
echo $?

[[ $1 = "zk" ]]
echo "test 2"
echo $?

#当比较的变量中有空格时,变量需要用""括起来                         
[ "$name" = "hello world" ]
                           
echo "test 4"              
echo $?                    
                           
[[ $name = "hello world" ]]
                           
echo "test 5"              
echo $?

#备注:
$?：执行上一个指令的返回值 (显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误
```



#### 4. shell脚本中的case语句

```shell
#脚本内容:

#! /bin/sh
case $1 in
    1) 
     echo "小二，来一碗米饭"
    ;; 
    2) 
     echo "小二，来一碗面条"
    ;; 
    3) 
     echo "小二，来一锅包子"
    ;; 
esac

#备注：以case开头，以esac结尾

```

