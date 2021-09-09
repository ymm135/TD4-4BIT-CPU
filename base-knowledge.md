### 电子元器件与电路基础
### 电路基础
#### 电流电压欧姆定律
<img src="./res/res-dianliu.png" width="80%" height="80%" title="电流"></img>  
  
<img src="./res/res-dianzu.png" width="80%" height="80%" title="电阻"></img>  
  
<img src="./res/res-dianya.png" width="80%" height="80%" title="电压"></img>  
  
<img src="./res/res-oumudinglv.png" width="80%" height="80%" title="欧姆定律1"></img>  
  
<img src="./res/res-oumudinglv-分析.png" width="80%" height="80%" title="欧姆定律2"></img>  
  

#### 通路、短路(开路)、短路
通路：电路有电流流过负载  
断路：电路中没有电流  
短路：电路中没有负载  
  
#### 基尔霍夫电路定律
<img src="./res/res-基尔霍夫电路定律.png" width="80%" height="80%" title="基尔霍夫电路定律"></img>  
  

#### 直流电与交流电
<img src="./res/res-直流电.png" width="80%" height="80%" title="直流电"></img>  
  
<img src="./res/res-交流电.png" width="80%" height="80%" title="交流电"></img>  
  
#### 数字电路
##### 模拟电路与数字电路
<img src="./res/res-模拟电路与数字电路.png" width="80%" height="80%" title="交流电"></img>  

因为数字电路只有两种电压状态，因此使用二进制是符合逻辑的。一个二进制由两个数字组成，0和1，也称数码(例如0=低电压，1=高电压)。  
一个二进制如11100(28$_{10}$)可以由位权依次相差二倍的数码表示:  
  
11100$_2$ = 1x2$^4$ + 1x2$^3$ + 1x2$^2$ + 0x2$^1$ + 0x2$^0$   

##### 二进制运算
* 加法 0+0＝0，0+1＝1，1+0＝1，1+1＝10
* 减法 0－0＝0，1－0＝1，1－1＝0，10－1＝1
* 乘法 0×0＝0，0×1＝0，1×0＝0，1×1＝1
* 除法 0÷1＝0，1÷1＝1
  
> 二进制减法的技巧是使用带符号位的补码代替减数，然后将正数与负数相加得到和。这种方法经常被数字电路使用，因为将加法与减法归功于一种算法，避免了小数中减去大数的令人头痛的问题。

> 原码、反码、补码的出现就是为了加减法的统一、简单。

<img src="./res/res-原码.png" width="60%" height="60%" title="原码"></img>  
  
<img src="./res/res-反码.png" width="60%" height="60%" title="反码"></img>  
  
<img src="./res/res-补码.png" width="60%" height="60%" title="补码"></img>  
  
#### 时钟周期
<img src="./res/res-时钟周期1.png" width="80%" height="80%" title="反码"></img>  
<img src="./res/res-时钟周期2.png" width="80%" height="80%" title="反码"></img>  
  

#### 逻辑门
<img src="./res/res-逻辑门1.png" width="80%" height="80%" title="逻辑门1"></img>  
  
<img src="./res/res-逻辑门2.png" width="80%" height="80%" title="逻辑门2"></img>  
  
<img src="./res/res-逻辑门3.png" width="80%" height="80%" title="逻辑门3"></img>  
  
<img src="./res/res-逻辑门4.png" width="80%" height="80%" title="逻辑门4"></img>  
  
<img src="./res/res-逻辑门5.png" width="80%" height="80%" title="逻辑门5"></img>  
  
<img src="./res/res-逻辑门6.png" width="80%" height="80%" title="逻辑门6"></img>  
  
<img src="./res/res-逻辑门7.png" width="80%" height="80%" title="逻辑门7"></img>  
  


##### 逻辑门应用


### 电子元器件


### proteus仿真使用
通过Proteus学习电路基本知识及元器件:point_right:[跳转](https://github.com/ymm135/proteus-learning/)
  
[英文手册](https://labcenter.s3.amazonaws.com/downloads/Tutorials.pdf)  

#### LED流水灯实验(入门)
实验目的([摘自爱课程大学单片机原理课程](https://www.icourses.cn/web/sword/portal/shareDetails?&&cId=5981#/course/chapter))  
<img src="./res/res-led流水灯实验1.png" width="80%" height="80%" title="实验目的"></img>  
  
<img src="./res/res-led流水灯实验2.png" width="80%" height="80%" title="原理图"></img>  
  
#### 示波器使用
示波器有4个通道，可以同时测试4路。只要把一个通道连接到导线上即可，在仿真的时候就会有波形出行。如果没有示波器出现，可在调试菜单中弹出。另外有可能没有抓到波形，可以通过按钮控制，点击按钮后再触发，选择oneshot模式，定格图像。

不知道的情况下选择自动模式，调试增益，查看结果！

<img src="./res/res-示波器分析串口协议.png" width="80%" height="80%" title="示波器分析串口协议"></img>  
  
<img src="./res/res-示波器显示.gif" width="80%" height="80%" title="直流电示波器"></img>  
