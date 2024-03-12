---
title: OS Develop 2
date: 2022-03-22 20:10:06
tags: OS
---

### 简介
本章介绍汇编语言。


### 标号
标号是充当指令或数据位置标记的标识符。
- 数据标号
{% blockquote %}
count DW 100 ;定义一个名为count的变量a
{% endblockquote %}

- 代码标号
  以冒号结尾，表示一个位置，可以用于跳转。
{% blockquote %}
loop:
	jmp loop
{% endblockquote %}

### 伪指令
伪指令不是真正的指令，它是由NASM的编译器提供支持，伪指令在编译的时候执行。
- Dx族
- Resx族
- equ
- times
- vstar & org
- incbin

### 7种数据Size
- byte ： 8 位
- word ： 16 位
- dword ： 32 位
- qword ： 64 位
- tword ： 80 位
- oword ： 128 位
- yword ： 256 位

### Dx族
Dx族用来在内存中，顺序定义初始化数据。
nasm 定义了用于初始化上面 7 种 size 的 db 家族，它们用于定义初化常量值。
- db : define byte
- dw ：define word
- dd ：define doubleword
- dq ：define quadword
- dt ：define tword
- do ：define oword
- dy ：define yword

例如：
{% blockquote %}
db    0x55                ; just the byte 0x55
db    0x55,0x56,0x57      ; three bytes in succession
db    'a',0x55            ; character constants are OK
db    'hello',13,10,'$'   ; so are string constants
dw    0x1234              ; 0x34 0x12
dw    'a'                 ; 0x61 0x00 (it's just a number)
dw    'ab'                ; 0x61 0x62 (character constant)
dw    'abc'               ; 0x61 0x62 0x63 0x00 (string)
dd    0x12345678          ; 0x78 0x56 0x34 0x12
dd    1.234567e20         ; floating-point constant
dq    0x123456789abcdef0  ; eight byte constant
dq    1.234567e20         ; double-precision float
dt    1.234567e20         ; extended-precision float
{% endblockquote %}

### Resx族
这类伪指令，用来定义非初始化数据，非初始化数据存放在bass section中。
- resb ：reserve byte
- resw ：reserve word
- resd ：reserve doubword
- resq ：reserve quadword
- rest ：reserve tword
- reso ：reserve oword
- resy ：reserve yword

例如：
{% blockquote %}
buffer:         resb    64              ; reserve 64 bytes
wordvar:        resw    1               ; reserve a word
realarray       resq    10              ; array of ten reals
ymmval:         resy    1               ; one YMM register
{% endblockquote %}


### 变量定义
【变量名】 变量类型  表达式
例如：
{% blockquote %}
a db 1,2,3,4,5
db 7,8,9,10 ;非变量定义，只定义数据
b dw 123,123,123
c db 'hello, world!'
{% endblockquote %}

上面定义的数据都是顺序存放在内存中的，所以虽然第二行的数据没有定义名字，但是因为他紧跟在a 数组之后 b数组之前，所以使用a+5,或者 b-1 也是可以访问的，只是需要注意的是数据类型和 大端小端的问题。
声明的变量代表后面数据的地址，例如c是后面字符串的首地址，如果要使用，需要用中括号：

{% blockquote %}
mov ax, [c]
{% endblockquote %}

### equ
它的作用和#define相同，它并不占用内存，在编译之后，它将替换被调用的地方，例如：
{% blockquote %}
num equ 110
mov ax, num
mov bx, num
mov cx, num
{% endblockquote %}

编译后：
{% blockquote %}
mov ax, 110
mov bx, 110
mov cx, 110
{% endblockquote %}

### \$和\$$
\$表示当前行的汇编地址。
\$$表示当前程序的起始汇编地址。

### times
后面指令重复次数。
{% blockquote %}
buffer: db 'hello, world'
times 64-$+buffer db '' ;补齐64字节
{% endblockquote %}

### incbin
包含binary文件。例如
{% blockquote %}
ncbin “file.dat” ; include the whole file  
incbin “file.dat”,1024 ; skip the first 1024 bytes  
incbin “file.dat”,1024,512 ; skip the first 1024, and  
; actually include at most 512
{% endblockquote %}

### vstart & org
程序的加载位置是由加载它的一方来决定的(因为加载后，加载方需要去访问被加载方，去哪里访问，这个是加载方来确定的)。vstart和org是告诉编译器，将后面的所有数据(指令和变量)的地址，加上一个偏移量。
编译器以相对于文件开头偏移来编址的好处是利于重定位。程序内的地址是从0开始算的，当程序被加载到内存的某个地址(不一定是0)后，就需要用vstart或org来指定偏移量，这个偏移量也就是加载到内存的地址。


### mov
格式：
{% blockquote %}
mov 位宽 目的操作数，源操作数
{% endblockquote %}

目的操作数必需是一个容器，如内存单元、寄存器；源操作数可以是一个立即数，也可以是一个容器。

举例：
{% blockquote %}
mov byte [0x00],'2'
{% endblockquote %}

### loop
Loop指令需要和cx寄存器配合使用, 用于循环操作, 类似于高级语言中的for, while等。
cx存放的是循环次数，循环一次，cx中的值减1，当cx中的值大于0时，执行loop循环。

{% blockquote %}
  mov cx, 11
s:  
  add ax, ax
  loop s
{% endblockquote %}

### jmp
jmp 是无条件转移指令，无条件转移指令可转到内存中任何程序段，它可以只修改IP，也可以同时修改CS和IP。
1，短程转移
指令格式：JMP SHORT OPRD
语法格式：JMP 地址标号 ；（IP）=（IP）+8位位移量
指令功能：OPRD为转移地址的标号，指令中的SHORT规定了OPRD为有符号的8位二进制数，OPRD为转移地址的偏移量。该指令将程序执行的顺序转移到由（IP）+OPRD形成的新的程序执行的目标地址，从而实现程序的转移。转移的目标地址OPRD在指令中可以直接使用标号地址，但要求转移的目标地址的范围只能在JMP指令所处地址的－128～+127字节范围之内，如超出该范围，汇编时出错。

2，近程转移
指令格式：JMP NEAR PTR OPRD
语法格式：JMP 地址标号 ；（IP）=（IP）+16位位移量
指令功能：与短程转移的功能和要求相同，不同之处是近程转移的OPRD为有符号的16位二进制数，指令将程序执行的顺序转移到由（IP）+OPRD形成的新的程序执行的目标地址，转移的目标地址的范围只能在JMP指令所处地址的－32768～+32767字节范围之内，如超出该范围，汇编时出错。使用该指令时NEAR可省略。

3，段间转移
指令格式：JMP FAR PTR OPRD
语法格式：JMP 地址标号 ；（IP）= 新的偏移地址, ；（CS）= 新的代码段地址
指令功能：指令中用FAR PTR规定了该指令为段间的转移，OPRD为目的地址的标号，目的地址与JMP指令所在地址不在同一段内。执行该指令时要修改CS和IP的内容，将OPRD所在段的段地址送CS中，OPRD的段内偏移地址送IP中。

4，段内间转移
指令格式：JMP WORD PTR OPRD
语法格式：JMP reg16/mem ；（IP）= 新的偏移地址
指令功能：与短程转移的功能和要求相同，不同之处是段内间接转移的OPRD 可以是除立即数外的任何寄存或存储器寻址方式，转移的目标地址由OPRD的内容确定。

5，段间间接转移
指令格式：JMP DWORD PTR OPRD
语法格式：JMP mem32
指令功能：指令中用DWORD PTR规定了该指令为段间间接转移，OPRD只能是存储器寻址方式。执行该指令时将寻址到的内存单元的第一个字送入IP中，第二个字送入CS中。

### 条件转移
{% asset_img jmp_condition.png flag registor %}

### call & ret
在高级语言中，常有主程序调用其他子程序，子程序还可以调用子程序。
call指令可以实现调用一个子程序，在子程序里使用ret指令结束子程序的执行并返回主程序（类比C语言中的main函数调用cube函数进行理解），主程序继续往下执行。

{% blockquote %}
mov ax,0
call s     ;s是标号，执行call后，执行s标号所在的子程序
mov ax,4c00h
int 21h

s:add ax,1   ;标号所在子程序
  mov bx,10
  ret        ;子程序执行完后，返回主程序
{% endblockquote %}

### push & pop
push和pop是用来操作栈的2个指令；push将一个寄存器中的数据入栈，pop出栈用一个寄存器接收数据；相关的寄存器`SS:SP`。
举例：
mov ax 123H
push ax 的执行，由以下两步完成。
(1) SP = SP - 2, SS:SP 指向当前栈顶前面的单元，以当前栈顶前面的单元为新的栈顶；
(2) 将ax中的内容送入SS:SP 指向的内存单元处，SS:SP此时指向新栈顶。


### int
int指令是X86汇编语言中最重要的指令之一。它的作用是引发中断，调用“中断例程”（interrupt routine）

指令格式：
{% blockquote %}
int n ;n是中端号
{% endblockquote %}

指令执行过程：
{% blockquote %}
1，取中断类型码n；
2，标志寄存器入栈（pushf），IF=0，TF=0（重置中断标志位）；
3，CS、IP入栈；
4，查中断向量表， (IP)=(n\*4)，(CS)=(n\*4+2)。
{% endblockquote %}

