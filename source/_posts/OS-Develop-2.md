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


### loop




