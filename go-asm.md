## Go 汇编指令学习
#### 汇编指令学习
如果想单纯学习汇编指令，我使用的书籍是《汇编语言:基于x86处理器(原书第7版)》,汇编IDE使用vs2015+MASM。  

> 关于MASM:  
> Visual Studio包括面向 x64 代码的 Microsoft Assembler (MASM) 32 位和 64 位托管版本。 命名为 ml64.exe，这是接受 x64 汇编程序语言的汇编程序。 在安装过程中选择 C++ 工作负载时，将安装 MASM Visual Studio工具。 MASM 工具不单独下载。 有关如何下载和安装副本的说明，Visual Studio安装 Visual Studio。 如果不想在 IDE 中安装完整的 Visual Studio，但只想安装命令行工具，请下载适用于Visual Studio。

[MASM基本使用](https://github.com/ymm135/Irvine)


<br>

本书籍的[官方网站](http://www.asmirvine.com/index6th.htm).  [源码仓库](https://github.com/ymm135/Irvine). [vs2015工程](../TD4-4BIT-CPU/res/files/汇编语言vs2015工程)

<br>

可以通过其他工具查看寄存器及调用栈 [asm-cli](https://github.com/cch123/asm-cli)  

![汇编指令工具](res/汇编指令工具.png)  

#### x86/AT&T/Plan9 汇编
##### x86汇编指令  
[x86汇编官方文档介绍](https://software.intel.com/content/www/us/en/develop/articles/introduction-to-x64-assembly.html)  

多年来，PC程序员使用 x86 汇编来编写性能关键代码。但是，32 位 PC 正在被 64 位 PC 取代，并且底层汇编代码已更改。本白皮书是对 x64 汇编的介绍。不需要 x86 代码的先验知识，尽管它使转换更容易。

**x64 是对 Intel 和 AMD 32 位 x86 指令集架构 (ISA) 的 64 位扩展的通用名称。** AMD 推出了 x64 的第一个版本，最初称为 x86-64，后来更名为 AMD64。英特尔将他们的实现命名为 IA-32e，然后是 EMT64。两个版本之间存在一些轻微的不兼容，但大多数代码在两个版本上都可以正常工作；详细信息可以在英特尔® 64 和 IA-32 架构软件开发人员手册中找到和 AMD64 架构技术文档。我们称这种交叉风格为 x64。两者都不要与 64 位英特尔® 安腾® 架构（称为 IA-64）混淆。

本白皮书不会涵盖硬件细节，例如缓存、分支预测和其他高级主题。文章末尾将提供一些参考资料，以便在这些领域进一步阅读。

汇编通常用于程序的性能关键部分，尽管对于大多数程序员来说，它的性能很难超过一个好的 C++ 编译器。汇编知识对于调试代码很有用——**有时编译器会生成不正确的汇编代码，在调试器中单步调试代码有助于找到原因。代码优化器有时会出错。汇编的另一个用途是连接或修复没有源代码的代码。反汇编可让您更改/修复现有的可执行文件。如果您想知道您选择的语言在底层是如何工作的——为什么有些事情很慢而有些事情很快，那么汇编是必要的。最后，在诊断恶意软件时，汇编代码知识是必不可少的。**
  
通用架构
由于 64 位寄存器允许访问多种大小和位置，我们定义一个字节为 8 位，一个字为 16 位，双字为 32 位，四字为 64 位，双四字为 128 位. Intel 将字节存储为“little endian”，这意味着较低的有效字节存储在较低的内存地址中。  
x86通用架构  

<br>

![x86通用架构](/res/x86通用架构.png)  

<br>

汇编器工具: Assemblers  
An Internet search reveals x64-capable assemblers such as the Netwide Assembler [NASM](https://www.nasm.us/), a NASM rewrite called [YASM](http://yasm.tortall.net/), the fast Flat Assembler [FASM](http://flatassembler.net/), and the traditional Microsoft MASM. There is even a free IDE for x86 and x64 assembly called WinASM. Each assembler has varying support for other assemblers' macros and syntax, but assembly code is not source-compatible across assemblers like C++ or Java* are.

For the examples below, I use the 64-bit version of MASM, ML64.EXE, freely available in the platform SDK. For the examples below note that MASM syntax is of the form Instruction Destination, Source

Some assemblers reverse source and destination, so read your documentation carefully.

##### plan9汇编指令 
现在是有plan9操作系统,但是也有一套plan9的汇编指令。  



#### Go汇编指令学习
##### Go Slice汇编指令学习
源代码  


汇编指令  

寄存器与调用栈图  


总结  

##### 

