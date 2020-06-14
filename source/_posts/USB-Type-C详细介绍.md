---
title: USB-Type-C详细介绍
date: 2020-06-13 21:32:52
updated: 2020-06-13 21:32:52
categories:
tags:
- [USB]
---

# 1. USB-Type-C接口包括

- 支持DisplayPort, PCIe等alternate mode的两组Superspeed USB线路(RX1/TX1和RX2/TX2)
- 用于Alternate Mode的两条Sideband Use(SBU)线路
- 用于Hi-Speed USB 2.0的Dp和Dn线路
- 用于总线供电的VBUS线路
- 用于为线缆控制器供电的VCONN 线路(只位于Type-C插头接口上)
- 用于PD通信的配置信道(CC)。

# 2. 插头反转

Type-C插座可处理线缆插头的任意方向。USB-Type-C插座完全对称。所有的供电、接地和信号引脚两边对称，从而让USB-Type-C插头能够在Type-C接口中任意翻转。

- GND、USB 2.0和VBUS 信号保持连接。USB 2.0信号被复制到Type-C插座的上下两层，以保持任意方向的连接。
- 插头上的VCONN 或CC引脚可以连接插座中的任意一个配置信道引脚-CC1或CC2 (取决于插入方向)。
- 两条Superspeed线路的其中一条保持正确连接，USB-Type-C 插座必须使用SuperSpeed mux合理地进行连接

