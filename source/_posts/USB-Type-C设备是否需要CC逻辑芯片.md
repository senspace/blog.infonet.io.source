---
title: USB-Type-C设备是否需要CC逻辑芯片
date: 2020-06-13 21:32:52
updated: 2020-06-13 21:32:52
categories:
tags:
- [USB]
---

# 1. 基本概念

1. DFP(Downstream Facing Port)
   
   下行端口，可以理解为Host，DFP提供VBUS，也可以提供数据。典型的DFP设备是电源适配器，因为它永远都只是提供电源。

2. UFP(Upstream Facing Port)
   
   上行端口，可以理解为Device，UFP从VBUS中取电，并可提供数据。典型设备是U盘，移动硬盘，因为它们永远都是被读取数据和从VBUS取电，当然不排除未来可能出现可以作为主机的U盘。

3. DRP(Dual Role Port)
   
   双角色端口，DRP既可以做DFP(Host)，也可以做UFP(Device)，也可以在DFP与UFP间动态切换。典型的DRP设备是电脑(电脑可以作为USB的主机，也可以作为被充电的设备（苹果新推出的MAC Book Air）)，具OTG功能的手机(手机可以作为被充电和被读数据的设备，也可以作为主机为其他设备提供电源或者读取U盘数据)，移动电源(放电和充电可通过一个USB Type-C，即此口可以放电也可以充电)。
 
<!---more--->

4. CC（Configuration Channel）
   
   配置通道，这是USB Type-C里新增的关键通道，它的作用有检测USB连接，检测正反插，USB设备间数据与VBUS的连接建立与管理等。
 
5. USB PD(USB Power Delivery)
   
   PD是一种通信协议，它是一种新的电源和通讯连接方式，它允许USB设备间传输最高至100W（20V/5A）的功率，同时它可以改变端口的属性，也可以使端口在DFP与UFP之间切换，它还可以与电缆通信，获取电缆的属性。
 
6. Electronically Marked Cable
   
   封装有E-Marker芯片的USB Type-C有源电缆，DFP和UFP利用PD协议可以读取该电缆的属性：电源传输能力，数据传输能力，ID等信息。所有全功能的Type-C电缆都应该封装有E-Marker，但USB2.0 Type-C电缆可以不封装E-Marker。

# 2. USB Type-C设备DFP-to-UFP配置流程与VBUS管理有如下主要流程：
 
1. 设备连接与分开检测
   
   DFP需要检测到CC管脚上有某个电压时，判断UFP设备已插入或拔出，来提供和管理VBUS。当没有UFP设备插入时，必须关闭VBUS。因此所有的DFP设备需要CC逻辑检测与控制芯片。
 
2. 插入方向检测
   
   如图1，虽然USB Type-C插座和插头的两排管脚上下对称，USB数据信号都有两组重复的通道，但主控芯片通常只有一组TX/RX和D+/-通道。由于USB2.0的数据率最高只有480Mbps, 可以不考虑信号走线的阻抗连续性而得到较好地数据传输质量，因此USB2.0的D+/-信号可以不被MUX控制而直接从主控芯片一分二连接至USB Type-C插座的两组D+/-管脚上。但USB3.0或者USB3.1的数据率高达5Gbps或者10Gbps，如果信号线还是被简单地一分二的话，不连续的信号线阻抗将严重破坏数据传输质量，因此必须由MUX切换来保证信号路径阻抗的一致性，以确保信号传输质量。下图中右侧所示的MUX从TX1/RX1和TX2/RX2中选择一路连接至主控芯片，而这个MUX就必须被CC Logic控制。

   ![图1.USB-Type-C数据走线逻辑模型](https://file.infonet.io/blog-files/USB/%E5%9B%BE1.USB-Type-C%E6%95%B0%E6%8D%AE%E8%B5%B0%E7%BA%BF%E9%80%BB%E8%BE%91%E6%A8%A1%E5%9E%8B.jpg)

   但必须注意的是在USB3.0/USB3.1的应用中，有一种情况也可以不需要MUX，即不需要方向检测，如图2所示，不管是正插还是反插，左侧主机都可以根据CC管脚上的状态来切换MUX来连通USB3.0/USB3.1信号。此场景发生在右侧设备永远是UFP的情况下，比如U盘，移动硬盘等。

   因此，在USB2.0应用中，无需考虑方向检测问题，但USB3.0或者USB3.1应用中，必须考虑方向检测问题。USB3.0/USB3.1应用中，除UFP设备以外的所有设备都需要CC逻辑检测与控制芯片。

   ![图2.USB-Type-C直接连接数据走线逻辑模型](https://file.infonet.io/blog-files/USB/%E5%9B%BE2.USB-Type-C%E7%9B%B4%E6%8E%A5%E8%BF%9E%E6%8E%A5%E6%95%B0%E6%8D%AE%E8%B5%B0%E7%BA%BF%E9%80%BB%E8%BE%91%E6%A8%A1%E5%9E%8B.jpg)

1. 建立DFP-to-UFP和VBUS管理与检测
   
   DRP在待机模式下每50ms在DFP和UFP间切换一次。当切换至DFP时，CC管脚上必须有一个上拉至VBUS的电阻Rp或者输出一个电流源，当切换至UFP时，CC管脚上必须有一个下拉至GND的电阻Rd。此切换动作必须由CC Logic芯片来完成。当DFP检测到UFP插入之后才可以输出VBUS，当UFP拔出以后必须关闭VBUS。此动作必须由CC Logic芯片来完成。

2. USB Type-C VBUS电流检测与使用
   
   USB Type-C中新增了电流检测与使用功能，新增三种电流模式：默认的USB电源模式(500mA/900mA)，1.5A，3.0A。三种电流模式由CC管脚来传输和检测，对于需要广播电流输出能力的DFP而言，需要通过不同值的CC上拉电阻Rp来实现；对于UFP而言，需要检测CC管脚上的电压值来获取对方DFP的电流输出能力。

3. USB PD通信
   
   USB PD看似只是电源传输与管理的协议，实际上它可改变端口角色，可与有源电缆通讯，允许DFP成为受电设备等诸多高级功能，因此支持PD的设备必须采用CC Logic芯片。

4. 发现与配置扩展其他外设(Audio，Debug)

   USB Type-C支持语音附件以及Debug模式，USB Type-C接口的耳机如果只作为UFP且因为其功耗较小而无需检测DFP的供电能力时，无需CC Logic芯片。

# 3. 总结

综上，所有的DFP(如电源适配器)，所有的DRP(如电脑，手机，平板，移动电源), 所有需要检测DFP电流输出能力的UFP，所有支持PD的设备，都需要CC逻辑检测与端口控制芯片。换句话说，只有因为功耗较低而不需要检测电流能力的UFP(U盘，耳机，鼠标等)才不需要CC逻辑检测端口控制芯片。

# 4. 版权

本文摘自电子工程世界：http://news.eeworld.com.cn/xfdz/2015/0323/article_40868.html

如有侵权烦请联系博主删除。
