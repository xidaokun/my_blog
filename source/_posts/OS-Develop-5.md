---
title: OS Develop 5
date: 2022-04-23 14:13:02
tags:
   - IT-Basics
   - OS
---

### 简介
本章讨论Loader。
MBR被写在硬盘的0盘0道2扇区；MBR会从该扇区读取loader.bin，然后将它加载到0x900地址处。
同时在loader中，要设置vstart=0x900，以便MBR在加载完成后，可以jmp到0x900来执行loader。

接下来，loader会依次做如下工作：
- 定义GDT
- 定义Selector
- 开启保护模式
- 开启段机制
- 开启也机制
- 加载kernel
- 将执行权交给kernel


### Code
```nasm
%include "boot.inc"
section loader vstart=LOADER_BASE_ADDR
LOADER_STACK_TOP equ LOADER_BASE_ADDR
jmp loader_start		


GDT_BASE:   dd    0x00000000 
	   dd    0x00000000

CODE_DESC:  dd    0x0000FFFF
	   dd    DESC_CODE_HIGH4

DATA_STACK_DESC:  dd    0x0000FFFF
		 dd    DESC_DATA_HIGH4

VIDEO_DESC: dd    0x80000007
	   dd    DESC_VIDEO_HIGH4

GDT_SIZE   equ   $ - GDT_BASE
GDT_LIMIT   equ  GDT_SIZE-1
times 60 dq 0	; 此处预留60个描述符的slot

SELECTOR_CODE equ (0x0001<<3) + TI_GDT + RPL0
SELECTOR_DATA equ (0x0002<<3) + TI_GDT + RPL0
SELECTOR_VIDEO equ (0x0003<<3) + TI_GDT + RPL0

;以下是定义gdt的指针，前2字节是gdt界限，后4字节是gdt起始地址
gdt_ptr  dw  GDT_LIMIT 
	dd  GDT_BASE
loadermsg db '2 loader in real.'


loader_start:
;------------------------------------------------------------
;INT 0x10    功能号:0x13    功能描述:打印字符串
;------------------------------------------------------------
mov	 sp, LOADER_BASE_ADDR
mov	 bp, loadermsg
mov	 cx, 17
mov	 ax, 0x1301
mov	 bx, 0x001f
mov	 dx, 0x1800
int	 0x10


;----------------- Open A20  ----------------
in al,0x92
or al,0000_0010B
out 0x92,al

;-----------------  Set GDT  ----------------
lgdt [gdt_ptr]

;-----------------  Set PE ----------------
mov eax, cr0
or eax, 0x00000001
mov cr0, eax

;jmp dword SELECTOR_CODE:p_mode_start ; 刷新流水线
jmp  SELECTOR_CODE:p_mode_start	     ; 刷新流水线和cs


[bits 32]
p_mode_start:
   mov ax, SELECTOR_DATA
   mov ds, ax
   mov es, ax
   mov ss, ax
   mov esp,LOADER_STACK_TOP
   mov ax, SELECTOR_VIDEO
   mov gs, ax

   mov byte [gs:320], 'P'

   jmp $
```
