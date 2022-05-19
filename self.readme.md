# 自己动手制作札记
### [宏晶STC单片机官方数据手册(DATASHEET)](https://www.stcisp.com/stcmcu-pdf.html)  
### [宏晶STC单片机官方数据手册(DATASHEET)-离线](res/files/stc)  

### [cpu自制入门电子书](https://github.com/ymm135/AZPRcpu/tree/master/book)  

### [入门-面包板电子制作130.md](md/面包板电子制作130.md)  
### [练习板及焊接学习](md/练习板及焊接学习.md)  

## 基础知识(CPU逻辑电路)
### [冯·诺伊曼结构](https://zh.m.wikipedia.org/zh-hans/%E5%86%AF%C2%B7%E8%AF%BA%E4%BC%8A%E6%9B%BC%E7%BB%93%E6%9E%84)  
冯·诺伊曼结构（英语：Von Neumann architecture），也称冯·纽曼模型（Von Neumann model）或普林斯顿结构（Princeton architecture），是一种将程序指令存储器和数据存储器合并在一起的电脑设计概念结构。本词描述的是一种实作通用图灵机的计算装置，以及一种相对于平行计算的序列式架构参考模型（referential model）  

<div align=center>
<img src="./res/Von_Neumann_architecture.svg.png" width="60%" height="60%"></img>  
</div>

存储程序计算机在体系结构上主要特点有：

1. 以运算单元为中心
2. 采用存储程序原理
3. 存储器是按地址访问、线性编址的空间
4. 控制流由指令流产生
5. 指令由操作码和地址码组成
6. 数据以二进制编码

### TD4 结构

<div align=center>
<img src="./res/TD4-CPU-Block.png" width="80%" height="80%"></img>  
</div>

### 原理图 
[TD4-SCH.pdf](origin/hardware/v1.3/TD4-SCH.pdf)   

<div align=center>
<img src="./res/原理图预览.png" width="100%" height="100%"></img>  
</div>

### PCB标识

Td4的PCB元器件标识  
<div align=center>
<img src="./res/Td4的PCB标识.png" width="100%" height="100%"></img>  
</div> 

### BOM 
[BOM](origin/hardware/v1.3/TD4-BOM.htm)  

| Designator | Comment  | LibRef  | Description | Quantity |
| ---------- | ------- | -------- | ---------- | --------- 
| U1  | SN74HC74N | SN74HC74N  | Dual D-Type Positive-Edge-Triggered Flip-Flop with Clear and Preset  | 1  |  
| U2  | SN74HC14N  | SN74HC14N  | Hex Schmitt-Trigger Inverter  | 1  |  
| U3  | SN74HC161N  | SN74HC161N  | 4-Bit Synchronous Binary Counter  | 1  |  
| U4  | SN74HC161N  | SN74HC161N  | 4-Bit Synchronous Binary Counter  | 1  |  
| U5  | SN74HC161N  | SN74HC161N  | 4-Bit Synchronous Binary Counter  | 1  |  
| U6  | SN74HC161N  | SN74HC161N  | 4-Bit Synchronous Binary Counter  | 1  |  
| U7  | SN74HC153N  | SN74HC153N  | Dual 4-Line to 1-Line Data Selector/Multiplexer  | 1  |  
| U8  | SN74HC153N  | SN74HC153N  | Dual 4-Line to 1-Line Data Selector/Multiplexer  | 1  |  
| U9  | SN74HC283N  | SN74HC283N  | 4-Bit Binary Full Adder with Fast Carry  | 1  |  
| U10 | SN74HC32N  | SN74HC32N  | Quadruple 2-Input Positive-OR Gate  | 1  |  
| U11 | SN74HC10N  | SN74HC10N  | Triple 3-Input Positive-NAND Gate  | 1  |  
| U12 | SN74HC540N  | SN74HC540N  | Octal Buffer and Line Driver with 3-State Outputs  | 1  |  
| U13 | SN74HC154NT  | SN74HC154NT  | 4-Line to 16-Line Decoder / Demultiplexer  | 1  |  

## 原理分析
### 数字逻辑设计
<div align="center">
    <font face="黑体" color=gray size=3>TD4-HC161</font> 
</div>
<div align=center>
    <img src="./res/TD4-HC161.png" width="80%" height="80%"></img>  
</div>
<br>

<div align="center">
    <font face="黑体" color=gray size=3>TD4-HC153</font> 
</div>
<div align=center>
    <img src="./res/TD4-HC153x2.png" width="60%" height="60%"></img>  
</div>
<br>

<div align="center">
    <font face="黑体" color=gray size=3>TD4-ALU</font> 
</div>
<div align=center>
    <img src="./res/TD4-ALU.png" width="60%" height="60%"></img>  
</div>
<br>

<div align="center">
    <font face="黑体" color=gray size=3>TD4-HC161PC</font> 
</div>
<div align=center>
    <img src="./res/TD4-HC161PC.png" width="80%" height="80%"></img>  
</div>
<br>

<div align="center">
    <font face="黑体" color=gray size=3>TD4-Decoder</font> 
</div>
<div align=center>
    <img src="./res/TD4-Decoder.png" width="80%" height="80%"></img>  
</div>
<br>

<div align="center">
    <font face="黑体" color=gray size=3>TD4-Main</font> 
</div>
<div align=center>
    <img src="./res/TD4-Main.png" width="100%" height="100%"></img>  
</div>
<br>



### 时钟、复位 
### 存储器
### 输入、输出
### 控制器
### 运算器



## CPU焊接  
### USB 
<div align=center>
<img src="./res/USB座子焊接.jpg" width="60%" height="60%"></img>  
</div>
<br>

<div align=center>
<img src="./res/micro-usb-sch.png" width="50%" height="50%"></img>  
</div>

### 正面图  
<div align=center>
<img src="./res/自己制作的TD4.jpg" width="100%" height="100%"></img>  
</div>

### 背面图    
<div align=center>
<img src="./res/Td4-背面.png" width="100%" height="100%"></img>  
</div>

## CPU编写程序
[Verilog HDL学习](https://www.bilibili.com/video/BV12y4y1v7V3?p=1)  
### [CPU Verilog代码](https://github.com/ymm135/AZPRcpu)   


## CPU展示  


## 疑问及问题

### 有一个LED一直常量 ?  

`U4`的`Q3`脚一直处于高电平，更换其他的161封装时，问题依旧存在，说明不是封装问题。那就需要沿着上游的`P3`接着找。  




