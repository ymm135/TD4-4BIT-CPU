# 自己动手制作札记
### [宏晶STC单片机官方数据手册(DATASHEET)](https://www.stcisp.com/stcmcu-pdf.html)  
### [宏晶STC单片机官方数据手册(DATASHEET)-离线](res/files/stc)  

### [cpu自制入门电子书](https://github.com/ymm135/AZPRcpu/tree/master/book)  

### [入门-面包板电子制作130.md](md/面包板电子制作130.md)  
### [练习板及焊接学习](md/练习板及焊接学习.md)  

## 基础知识(CPU逻辑电路)

## CPU电路板设计及焊接  

原理图: 
[TD4-SCH.pdf](origin/hardware/v1.3/TD4-SCH.pdf)  
[BOM](origin/hardware/v1.3/TD4-BOM.htm)  

Td4的PCB元器件标识  
<div align=center>
<img src="./res/Td4的PCB标识.png" width="100%" height="100%"></img>  
</div> 

原理图预览  
<div align=center>
<img src="./res/原理图预览.png" width="100%" height="100%"></img>  
</div>


自己焊制图:  
<div align=center>
<img src="./res/自己制作的TD4.jpg" width="100%" height="100%"></img>  
</div>


## CPU编写程序
### [CPU Verilog代码](https://github.com/ymm135/AZPRcpu)   


## CPU展示  


## 疑问及问题

### 有一个LED一直常量 ?  

`U4`的`Q3`脚一直处于高电平，更换其他的161封装时，问题依旧存在，说明不是封装问题。那就需要沿着上游的`P3`接着找。  