![USB-Type-C正反插](https://file.infonet.io/blog-files/USB/USB-Type-C%E6%AD%A3%E5%8F%8D%E6%8F%92.png)
 
<!---more--->

# 3. USB-Type-C设备基本概念

   1. DFP(Downstream Facing Port)
      
      下行端口，可以理解为Host，DFP提供VBUS，也可以提供数据。典型的DFP设备是电源适配器，因为它永远都只是提供电源。

   2. UFP(Upstream Facing Port)
      
      上行端口，可以理解为Device，UFP从VBUS中取电，并可提供数据。典型设备是U盘，移动硬盘，因为它们永远都是被读取数据和从VBUS取电，当然不排除未来可能出现可以作为主机的U盘。

   3. DRP(Dual Role Port)
      
      双角色端口，DRP既可以做DFP(Host)，也可以做UFP(Device)，也可以在DFP与UFP间动态切换。典型的DRP设备是电脑(电脑可以作为USB的主机，也可以作为被充电的设备（苹果新推出的MAC Book Air）)，具OTG功能的手机(手机可以作为被充电和被读数据的设备，也可以作为主机为其他设备提供电源或者读取U盘数据)，移动电源(放电和充电可通过一个USB-Type-C，即此口可以放电也可以充电)。

   4. CC（Configuration Channel）
      
      配置通道，这是USB-Type-C里新增的关键通道，它的作用有检测USB连接，检测正反插，USB设备间数据与VBUS的连接建立与管理等。
   
   5. USB PD(USB Power Delivery)
      
      PD是一种通信协议，它是一种新的电源和通讯连接方式，它允许USB设备间传输最高至100W（20V/5A）的功率，同时它可以改变端口的属性，也可以使端口在DFP与UFP之间切换，它还可以与电缆通信，获取电缆的属性。
   
   6. Electronically Marked Cable
      
      封装有E-Marker芯片的USB-Type-C有源电缆，DFP和UFP利用PD协议可以读取该电缆的属性：电源传输能力，数据传输能力，ID等信息。所有全功能的Type-C电缆都应该封装有E-Marker，但USB2.0 Type-C电缆可以不封装E-Marker。


# 4. USB-Type-C设备是否需要CC逻辑芯片

## 1. USB-Type-C设备DFP-to-UFP配置流程与VBUS管理有如下主要流程：

### 1. 设备连接与分开检测

DFP需要检测到CC管脚上有某个电压时，判断UFP设备已插入或拔出，来提供和管理VBUS。当没有UFP设备插入时，必须关闭VBUS。因此所有的DFP设备需要CC逻辑检测与控制芯片。

### 2. 插入方向检测

#### 1. 解释1

   如图下图，虽然USB-Type-C插座和插头的两排管脚上下对称，USB数据信号都有两组重复的通道，但主控芯片通常只有一组TX/RX和D+/-通道。由于USB2.0的数据率最高只有480Mbps, 可以不考虑信号走线的阻抗连续性而得到较好地数据传输质量，因此USB2.0的D+/-信号可以不被MUX控制而直接从主控芯片一分二连接至USB-Type-C插座的两组D+/-管脚上。但USB3.0或者USB3.1的数据率高达5Gbps或者10Gbps，如果信号线还是被简单地一分二的话，不连续的信号线阻抗将严重破坏数据传输质量，因此必须由MUX切换来保证信号路径阻抗的一致性，以确保信号传输质量。下图中右侧所示的MUX从TX1/RX1和TX2/RX2中选择一路连接至主控芯片，而这个MUX就必须被CC Logic控制。

   ![USB-Type-C数据走线逻辑模型](https://file.infonet.io/blog-files/USB/USB-Type-C%E6%95%B0%E6%8D%AE%E8%B5%B0%E7%BA%BF%E9%80%BB%E8%BE%91%E6%A8%A1%E5%9E%8B.jpg)

   但必须注意的是在USB3.0/USB3.1的应用中，有一种情况也可以不需要MUX，即不需要方向检测，如下图所示，不管是正插还是反插，左侧主机都可以根据CC管脚上的状态来切换MUX来连通USB3.0/USB3.1信号。此场景发生在右侧设备永远是UFP的情况下，比如U盘，移动硬盘等。

   因此，在USB2.0应用中，无需考虑方向检测问题，但USB3.0或者USB3.1应用中，必须考虑方向检测问题。USB3.0/USB3.1应用中，除UFP设备以外的所有设备都需要CC逻辑检测与控制芯片。

   ![USB-Type-C直接连接数据走线逻辑模型](https://file.infonet.io/blog-files/USB/USB-Type-C%E7%9B%B4%E6%8E%A5%E8%BF%9E%E6%8E%A5%E6%95%B0%E6%8D%AE%E8%B5%B0%E7%BA%BF%E9%80%BB%E8%BE%91%E6%A8%A1%E5%9E%8B.jpg)

#### 2. 解释2

   USB-Type-C规范解释了上行面端口(UFP)如何利用CC引脚CC1和CC2上的下拉电阻(Rd)申报成为一个外设。下行面端口(DFP)需要在CC1和CC2上配备上拉电阻(Rp)。所构成的电阻分压器用于确定Type-C外设是连接还是分离状态;以及Type-C插头的方向，因为线缆中只连接了一个CC引脚 (如下图所示)。

   DFP，尤其扮演USB 数据主机的DFP，通常是指PC等主机上的端口或者设备连接的集线器上的下行端口。在其初始状态，DFP为VBUS 和VCONN供电。UFP是指设备上的端口或者连接DFP的集线器上的上行端口。

   Type-C电子标记线缆组件(EMCA)需要使用VCONN 向线缆内部的标记电子元件供电。这些EMCA在Type-C 插头的VCONN 引脚上配有Ra终结电阻器。没有电子标记的Type-C线缆不含电子元件，因此不需要VCONN 供电。这些非EMCA线缆 中的 VCONN 引脚没有固定在Type-C插头。

   ![USB-Type-C连接方向检测](https://file.infonet.io/blog-files/USB/USB-Type-C%E8%BF%9E%E6%8E%A5%E6%96%B9%E5%90%91%E6%A3%80%E6%B5%8B.png)

   Type-C插座CC引脚上的Rp和Rd终结电阻器能够检测连接事件，并识别插座中Type-C插头的方向。DFP和UFP监测Type-C插座中两个CC引脚的电压相对于无端接电压的电压变化，以此监测连接事件。UFP还监测VBUS，是检测DFP连接的另一个指标。DFP和UFP都能从各自连接端识别Type-C插头方向，这是因为线缆中只连接了一个CC引脚。确定连接和方向后，如果检测到一个Ra终结电阻器，DFP将把另一个CC引脚更改为VCONN，以便向USB-Type-C EMCA插头中的电子元件供电。

### 3. 建立DFP-to-UFP和VBUS管理与检测

   DRP在待机模式下每50ms在DFP和UFP间切换一次。当切换至DFP时，CC管脚上必须有一个上拉至VBUS的电阻Rp或者输出一个电流源，当切换至UFP时，CC管脚上必须有一个下拉至GND的电阻Rd。此切换动作必须由CC Logic芯片来完成。当DFP检测到UFP插入之后才可以输出VBUS，当UFP拔出以后必须关闭VBUS。此动作必须由CC Logic芯片来完成。

   电阻和充电电流Rp和Rd具体值，如下表（以上拉电压5V为例）

   | DFP Rp | UFP Rd | CC Voltage | VBUS Current Limit |
   |:--:|:--:|:----------:|:------------------:|
   | 56K | 5.1K | 0.42V | Default USB Current |
   | 22K | 5.1K | 0.94V | 1.5A@5V |
   | 10K | 5.1K | 1.67V | 3.0A@5V |

### 4. USB-Type-C VBUS电流检测与使用

   USB-Type-C中新增了电流检测与使用功能，新增三种电流模式：默认的USB电源模式(500mA/900mA)，1.5A，3.0A。三种电流模式由CC管脚来传输和检测，对于需要广播电流输出能力的DFP而言，需要通过不同值的CC上拉电阻Rp来实现；对于UFP而言，需要检测CC管脚上的电压值来获取对方DFP的电流输出能力。

### 5. USB PD通信

   USB PD看似只是电源传输与管理的协议，实际上它可改变端口角色，可与有源电缆通讯，允许DFP成为受电设备等诸多高级功能，因此支持PD的设备必须采用CC Logic芯片。

### 6. 发现与配置扩展其他外设(Audio，Debug)

   USB-Type-C支持语音附件以及Debug模式，USB-Type-C接口的耳机如果只作为UFP且因为其功耗较小而无需检测DFP的供电能力时，无需CC Logic芯片。

## 2. 小结

   综上，所有的DFP(如电源适配器)，所有的DRP(如电脑，手机，平板，移动电源), 所有需要检测DFP电流输出能力的UFP，所有支持PD的设备，都需要CC逻辑检测与端口控制芯片。换句话说，只有因为功耗较低而不需要检测电流能力的UFP(U盘，耳机，鼠标等)才不需要CC逻辑检测端口控制芯片。

# 5. Type-C电力传输

USB-Type-C规范允许通过VBUS和接地信号，从DFP 到UFP最多可以传递15W电能。当使用 “纯Type-C”解决方案时，只能使用5V电压传送这15W电能。如果 您为“纯Type-C”系统增加了USB PD规范，您就创建了一个“Type-C PD”系统，可将VBUS电压提至5V以上，最高提至20V，并将VBUS电流最大升至5A。在“纯Type-C”系统中，由DFP和UFP分别供电的Rp和Rd电阻器构成的分压器决定了VBUS电源的电流上限。UFP必须检测这个Rp/Rd 电压分压器电压，并用它来决定从VBUS电源获得的最大电流。这个Rp/Rd 电压分压器电压不是静态的;随着充电生态系统的环境变量不断改变，DFP可以动态改变其电流上限。UFP必须始终监测这个电压，并遵守DFP指示的新的VBUS电流上限。

纯Type-C”解决方案的这个行为-即“DFP指示，UFP遵守”的行为-揭示了“纯Type-C”系统的一个弱点。 “纯Type-C”系统中不存在协商，而Type-C PD系统可双向协商VBUS 电压和电流。通过向一个“纯Type-C”系统增添USB-PD，从而创建一个“Type-C PD”系统，您就能够为VBUS功率协商提供必要的灵活性。

实现USB-PD时，CC导线上承载的USB-PD双相标记编码(BMC)用于实现USB-Type-C端口之间的USB-PD通信。图7显示了USB-PD控制器如何连接CC导线，并向其引入BMC信令。图中只显示了一条CC线路(通过线缆连接的线路)。

![USB-Type-C-PD系统](https://file.infonet.io/blog-files/USB/USB-Type-C-PD%E7%B3%BB%E7%BB%9F.png)

# 6. 标记芯片如何以电子方式标记线缆组件

一个电子标记线缆组件(EMCA)就是一条USB-Type-C线缆，它使用标记芯片向DFP提供线缆的特性。线缆标记是通过将一个USB PD控制器芯片嵌入到线缆的一端或两端实现的 。这些标记芯片由VCONN 供电(或VBUS 供电，取决于具体设计)。VCONN是一个工作于2.7V和5.5V之间的低压轨，但功率被限制在1W。 VBUS 可以是一个高达20V的高压轨。虽然由VBUS 供电的标记芯片具备更大功率的优势，但电压较高的标记芯片也更贵。因此，大多数标记芯片由VCONN供电。

![USB-Type-C电子标记线缆组件](https://file.infonet.io/blog-files/USB/USB-Type-C%E7%94%B5%E5%AD%90%E6%A0%87%E8%AE%B0%E7%BA%BF%E7%BC%86%E7%BB%84%E4%BB%B6.png)

## 1. 标记芯片的配置

某些EMCA在其中一个桨片卡上配备一个标记芯片，另一些则在两个桨片卡上分别配备一个标记芯片。如果使用了一个标记芯片，则必须在连接两个桨片卡的线缆中增加一根VCONN 导线。此外，必须在桨片卡上进行隔离，以防止两个VCONN同时由电压驱动，在VCONN 导线上发生冲突。设计一个或两个标记芯片的决定应考虑以下成本和收益：

- 如果线缆是一条光缆，两个桨片卡之间没有铜线，您必须将两个标记芯片设计到线缆中。这样，无论插入DFP中的是哪一端，DFP都可以与线缆通信。
- 如果增加一条与线缆同长的VCONN导线以及增加一个用于安全整合两条VCONN导线的隔离电路的总成本高于增加两个标记芯片的成本，您应该考虑增加两个标记芯片。
- 有时候，为了简化制造和库存，在线缆中配备两个相同的桨片卡要比配备两个不同的桨片卡更简单。

从DFP的角度而言，EMCA配备一个或两个标记芯片没有成本或收益问题。DFP以此只能为一个芯片供电，而且只与一个标记芯片通信。

## 2. 配置特性

线缆控制器存储与线缆ID和能力有关的配置数据。这些特性包括：

- VBUS 导线的额定电流
- 线缆长度
- EMCA的类型：被动或主动
- 线缆两端连接器的类型：Type-C to Type-C、Type-C to Type-A等
- 线缆中控制器的数量: 一个或两个
- 信令类型： USB 2.0、USB 3.1 Gen 1或USB 3.1 Gen 2
- 厂商ID：用于标识EMCA制造商的16位ID
- 用于标识EMCA产品的16位ID
- 对Alternate Modes(如DisplayPort、PCIe)的支持
- 对厂商专有协议(如厂商专有的对接协议)的支持

DFP必须利用USB PD或 SOP* 枚举发现线缆的特性和UFP的功率要求。SOP是一个通配符ID，代表SOP、SOP’和SOP”。这些SOP ID可被视为Type-C多分支连接中的地址。SOP’代表距离DFP最近的EMCA标记芯片。SOP”代表距离DFP最远的EMCA标记芯片。SOP*枚举是建立PD联系的第一步，电子标记只能通过USB PD BMC实现。

## 3. 何时需要电子标记

出现以下任意情况时，Type-C线缆需要电子标记：

- VBUS电流需要超过3A
- 需要USB 3.1 Gen2或10GHz USB
- 需要Alternate Mode

## 4. EMCA的类型

USB-Type-C 中有两种电子标记线缆组件(EMCA)：即被动EMCA和主动EMCA。其中的主要区别是：主动EMCA为SuperSpeed USB提供信号调节功能，如转接驱动器和重定时器功能。

以下是每种配置的一些例子。

### 1. 被动EMCA

不改变USB数据信号的EMCA就是被动EMCA，可采用两种方式设计：配备或不配备贯穿整条线缆的VCONN 导线。

- 每个插头配备一个线缆控制器的被动EMCA(即每条线缆配备两个线缆控制器)。此时， VCONN 导线不需要贯穿整条线缆(如下图所示)。

![每个插头配备一个线缆控制器的被动EMCA](https://file.infonet.io/blog-files/USB/USB-Type-C%E6%AF%8F%E4%B8%AA%E6%8F%92%E5%A4%B4%E9%85%8D%E5%A4%87%E4%B8%80%E4%B8%AA%E7%BA%BF%E7%BC%86%E6%8E%A7%E5%88%B6%E5%99%A8%E7%9A%84%E8%A2%AB%E5%8A%A8EMCA.png)

- 被动EMCA ,每条线缆只配备一个线缆控制器。此时，VCONN 导线将贯穿整条线缆。需要使用隔离元件以便于从线缆的一端的引入VCONN用于向线缆控制器供电(如下图所示)。

![每条线缆配备一个线缆控制器的被动EMCA](https://file.infonet.io/blog-files/USB/USB-Type-C%E6%AF%8F%E6%9D%A1%E7%BA%BF%E7%BC%86%E9%85%8D%E5%A4%87%E4%B8%80%E4%B8%AA%E7%BA%BF%E7%BC%86%E6%8E%A7%E5%88%B6%E5%99%A8%E7%9A%84%E8%A2%AB%E5%8A%A8EMCA.png)

### 2. 主动EMCA

加了一些额外器件比如信号驱动芯片等用于调节USB数据信号的EMCA。这可以实现更长的线缆或光缆。

很多人认为被动EMCA就是不需要电能的线缆，但是实际情况并非如此。被动和主动EMCA都需要某种形式的电能来驱动标记电路。

## 5. 设计考量

探讨设计EMCA时应该考虑的USB-Type-C规范中的不同功率要求。

### 1. 通过USB-Type-C线缆提供的VBUS电能

对通过USB-Type-C 线缆提供的VBUS 电能的最低要求与现有USB线缆相同。EMCA可以使用VBUS -而不是VCONN -来驱动线缆电路，因为VBUS 贯穿了整条线缆。配有PD的VBUS支持更高的电压(最高20V)。因此，任何一条含有由VBUS 供电的电子元件的USB-Type-C线缆都必须能够承受20V电压。

所有VBUS引脚必须在USB-Type-C插头中互连。 全功能线缆每一端的VBUS引脚需要一个10毫微法的旁路电容器(30V的最小额定电压)。该旁路电容器应尽量靠近电源垫。所有GND引脚必须在USB-Type-C插头中互连。

### 2. 对VCONN的要求

VCONN的功能不同于VBUS，因为VCONN 与线缆另一端相互隔离。VCONN独立于VBUS，而且与能够使用USB PD支持更高电压的VBUS 不同，VCONN 的固定为5V。表1显示了所支持的VCONN 范围，以及VCONN源应满足的其它功率要求。

![VCONN源的特性](https://file.infonet.io/blog-files/USB/USB-Type-C-VCONN%E6%BA%90%E7%9A%84%E7%89%B9%E6%80%A7.png)

为了降低VCONN上的功率 ，DFP可以在以下任意情况发生时关闭VCONN ：

- 在一个CC引脚上检测到有效电压后(Rd在该引脚上)，未在另一个CC引脚上检测到Ra ;
- 完成线缆发现过程后，确定不再需要 VCONN ;
- 线缆发现消息未被线缆响应

EMCA必须向VCONN引脚上的Ra接地层提供一个最大DC阻抗。电容器允许出现±20% 的公差，以便通过EMCA标记芯片中未经调整的片上电容器得到实现。表2列出了对EMCA中VCONN引脚上接地层的阻抗值。

![EMCA终结要求](https://file.infonet.io/blog-files/USB/USB-Type-C-EMCA%E7%BB%88%E7%BB%93%E8%A6%81%E6%B1%82.jpg)

如果没有VCONN，供电线缆不应妨碍CC的正常工作，其中包括UFP检测、电流宣称和USB PD运行。表3列出了使用VCONN电能的线缆应满足的要求。

![VCONN特性](https://file.infonet.io/blog-files/USB/USB-Type-C-VCONN%E7%89%B9%E6%80%A7.jpg)

- USB挂起模式下，电子标记线缆从VCONN 获取的电流不应超过7.5mA。
- 被动EMCA(不含数据总线信号调节电路)从VCONN 获取的功率不应超过70 mW。
- 主动EMCA(含数据总线信号调节电路)从VCONN 获取的功率不应超过1W。

### 3. VCONN供电配件

VCONN供电配件是一个直接附着式UFP，它实现了Alternate Mode，而且能够只使用VCONN进行工作。VCONN供电配件向VCONN引脚上的Ra接地层提供一个最大阻抗。VCONN供电配件应能在2.7V-5.5V VCONN电压范围内工作。

## 6. 其它设计考量

借助Type-C，一条线缆将具备多个功能，其中包括VBUS配电、USB数据和 alternate mode。alternate mode的一个常见例子是在一个或两个USB SuperSpeed 线路上传输的DisplayPort。电力、数据和图像的这种三合一打造了一个高电力、单线缆、超薄、易用的对接解决方案。为了使这条Type-C线缆同时支持Type-C和USB-PD功能，我们还需要采用其它技术。由于并非所有线缆都具备相同的能力或性能，需要一项USB-IF认证来表明某条线缆满足规范中的所有要求。谷歌、亚马逊等公司已公开表明，USB-Type-C线缆必须通过认证，任何未经认证的线缆都会给DFP和UFP带来安全风险。 在发起任何USB-PD供电确认之前，DFP将通过SOP*枚举来确认线缆是否获得了USB-IF认证。

与OEM线缆成品必须通过认证一样，Type-C线缆中的标记芯片也必须通过认证。赛普拉斯半导体公司的CCG2 EZ-PD PD控制器是市场上在首个认证测试日中首款通过认证的标记芯片。

赛普拉斯CCG2 EZ-PD标记芯片的可编程性可让用户轻松编程线缆特性。因为现在不同的线缆中的标记芯片内都植入了不同的厂商自定义信息，这一点变得越来越重要。此外，当最终用户的要求或参数发生变更时，或当USB-IF修改USB-Type-C或USB-PD规范时，这个特性也是必不可少的。用户可以使用赛普拉斯的CC引导装载程序技术，轻松地将这些变更重新编程到线缆成品中。

对EMCA应用中的线缆控制器的主要应用级要求包括：

- 支持最新PD规范中定义的USB-PD协议;
- 支持BMC编码的物理层;
- 支持VCONN导线上内置的Ra电阻器;
- 能够使用VCONN电源向芯片供电;
- 支持用于实现每条线缆只配备一个线缆控制器的被动EMCA的内置隔离元件(图10);
- 支持通过断开Ra电阻器达到节能目的;
- 支持CC 和VCONN 引脚上内置的系统级防护;
- 支持引导装载程序，以便随着USB-Type-C和USB PD的演进，支持通过CC进行固件升级。

### 1. 每条线缆配备一个CCG2的被动EMCA

在这种EMCA架构中，其中一个插头中包含一个CCG2标记芯片。这种方法要求一条VCONN 导线贯穿整条线缆，这样一来，无论哪一头连接主机(DFP)，芯片都能获得供电。所需的隔离元件内置于CCG2中。有关这种方法和其它方法的详情，请参阅：设计USB 3.1 Type-C线缆。

![每条线缆配备一个CCG2的被动EMCA解决方案](https://file.infonet.io/blog-files/USB/USB-Type-C%E6%AF%8F%E6%9D%A1%E7%BA%BF%E7%BC%86%E9%85%8D%E5%A4%87%E4%B8%80%E4%B8%AACCG2%E7%9A%84%E8%A2%AB%E5%8A%A8EMCA%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88.png)

上图描述的是配备CCG2的被动EMCA，这种EMCA架构包含两个CCG2，一个插头包含一个。VCONN信号不贯穿整条线缆，而是终结于每个插头的CCG2。

![每个线缆插头配备一个CCG2的被动EMCA解决方案](https://file.infonet.io/blog-files/USB/USB-Type-C%E6%AF%8F%E4%B8%AA%E7%BA%BF%E7%BC%86%E6%8F%92%E5%A4%B4%E9%85%8D%E5%A4%87%E4%B8%80%E4%B8%AACCG2%E7%9A%84%E8%A2%AB%E5%8A%A8EMCA%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88.png)

上图描述的是配备CCG2的被动EMCA ，该EMCA支持PD。在线缆的两端各嵌入一个CCG2，分别由两端的USB-Type-C端口供电。第二个CCG2的存在，使得VCONN不需要贯穿整个线缆。

### 2. 每条线缆配备一个CCG2的主动EMCA

主动EMCA的主要功能是通过在数据路径上添加一个信号驱动器提供信号调节功能。主动EMCA如需要配置/信号调节功能，可使用USB Power Delivery厂商自定义消息来寻找和枚举线缆属性。该方案需要从 VCONN电源获取连续电能，因此DFP不能关闭线缆的VCONN 供电。

![每条线缆配备一个CCG2的主动EMCA解决方案](https://file.infonet.io/blog-files/USB/USB-Type-C%E6%AF%8F%E6%9D%A1%E7%BA%BF%E7%BC%86%E9%85%8D%E5%A4%87%E4%B8%80%E4%B8%AACCG2%E7%9A%84%E4%B8%BB%E5%8A%A8EMCA%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88.png)

上图描述的是配备CCG2的主动EMCA，其内嵌一个用于延长线缆长度的再驱动。


## 7. 小结

“设计USB 3.1 Type-C线缆”详细阐述了制造商如何使用CCG2轻松设计被动电子标记线缆组件(EMCA)。“硬件设计指南”提供EZ-PD CCG2的硬件设计和PCB版图指南。这些指南有助确保最佳的信号完整性，以及对USB Power Delivery和Type-C规范的全面遵从。赛普拉斯拥有一个广泛的Type-C控制器产品组合(从单Type-C端口控制器EZ-PD CCG1, EZ-PD CCG2, EZ-PD CCG3到双端口控制器EZ-PD, CCG4)，此外还提供产品手册、开发套件、应用说明、软件下载、示例项目、演示视频等有用工具。

# 7. 其他介绍连接

- https://blog.csdn.net/zoosenpin/article/details/49963031
- https://www.itread01.com/content/1495119613.html
- http://www.via-ic.com/news_show.php?id=132

# 8. 版权

- 本文摘自电子工程世界：http://news.eeworld.com.cn/xfdz/2015/0323/article_40868.html

- dzsc.com: http://www.dzsc.com/data/2016-8-11/110409.html

如有侵权烦请联系博主删除。
