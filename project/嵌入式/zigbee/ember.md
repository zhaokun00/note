### ember zigbee

#### 1.zigbee基础知识

- 概述

  zigbee技术是一种短距离、低复杂度、低功耗、低速率、低成本的双向无线通信技术或无线网络技术,是一组基于IEEE802.15.4无线标准。由于IEEE802.15.4标准只定义了物理层和MAC层协议,于是成立了zigbee联盟，zigbee联盟对其网络层协议和API进行了标准化，还开发了安全层

- 无线通信方式对比

  ![](../../../img/无线通信对比.png)

- 设备类型

  zigbee的基本成员被称为设备,在zigbee网络中,有3种不同类型的设备，分别是协调器-coorinator、路由器-router、终端节点-end device，如下图所示:

  ![](../../../img/zigbee设备类型.png)

  - 协调器(ZC)
    - 定义:每个zigbee网络只允许有一个zigbee的协调器，协调器首先选择一个信道和网络标识(PANID),然后开始这网络。因为协调器是整个网络的开始，它具有网络的最高权限,是整个网络的维护者,还可以保持间接寻址的表格绑定,同时还可以设计安全中心和执行其他动作，保持网络其他设备的通信
    - 功能特点:
      - 选择一个信道和PANID,组建网络
      - 允许路由和终端节点加入这个网络
      - 对网络中的数据进行路由
      - 必须常供电,不能进入睡眠模式
      - 可以为睡眠的终端节点保留数据，至其唤醒后获取

  - 路由器(ZR)
    - 定义:一种支持关联的设备，能够允许其他设备节点入网
    - 功能特点:
      - 在进行数据收发之前,必须首先加入到一个zigbee网络
      - 本身加入网络后,允许路由和终端节点加入
      - 加入网络后，可以对网络中的数据进行路由
      - 必须长供电，不能进入睡眠模式
      - 可以为睡眠的终端节点保留数据，至其唤醒后获取
  - 终端设备(ZE)
    - 定义:具有执行的数据采集传输的设备，不能转发其他节点的消息
    - 功能特点:
      - 在进行数据收发之前，必须首先加入一个zigbee网络
      - 不能允许其他设备的加入
      - 必须通过其他父节点收发数据，不能对网络中的数据进行路由
      - 可由电池供电，进入睡眠模式

- 组网方式

  - 星形拓扑

    - 定义:最简单的一种拓扑形式,它包含一个coordinator节点和一系列的End Device节点。每个End Device节点只能和Coordinator节点进行通讯。如果需要在两个End Device节点之间进行通讯必须通过Coordinator节点进行信息的转发

      ![](../../../img/zigbee-星形拓扑.png)

  - 树形拓扑

    - 定义:树形拓扑包括一个Coordinator以及一些列的router和End Device节点。Coordinator连接一些列的Router和End Device,他的子节点Router也可以连接一些列的Router和EndDevice,这样可以重复多个层级

    ![](../../../img/zigbee-树形拓扑.png)

  - 网络拓扑-Mesh拓扑

    - 定义:Mesh拓扑包含一个Coordinator和一些列的Router和End Device。这种拓扑形式和树形拓扑相同。但是，网状拓扑具有更加灵活的信息路由规则，在可能的情况下，路由节点之间可以直接的通讯。这种路由机制使得信息的通讯变得更有效率，而且意味着一旦一个路由路径出现了问题，信息可以自动的沿着其他路由路径进行传输

      ![](../../../img/zigbee-  网络拓扑.png)

- 通信方式

  - 单播(unicatst) - 网络短地址

    unicast是标准寻址模式,它将数据包发送给一个已经知道的网络地址的设备，将afAddrMode设置为Addr16Bit并且在数据包中携带目标设备地址

  - 组播(multicats)

  - 广播(broadccast)

    当应用程序需要将数据包发送给网络的每一个设备时,使用这种模式，将afAddrMode设置为AddrBroadccast，目的地址可以设置下面的其中一种:

    NWK_BROADCAST_SHORTADDR_DEVALL(0xFFFF)——数据包将被传送到网络上的所有设备，包括睡眠中的设备。对于睡眠中的设备，数据包将被保留在其父亲节点直到查询到它，或者消息超时(NWK_INDIRECT_MSG_TIMEOUT 在f8wConifg.cfg 中)。 

    NWK_BROADCAST_SHORTADDR_DEVRXON(0xFFFD)——数据包将被传送到网络上的所有在空闲时打开接收的设备(RXONWHENIDLE)，也就是说，除了睡眠中的所有设备。 

    NWK_BROADCAST_SHORTADDR_DEVZCZR(0xFFFC)——数据包发送给所有的路由器，包括协调器。  

  - MAC地址 - 物理地址

  - 绑定

    - 定义
    - 绑定的方式

