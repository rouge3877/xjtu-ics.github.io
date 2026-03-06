# XJTU-ICS Lab 1: Data Lab

## 实验简介

相信大家在前几节课中学习中已经学到了不少东西，这个小实验的目的是让大家更加熟悉计算机中信息表示的常见模式和整数的位级表示。你将通过解决一系列编程“谜题”来做到这一点。其中许多谜题可能看起来构造的非常刻意（human-made），但你会发现自己在处理它们的过程中更多地考虑了计算机中比特的表示法。

这些题目并不简单，但是相信在尝试求解它们的过程中你能感受到一些乐趣，并提升一些动手与编程能力。

Enjoy and Have fun! 😉

## 注意事项

- 这是一个**个人**作业，请不要抄袭或者尝试几个人一起完成.
- 这些问题并不是一些完全全新的问题，我们鼓励“Google”学习解决一些代码之外的问题（环境问题/工具使用问题），但是频繁的查询代码谜题相关的解将使得这个实验与课程失去意义.
- **不要使用各种AI Agent工具辅助开发**

## 开发环境和实验资源获取

仔细阅读[Lab0: Enviroment Setup](lab0.md)，根据自己的需求准备好linux开发环境和必备工具链（gcc，make，git，gdb等）。

### 远程开发

已经拥有ICS-Server账号的同学，在自己的家目录下（`/home/username`），应该可以看到一个名为`datalab-sp26`的目录：

如果你缺少这个目录，有三种解决方式：

