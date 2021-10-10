## Go 汇编指令学习
#### 汇编指令学习
如果想单纯学习汇编指令，我使用的书籍是《汇编语言:基于x86处理器(原书第7版)》,汇编IDE使用vs2015+MASM。  

> 关于MASM:  
> Visual Studio包括面向 x64 代码的 Microsoft Assembler (MASM) 32 位和 64 位托管版本。 命名为 ml64.exe，这是接受 x64 汇编程序语言的汇编程序。 在安装过程中选择 C++ 工作负载时，将安装 MASM Visual Studio工具。 MASM 工具不单独下载。 有关如何下载和安装副本的说明，Visual Studio安装 Visual Studio。 如果不想在 IDE 中安装完整的 Visual Studio，但只想安装命令行工具，请下载适用于Visual Studio。

[MASM基本使用](https://github.com/ymm135/Irvine)


<br>

本书籍的[官方网站](http://www.asmirvine.com/index6th.htm).  [源码仓库](https://github.com/ymm135/Irvine). [vs2015工程](../TD4-4BIT-CPU/res/files/汇编语言vs2015工程)  
<br>

网络公开课[汇编语言程序设计](https://www.icourses.cn/web/sword/portal/shareDetails?cId=2720#/course/chapter)
[ppt讲义](./doc/汇编程序设计)

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



因为plan9是抽象的汇编指令，最终还要转换为对应CPU的汇编指令，如果在debug时，能够反汇编，并且知道对应关系，那就可以直接查看反汇编指令。(最重要的是x86汇编可以调试,便于理解).  

但是现在gdb调试go语言不是最好的，最佳的攻击是: [delve](https://github.com/go-delve/delve) deleve的[指令详解](https://github.com/go-delve/delve/tree/master/Documentation/cli)  参考文档:[go语言调试](https://golang.org/doc/gdb) 

#### Go汇编指令学习
##### Go Slice汇编指令学习
- go中slice的具体实现

src/runtime/slice.go
```
type slice struct {
	array unsafe.Pointer
	len   int
	cap   int
}
```

slice 共有三个属性： 指针，指向底层数组； 长度，表示切片可用元素的个数，也就是说使用下标对 slice 的元素进行访问时，下标不能超过 slice 的长度； 容量，底层数组的元素个数，容量 >= 长度。在底层数组不进行扩容的情况下，容量也是 slice 可以扩张的最大限度。  

- slice的创建

```
直接声明     var slice[]int
new         slice := *new([]int)
字面量       slice := []int{1,2,3,4,5}
make        slice := make([]int, 5, 10)
从切片或数据截取     slice := array[1:5] 或 slice := sourceSlice[1:5]
```
  


- 源代码  

```
> main.main() ./slicelearn.go:5 (hits goroutine(1):1 total:1) (PC: 0x10aba2f)
     1:	package main
     2:	
     3:	import "fmt"
     4:	
=>   5:	func main(){
     6:		slice := make([]int , 5, 10)
     7:		slice[2] = 2
     8:		slice[3] = 256
     9:		length := len(slice)
    10:		fmt.Println(length)
```

**使用delve调试界面**  
delve功能非常强大，基本操作有增删断点，全局局部变量打印，断点调试等，另外还反汇编代码查看、汇编断点调试、内存查看等
vscode使用的是DAP(debug adapter). 也可以使用[gdlv](https://github.com/aarzilli/gdlv)独立的IDE,macos需要安装dlv.(brew install delve)

编译问题:  
```
vendor/github.com/go-gl/glfw/v3.3/glfw/c_glfw.go

  1 package glfw
  2 
  3 /*
  4 #include "glfw/src/context.c"
  5 #include "glfw/src/init.c"
  6 #include "glfw/src/input.c"
  7 #include "glfw/src/monitor.c"
  8 #include "glfw/src/vulkan.c"
  9 #include "glfw/src/window.c"
 10 #include "glfw/src/osmesa_context.c"
 11 */
 12 import "C"
```

找不到c文件??  
把glfw的源码放在了/usr/local/include文件夹下。  


![DAP](./res/vscode-go-debug-dap.png)  
[Delve调试器](https://chai2010.cn/advanced-go-programming-book/ch3-asm/ch3-09-debug.html)

```
# 开始调试
dlv debug

# 设置断点
b slicelearn.go:5

# 开始调试
continue

# 下一个语句
next

# 跳入
s

# 在调试汇编时，可以执行si，进行单个汇编指令调试，对于分析非常好用
si

(dlv) help
The following commands are available:
    args ------------------------ Print function arguments.
    break (alias: b) ------------ Sets a breakpoint.
    breakpoints (alias: bp) ----- Print out info for active breakpoints.
    clear ----------------------- Deletes breakpoint.
    clearall -------------------- Deletes multiple breakpoints.
    condition (alias: cond) ----- Set breakpoint condition.
    config ---------------------- Changes configuration parameters.
    continue (alias: c) --------- Run until breakpoint or program termination.
    disassemble (alias: disass) - Disassembler.(反汇编)
    down ------------------------ Move the current frame down.
    exit (alias: quit | q) ------ Exit the debugger.
    frame ----------------------- Set the current frame, or execute command...
    funcs ----------------------- Print list of functions.
    goroutine ------------------- Shows or changes current goroutine
    goroutines ------------------ List program goroutines.
    help (alias: h) ------------- Prints the help message.
    list (alias: ls | l) -------- Show source code.
    locals ---------------------- Print local variables.
    next (alias: n) ------------- Step over to next source line.
    on -------------------------- Executes a command when a breakpoint is hit.
    print (alias: p) ------------ Evaluate an expression.
    regs ------------------------ Print contents of CPU registers.
    restart (alias: r) ---------- Restart process.
    set ------------------------- Changes the value of a variable.
    source ---------------------- Executes a file containing a list of delve...
    sources --------------------- Print list of source files.
    stack (alias: bt) ----------- Print stack trace.
    step (alias: s) ------------- Single step through program.
    step-instruction (alias: si)  Single step a single cpu instruction.
    stepout --------------------- Step out of the current function.
    thread (alias: tr) ---------- Switch to the specified thread.
    threads --------------------- Print out info for every traced thread.
    trace (alias: t) ------------ Set tracepoint.
    types ----------------------- Print list of types
    up -------------------------- Move the current frame up.
    vars ------------------------ Print package variables.
    whatis ---------------------- Prints type of an expression.
Type help followed by a command for full documentation.

寄存器查看
(dlv) regs
    Rip = 0x0000000000494766
    Rsp = 0x000000c000091eb0
    Rax = 0x0000000000494740
    Rbx = 0x0000000000000000
    Rcx = 0x0000000000000000
    Rdx = 0x00000000004b2ae0
    Rsi = 0x0000000000000000
    Rdi = 0x00000000ffffffff
    Rbp = 0x000000c000091f70
     R8 = 0x0000000000000010
     R9 = 0x0000000000000008
    R10 = 0x0000000000000002
    R11 = 0x0000000000000000
    R12 = 0x000000c000091f30
    R13 = 0x0000000000000000
    R14 = 0x000000c0000001a0
    R15 = 0x0000000000000058
 Rflags = 0x0000000000000202	[IF IOPL=0]
     Es = 0x0000000000000000
     Cs = 0x0000000000000033
     Ss = 0x000000000000002b
     Ds = 0x0000000000000000
     Fs = 0x0000000000000063
     Gs = 0x0000000000000000
Fs_base = 0x0000000000530870
Gs_base = 0x0000000000000000

查看内存视图
(dlv) print &slice
(*[]int)(0xc000091f40)

(dlv) x -fmt hex -count 24 -size 1 0xc000091f40
0xc000091f40:   0xd0   0x1e   0x09   0x00   0xc0   0x00   0x00   0x00   
0xc000091f48:   0x05   0x00   0x00   0x00   0x00   0x00   0x00   0x00   
0xc000091f50:   0x0a   0x00   0x00   0x00   0x00   0x00   0x00   0x00

```


**使用gdb调试界面**  
类似于C语言的调试，可以清晰看到slice的内部实现(数据结构),并且可以查看slice的内存视图  
![](./res/slice的调试.png)


- 汇编指令  
  
以下这两条指令得到反汇编指令: 
objdump -d slicelearn > att.asm  
go tool objdump -S slicelearn > plan9.asm  

查看汇编指令: 
go tool compile -N -l -S slicelearn.go

使用delve调试，disass查看反汇编

```
    sl.go:5		0x10aba20	  lea r12, ptr [rsp-0x48]          //lea 返回间接操作符的地址,
	sl.go:5		0x10aba25	  cmp r12, qword ptr [r14+0x10]    //cmp 比较, r11,r12临时数据存放
	sl.go:5		0x10aba29	  jbe 0x10abb62                    //JBE/JNA     小于或等于转移
	sl.go:5		0x10aba2f*	  sub rsp, 0xc8                    //c8=200字节, 也就是偏移200/8=25
	sl.go:5		0x10aba36	  mov qword ptr [rsp+0xc0], rbp    //[]是取操作数, 把rbp的内容保存到rsp+0xc0,保留上一次rbp的地址
	sl.go:5		0x10aba3e	  lea rbp, ptr [rsp+0xc0]          //把[rsp+0xc0]地址赋给rbp
=>	sl.go:6		0x10aba46	  lea rdi, ptr [rsp+0x20]          //把rsp+0xc0后的地址赋给rdi
	sl.go:6		0x10aba4b	  lea rdi, ptr [rdi-0x30]          //最终rdi指向rsp的的下16个字节(0x10)
	sl.go:6		0x10aba4f	  nop word ptr [rax+rax*1], ax     //NOP空操作. ax是eax地址的一般
	sl.go:6		0x10aba58	  nop dword ptr [rax+rax*1], eax
	sl.go:6		0x10aba60	  mov qword ptr [rsp-0x10], rbp    //rsp-0x10位置存储rbp的值
	sl.go:6		0x10aba65	  lea rbp, ptr [rsp-0x10]          //把rbp存储rsp-0x10的地址
	sl.go:6		0x10aba6a	  call 0x1060170                   //调用0x1060170函数,可以使用si进行单个cpu指令调试,在调用处设置断点 b duff_amd64.s:95
	sl.go:6		0x10aba6f	  mov rbp, qword ptr [rbp]         
	sl.go:6		0x10aba73	  lea rcx, ptr [rsp+0x20]         //把rsp+0x20内存存放的值赋给rcx rcx=*(rsp+0x20),这个就是slice数组地址
	sl.go:6		0x10aba78	  test byte ptr [rcx], al         //test进行and操作,选择跳转
	sl.go:6		0x10aba7a	  jmp 0x10aba7c
	sl.go:6		0x10aba7c	  jmp 0x10aba7e
	sl.go:6		0x10aba7e	  mov qword ptr [rsp+0x90], rcx   // 往slice的array填充地址(0xc00011fed0)
	sl.go:6		0x10aba86	  mov qword ptr [rsp+0x98], 0x5   // slice的len值5
	sl.go:6		0x10aba92	  mov qword ptr [rsp+0xa0], 0xa   // slice的cap值10
	sl.go:6		0x10aba9e	  data16 nop
	sl.go:7		0x10abaa0	  jmp 0x10abaa2                  //slice[2] = 2
	sl.go:7		0x10abaa2	  mov qword ptr [rsp+0x30], 0x2  //RSP指向的地址是c00011feb0, rsp+0x30=c00011fee0 = 0xc00011fed0+0x10
	sl.go:8		0x10abaab	  mov rcx, qword ptr [rsp+0x90]  // slice[2] = 256
	sl.go:8		0x10abab3	  jmp 0x10abab5
	sl.go:8		0x10abab5	  mov qword ptr [rcx+0x18], 0x100
	sl.go:9		0x10ababd	  mov rcx, qword ptr [rsp+0x98]  //length := len(slice)
	sl.go:9		0x10abac5	  mov qword ptr [rsp+0x18], rcx
	sl.go:10	0x10abaca	  movups xmmword ptr [rsp+0x80], xmm15  //fmt.Println(length)
	sl.go:10	0x10abad3	  lea rcx, ptr [rsp+0x80]
	sl.go:10	0x10abadb	  mov qword ptr [rsp+0x78], rcx
	sl.go:10	0x10abae0	  mov rax, qword ptr [rsp+0x18]
	sl.go:10	0x10abae5	  call $runtime.convT64
	sl.go:10	0x10abaea	  mov qword ptr [rsp+0x70], rax
	sl.go:10	0x10abaef	  mov rcx, qword ptr [rsp+0x78]
	sl.go:10	0x10abaf4	  test byte ptr [rcx], al
	sl.go:10	0x10abaf6	  lea rdx, ptr [rip+0x7443]
	sl.go:10	0x10abafd	  mov qword ptr [rcx], rdx
	sl.go:10	0x10abb00	  lea rdi, ptr [rcx+0x8]
	sl.go:10	0x10abb04	  cmp dword ptr [runtime.writeBarrier], 0x0
	sl.go:10	0x10abb0b	  jz 0x10abb0f
	sl.go:10	0x10abb0d	  jmp 0x10abb15
	sl.go:10	0x10abb0f	  mov qword ptr [rcx+0x8], rax
	sl.go:10	0x10abb13	  jmp 0x10abb1c
	sl.go:10	0x10abb15	  call $runtime.gcWriteBarrier
	sl.go:10	0x10abb1a	  jmp 0x10abb1c
	sl.go:10	0x10abb1c	  mov rax, qword ptr [rsp+0x78]
	sl.go:10	0x10abb21	  test byte ptr [rax], al
	sl.go:10	0x10abb23	  jmp 0x10abb25
	sl.go:10	0x10abb25	  mov qword ptr [rsp+0xa8], rax
	sl.go:10	0x10abb2d	  mov qword ptr [rsp+0xb0], 0x1
	sl.go:10	0x10abb39	  mov qword ptr [rsp+0xb8], 0x1
	sl.go:10	0x10abb45	  mov ebx, 0x1
	sl.go:10	0x10abb4a	  mov rcx, rbx
	sl.go:10	0x10abb4d	  call $fmt.Println
	sl.go:11	0x10abb52	  mov rbp, qword ptr [rsp+0xc0]
	sl.go:11	0x10abb5a	  add rsp, 0xc8
	sl.go:11	0x10abb61	  ret
	sl.go:5		0x10abb62	  call $runtime.morestack_noctxt
	.:0			0x10abb67	  jmp $main.main
```

```
汇编调用的实现: call 0x1060170 src/runtime/duff_amd64.s
Code generated by mkduff.go
runtime·duffzero is a Duff's device for zeroing memory.  主要用于内存清零初始化!
XMM, registers of x86 microprocessors with Streaming SIMD Extensions
在计算机科学领域，达夫设备（英文：Duff's device）是串行复制（serial copy）的一种优化实现，通过汇编语言编程时一常用方法，实现展开循环，进而提高执行效率。这一方法据信为当时供职于卢卡斯影业的汤姆·达夫于1983年11月发明，并可能是迄今为止利用C语言switch语句特性所作的最巧妙的实现。(维基百科)

在调用duff清零函数之前,rdi已经指向slice数组的地址了，等把数组清零后，再去创建slice，最终再去填充array地址/len/cap

(dlv) disass -a 0x1060170 0x10601a0
TEXT runtime.duffzero(SB) /usr/local/go-1.17/src/runtime/duff_amd64.s
	duff_amd64.s:95		0x1060170*		movups xmmword ptr [rdi+0x30], xmm15  //128bit寄存器,打印寄存器的值 print %x XMM15 
	duff_amd64.s:96		0x1060175		lea rdi, ptr [rdi+0x40]               //print %x
	duff_amd64.s:98		0x1060179		movups xmmword ptr [rdi], xmm15
	duff_amd64.s:99		0x106017d		movups xmmword ptr [rdi+0x10], xmm15
	duff_amd64.s:100	0x1060182		movups xmmword ptr [rdi+0x20], xmm15
	duff_amd64.s:101	0x1060187		movups xmmword ptr [rdi+0x30], xmm15
	duff_amd64.s:102	0x106018c		lea rdi, ptr [rdi+0x40]
	duff_amd64.s:104	0x1060190	    ret

对应duff_amd64.s代码
// X15: zero
// DI: ptr to memory to be zeroed    //Source Index (SI)  Destination Index (DI)  
// DI is updated as a side effect.
95:	    MOVUPS	X15,48(DI)           //在DI地址基础上偏移0x30  slice地址:0xc00011ff40, slice数组地址:0xc00011fed0
96:	    LEAQ	64(DI),DI            //对照反汇编查看，64为cpu，int占用8个byte， 数组大小就是80byte = 0x50, 40byte=0x28
97:    
98:	    MOVUPS	X15,(DI)
99:	    MOVUPS	X15,16(DI)
100:	MOVUPS	X15,32(DI)
101:	MOVUPS	X15,48(DI)
102:	LEAQ	64(DI),DI
103:
104:	RET


对应mkduff.go代码为:
func zeroAMD64(w io.Writer) {
	// X15: zero
	// DI: ptr to memory to be zeroed
	// DI is updated as a side effect.
	fmt.Fprintln(w, "TEXT runtime·duffzero<ABIInternal>(SB), NOSPLIT, $0-0")
	for i := 0; i < 16; i++ {
		fmt.Fprintln(w, "\tMOVUPS\tX15,(DI)")
		fmt.Fprintln(w, "\tMOVUPS\tX15,16(DI)")
		fmt.Fprintln(w, "\tMOVUPS\tX15,32(DI)")
		fmt.Fprintln(w, "\tMOVUPS\tX15,48(DI)")
		fmt.Fprintln(w, "\tLEAQ\t64(DI),DI") // We use lea instead of add, to avoid clobbering flags
		fmt.Fprintln(w)
	}
	fmt.Fprintln(w, "\tRET")
}
```

```
slice地址及数组地址
(dlv) print %x RSP
c00011feb0

(dlv) print %x slice
[]int len: 5, cap: 10, [0,0,0,0,0]

(dlv) print %x &slice
(*[]int)(0xc00011ff40)

(dlv) x -fmt hex -count 48 -size 1 0xc00011ff40
0xc00011ff40:   0xd0   0xfe   0x11   0x00   0xc0   0x00   0x00   0x00   
0xc00011ff48:   0x05   0x00   0x00   0x00   0x00   0x00   0x00   0x00   
0xc00011ff50:   0x0a   0x00   0x00   0x00   0x00   0x00   0x00   0x00   
0xc00011ff58:   0xd3   0xd7   0x03   0x01   0x00   0x00   0x00   0x00   
0xc00011ff60:   0x00   0xe0   0x10   0x00   0xc0   0x00   0x00   0x00   
0xc00011ff68:   0xa0   0x01   0x00   0x00   0xc0   0x00   0x00   0x00   

(dlv) x -fmt hex -count 48 -size 1 0xc00011fed0
0xc00011fed0:   0x00   0x00   0x00   0x00   0x00   0x00   0x00   0x00   
0xc00011fed8:   0x00   0x00   0x00   0x00   0x00   0x00   0x00   0x00   
0xc00011fee0:   0x00   0x00   0x00   0x00   0x00   0x00   0x00   0x00   
0xc00011fee8:   0x00   0x00   0x00   0x00   0x00   0x00   0x00   0x00   
0xc00011fef0:   0x00   0x00   0x00   0x00   0x00   0x00   0x00   0x00   
0xc00011fef8:   0x00   0x00   0x00   0x00   0x00   0x00   0x00   0x00 

```


- 寄存器与调用栈图  


- 总结  
在创建slice之前，首先分配array的地址rdi/rcx = [rsp+0x20], 调用系统函数进行清零，最终把array、len、cap填充到slice指向的内存空间。
在赋值的过程中，就是把值填充到对应数组起始地址的内存，获取slice长度时，就是去除slice对应len内存地址的值mov [rsp+0x18] [rsp+0x98] (简化指令,rsp+0x18代表len接收变量地址, rsp+0x98代表len内存地址8byte)

备注:首先要区分堆栈地址和堆栈地址内存存储的值。rsp+0x20表明栈顶偏移0x20的栈地址，[rsp+0x20] 代表栈顶偏移0x20的栈**地址的值**  

##### Go 函数调用及返回值
源代码  


汇编指令  

寄存器与调用栈图  


总结  

##### Go 结构体和接口实现
源代码  


汇编指令  

寄存器与调用栈图  


总结  