- 通信方式代码分析

  - 单播(unicatst) - 网络短地址
  - 组播(multicats)
  - 广播(broadccast) 
  - MAC地址 - 物理地址
  - 绑定

- 专业名词

  - 物理地址(MAC地址):64位IEEE地址,即MAC地址,通常也成为长地址，是全球唯一的地址，8字节

  - 网络地址:在zigbee无线局域网里，每个模块都在一个局域网络里有一个唯一的2字节的地址，叫做网络地址又称为短地址(在网络中通信使用的是短地址,相当于互联网通信中的ip地址)

  - PANID:局域网id一个2字节的编码，用来区别不同的zigbee无线局域网。

  - 端点(Endpoint):用来表示应用层的功能,相当于网络编程中的端口号
    - 1.数据收发的基本单元,用一个字节进行编号,在模块通信的时候，发送模块必须指定收发双发模块的网络地址和端点
    - 2.端点要使用必须和模块里面的某个任务(任务相当于一个应用)挂钩定义,首先每一个端点可以看成1个字节数字编号的房间，数据最终的目标是进入到无线数据包指定的目标端点的房间
    - 3.一个端点只能挂接在一个任务上，而一个任务可以挂接多个端点。相当于在网络编程中一个端口只能让一个应用使用，而一个应用可以占用多个端口
    - 在实际zigbee应用中总共定义了240个不同的应用对象，通过端点来描述，端点接口索引号为1~240，还有两个特殊的端点:
      - 端点0:只为ZDO的数据接口服务
      - 端点255:供应用对象的广播数据接口功能

  - 簇(Cluster ID):用来表示应用的主题,相当于网络通信中MQTT中的主题
    - 簇相当于端点房间中的里面的人，是接收最终的目标，簇是2个字节的编号，在射频发送的时候，必须要指定接收模块的簇，发送模块不需要指定
  - 发送方和接收方收发收据必要素
    - 发送方网络短地址、端点号
    - 接收方网络短地址、端点号、簇id

  - 属性:是应用程有用的数据载荷
  - MAC地址管理器
  - 绑定表

- 协议栈分析

  ![](../../../img/zigbee体系结构.png)

  - 分层架构简介

    - 1、PHY:物理层

    - 2、MAC:数据链路层

    - 3、NWK:网络层

    - 4、应用层(包括APS、ZDO、AF)

      - APS：应用支持子层, Application Support Sub Layer
      - ZDO：设备对象, Zigbee Device Object

      - AF：应用程序框架，Application Framework

  - 各层具体功能

    - 物理层（PHY层）

      - 主要功能:激活硬件发送和接收数据；选择Channel Frequency。

      - 基本构成:

        在整个zigbee网络中，物理层是距离硬件最低的层，因此它直接控制并发送无线收发器通信，负责激活发送或接收数据包的无线设备。还具有选择信道的频率并确保该信道当前没有被任何一个其他网络所使用。标准规定的物理层包括一个管理实体，即物理层管理实体（Physical Layer Management Entity, PLME），分别提供以下两个服务：

        PD-SAP：数据服务接入点，Physical Data SAP

        PLME-SAP：管理服务接入点，Physical Layer Management Entity SAP

        在 PLME-SAP 中，包含PHY-PIB（物理层个域网信息数据库），整个物理层也有一个RF-SAP（无线发送接收访问接口）的称呼。

    - MAC层

      - 主要功能:负责产生信标（Beacon）和为信标（beacon-enable网络）同步设备。MAC层还提供建立连接和解除连接的服务。

      - 基本构成:

        如下图所示。MAC层和物理层一样，也包含一个管理实体，称为MLME（MAC Layer Management Entity）。负责维护和 MAC 子层相关的管理目标数据库。，也就是 MAC 子层的PAN（Personal Area Network）信息数据库。

    - 网络层(NWK层)

      - 主要功能:负责形成网络及路由信息的建立（选择将信息发送到目标设备的路径）。此外，协调器的NWK层还负责建立新的网络及选择网络拓扑（星形，树形及网状结构）、分配节点地址等功能。

      - 基本构成:

        如下图所示，同样的NWK层也有相应的数据服务实体NLDE（NWK Layer Data Entity）和管理服务实体：MLME,（NWK Layer Management Entity）

        其中NLDE提供的服务有：生成网络数据单元：也就是对上层的数据进行分段、封装，以及指定路由拓扑和安全支持。

        MLME提供的服务有：配置新设备，建立新网络，允许设备加入或离开网络，路由的发现邻居寻址。

    - 应用层（APL层）
      应用层包含了以下三个部分:APS：应用支持子层，ZDO：设备对象，AF：应用程序框架。

      - 应用支持子层APS

        提供以下两个服务实体：

        应用支持子层数据实体：APSDE，Application Support Sub Layer Data Entity
        应用支持子层管理实体：APSME, Application Support Sub Layer Management Entity
        相应的，也就有APSDE-SAP和APSME-SAP。

      - 应用程序框架

        在应用程序框架（AF, Application Framework）内部，ZigBee 设备对象通过APSDE-SAP来收发数据。总共定义了240个不同的应用对象（Application Object），通过端点来描述，端点接口索引号为1~240。此外还有两个特殊端点：

        ​	端点0：只为ZDO的数据接口服务

        ​    端点255：供应用对象的广播数据接口功能

      - ZDO
        ZDO负责初始化APS,NWK及安全子层，ZigBee 协议栈中的ZDO特指端点号为0的ZigBee 设备对象。ZDO 管的事情实际上纵跨几个层：

        网络角色定义：设备是 coordinator、router 还是end-point

        设备发现:设备发现是ZigBee设备为什么能发现其他设备的过程。这有两种形式的设备发现请求：IEEE地址请求和网络地址请求。IEEE地址请求是单播到一个特殊的设备且假定网络地址已经知道。网络地址请求是广播且携带一个已知的IEEE地址作为负载。

        服务发现:服务发现是为什么一个已知设备被其他设备发现的能力的过程。服务发现通过在一个已知设备的每一个端点发送询问或通过使用一个匹配服务（广播或者单播）。服务发现方便定义和使用各种描述来概述一个设备的能力。服务发现信息在网络中也许被隐藏，在这种情况下，设备提供的特殊服务便可能不在操作发生的时候到达。