- 在课程网站上下载[datalab-sp26.tar](../assets/files/datalab-sp26.tar)，然后自行解压（不推荐）
- 进入本次实验的[公有仓库](https://github.com/xjtu-ics/datalab-sp26)，使用`git clone`克隆仓库到本地（推荐）
- 寻找助教进行解决（不推荐）

当然，助教们希望同学们还是尽量可以自己解决，毕竟一方面助教们平时都很忙，另一方面自己动手查阅资料，解决问题也可以培养自己这方面的能力，这也是这门课开设的初衷。

### 本地开发

本地搭建实验环境的同学需要自己获取实验资源，方式如下：

- 在课程网站上下载[datalab-sp26.tar](../assets/files/datalab-sp26.tar)，然后自行解压（不推荐）
- 进入本次实验的[公有仓库](https://github.com/xjtu-ics/datalab-sp26)，使用`git clone`克隆仓库到本地（推荐）
- 寻找助教进行解决（不推荐）

如果你已经顺利的在本地安装或者申请到了一个稳定的Linux环境，并且准备好了实验资源，那么就可以顺利的开始你的实验。

### 实验数据备份

不论是哪种开发方式，实验环境发生崩溃是非常常见的事情，因此我们**强烈建议**各位同学在做实验的时候使用git关联一个远程仓库（如github）进行备份，随时更新并同步。

有关git的使用和快速上手可以参考[textbook-git](https://xjtu-ics.github.io/textbook/dev/git.html)

## 实验前置准备

### 检查文件完整性

本实验所有资源位于`datalab-sp26`目录下，进入这个目录，运行`ls`查看该目录下的所有内容：

```bash
bits.c  bits.h  btest.c  btest.h  decl.c  dlc  Driverlib.pm  driver.pl  fshow.c  ishow.c  Makefile  tests.c
```

!!!danger
    如有**缺失文件**等问题请及时询问同学或者找助教解决相关问题。

### 尝试构建

进入`datalab-sp26`目录，运行以下命令：

```bash
make
```

如果`make`成功，一个典型的输出结果如下所示：

```bash                         
gcc -O -Wall -m32 -lm -o btest bits.c btest.c decl.c tests.c
gcc -O -Wall -m32 -o fshow fshow.c
gcc -O -Wall -m32 -o ishow ishow.c
```

!!!note
    在make的过程中可能会出现一些warning，这里可以直接忽略

### 错误处理

一些可能出现的典型环境问题和解决方法将在附录中做简要解释，一个比较推荐的通用的问题解决方法的路径是：

- 查看报错，根据报错分析产生原因，可以通过google或者各种AI工具等
- 若无法成功解决则咨询助教或登录[Piazza](https://piazza.com/stu.xjtu.edu.cn/spring2026/xjtuics)询问

### 尝试运行

如果成功`make`此时你的实验目录将会变成如下：

```bash
bits.c  bits.h  btest  btest.c  btest.h  decl.c  dlc  Driverhdrs.pm  Driverlib.pm  driver.pl  fshow  fshow.c  ishow  ishow.c  Makefile  tests.c
```

简单验证可用性，我们简单运行一下`make`之后获得可执行文件`btest`，输入以下命令：

```bash
./btest
```

此时若运行成功，则会出现：

```bash  
Score   Rating  Errors  Function
ERROR: Test tmin() failed...
...Gives 2[0x2]. Should be -2147483648[0x80000000]
ERROR: Test upperBits(0[0x0]) failed...
...Gives 2[0x2]. Should be 0[0x0]
ERROR: Test sign(-2147483647[0x80000001]) failed...
...Gives 2[0x2]. Should be -1[0xffffffff]
ERROR: Test copyBit(-2147483648[0x80000000],0[0x0]) failed...
...Gives 2[0x2]. Should be 0[0x0]
ERROR: Test fitsBits(-2147483648[0x80000000],1[0x1]) failed...
...Gives 2[0x2]. Should be 0[0x0]
ERROR: Test anyOddBit(-2147483648[0x80000000]) failed...
...Gives 2[0x2]. Should be 1[0x1]
ERROR: Test distinctNegation(-2147483648[0x80000000]) failed...
...Gives 2[0x2]. Should be 1[0x1]
ERROR: Test isGreater(-2147483648[0x80000000],-2147483648[0x80000000]) failed...
...Gives 2[0x2]. Should be 0[0x0]
ERROR: Test isAbsEqual(-2147483648[0x80000000],-2147483648[0x80000000]) failed...
...Gives 2[0x2]. Should be 1[0x1]
ERROR: Test howManyBits(-2147483648[0x80000000]) failed...
...Gives 0[0x0]. Should be 32[0x20]
Total points: 0/80
```

出现如上或类似如上的输出结果，恭喜你！这证明您的本地环境已经准备好了，之后就可以快乐的开始Coding。

## 开始实验

本次实验中，唯一需要**更改与提交的代码文件**就是**bits.c**，其余文件都是为了辅助本次实验。

`bits.c`中已经详细说明了实验的步骤，以及各种代码的规则，实验开始前请大家仔细阅读`bits.c`的**顶部注释**部分：

```c
/* 
 * CS:APP Data Lab 
 * 
 * <Please put your name and userid here>
 * 
 * bits.c - Source file with your solutions to the Lab.
 *          This is the file you will hand in to your instructor.
 *
 * WARNING: Do not include the <stdio.h> header; it confuses the dlc
 * compiler. You can still use printf for debugging without including
 * <stdio.h>, although you might get a compiler warning. In general,
 * it's not good practice to ignore compiler warnings, but in this
 * case it's OK.  
 */

#if 0
/*
 * Instructions to Students:
 *
 * STEP 1: Read the following instructions carefully.
 */

You will provide your solution to the Data Lab by
editing the collection of functions in this source file.

INTEGER CODING RULES:

....
....
....

#endif

```

`#if 0`与`#endif`中的注释部分已经把这个实验要做什么以及怎么做写的很清楚了，这里只做一下总结。

### 实验要求解释

#### 代码规则

`bits.c`文件包含了针对10个编程谜题的基本框架。在这个实验中你的任务就是在一个严格的Coding Rules（代码编写规则）之下完成每个函数编写（一个函数就代表一个小谜题），即用一行或多行C代码替换每个函数中的“return”语句，以实现该函数，这个严格的代码代码编写规则如下：

假设你写的函数格式如下：

```c
int Funct(arg1, arg2, ...) {
    /* brief description of how your implementation works */
    int var1 = Expr1;
    ...
    int varM = ExprM;

    varJ = ExprJ;
    ...
    varN = ExprN;
    return ExprR;
}
```

每个"expr"**只能包含**以下这些：

- 整数常量`0`到`255`（`0xFF`），不允许使用大的常量如`0xffffffff`。
- 函数参数和局部变量（不允许使用全局变量）。
- 一元整数操作符 `! ~`
- 二元整数操作符 `& ^ | + << >>`
- 任意数量的括号

!!!note
    每个谜题（函数）进一步限制了运算符的个数，详细的规则在`bits.c`中每个函数上方的注释中有明确的说明。**注意，如果不按照运算符个数实现代码将无法拿到当前题目的“性能分”**(关于评分细则在后面评分中有明确说明)。

**明确禁止使用的操作包括：**

- 控制结构如`if, do, while, for, switch`等。
- 使用宏定义。例如`#define AND(a, b) ((a) & (b))`是不允许的。
- 在此文件中定义任何额外的函数。
- 调用任何函数。（我们在`bits.c`中包含了`printf`以供大家`debug`使用，其余只允许使用`C`语言基础的运算符进行求解，最终提交的代码请不要包含`printf`）
- 使用其他操作符，如`&&, ||, -, ?, :`，每个谜题的上方注释处都有明确的合法操作符种类（Legal Ops）。
- 使用任何形式的类型转换。
- 使用除`int`之外的任何数据类型，同时你不能使用数组、`struct`或`union`。
- 不允许使用`include`添加新的库。

#### 可以确定的假设

在编写代码的时候，你可以建立如下假设：

- 数据类型`int`的值为`32`位。
- 带符号数据的右移以算术右移方式执行。
- 对于`int`数据类型，如果移位量小于`0`或大于`31`时，行为不可预测，也就是说当移位运算时，移动量应该在`0` 和 `31` 之间。

#### 注意事项

- 每个函数的得分由两部分组成，分别是**正确性得分**和**性能得分**
- 每个函数只有**正确实现并符合代码规则**后，才能获得**正确性得分**，**否则函数为0分**。此外，每个函数都有最大操作符数量(`Max Ops`)，如果超过使用的最大操作符数量，将无法得到**性能得分**。这两个要求均可以通过`dlc`进行检查
- 具体测试方法将在后文详细解释

!!! warning
    由于dlc的一些实现细节所限，大家如果需要声明局部变量，**请务必将所有局部变量的声明语句放在代码块开始之前**，否则dlc会出现误判。

### 根据代码规则修改`bits.c`中的函数

本小节将通过一个具体的例子来解释这个实验到底需要做些什么。

`bits.c`中的每个小的谜题都会有谜题本身的限制，限制的具体内容写在了谜题函数上面的注释格式中。一个典型的谜题例子如下：

```c
/* 
 * minusOne - return a value of -1 
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 2
 *   Rating: 1
 */
int minusOne(void) {
  return 1;
}
```

如上题

- 题名：`minusOne`
- 题目要求：`return a value of -1` ，返回一个`-1`
- 合法的操作符：`! ~ & ^ | + << >>`
- 最多使用的操作符数量：2个
- 题目等级：1星

因此我们编写如下的程序代码：

```c
int minusOne(void) {
  return ~0;
}
```

如果在代码中使用了非法的操作符`-`，例如：

```c
int minusOne(void) {
  return -1;
}
```

尽管结果正确（使用`btest`测试没问题），这个谜题仍是不得分的。同样，如果在函数中使用了`if for`或`0xffffffff`，以及有任何规则禁止的操作，都是不得分的。

### 题目概览

这个小节简要展示了你将要解决的10道谜题，同时你可以参考这些谜题的**标准解法**用到的操作符数量：

```text
Correctness Results     Perf Results
Points  Rating  Errors  Points  Ops     Puzzle
8       1       0       2       1       tmin
8       1       0       2       9       upperBits
8       2       0       2       4       sign
8       2       0       2       3       copyBit
8       2       0       2       7       fitsBits
8       2       0       2       7       anyOddBit
8       2       0       2       3       distinctNegation
10      3       0       2       12      isGreater
10      3       0       2       7       isAbsEqual
4       4       0       2       37      howManyBits

Score = 100/100 [80/80 Corr + 20/20 Perf] (90 total operators)
```

!!!warning
    如果发现自己的题目和上述有出入，请立刻联系助教处理！

### 本地测试

本节通过一个典型例子详细介绍代码的测试方式

#### 正确性检查

可以使用`btest`检查代码的功能正确性。

首先运行：

```bash
make
```

在实现目录下，将会生成一个可执行文件`btest`，然后输入：

```bash
./btest
```

一个典型的结果为：

```bash
Score   Rating  Errors  Function
 8      1       0       tmin
 8      1       0       upperBits
 8      2       0       sign
ERROR: Test copyBit(-2147483648[0x80000000],0[0x0]) failed...
...Gives 2[0x2]. Should be 0[0x0]
 8      2       0       fitsBits
 8      2       0       anyOddBit
 8      2       0       distinctNegation
 10     3       0       isGreater
 10     3       0       isAbsEqual
 4      4       0       howManyBits
Total points: 72/80
```

如上输出中告诉你每一题的得分，如果你的程序出错，则会输出对应的出错的题目与出错时候的输入和输出。

如上中，`btest`告诉我们：

Test copyBit测试用例出差错，其中：

- 出错的输入是：`-2147483648[0x80000000]`和`0`（对应两个函数参数）
- 你的输出是：`2[0x2]`
- 正确的解应该是`0[0x0]`

根据输出的结果我们返回去继续修改我们的代码。

!!!note
    注意，每次修改了`bits.c`之后，都需要先运行`make`进行编译，否则`./btest`测试的是旧版的结果

#### 代码规则检查

`btest`并不会告诉我们写的代码是不是符合代码规则，这时候就需要使用`dlc`，输入如下命令：

```bash
./dlc bits.c
```

如果你的代码都符合代码规则，那不会输出任何结果。

如果你的代码中有不符合代码规则的地方，一个典型的输出结果如下所示：

```bash
dlc:bits.c:204:copyBit: Illegal operator (==)
dlc:bits.c:205:copyBit: Illegal if
dlc:bits.c:206:copyBit: Warning: 10 operators exceeds max of 8
```

如上，`dlc`告诉我们:

- 在204行，`copyBit`函数中，使用了非法的操作符`==`。
- 在205行，`copyBit`函数中，使用了非法的操作符`if`。
- 在206行，`copyBit`函数中，使用了`10`个操作符，超过了`Maxops=8`个。

根据输出的结果我们返回去继续修改我们的代码。

!!!note
    `dlc`同时还有很多功能，例如`./dlc -e bits.c`等，你可以通过`./dlc -h`输出的帮助信息去自行了解。

#### 实验最终得分

为了计算出自己的最终分数，我们提供了`driver.pl`来计算。

`driver.pl`的使用方法如下：

```bash
./driver.pl
```

一个典型的结果如下：
```bash
1. Running './dlc -z' to identify coding rules violations.

2. Compiling and running './btest -g' to determine correctness score.
gcc -O1 -g -Wall   -lm -o btest bits.c btest.c decl.c tests.c 

3. Running './dlc -Z' to identify operator count violations.

4. Compiling and running './btest -g -r 2' to determine performance score.
gcc -O1 -g -Wall   -lm -o btest bits.c btest.c decl.c tests.c 

5. Running './dlc -e' to get operator count of each function.

Correctness Results     Perf Results
Points  Rating  Errors  Points  Ops     Puzzle
8       1       0       2       1       tmin
8       1       0       2       9       upperBits
8       2       0       2       4       sign
0       2       1       0       0       copyBit
8       2       0       2       7       fitsBits
8       2       0       2       7       anyOddBit
8       2       0       2       3       distinctNegation
10      3       0       2       12      isGreater
10      3       0       2       7       isAbsEqual
4       4       0       2       37      howManyBits

Score = 90/100 [72/80 Corr + 18/20 Perf] (87 total operators)

```
可以看到，最终得了90分，其中`copyBit`函数由于不符合代码规则，不得分。

!!!warning
    `btest`的得分不代表最终得分，如果使用了非法操作符，即使`btest`可以得分，最后这个函数也会因为无法通过`dlc`的检查而得0分。本地测试和提交后的最终得分均以`driver.pl`的结果为准！

### 进制转换工具

实验资源中提供了相应的便捷工具`ishow`，可以使用它查看整数的十进制和十六进制表示形式。首先编译代码如下：

然后使用它来检查在命令行中键入的十六进制和十进制值：

```bash
./ishow 0x80000000
```

一般输出为：

```bash
Hex = 0x80000000,       Signed = -2147483648,   Unsigned = 2147483648
```

它会帮助你完成进制的转换。

## 评分和提交

### 代码提交

在`datalab-sp26` 目录下执行`make submit`

```bash
make submit
```

目录下都会生成一个名为 `<userid>-datalab-handin.zip `的文件（其中 `<userid> `为`Linux`系统中你的用户名）。（每次修改代码后，记着要重新运行`make submit`）

在[在线学习平台](https://class.xjtu.edu.cn/course/115939/learning-activity/full-screen#/5477056)上的作业模块中，将该文件作为附件提交即可。

### 评分标准

在每个谜题的描述部分，还有一行`Ratings`，这代表了题目的难度，同时也决定了如何评分。

谜题的`Ratings`分为1星，2星，3星和4星共四个等级。星级越高，难度越高，不同星级的题目难度可以概括为：

- 1星和2星题，只需要很简单的逻辑和少数几个操作符就可以完成
- 3星题，增加了思维难度，但是不需要大量的操作符（实现简单）
- 4星题，具有最高的思维难度或大量的操作符（实现复杂）

不过同学们也不用担心，我们为大家准备的10道题目包含了2道1星题，5道2星题，2道三星题和1道四星题，保证同学们可以很轻松的拿到一个基本分数。

你的最终得分的计算规则如下（共100分）：

- 1星和2星题，每题8分
- 3星题，每题10分
- 4星题，每题4分

因此，10道题的总分按照上述规则加起来一共80分，如果你可以通过`btest`的检查，并且代码符合上文提到的代码规则（即可以通过`dlc`的检查），你将得到基本的80分。

同时谜题的**最大操作符数量也是有限制**的，如果你可以实现在最大操作符数量之内完成函数，每个函数得2分，如果你可以保持正确性并且操作符数量在最大操作符数量之内，你将获得剩余的“性能分”20分。

### 迟交

在超过原定的截止时间后，我们仍然接受同学的提交。此时，在lab中能获得的最高分数将随着迟交天数的增加而减少，具体服从以下给分策略：

超时7天（含7天）以内时，每天扣除3%的分数
超时7~14天（含14天）时，每天扣除4%的分数
超时14天以上时，每天扣除7%的分数，直至扣完
以上策略中超时不足一天的，均按一天计，自ddl时间开始计算。届时在线学习平台将开放迟交通道。

评分样例：如某同学小H在lab中取得95分，但晚交3天，那么他的最终分数就为`95*(1-3*3%)=86.45`分。同样的分数在晚交8天时，最终分数则为`95*(1-7*3%-1*4%)=71.25`分。

## 写在最后

- **Start Early**
- 谜题可能比较复杂，可能一时间不可以直接想出优秀的满足要求的解法，但是不要气馁，没有人可以马上给出最优的解法。一个比较好的解题尝试路径是：
    - 先尝试在纸上写出一般的解法
    - 从一般解中找出更一般性的规律，这些规律可能通常需要你的灵光一现，但是对相信聪明的你来说这不是问题。
- 如果你是采用自己本地的环境，那么总会出现一些依赖缺失，软件未安装等等之类的错误。请大家不要慌张，一个好的程序员需要更好的了解自己手里的工具，而自己搭建环境往往就是其中很重要的一部分，请大家自行查询相关环境问题，自己动手解决问题（折腾）的能力在计算机专业的学习中是非常重要的。
- 找到适合自己的好工具的能力：请大家在不断地动手中学习并找到适合自己的工具，以提高自己的效率。

这是XJTU ICS课程的第一个正式Lab，难度不会太大，ICS团队希望的是循序渐进，逐步培养的是大家的动手能力，希望同学们可以慢慢熟悉ICS课程的强度和节奏，准备好迎接后续的挑战！ 😊

---

Copyright © 2026 XJTU ICS-TEAM

