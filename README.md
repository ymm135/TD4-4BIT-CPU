# 绝佳的数字电子技术实践

# TD4 CPU 原理详细教程

## 简介

如何使用 9 种 14 个 74 系列芯片，自己构造一个 4 位 CPU（中央处理）的可编程计算机系统

![](https://files.mdnice.com/user/16190/28a96cb7-9aa8-47ac-a5e5-cc4797270d75.jpg)

CPU 的核心原理其实非常简单，其实任何人都只需要非常基础的一点数字电子技术知识，就可以用 14 个 74 系列芯片，完成一个 4 位简单计算机的设计。这本大概在十几年前就已经在日本出版并风靡电子爱好者圈子的一本好书，既可以给对计算机工作原理好奇的青年爱好者作为制作范例，又可以成为让大学本科学完数字电子技术等相关课程后用于学生大作业或课程设计改进的雏形和范例（实话说，比交通的什么之类的例子实在是好太多，因为今天是计算机的时代，大家显然会对这样的例子兴趣更加浓厚）。我花了大概一天半的时间，就基本搞懂了这个例子的原理，写下这篇文档分享给大家，希望大家也能向我一样动手坐一坐，对数字计算机的工作原理感悟体会更深刻，共勉！

### 总电路图（点击可放大）
![](https://files.mdnice.com/user/16190/39f232d7-b015-43ff-a2ae-15a6895f89fa.jpg)


### 为什么要这么做？

虽然功能简单，但是个完整的 CPU，麻雀虽小五脏俱全
采用 9 种小规模集成电路芯片（74 系列），完成数字电路设计的基础学习，可以彻底直观理解到电路工作层的原理。

### 立即购买套件：

https://item.taobao.com/item.htm?spm=a1z10.1-c.w4004-23785864172.6.4f231fe7l9UOLu&id=647177997616

-现在可以轻松获得 3GHz-5GHz 的高速处理器，就连不到一美元的处理器，其工作频率也在 10M 左右，计算位数也在 16 甚至是 32 位，我们自己搭建一个一个意义是什么？

并非没有意义，原因如下

- 从电路层面直观了解运算过程
- 直观理解各个部分的组成原理（到数字电路层面）
- 简化的 4 位数，不至于眼花缭乱
- 只有 12 个指令，也足以完成一些编程任务

其次要制定 CPU 的规格

- 只操作 4 位数
- 仅 10 简单的中小规模集成电路芯片（74 系列）
- 可编程指令只有 16 条
- 演示和改造的实用性都很强

这是一个典型的解剖模型（麻雀虽小 五脏俱全）
采用机器语言编程，可以完全直观理解并看到其运行过程，可手动也可自动操作

### 这个项目适合哪些人？

有初步或者想初步学习数字电路的人
想了解 CPU 工作和设计思路的非专业人员、正在学习电子及计算机专业的学生

### 经过这个项目，你会收获些什么？

中规模集成电路，如 74 系列芯片，该怎么应用到实际需求中去
彻底理解一个简化版本的 CPU 的每个运作细节

# 教程开始啦！

### 74 系列芯片

什么是 74 系列芯片，相信学过数字电子技术的同学都不会陌生，课本里都有频繁的举例，但为了完整补充相关产业背景的知识，这里还是引用一下维基百科的说法：

7400 系列是典型的中小规模数字逻辑集成电路。 在 1960 年代中期，最初的 7400 系列集成电路由德州仪器 (TI) 引入，前缀为“SN”，以创建名称 SN74xx。 由于这些零件的普及，其他制造商发布了引脚对引脚兼容的逻辑器件，并保留了 7400 序列号作为识别兼容部件的辅助工具。 但是，其他制造商在其部件号上使用不同的前缀和后缀。

二值逻辑和逻辑电平、正负逻辑电平
二进制数正好是利用二值数字逻辑中的０和１来表示的。
二值数字逻辑是 Binary Digital Logic 的译称。

本系统中，由于采用 USB 接口供电，用 5V 表示 1，0V 表示 0。

在本系统中，一般采用正逻辑（即高电平有效），如所表示的数据线上有一横杠线或者器件输入输出口处有一小圆圈，则表示负逻辑（即低电平有效）。
![](https://files.mdnice.com/user/16190/c5ebdbfe-245f-4662-b369-564da536997f.png)

更多专业详尽的信息和说明，请查阅数字电子技术基础课本中的相关章节

### 如何查看 74 系列芯片的 Datasheet

规格表(Data sheet)数据表，数据表或规格表是对产品，机器，组件，材料，子系统或软件的性能和其他特性进行了足够详细的总结的文档，使买家能够了解产品的含义以及设计工程师要了解组件在整个系统中的作用。通常，数据表由制造商创建，以介绍文档其余部分的介绍性页面开头，然后列出特定特征，并提供有关设备连接的更多信息。

本项目使用了 8 种 74 系列芯片，我们都采用 74HC 型号，相关的规格书可以在https://www.alldatasheet.com/网站查询到，个人倾向于阅读德州仪器TI公司较新的Datasheet文档，因为其资料丰富且格式规范。德州仪器公司的SN54系列和SN74系列在功能上完全一致，规格书也是共用的，所以查找规格书时，如书上所示74LS10，网上也可以搜索SN54HC10，只是54系列芯片用于军用。

我刚上大学那会，一开始阅读英文规格书材料也非常不适应，因为从小到大的数理化教育都是非英语环境进行的，但作为电子、信息、自动化和计算机相关专业的学生和从业者，这一点将来无法避免，只好捧起英文资料一遍查资料一遍看，再借助一些中文参考书，慢慢的大家受到的困难的阻力越来越小，阅读越来越顺畅，希望读者也要尝试坚持下去，一定会受益匪浅的。

### 74HC/LS/HCT/F 系列芯片的区别

74 系列集成电路大致可分为 6 大类：

- 74××（标准型）；
- 74LS××（低功耗肖特基）；
- 74S××（肖特基）；
- 74ALS××（先进低功耗肖特基）；
- 74AS××（先进肖特基）；
- 74F××（高速）。

同型号的 74 系列、74HC 系列、74LS 系列芯片，逻辑功能上是一样的。74LSxx 的使用说明如果找不到的话，可参阅 74xx 或 74HCxx 的使用说明。74HC 的速度比 4000 系列快，引脚与标准 74 系列兼容 4000 系列的好处是有的型号可工作在+15V 。新产品最好不用 LS。

### 74 系列芯片和逻辑门电路的关系

像 74 这一类的所有芯片都需要电源正负供电，其余部分则往往作为信号输入输出的功能引脚，在电路设计布局时，对于简单功能的芯片，门电路符号和芯片引脚标识方式常常混用，这时需要我们查阅芯片 Datasheet 来确认其对应关系，以 74HC10 为例，我们查到规格书的地址是：https://pdf1.alldatasheet.com/datasheet-pdf/view/203930/TI/SN54HC10.html
通过在规格书中，下面两张示意图，即可理解在实际电路中如何使用该芯片：
![](https://files.mdnice.com/user/16190/7214855f-c00b-44bc-a5dd-245b61732692.png)

![](https://files.mdnice.com/user/16190/a732c77d-1bbc-4e3d-bd48-388ad4211d96.png)

### 什么是 CPU-逐项理解冯诺依曼原理

- 程序计数器从内存中获取指令（取指令）
- 根据解码结果执行操作（执行指令）
- 存储计算结果（存储结果）
  （上述三个过程可以比拟成逐页翻书，理解书中内容，并尝试拿笔记本记录下来的过程）
  机器的每个 时钟脉冲周期，都会重复上述的几个过程

### 仿真器

接下来，我们先来看一个仿真器，此时此刻你并不需要理解这个仿真器中的所有细节，等后续讲述中，每个部分理解后，再回来重看此动态仿真程序，即可全面理解。[https://vanya.jp.net/td4/](https://vanya.jp.net/td4/)
![](https://files.mdnice.com/user/16190/67ca234b-9f1f-43fb-b3a8-034669878bdb.png)

### 现在，你看此仿真程序运行图，所需要知道的事情有以下几点：

1、 红色线代表此时该线上电平为高（5V），蓝色代表电平为低（即 0V）

2、 整个系统由 CLOCK 脉冲驱动进行，RESET 低电平有效，即 RESET 被拉低后，系统里所有数据清零重启
如果你已经学过数字电路基础，那么你应该还可以尝试记住并理解以下几点，如果不能或全部理解也没关系，可以全部学完，回来再看看

3、 四个 74HC161 分别为寄存器 A、B、C、D，分别可用于存储 4 个比特的数据

4、 第三个寄存器 C 的输出，用于输出 4 位电平，驱动 LED

5、 第四个寄存器 D 的输出，用作指令的译码，即作为地址总线的 A0-A3（Address0-Address3）

6、 两个 74HC153 组成了一个 4 位四路数据选择器，用于选择哪一路的 4 位数据给后续的四位全加器做加数 A。其中，第三路的 4 位数据来源于 4 位拨码开关（即输入），第四路不连接被弃用。

7、 74HC283 是一个四路全加器，它的第一个加数来源于两个 74HC153 的数据选择通道，第二个加数来源于指令数据的低 4 位（即 D3-D0），这种被包含在指令中参与运算的数据又叫立即数 immediate。全加器的求和结果将输出给 4 位寄存器 A、B、C、D 均可，选择其中哪一个寄存器记忆结果，完全由每个寄存器输入端的 LOAD（~LD）信号决定，请注意 LOAD 是低电平有效，这是后续指令译码结果中控制总线的最重要一部分。

8、 全加器有溢出位输出 Carry，但是无法记忆上一次的计算结果是否溢出，故需要一个 D 触发器作为溢出标记位记住上一次的求和结果是否溢出，并送给译码器指令。

9、 图上所有用门电路表示的组合电路，是一个完整的指令解码器，它由取得指令的高 4 位（即 D7-D4）和溢出标记位一起作为译码器的输入，由控制四个寄存器的加载（LD）和两个数据选择器的 A、B 作为输出，这是典型的控制总线。

### TD-4 CPU 规格对比-理解和现有 CPU 规格之间的数量级差异

![](https://files.mdnice.com/user/16190/05d0ce1c-d6d0-4276-9912-c118142aabb4.png)

## 复位电路

### 补充点基础知识

#### 如何表达 0 和 1

- 判断电压低还是高
- 设置 0V=0，5V=1
  ![](https://files.mdnice.com/user/16190/393e49fe-716e-4d35-a018-5011ddda8778.png)

- 低电平有效，还是高电平有效
- 注意，每个 IC 为每个信号引脚都设置了正![](https://files.mdnice.com/user/16190/73b723ef-e66d-48a5-9f66-cc39aa44b3bf.png)
  逻辑和负逻辑

#### 上拉电阻、下拉电阻的概念

我们现在开始设计，如何将开关的开关两种状态，转换成电平的高低两种状态。这个转换过程，需要理解上下拉的概念。
在数字电路中，上拉电阻（英语：Pull-up resistors）是当某输入端口未连接设备或处于高阻抗的情况下，一种用于保证输入信号为预期逻辑电平的电阻元件。他们通常在不同的逻辑器件之间工作，提供一定的电压信号。

同样的，一个下拉电阻（Pull-down resistor）以类似的方式工作，不过是与地（GND）连接。它可以使逻辑信号保持在接近 0 伏特的状态，即使没有活动的设备连接在其所在的引脚上。
![](https://files.mdnice.com/user/16190/c75ff750-fbd7-41b6-93cc-5d86793222b9.png)

### 逻辑代数、组合逻辑电路
理解下面这些门电路的关键在于理解最关键的三个逻辑门（与、或、非）
下面给出另外与非门、或非门、异或门的符号图和真值表，其是由三种基本门串接形成

如果你对此毫无基础，请务必了解更多专业详尽的信息和说明，可查阅数字电子技术基础课本中的相关章节（逻辑代数、逻辑门电路、组合逻辑电路）
![](https://files.mdnice.com/user/16190/0a0b6135-a8b2-41f7-a208-e4c230768d3a.png)

### 按键的抖动与消除
我们上面所表示的上下拉电阻方式用于将开关状态转换为电位高低电平状态的设计是理想化的，在实际电路中仍需要改善，为什么呢？让我们来观察一下实际的输出电压波形
![](https://files.mdnice.com/user/16190/47e94458-80a8-4db6-8cd4-1098f532961a.png)

根据电路原理基础知识，我们知道电容是电荷的蓄水池，这就像个缓存，令其两端的电压无法突变
![](https://files.mdnice.com/user/16190/82ccc736-8b4d-4321-b008-30599e46475b.png)
![](https://files.mdnice.com/user/16190/090dde5a-0b1c-47d9-9bfe-8f3255b5d4e3.png)
![](https://files.mdnice.com/user/16190/5fd5f552-efe1-439c-9ee4-05409838f447.png)

### 施密特触发器、迟滞特性、消抖
在电子学中，施密特触发器（英语：Schmitt trigger）是包含正反馈的比较器电路。
![](https://files.mdnice.com/user/16190/17f93a61-c07e-4847-a8ac-d693bc098a4d.png)


反相施密特触发器

对于标准施密特触发器，当输入电压高于正向阈值电压，输出为高；当输入电压低于负向阈值电压，输出为低；当输入在正负向阈值电压之间，输出不改变，也就是说输出由高电准位翻转为低电准位，或是由低电准位翻转为高电准位对应的阈值电压是不同的。只有当输入电压发生足够的变化时，输出才会变化，因此将这种元件命名为触发器。这种双阈值动作被称为迟滞现象，表明施密特触发器有记忆性。从本质上来说，施密特触发器是一种双稳态多谐振荡器。
![](https://files.mdnice.com/user/16190/4bbef7d1-b8fd-48fb-a23f-989425214cf8.png)
反相施密特触发器的滞回曲线
理解施密特触发器消除抖动在于两点：1.什么是脉冲抖动——通俗得讲，在脉冲的上升沿或者下降沿由于人手的细微抖动（人手在触碰物体时常会“哆嗦”，仅仅是幅度极小人没有觉察而已，除非受到惊吓很紧张的时候抖动的幅度大到会被人觉察）或者其它电路硬件产生的幅度较小的震荡，这些原因造成了脉冲边沿的一些或大或小的毛刺引起了脉冲波形的畸变，这些称之为脉冲抖动；2.施密特触发器的特点：输入的电平高于某一个值（比如说 1.8V）触发器输出电平会向下翻转，产生一个下降沿，输入的电平低于某一个值（比如说 0.8V）触发器输出电平会向上翻转，产生一个上升沿，那么当输入电平介于两个翻转阈值之间时施密特触发器的输出不会动作；从上面的描述你就应该能明白：之所以能消除抖动是因为抖动所产生的电平上下波动的范围尚在施密特触发器翻转的两个阈值之间，施密特触发器不会动作，因而能消除脉冲抖动造成的不良反应。
![](https://files.mdnice.com/user/16190/533b80db-6e77-4639-849f-93c94b94268c.png)

### 最终该模块电路图
由于重置（RESET）信号是低电平有效，所以默认情况下应该维持高电平，采用串联两个非门施密特触发器我们得到了最终的复位电路的设计

![](https://files.mdnice.com/user/16190/5d6a3dae-4ec4-4efe-88c9-ec568901562e.png)

![](https://files.mdnice.com/user/16190/abea0e0c-4816-4b8b-a358-08162968159d.png)

### 时钟脉冲产生电路
时钟脉冲是整套数字系统的指挥棒，就像交响乐队指挥手中的指挥棒，其时间基准完全由它决定

![](https://files.mdnice.com/user/16190/54ba9098-77a9-4e74-ae45-3fc5aea4ebe5.png)

振荡电路（时钟发生器） 

如果你想手动操作，也可以快速按下它，模拟连续的脉冲束，原理几乎和复位电路一样，只是电平相反。

时钟脉冲 Clock 由于是高电平有效，手动产生时钟脉冲（Manual Clock）的按钮应该采用下拉电阻接法。但是由于采用了一个反向施密特触发器，所以改位选用上拉电阻接法。

-当然，它也可以由正反馈电路发生振荡来产生


![](https://files.mdnice.com/user/16190/4560501e-2f61-498f-a9e1-f534dc6095bc.png)


门电路组成的多谐振荡器
振荡原理：

![](https://files.mdnice.com/user/16190/274be643-79b6-444c-b81b-456a0a200b1f.png)

假设 Q 为低电平，则非门 2 的输入端为高电平，经过 R 对 C 充电，C 的电压上升，直到非门 1 输入端的电压达到反转电压，此时非门 1 的输出变为低电平，Q 变为高电平。

　　此时，Q 点、C、R、非门 2 的输入端，极性反转，相对于之前变为放电回路，然后转为反向充电，C 的电压下降，直到非门 1 输入的电压达到反转电压，此时非门 1 的输出变为高电平，Q 变为低电平。
  
　　如此循环，形成振荡，在 Q 端输出方波。
  
　　如果非门的反转电压为电源电压的 1/2。
  
　　则振荡周期：T≈2.2·R·C
  
　　 Rs 用于稳定振荡频率，驱使为 6~10 倍的 R。
   
时钟电路电阻值在 TD4 教科书的电路图中，规定了 1Hz 为 33KΩ，10Hz 为 3.3KΩ 的电阻值，但这个值实际上慢了一点。因此，下表显示了以电阻值计算的 60 秒内的实际时钟数。

![](https://files.mdnice.com/user/16190/86034615-d9ec-495a-b4bd-d93524d62e85.png)

从此表中可以看出，在 33KΩ 时，它变为 51/60 = 0.85 Hz。因此，为了获得 1Hz，从表中可以看出，当使用 28.3KΩ（从 E24 系列的电阻值中选择，33K 和 200K 并联）时，正好 60/60=1 赫兹。它变成了。

对于 10Hz，使用 2.83KΩ（3.3K 和 20K 并联），1Hz 的电阻值为 1/10。

RC 充放电回路

![](https://files.mdnice.com/user/16190/6713aa91-746c-4182-bd2c-e2a7c56c3fde.png)

上面的图来自维基百科(后面也有图片取自维基百科)。观察上面的图，当电源通过电阻 R 向电容 C 充电的时候，电容 C 两端的电压会如何变化呢(也就是会呈现出何种规律)？这可以应用基尔霍夫电路定律来建立一个微分方程，然后解出这个微分方程就会得到电容 C 在充电时的电压变化情况(也可以用拉普拉斯变换求解，关于如何求解这个方程则需要另一篇文章 :)，它是时间 t 的函数：
　　有了公式我们就可以画出它的曲线，如图所示。
如果你对此过程有疑惑或者需要了解更多，请参考电路或电路原理相关课本中的相关内容。
### 最终该模块电路图

![](https://files.mdnice.com/user/16190/fc62ab96-9c6a-4f82-a4be-7287d530fe76.png)

![](https://files.mdnice.com/user/16190/88286b4f-0db1-4825-af13-bd82b7792986.png)

### 用来存储程序的-只读存储器（ROM）
ROM（Read Only Memory）只读存储器，这种存储器（Memory）的内容任何情况下都不会改变，电脑与用户只能读取保存在这里的指令，和使用存储在 ROM 的资料，但不能变更或存入资料。ROM 被存储在一个非易失性芯片上，也就是说，即使在关机之后记忆的内容仍可以被保存，所以这种存储器多用来存储特定功能的程序。

在本项目案例中，ROM 的意思并非指程序不可改变，而是 CPU 在运行过程中，只能从该存储器阵列中读取，而不能写入或者改变其中的内容，但我们可以通过人手波动开关来写入或修改其中的内容。

![](https://files.mdnice.com/user/16190/0530392c-df1a-45fe-9a9f-b22167c94e20.png)

-Memory (ROM) 电路使用 16 个 8 位 DIP 开关来指定 ROM 电路中的 0/1 位（DIP 开关的使用也可 2 x 8 16 接头和跳线针被用作替代品）。拨码开关相对昂贵，取决于类型，最大的问题是印刷电路板上比较占用空间。TD4 教科书由拨码开关和二极管阵列组成，但二极管阵列比较特殊，很难获得。因此，不可避免地要使用普通的轴向引线型二极管。

### 组成开关阵列，并串联二极管
--CPU 无法一次从 ROM 中读取所有程序，按照冯诺依曼原理，逐条指令读取逐条指令执行
这个例子是 4 位宽度的地址总线，就是 2 的四次方，共可以读取存储 16 个 8 位
--只需要一点一点的将其读取。就像看书，要一页一页翻着看
我想读取一位的电位，该怎么办？

![](https://files.mdnice.com/user/16190/dfe5bf45-c454-450a-8e64-ef781c9591e1.png)

1 位 ROM

![](https://files.mdnice.com/user/16190/546c9a9e-664c-471a-bb50-0dd0dac7e97b.png)

3\*4 位 ROM

![](https://files.mdnice.com/user/16190/2a323ed9-5c61-4950-8b01-9854d20c162e.png)

通过选择哪一竖电位到地为低电位，横向导线分别读取每行电位的高低

![](https://files.mdnice.com/user/16190/2c7533dc-574d-4fb2-8834-17e367404566.png)

组成交错的开关阵列很不错，但是读取第二竖列开关每行电位的时候，会受到第一竖列的开关选择影响，从而导致错误。

为什么要加二极管？二极管有什么作用？

![](https://files.mdnice.com/user/16190/62be8c74-d045-4819-b99e-c9646d74ddab.png)

![](https://files.mdnice.com/user/16190/7aba5043-0b0b-472c-bc43-d2987c82e475.png)

每个开关连接到地参考电位前，都用二极管串联，即可避免干扰

#### 指令地址译码

![](https://files.mdnice.com/user/16190/3a937fe1-792f-4ccd-94aa-6d5e1ec08b61.png)

我们需要把每一竖列开关连接到地的选择过程自动化，和输入的地址编码对应唯一的选择，这个过程叫做译码。比方说，从 CPU 发出的地址总线请求（A3-A0）为 0110，则读取第六个 ROM 中的 8 位数据作为程序的二进制指令通过（D7-D0）传给 CPU 加载该指令。

TD4 教科书的电路图中，74HC154 一次可以解码 4 位，用于指定 ROM 电路地址，但这里使用了两个 74HC138 3-8 解码器。得到等效电路。这仅仅是因为 74HC138 在更容易购买也更便宜，经销商的库存中也比较多。

译码器 3-8 译码器 4-16 译码器

译码器是电子技术中的一种多输入多输出的组合逻辑电路，负责将二进制代码翻译为特定的对象（如逻辑电平等），功能与编码器相反。 译码器一般分为通用译码器和数字显示译码器两大类。 ... 输入使能信号必须接在译码器上使其正常工作，否则输出将会是一个无效的码字。

![](https://files.mdnice.com/user/16190/d84a5314-6d96-4a9c-b079-298d81875fed.png)

![](https://files.mdnice.com/user/16190/754de70c-175a-4e1a-9df6-02b5fdff0e4a.png)

大多数随机存取存储器使用 n 线－2n 线译码器来将地址总线上已选择的地址转换为行地址选择线中的一个。

![](https://files.mdnice.com/user/16190/8819aa4c-ea2f-4ebd-9fc4-1120600d251b.png)

![](https://files.mdnice.com/user/16190/8e47a9c2-03c1-4c88-936d-4120924f6128.png)

引脚布局图和功能逻辑图块

![](https://files.mdnice.com/user/16190/f1544d56-bd91-4532-a348-a83bca6cd23f.png)

![](https://files.mdnice.com/user/16190/2bec3397-b8a0-41e0-ac55-36f488474c74.png)

![](https://files.mdnice.com/user/16190/8f8eaf09-0be3-4c49-a560-41ca7361a255.png)

两个 3-8 译码器组成一个 4-16 译码器，这是数字电子技术课程中组合逻辑电路译码编码部分的经典问题，绝大部分数字电子技术书籍和课本都会举例讲述，详情请参阅相关书籍。

75HC540 的作用较为简单，作为练习，请大家自行阅读 Datasheet。
### 最终该模块电路图

![](https://files.mdnice.com/user/16190/c394fac5-0010-4483-84a5-6b011a21d121.png)

![](https://files.mdnice.com/user/16190/8b01e190-1b26-413c-a875-fcd330415e42.png)

### 寄存器电路
4 个四位寄存器，由 4 个 74HC161 芯片构成，接下来我们一步一步看看它们是怎么工作的

### 补充知识双稳态电路 D 触发器原理
触发器（英语：Flip-flop, FF），中国大陆译作“触发器”、台湾及香港译作“正反器”，是一种具有两种稳态的用于储存的组件，可记录二进制数字信号“1”和“0”。触发器是一种双稳态多谐振荡器（bistable multivibrator）。该电路可以通过一个或多个施加在控制输入端的信号来改变自身的状态，并会有 1 个或 2 个输出。触发器是构成时序逻辑电路以及各种复杂数字系统的基本逻辑单元。

D 触发器有一个输入、一个输出和一个时脉输入，当时脉由 0 转为 1 时，输出的值会和输入的值相等。此类触发器可用于防止因为噪声所带来的错误，以及通过管线增加处理资料的数量。
带直接复位置位端的上升沿边沿触发的 D 触发器真值表

![](https://files.mdnice.com/user/16190/e7b2abf0-d4be-4e6c-9ee0-37c7eeb4cb56.png)

D 触发器符号。> 是时脉输入，D 是资料输入，Q 是暂存资料输出，Q'则是 Q 的反相值，S 为 1 时强迫 Q 值为 1，R 为 1 时强迫 Q 值为 0

关于从基本 RS 触发器出发，是怎样由两个带有反馈的与非门或者或非门逐步建立起来的，并通过各种组合添加进一步形成其它更高级的触发器和计数器等内容，内容较为冗长，但对于打好相关基础非常重要，这里只大概解释各模块的外部主要特性。如果完全不了解，请读者读者查阅《数字电子技术基础》课本进行学习。

![](https://files.mdnice.com/user/16190/6f680f96-57f8-47af-b1c6-9eb360c53436.png)

当上升沿建立起来的时候，D 的状态被记录在 Q 中，在下一个上升沿到来之前，Q 值不受 D 值改变的影响。

![](https://files.mdnice.com/user/16190/8c2800e2-746f-46ce-b4f6-52d7f753fbaf.png)

### 四位同步二进制计数器
在有了 D 触发器基础后，我们可以通过串联 D 触发器，得到一种叫做计数器的器件

![](https://files.mdnice.com/user/16190/46d12069-7315-47e5-b40d-0bda4429d986.png)

逻辑功能图

![](https://files.mdnice.com/user/16190/2467ccc5-dab8-4cde-9b06-28a1419e24cb.png)

时序逻辑图

![](https://files.mdnice.com/user/16190/4c1cabc1-fa8b-4c21-be0a-1f9d3cc9de33.png)

状态转换图

通过参考查看 74HC161 的 Datasheet 后，我们知道此芯片正是这一类器件

![](https://files.mdnice.com/user/16190/21b8f542-f5c5-4136-9698-d395bfdc4f38.png)

74HC161 引脚布局图

![](https://files.mdnice.com/user/16190/a1b0ac00-3b3d-4a83-9874-6c03aae69ad3.png)

74HC161 的 Datasheet 中的时序逻辑图可以看出，~CLR 是异步清零端，所谓异步，就是指其作用并不随时钟脉冲才发生作用，即与时钟脉冲不同步，随时有效随时即刻清零所存储的数据。~LOAD 低电平时，A、B、C、D 的值被装载到 QA，QB，QC，QD 中，当 ENP 和 ENT 置高电平时，QA，QB，QC，QD 组成的 4 位二进制数开始对外部脉冲 CLK 上升沿进行计数（0-15），当计数达到 15 后再来一个脉冲上升沿，计数溢出归零，并在 RCO 端给出一个正脉冲。

![](https://files.mdnice.com/user/16190/e43441c4-dc02-4f92-b1b8-cb989e198749.png)

用带脉冲计数功能的 74HC161 的 D 触发器阵列构成寄存器

输入/输出

A 寄存器和 B 寄存器受同一个时钟脉冲控制

![](https://files.mdnice.com/user/16190/58b11dc1-8dd8-4141-8fce-788e33b9db26.png)

![](https://files.mdnice.com/user/16190/105385c1-a637-4015-9dad-193926c79c46.png)

将 D 触发器的输出端 Q 的信号，重新引回给触发器的输入端 D，通过选择开关，即可实现数据在不同的 D 触发器之间传送。

![](https://files.mdnice.com/user/16190/22951a23-161c-4daa-be02-72e706eda78c.png)

如何在 A 寄存器和 B 寄存器之间交换数据，切换选择输入给哪个寄存器并保持。

![](https://files.mdnice.com/user/16190/b50b1bdf-b536-4d0b-ae89-761352a81912.png)

譬如指令 MOV A, D，就是将 D 寄存器内存储的数据，转存到 A

### 数据选择器
我们需要将上述选择数据移动过程，转换成用特定芯片处理，这个过程所需要类型的芯片叫数据选择器。同时我们需要使用 4 个 4 位 D 触发器存储数据，即数据总线宽度位 4bit，下图用划线加 4bit 标识。

![](https://files.mdnice.com/user/16190/1130b8fc-01f1-48df-9454-6d609887289b.png)

通过 75HC153 的 Datasheet 查真值表，我们可以确认其功能是通过 B，A 的值，从 DATA 中选择 C0，C1，C2，C3 其中一个传输给输出 Y。

![](https://files.mdnice.com/user/16190/13e78663-0d02-4467-968d-fd5411d3ac96.png)

通过 Datasheet 里的引脚分布和介绍，我们知道 74HC153 是两路数据选择器。由于我们传输的数据带宽是 4bit，故需要两个 74HC153 芯片，分别作为数据选择器 1 和数据选择器 2，共用选择信号 A、B。

![](https://files.mdnice.com/user/16190/540e5666-b33d-4bac-b61b-d2b300eeea29.png)

局部功能完成的电路图

![](https://files.mdnice.com/user/16190/8dbfb1aa-75e0-48f4-b652-053f6f22f02e.png)

### 算术逻辑单元（ALU）
#### 补充知识加法器、全加器
74HC283 用于 TD4 教科书的电路图中作为 ALU 使用的全加器。

加法器、半加器、全加器
相信认真阅读过数字电子技术基础课本中有关加法器章节的读者对此类器件的实现过程应该都印象深刻。加法操作是几乎所有算数运算中最基本的操作，原则上其它类型的算数逻辑操作都可以由此推延产生，因此仔细研究清楚其内部原理非常有必要，请读者认真参阅课本中相关章节的讲述内容，这里不再赘叙，仅给出真值表和逻辑图仅供参考。

![](https://files.mdnice.com/user/16190/73c0149c-7f85-4cd0-9489-d384bd790e73.png)

![](https://files.mdnice.com/user/16190/2123e59c-0f5f-4a5a-ab84-ec69af5cd8ae.png)

半加器逻辑图和真值表

![](https://files.mdnice.com/user/16190/cf3c4bbf-ac38-4fef-8769-0eae7361805b.png)

![](https://files.mdnice.com/user/16190/e01ae907-4937-46a0-9d0c-0ae231df8bf6.png)

一位全加器的逻辑图和真值表

![](https://files.mdnice.com/user/16190/5151ec40-fa45-49b2-8e61-b79f221333a3.png)

一位全加器示意图

![](https://files.mdnice.com/user/16190/5c6b56d3-e6aa-4271-b9d7-2f83a3b1e904.png)

4 个一位全加器串联组成的 4 位全加器

此类推如果增加更多的数字，位数增加即可

![](https://files.mdnice.com/user/16190/bcd495db-0dd2-42b4-9213-538d32e5e629.png)

4 位全加器

![](https://files.mdnice.com/user/16190/79fb3592-6ab0-487c-812c-cf9cec7948a6.png)

![](https://files.mdnice.com/user/16190/c57ff647-9b6f-4750-8e5b-1202eb9dd92d.png)

Datasheet 中 74HC283 的引脚图

### 组合一个最小系统
现在是时候组合寄存器、数据选择器、加法器，组成一个完整的指令执行系统。
无论如何，尝试做一个完整的 ALU
我们来动手吧譬如执行指令 ADD, Im

![](https://files.mdnice.com/user/16190/b4647355-351e-4d45-aac8-03dbea1dc196.png)

![](https://files.mdnice.com/user/16190/baca687c-1153-48d9-ac4d-6da1ce055ffd.png)

这样以来，就多了很多额外的东西 所以，如何执行 MOV A, B 呢？

![](https://files.mdnice.com/user/16190/bdaa37d4-4acc-473c-bc8f-47f7d4905a64.png)

立即数 Im 作为 ALU 的第二个输入数 B，加法器得以实现，当指令用于传送时，令立即数 Im=0 即刻。这样，通过数据选择器 A、B 的信号组合与立即数的配合，上图的指令执行框图基本上可以实现如下类似指令
MOV A, Im 将立即数数据传送到 A 寄存器。
MOV B, Im 将立即数传送到 B 寄存器。
MOV A, B 将 B 寄存器传送到 A 寄存器。
MOV B, A 将 A 寄存器转移到 B 寄存器。
ADD A, Im 将立即数添加到 A 寄存器。
ADD B, Im 将立即数添加到 B 寄存器。

### 程序计数器

- 这是一个计数器，每次遇到一个脉冲则加一
- 如果它复位，则返回到零

![](https://files.mdnice.com/user/16190/1e123000-5588-4a28-8039-254af3a5e5d6.png)

这样很棒，在没有遇到跳转指令的时候，每次脉冲来都自动加一，很适合做储存地址的程序存储器 PC（Program Counter），我们放弃了 D 寄存器用于通用数据的存储，将其作为程序存储器，而在遇到指令直接跳转的指令时，其相当于将立即数或者加法结果存储到寄存器 D 中。

![](https://files.mdnice.com/user/16190/45c5bd03-3042-4feb-8430-37935f28eead.png)

### 整合输入输出
此外，我们还需要一个寄存器，作为输出驱动 4 个 LED 显示的输出寄存器，那么我们也放弃第三个寄存器，即 C 寄存器，作为 OUT 寄存器，其结果直接作为输出寄存器。

![](https://files.mdnice.com/user/16190/ac4e8dfe-2630-4a95-98c8-014869b1309f.png)

此外，你很可能觉得，还需要输入寄存器 IN，那么又将牺牲多一个通用寄存器或者需要再添加一个 74HC161。其实细想之下，这是不必要的，因为当使用的 C、D 寄存器作为特殊功能的寄存器后，4 路的 4 位数据选择器只剩下 2 路被通用寄存器占用，我们把其中一路作为输入，另一路全部默认置零即可。这样，立即数就通过与全为零的数相加而可以被直接传送至通用寄存器 A 和 B 了。

保持进位的触发器 Flip-Flop

带有条件的跳转指令往往需要通过比较计算和查看加法器的计算结果是否溢出。但加法器的溢出位 C4 记录的是当下的加法结果是否溢出，而非上一条指令执行时的加法结果是否溢出，这就需要一个触发器记录上一次的溢出结果，等到下一个脉冲来到时决定条件跳转指令是否执行。

![](https://files.mdnice.com/user/16190/90a9c919-631c-45da-9e39-2ff32c72fe8e.png)

我们选用了 74HC74 作为正边沿触发的 flip-flop 触发器，可以满足这个要求，详情请自行阅读 Datasheet。

### 最后的整合与调整

![](https://files.mdnice.com/user/16190/f5100761-1df7-4b85-b6f4-9968804f8301.png)

指令译码电路（取指令）

- 每个时钟周期的脉冲来临时，就进行一次取指令、译码、并执行
- 被译码后，相应的电路执行指令预设的动作进行操作

![](https://files.mdnice.com/user/16190/64806fb9-dbeb-4368-be42-77df071f293e.png)

这个译码电路相当有 5 个输入，共 2 的 5 次方种输入可能性，6 个输出，2 的 6 次方种输出可能性。然而仔细分析之下，我们需要的有意义的操作指令和相应的输出无外乎以下这些情况。

![](https://files.mdnice.com/user/16190/1b01db00-0708-41aa-8c3d-7c5c8be5685c.png)

所以，总的运行架构框图如图所示

![](https://files.mdnice.com/user/16190/b6513a8a-1874-4faa-8acb-491bed27ce4a.png)

这个指令译码电路，设计过程看起来稍显复杂，然而它只是一个与时序无关的纯组合逻辑电路。参照表格所列，将所有输出结果用采用数字电子技术课本中的最小与或表达式写下，并运用包括德摩根定律在内的逻辑代数定律适度整理后，权衡器件选择和数量，就可得出如下最终的译码电路。

![](https://files.mdnice.com/user/16190/11dc698a-ca92-4fe8-b26f-6ed1ff0f8393.png)

![](https://files.mdnice.com/user/16190/def5b7e6-8985-4e78-9fd7-35dbf3dcf2ae.png)

![](https://files.mdnice.com/user/16190/d2c99f50-a7f4-43ba-9a8b-30899cd6971b.png)

![](https://files.mdnice.com/user/16190/c1c62533-61b4-45a6-9d93-6b73b0b5a139.png)

![](https://files.mdnice.com/user/16190/3a94bf10-0820-4ad1-a4b0-e65696baaf2f.png)

![](https://files.mdnice.com/user/16190/d93fe7f8-8e65-4095-9c41-135c60423cfa.png)

## 最后是编程指令集

TD4 中定义了十二种类型的指令，这里汇总了所有指令的列表。下图右端所示的 SelB、SelA、Ld0、Ld1、Ld2、Ld3 表示根据操作码和标志创建的解码信号。

![](https://files.mdnice.com/user/16190/d9548110-eaf8-4f5f-bf7d-defe938fa9e6.png)

![](https://files.mdnice.com/user/16190/8765f79b-f1f8-44a6-abb4-302caed010e2.png)

将立即数数据传送到 A 寄存器。
　　　　　在运行时，它不受 C 标志的影响。执行后，C 标志变为 0。

![](https://files.mdnice.com/user/16190/6b3d06dc-288f-475a-af7c-dd8ecd74b702.png)

将立即数传送到 B 寄存器。
　　　　　在运行时，它不受 C 标志的影响。执行后，C 标志变为 0。

![](https://files.mdnice.com/user/16190/8866cf5a-816c-4601-b38d-0cde0ec5748c.png)

将 B 寄存器传送到 A 寄存器。
　　　　　在运行时，它不受 C 标志的影响。执行后，C 标志变为 0。

![](https://files.mdnice.com/user/16190/0c94bdc6-2810-432b-8a96-a6e1ef80edfb.png)

将 A 寄存器转移到 B 寄存器。
　　　　　在运行时，它不受 C 标志的影响。执行后，C 标志变为 0。

![](https://files.mdnice.com/user/16190/092f21b9-7413-4731-a276-03806a1ac323.png)

将立即数添加到 A 寄存器。
　　　　　在运行时，它不受 C 标志的影响。执行后，当发生进位时，C 标志设置为 1。

![](https://files.mdnice.com/user/16190/4c6ffa75-1f03-4780-b8b2-2527946649ef.png)

将立即数添加到 B 寄存器。
　　　　　在运行时，它不受 C 标志的影响。执行后，当发生进位时，C 标志设置为 1。

![](https://files.mdnice.com/user/16190/c2866e62-4ed8-4068-890f-b114a20e2d38.png)

将数据从输入端口传输到 A 寄存器。
　　　　　在运行时，它不受 C 标志的影响。执行后，C 标志变为 0。

![](https://files.mdnice.com/user/16190/9db7c8cb-0e86-4ff4-b3cd-c104e9ed49ba.png)

将数据从输入端口传输到 B 寄存器。
　　　　　在运行时，它不受 C 标志的影响。执行后，C 标志变为 0。

![](https://files.mdnice.com/user/16190/6c0f0e1e-a2d7-4657-8654-e12c7e16d937.png)

将立即数据传输到输出端口。
　　　　　在运行时，它不受 C 标志的影响。执行后，C 标志变为 0。

![](https://files.mdnice.com/user/16190/788a10bb-1afa-4bbb-bff8-09d3ebc31f2b.png)

B 将寄存器转发到输出端口。
　　　　　在运行时，它不受 C 标志的影响。执行后，C 标志变为 0。

![](https://files.mdnice.com/user/16190/748d0b67-d17c-4abd-a1d3-a1e7bb432d9f.png)

跳转到立即数指示的地址。
　　　　　在运行时，它不受 C 标志的影响。执行后，C 标志变为 0。

![](https://files.mdnice.com/user/16190/ec37f529-828e-49cb-a3ab-ef54747f6b6c.png)

当 C 标志为 0 时，它跳转到立即数所指示的地址。当 C 标志为 1 时，什么都不做。
　　　　　在运行时，C 标志会更改行为。执行后，C 标志变为 0。

# 没错，设计方案源头的书是日文的，但是不要怕，我们会有很好的中文解析，大家完全能理解它！
哈哈哈，这句话我本来打算写在开头，但是怕读者畏惧，现在你已经读完了所有的解析，简单吧? 这句话成为了废话。

参考书籍：

![](https://files.mdnice.com/user/16190/bd2d8195-d402-4065-83de-36379f26cc23.png)

![](https://files.mdnice.com/user/16190/c883158b-457c-4774-be05-a8c09633b26b.png)

图书出版社收到的读者书评
用 10 个 IC 轻松介绍 CPU 设计!
计算机的核心是一个名为 CPU 的黑盒子。 被称为 CPU 的黑匣子是计算机的核心，以 4 位 CPU 为例，从其运作的 "超级 "基本原理到设计进行解释。 你实际上可以只用秋叶原上的零件来制作你自己的 CPU! 即使你没有真正做到这一点，它肯定是有趣的阅读。 -来自 "BOOK "数据库
一个叫做 CPU 的黑盒子是计算机的核心。 它解释了从其运作的 "超级 "基本原则到具体的设计实例的一切。 也可以只用秋叶原上的零件来实际建造。 -来自 "MARC "数据库
计算机的核心是一个名为 CPU 的黑盒子。 本书解释了从其运作的 "超级 "基本原则到具体的设计实例的一切。 你甚至可以只用秋叶原上的零件来建造你自己的东西!

作者简介
Iku Watanami
电路工程师。 从一家计算机制造商退休后，他成为一名独立的工程师。

基本信息
 出版社 ‏ : ‎ 毎日コミュニケーションズ (2003 年 10 月 1 日)
 出版日期 ‏ : ‎ 2003 年 10 月 1 日
 语言 ‏ : ‎ 日语
 单行本-平装 ‏ : ‎ 328 页
 ISBN-10 ‏ : ‎ 4839909865
 ISBN-13 ‏ : ‎ 978-4839909864