- 空中抓包分析

- 数据帧格式分析

#### 2.ember

- 硬件选型

  ##### A、ZigBee芯片方案选型

  | 选项                  | Silicon Labs                                                 | TI                                                  | Nordic           |
  | --------------------- | ------------------------------------------------------------ | --------------------------------------------------- | ---------------- |
  | 型号                  | EFR32MG                                                      | CC2652R                                             | nRF52840         |
  | 内核                  | Cortex-M4                                                    | Cortex-M4                                           | Cortex-M4        |
  | 主频                  | 40MHz                                                        | 48MHz                                               | 64MHz            |
  | FLASH                 | 256-1024 KB                                                  | 352 KB                                              | 1024KB           |
  | RAM                   | 32-265 KB                                                    | 80KB（256KB ROM）                                   | 256KB            |
  | <u>*最大输出功率*</u> | <u>*8-19.5 dBm*</u>                                          | <u>*5dBm*</u>                                       | <u>*8dBm*</u>    |
  | <u>*接收灵敏度*</u>   | <u>*-99到-103.3 dBm*</u>                                     | <u>*-100dBm*</u>                                    | <u>*-100dBm*</u> |
  | RX电流                | 9.8mA                                                        | 6.9mA                                               | 4.6mA            |
  | TX电流                | 8.2A(0dBm)                                                   | 7.3mA(0dBm)                                         | 4.8mA            |
  | 睡眠                  | 1.5uA(EM2)                                                   | 0.95uA                                              | 0.4uA            |
  | ZigBee协议栈          | EmberZNet                                                    | Z-STACK                                             | nRF5 SDK         |
  | 支持协议              | ZigBeeThreadBLEProprietary ProtocolsWireless M-Bus802.15.4gLPWA | ZigBeeThreadBLEProprietary ProtocolsWi-SUN802.15.4g | ZigBeeThreadBLE  |
  | 软件开发平台          | Simplicity StudioIAR                                         | IAR                                                 | IARKeil          |

  基于更好的无线性能选择，我们选择Silicon Labs（芯科）的芯片。

  我们知道作为ZigBee无线通信频段2.4G上是有多种无线通信协议（如ZigBee、蓝牙、Wi-Fi等）在运行的，由于同频的干扰，多一个dBm就多一份通信保证，多一段通信距离。

  ##### B、ZigBee开发方案选型

   1、网状网片上系统（SoC）

  ​	在Silicon Labs（芯科科技）公司官网上，可以看到Silicon Labs公司所有的ZigBee系列SoC。Silicon Labs推出的ZigBee SoC解决方案主要分为两个系列：EM 和 EFR。

   2、网状网模组（Module）

  ​	在Silicon Labs公司官网上，还可以看到该公司所有的ZigBee系列模组，Silicon Labs推出的ZigBee模组主要也分为两个系列：ETRX 和MGM。

   3、EM与EFR系列

  ​	关于EM系列，其实是Ember的简写，取自于Ember的前两个字母。EM系列SoC可能就是Ember公司的ZigBee在Silicon Labs公司的延续。Silicon Labs收购Ember。

  ​	Silicon Labs公司EM系列的ZigBee SoC，主要采用的是ARM Cortex-M3的内核架构，是Silabs较为成功的代表性产品。在芯片制造和协议栈stack的开发上，都相当的不错。此外，EM系列的ZigBee芯片相比于EFR系列来说，价格要便宜一些。

  ​	关于EFR系列，主要采用的是ARM Cortex-M4的内核架构。相比于EM系列，EFR系列的芯片可以做到更低的功耗，更省电。此外，同等的footprint的芯片，EFR系列的RF射频性能更优。当然，相比于EM系列芯片来说，EFR系列价格要略高一些。

  ​	从工艺角度来说，EM大多采用的是140 nm工艺制造，而EFR大多是90 nm工艺。Silicon Labs最新推出的ZigBee芯片中，已经或将会采用40 nm的制造工艺。

  ​	EM系列已经非常成熟，芯片和模组都非常丰富；新出的EFR系列目前以芯片为主，提供的模组暂时不如EM系列多。

  ​	EM系列的ZigBee SoC大多仅支持ZigBee，部分还支持Thread；而EFR32系列SoC中（如：EFR32MG12P332F1024IM48）最多可支持高达4种协议：ZigBee、Bluetooth、Thread、Sub-GHz。

   4、SoC与Module

  ​	大家对TI的ZigBee方案比较熟悉，知道TI有：CC2530、CC2630、CC2652R等等。一般只有SoC，而没有所谓的Module。这是因为，TI一般只提供SoC，同时会将相应最小系统的外围电路公开，需要Module的用户直接参考着自己进行设计即可。

  ​	对于Silicon Labs，乍一看好像提供了两套ZigBee解决方案。其实不然，将Silicon Labs的ZigBee SoC与Module稍作对比，即可看出：Silicon Labs所有的ZigBee Module，其实全部都是基于该公司的SoC设计而成的。这是因为，Silicon Labs不仅提供SoC，同时也提供功能非常完善的Module（可选择出厂是否带有ZigBee固件）。这样就清楚了，要理清Silicon Labs的ZigBee体系，理清其ZigBee SoC的体系即可。

  当然，不管是哪家的ZigBee方案，都可以做成Module，而市面上也有非常多现成的ZigBee Module可以选择。有以下几点供大家参考：

  1、如果用户不想关心硬件设计过程，只想从事ZigBee固件开发，可以选择出厂不带任何固件的ZigBee Module。

  利：到手即可进行开发，类似于开发套件，非常方便。

  弊：当然，这样肯定会比从SoC开始设计的硬件成本要多出很多。

  2、如果用户既不想关心硬件设计，也不想关心固件开发，而只是想利用ZigBee低成本、大容量、自组织等特性，那么可以选择出厂即带完善功能固件的Module。

  利：这样，用户不必关心复杂的ZigBee原理，以及漫长的研发过程，直接通过串口发送AT指令等简单的方式，即可使系统拥有非常完善的ZigBee功能。

  弊：当然，除了上面提到会增加硬件成本之外，灵活性自然也会不如完全自主开发出来的ZigBee固件。

  3、用户需要把ZigBee芯片嵌入到自己产品内，需要进行硬件设计，则需要选择ZigBee SOC。

  利：灵活方便，完全按自己产品功能进行设计，可进行技术创新和技术保密。

  弊：更长的开发周期。

  - 官方参考链接:https://cn.silabs.com/wireless/zigbee

  ![](../../../img/zigbee-硬件型号1.png)

  

  - EFR芯片系列型号:

  ![](../../../img/zigbee-EFR型号.png)

  - EFR芯片型号命名规则:

  ![](../../../img/zigbee-EFR型号命名规则.png)

  - EFR1系列与2系列射频对比分析

    ![](../../../img/zigbee-EFR型号射频对比.png)

- 软件环境搭建

  