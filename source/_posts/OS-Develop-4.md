---
title: OS Develop 4
date: 2022-04-15 19:34:01
tags:
   - IT-Basics
   - OS
---

### 简介
本章讨论MBR。
MBR被写在硬盘的0盘0道1扇区。MBR大小是512字节，这是为了保证0x55和0xaa这两个魔术恰好出现在该扇区的最后两个字节。MBR受限于512字节大小限制，没法完成初始化环境和加载内核的任务，所以我们需要另外一个程序来继续完成这两项任务，即loader。


### Code
```nasm
SECTION MBR vstart=0x7c00         
mov ax,cs      
mov ds,ax
mov es,ax
mov ss,ax
mov fs,ax
mov sp,0x7c00


mov ax, 0x600
mov bx, 0x700
mov cx, 0
mov dx, 0x184f
int 0x10


mov ah, 3
mov bh, 0
int x010

mov ax, message
mov bp, ax
mov cx, 5
mov ax, 0x1301
mov bx, 0x2
int 0x10

jmp $

message db "1 MBR"
times 510-($-$$) db 0
db 0x55, 0xaa
```

### 结束
MBR将磁盘中0盘0道2扇区的loader加载到内存的0x900处，然后通过jmp LOADER_BASE_ADDR，将执行权交接给loader。
