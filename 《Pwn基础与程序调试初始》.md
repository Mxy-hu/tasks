# Pwn 基础实验报告
## 从简单 C 程序到二进制分析与调试

---

## 1. 简单 C 程序源码
#include <stdio.h>

// 自定义函数：计算两个整数的和
int add(int a, int b) {
    int sum = a + b;   // 局部变量 sum
    return sum;
}

int main() {
    int x = 5;         // 局部变量 x
    int y = 10;        // 局部变量 y
    int result = add(x, y);  // 函数调用，参数传递
    printf("Result: %d\n", result);
    return 0;
}
## 2.编译与·运行
gcc -g -o pwn_demo pwn_demo.c   # -g 添加调试信息
./pwn_demo
输出
Result: 15
## 3.GDB单步调试命令及关键观察
启动调试
gdb ./pwn_demo
常用调试命令序列
(gdb) break main          # 在 main 设置断点
(gdb) run                 # 运行
(gdb) disassemble main    # 查看 main 的汇编（可选）
(gdb) step                # 单步进入（或 next 跳过）
(gdb) step                # 进入 add 函数
(gdb) info registers      # 查看寄存器
(gdb) backtrace           # 查看调用栈
(gdb) frame               # 查看当前帧信息
(gdb) info locals         # 查看局部变量
(gdb) x/10gx $rsp         # 查看栈内存
关键观察点：

参数传递：x86_64 System V ABI 中，第一个参数在 %rdi，第二个在 %rsi。

call add 将返回地址压栈。

函数序言：push %rbp；mov %rsp, %rbp。

局部变量 sum 分配在栈上（sub $0x10, %rsp 等）。

返回值存于 %rax，函数尾声执行 leave 和 ret。
## 4.栈和寄存器的变化记录
## 5.ELF文件基本信息查看
### 5.1使用readelf-h查看ELF头
readelf -h pwn_demo
示例输出:
ELF Header:
  Magic:   7f 45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00
  Class:                             ELF64
  Data:                              2's complement, little endian
  Version:                           1 (current)
  OS/ABI:                            UNIX - System V
  ABI Version:                       0
  Type:                              EXEC (Executable file)
  Machine:                           Advanced Micro Devices X86-64
  Version:                           0x1
  Entry point address:               0x401030
  Start of program headers:          64 (bytes into file)
  Start of section headers:          13904 (bytes into file)
  Flags:                             0x0
  Size of this header:               64 (bytes)
  Size of program headers:           56 (bytes)
  Number of program headers:         13
  Size of section headers:           64 (bytes)
  Number of section headers:         31
  Section header string table index: 30
  ### 5.1使用objdump-d反汇编
  objdump -d pwn_demo | less
  ### 5.3使用readelf-s查看节区
  readelf -S pwn_demo
  ## 6.程序保护制检查
  checksec ./pwn_demo
  示例输出：
  [*] '/home/user/pwn_demo'
    Arch:     amd64-64-little
    RELRO:    Partial RELRO
    Stack:    No canary found
    NX:       NX enabled
    PIE:      PIE enabled
    保护机制说明：

RELRO：Partial RELRO – 部分 GOT 表只读，仍存在覆写风险。

Stack Canary：未启用 – 栈溢出可能直接覆盖返回地址。

NX：启用 – 数据页不可执行，需 ROP 绕过。

PIE：启用 – 程序基址随机化，需地址泄露。
## 7.学习笔记
### 7.1引言
Pwn（二进制漏洞利用）的核心在于理解程序在内存中的布局和执行流程。掌握汇编、栈帧、寄存器、ELF 文件格式及保护机制是入门的关键。本实验通过一个极简的加法程序，逐步演示编译、调试、观察栈/寄存器变化以及分析可执行文件属性，为后续漏洞挖掘打下基础
### 7.2程序与编程
源码分析：pwn_demo.c 包含 main 和 add 两个函数，演示了参数传递、局部变量、返回值等基本概念。

编译选项：使用 gcc -g 生成带调试符号的可执行文件，便于 gdb 关联源码和单步调试。
### 7.3调试过程与关键观察
设置断点：在 main 和 add 函数入口打断点，单步执行。

寄存器观察：使用 info registers 查看通用寄存器。注意 x86_64 调用约定：

参数 1～6 依次存于 %rdi, %rsi, %rdx, %rcx, %r8, %r9。

返回值存于 %rax。

栈帧变化：

call 指令压入返回地址。

函数序言（prologue）保存 %rbp，分配局部变量空间。

函数尾声（epilogue）恢复栈帧，ret 弹出返回地址。

内存查看：通过 x/10gx $rsp 查看栈内容，可直观看到参数、返回地址、局部变量的存储位置。
### 7.4ELF文件结构
ELF 头：使用 readelf -h 查看，包括魔数、架构、入口点、程序头和节头偏移等信息。

反汇编：objdump -d 可查看程序的机器指令，便于结合源码分析指令流。

节区信息：readelf -S 列出所有节区（.text 代码段、.data 数据段、.debug_* 调试信息等）。
### 7.5安全保护机制
checksec 是快速检查二进制安全属性的工具：

RELRO：Partial RELRO – 允许某些 GOT 条目在运行时修改（存在 GOT 覆写风险）。

Stack Canary：未启用 – 栈溢出可能直接覆盖返回地址，利于攻击。

NX：启用 – 栈或堆不可执行，需要 ROP 等技巧绕过。

PIE：启用 – 程序基址随机化，需要结合信息泄露绕过 ASLR。
### 7.6对Pwn学习的启示
调试能力：熟练使用 gdb 是必备技能，尤其是 step、next、info reg、x、disassemble、break 等命令。

内存视角：将高级语言代码映射到栈、寄存器，是理解缓冲区溢出、格式化字符串等漏洞的前提。

保护机制对抗：不同保护组合影响利用方法（如 NX + PIE 需要 ROP + 地址泄露）。

工具链：gcc、gdb、readelf、objdump、checksec 构成了二进制分析的基础工具链，应熟练运用
### 7.7下一步探索方向
修改程序引入缓冲区溢出，观察栈破坏后的行为及控制流劫持。

学习 ROP（返回导向编程）构造，绕过 NX 保护。

了解动态链接与 GOT/PLT 表，实践 GOT 覆写攻击。
