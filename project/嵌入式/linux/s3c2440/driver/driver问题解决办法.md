#### 1. 内核启动问题

##### 1.1 问题现象

```
当咋uboot中，用setenv、saveenv修改环境变量并保存时，上电重新启动时，内核启动不起来，并报校验错误。如果不修改即能启动起来
```

##### 1.2 问题排除步骤

```shell
1.从nandflash中读取数据到内存中,读取前后环境变量的数据和内核的数据

//读取环境变量分区参数
nand read.jffs2 0x30007FC0 params
//读取内核分区参数
nand read.jffs2 0x30007FC0 kernel
 
//读取内存中的数据
md 0x30007FC0

2.发现修改前后环境变量数据没有发生变化，但是内核数据发生变化，怀疑使用saveenv保存环境变量时，数据存储位置错误
```

##### 1.3 问题原因

```c
修改uboot环境变量参数保存位置，需要修改uboot的源码,修改变量宏定义CFG_ENV_OFFSET
源码位置:include/configs/100ask24x0.h
    
#define MTDIDS_DEFAULT "nand0=nandflash0"
#define MTDPARTS_DEFAULT "mtdparts=nandflash0:256k@0(bootloader)," \
                                                        "128k(device_tree)," \
                                                        "128k(params)," \
                            "4m(kernel)," \
                            "-(root)"

//#define       CFG_ENV_IS_IN_FLASH     1
#define CFG_ENV_IS_IN_NAND  1
//对应上面的params分区,bootloader起始地址0x0，device_tree起始地址0x40000，params起始地址0x60000
#define CFG_ENV_OFFSET      0x60000
//params分区长度，128k
#define CFG_ENV_SIZE            0x20000 /* Total Size of Environment Sector */
    
参考文档:
//U-Boot移植--环境变量保存位置
https://blog.csdn.net/q1302182594/article/details/51355285
```

##### 1.4 问题知识点

```shell
#知识点1:查看uboot中有哪些命令,由于不同环境不同，因此uboot中命令可能有所不同，使用?查看具体有哪些命令
  ?
	?       - alias for 'help'
	autoscr - run script from memory
	base    - print or set address offset
	bdinfo  - print Board Info structure
	boot    - boot default, i.e., run 'bootcmd'
	...
	
#知识点2:查看uboot中具体某个命令的用法
 ? nand
	nand info                  - show available NAND devices
	nand device [dev]     - show or set current device
	nand read[.jffs2]     - addr off|partition size
	nand write[.jffs2]    - addr off|partiton size - read/write `size' bytes starting
		at offset `off' to/from memory address `addr'
	...
	
#知识点3:uboot中Nand flash常用操作命令解析
	(1)nand info
	查看nandflash 信息
	Wisdom # nand info
	Device 0: MX35LF1GE4AB, sector size 128 KiB

    (2)nand device
        与nand info 的信息是一样的。
        Wisdom # nand device
        Device 0: MX35LF1GE4AB, sector size 128 KiB

    (3)nand read(.oob) [addr] [off] [size]
        读取命令，这里是两条命令：
        nand read [addr] [off] [size]
        nand read.oob [addr] [off] [size]
        不管是读取data,使用nand read,还是读取oob,使用命令nand read.oob,后面跟的地址addr,都是ram的地址,off指的是nand flash的地址,size指要读取nand flash的数据大小,但是如果是读取oob,size不能超过一个page的oob size,如果page size为512个字节,ob size就是16个字节.
        如果一次想读取完整的一个page 的值,包含oob,使用下面将的命令,nand dump. 

    (4)nand dump [addr] [size]
        读取flash addr地址开始的size 大小数据出来。最小单位是一个page.也就是说size小于一个page,也会读出一个page的数据。该数据包括oob数据。

    (5)nand write [addr] [off] [size]
        与nand read 命令类似，将内存地址addr的size大小数据写入到flash的off偏移地址去，该命令会自动跳过坏块。

    (6)nand erase/clean [off] [size]
        清除flash off偏移地址开始的size大小的数据，最小单位是一个page。
    
参考文档:
uboot中Nand flash 常用操作命令解析
https://blog.csdn.net/li_wen01/article/details/88918066

#知识点4:uboot分区与系统内核中MTD分区的关系
(1)uboot对分区的概念不重要，只要把bootloader、内核、根文件系统烧写相应的位置即可，为了方便有了分区的概念
分区中最关键参数2个：起始地址、长度，一般情况下使用分区名字替代起始地址和长度
分区源码位置:include/configs/100ask24x0.h
//分区名字(分区大小)
#define MTDIDS_DEFAULT "nand0=nandflash0"
#define MTDPARTS_DEFAULT "mtdparts=nandflash0:256k@0(bootloader)," \
                                                        "128k(device_tree)," \
                                                        "128k(params)," \
                            "4m(kernel)," \
                            "-(root)"
                            
(2)分区只是内核的概念，也就是哪个地址存放内核、哪个地址存放根文件系统
分区源码位置:arch/arm/mach-s3c24xx/common-smdk.c
static struct mtd_partition smdk_default_nand_part[] = { 
        [0] = { 
                .name   = "bootloader",
                .size   = SZ_256K, //大小
                .offset = 0, //起始地址
        },
        [1] = { 
                .name   = "device_tree",
                .offset = MTDPART_OFS_APPEND,
                .size   = SZ_128K,
        },
        [2] = { 
                .name   = "params",
                .offset = MTDPART_OFS_APPEND,
                .size   = SZ_128K,
        },
        [3] = { 
                .name   = "kernel",
                .offset = MTDPART_OFS_APPEND,
                .size   = SZ_4M,
        },
        [4] = { 
                .name   = "rootfs",
                .offset = MTDPART_OFS_APPEND,
                .size   = MTDPART_SIZ_FULL,
        }
};

参考文档:
修改分区:
https://blog.csdn.net/manshq163com/article/details/50995600?utm_source=blogxgwz4
uboot分区与系统内核中MTD分区的关系:
https://blog.csdn.net/jsn_ze/article/details/53766877

```



