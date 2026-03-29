# XJTU-ICS Lab 4: Cache Lab

> *"Raise your quality standards as high as you can live with, avoid wasting your time on routine problems, and always try to work as closely as possible at the boundary of your abilities. Do this, because it is the only way of discovering how that boundary should be moved forward."*

> *— Edsger W. Dijkstra*

## 前言

Welcome to Cache Lab 😉

开课之前，助教组对修过 2025 年 ICS 课程的同学做了一次小调研。当被问到"哪个 Lab 让你印象最深"时，几乎所有人都不约而同地提到了 Cache Lab——对亲手实现三级 Cache 模拟器的难度，至今心有余悸。

这并不奇怪。相较于前三个 Lab 对计算机基础知识（位表示、GDB 调试、逆向分析）的考察，Cache Lab 更侧重于工程代码能力的锤炼：**如何写出一个简洁、优雅且 bug-free 的程序**，是这个实验最核心的挑战。

也许这是你第一次面对真正具有工程复杂度的编程任务；也许你会被一次次的 Error 折磨到夜不能寐；也许你曾动过"走捷径"的念头——但请记住，**big brother is watching you** 🫵

然而有意思的是，当被追问"为什么对 Cache Lab 印象深刻"时，许多同学不约而同地提到了另一个词：**成就感**。

或许是完全理解三级 Cache 运行机制后的豁然开朗，或许是第一次独立写出大型 bug-free 程序时的如释重负，又或许是看着自己精心设计的代码优雅地 scale 起来时难以言说的满足。

我们希望你也能体验到这种成就感。为此，我们希望：

- 独立完成这个实验，不借助任何代码生成工具（Claude Code、Codex 等）
- 深入理解三级 Cache 的工作原理及其对程序性能的影响，能够权衡不同访问方式的 trade-off
- 在实现模拟器的过程中，感受到精心设计所带来的简洁之美
- 完成后，你可以自豪地将"独立实现三级 Cache 模拟器"写入简历，并有能力将其扩展至更复杂的场景（如多核一致性）

最重要的是，我们希望这段经历能成为你的一种底气。当你在未来的学习或工作中，遭遇更复杂、更晦涩的工程挑战时——无论是十万行的代码库，还是从未见过的系统架构——完成 Cache Lab 的记忆会告诉你：**你曾经做到过**☺️

现在，带着对难度的敬畏，带着对自己的信心，出发吧。

## 实验简介

这个实验的目的是为了让大家在学习课堂理论知识的基础上，更好地理解Cache的运行过程以及Cache对于程序运行性能的影响。

本次实验由两部分构成：

- Lab4A，总共500分。Part A要求大家使用C语言实现一个**三级Cache模拟器**。实现模拟器并逐渐做到Bugfree的过程会让你加深对Cache基本结构，多级cache的设计和访问方式以及系统设计中的trade-off的理解。

- Lab4B，总共100分。Part B要求大家**优化矩阵转置的函数**，这个部分的目标是使我们的代码跑的尽可能的快。不断减少Cache Miss次数的过程会帮助你理解Cache对程序运行性能的重要作用。

!!!warning
    Lab4A和Lab4B都是必做实验

最终得分的计算公式为：
$score_{total} = score_{Lab4A} \div 5 \times {0.8} + {score_{Lab4B}} \times {0.2}$



## 注意事项

- 这是一个 **个人** 作业，请大家独立完成。

- 实验可能比较有难度，please **start early** and **ask more**

- 上交的代码文件请注意一定不要有编译问题。**0 Warnings的程序**（那肯定更不能有error）才会帮助你顺利获得属于你的分数。

- **不要在piazza上寻求同学或者直接私聊助教帮你debug**，有关要求帮忙debug的内容我们有权利不回答。

- **禁止使用打表的方式完成实验**，一旦被发现，你将收到课程组准备的"惊喜" ☠️

- 不要抄袭同学的代码，本次实验会有**严格的查重机制**

- 文本中使用的**较高级cache**指**靠近CPU**的cache，而**较低级cache**指**靠近内存**的cache。比如，对于L2和L3 cache，则L2是较高级cache, L3是较低级cache

## 实验环境准备

前置要求：学习完[lab0](./lab0.md)，**不要尝试在windows环境下完成实验**

Linux is all you need

### 远程开发

???todo
    更新获取文件方式@blowinding

使用远程开发的同学，可以登陆任意一台ics服务器完成实验，成功登陆之后在自己的**家目录**
底下可以看到名为**cachelab-sp25**的目录。进入目录，就可以开始实验啦~

如果你缺少这个目录，有下面几种解决方式

- 下载实验压缩包[cachelab-sp25.tar](../assets/files/cachelab-sp25.tar)
- 访问本次实验的[公有仓库](https://github.com/xjtu-ics/cachelab-sp25)并clone到本地（推荐）
- 询问助教或者同学发给你（不推荐）

### 本地开发

本次实验也可以使用本地开发的方式进行，使用本地开发请确保环境中有以下工具

- gcc
- make
- gdb (也许有用)

实验材料的获取方式：

- 下载实验压缩包[cachelab-sp25.tar](../assets/files/cachelab-sp25.tar)
- 访问本次实验的[公有仓库](https://github.com/xjtu-ics/cachelab-sp25)并clone到本地（推荐）
- 询问助教或者同学发给你（不推荐）

## 实验前置知识

本实验与Cache强相关，所以我们首先从总体上带大家来梳理一下Cache相关的知识。

这部分包含两个模块，第一个模块带大家复习cache基本结构和一些基本统计量，包括cacline line的结构，set的组织方式，cache hit/miss的定义等等。

第二部分简要讲解有关多级cache的组织方式，以及访问流程等等。

如果你认为自己对于这部分的内容已经完全掌握了，可以跳过这一节，直接开始实验~ 💪

### Cache基础知识

一般而言，Cache是一个**小而快速**的存储设备，它负责存放下一级**更大、更慢**的存储设备的数据的子集（关于子集的要求其实也不严格，具体见后文**多级cache的包含准则**）。使用cache的过程称为Caching。Cache的原理图如下：

![cache Hierarchy](../assets/images/cachelab_cache_memory.png)

!!!note
    虽然课程中主要讲述的是**CPU中的cache（高速缓存）**，其作为**内存的cache**，但cache实际上是一个十分宽泛的概念，任何小而快的存储设备都可以看作是更大且更慢的设备的cache，其核心是利用**空间局部性**和**时间局部性**。比如内存实际上可以作为磁盘的cache，这在后续的虚拟内存章节也会更加细致的讲述，亦或是使用Redis作为数据库的cache。总而言之，cache的概念在系统设计中非常重要，需要同学们深入理解和掌握。

### Cache 基本结构

在cache中，最基本的单位是cache line。任何不同的cache结构，都是将cache line以不同的方式组织起来，其中主要包括**组相联（set associative）**，**直接映射（direct mapped）**和**全相联（fully associative）**。其中后两种组织方式都可以看作第一种组织方式的特殊形式。下面，我们将围绕这些部分重点进行讲解。

### Cache Line

每个Cache Line由三部分构成：

- 有效位(valid bit)：指示Cache Line是否有效。Cache Line有效意味着它存储着来自主存的数据块；否则Cache Line的其他信息都是无效的，应该被忽略。
- 标记位(tag bit)：帮助确定Cache Line中存储的数据地址。
- 数据块(block)：主存中某个数据块的副本。

!!!note
    我们常说的cache hit，从cache line的角度来看实际上就是满足两点：

    - valid字段有效
    - tag字段匹配

Cache工作时，在Cache看来，主存的地址被分成三部分，如下图所示：

![cache memory addr](../assets/images/cachelab_cache_mem_addr.png)

其中，$s$位的Set Index字段（组索引）是$S$个Cache Set的索引。例如，第一个组的索引是$0$，第二个组的索引是$1$, 以此类推。Set Index字段告诉我们这个数据必须存在哪个Cache Set中。

$t$位的Tag字段是用来确定该Cache Line是否存储着目标地址的数据。

$b$位Block Offset是可以在确定目标数据在块中偏移量。

!!!note
    Cache Line是cache和cache以及cache和内存之间的**最小传输单位**，其大小一般指**block**，也就是数据块的大小，常见为64字节，当然也存在32字节或者128字节等，在实际的实现中，cache line内部的block和tag字段也通常是**两个独立的内存区域**。 
    
    正如课本在前面几章提到的，CPU内部的数据处理通常以**字**为单位，比如常见的64位CPU，其一次处理的数据大小就是64位，或者说字长为64位，也就是8字节。这也决定了CPU内部的寄存器宽度是64位（当然有些SIMD指令可能用到128或者256位寄存器）。 

    那为什么cache line的大小通常会大于CPU字长呢，一个原因是**空间局部性**，因为访问某个内存地址后，有很大概率会访问其附近的地址，因此将附近这一片连续的内存数据全部加载到cache中，对于良好的空间局部性的程序来说性能就可以得到很大的优化。另一个原因则是一次操作一块数据可以**均摊**诸如总线传输等带来的开销。对于CPU来说，如果要从cacheline中访问，通常是将需要的数据（通常8字节）从数据块中加载到寄存器中，再进行访问。对于cache和内存之间的交互，一般设计上会将cacheline和内存的**突发传输大小（burst length）**进行对齐，而这个大小一般是64字节。内存使用**突发传输数据方式**也是为了优化内存总线的带宽使用，这里不展开讲，感兴趣的同学可以自行google有关dram内存结构和时序的内容。

### Cache Line组织方式

一般而言，cache在逻辑上可以看作关于cache line组织而成的**二维数组**，这通常形成了最通用的**组相联（set associative）**结构，如下图所示：

![cache org](../assets/images/cachelab_memory_orgnization.png)

其中，这个二维数组的**行数**，决定了组相连的Set数量，课本中使用$S$表示。而每个组内部的cache line的个数，也就是二维数组的**列数**，决定了**相联度（associativity）**，课本上使用$E$表示。比如，一个组内包含了8个cache line, 那么通常叫做**8路组相联（8-way）**

上述的结构有两个发展的方向，因此也有两个极端，一个方向是增加行数，也就是Set的数量，另一个方向就是增加列数，也就是associativity。

假设Set数量只有一个，那实际上就形成了**全相联（fully associative）**结构，数据块可以放入cache中的任何位置，只要有空闲空间。

假设associativity的数量只有一个，即每个组内只有一个cacheline，这实际上就形成了**直接映射（direct mapped）**结构，即数据块被**唯一分配**到一个组内。

!!!note
    在计算机系统中，很多时候很难评价一种系统的好坏，因为某个系统在某个场景下可能性能很差，但是在另一个场景下可能非常适用，很多时候系统设计需要考虑的就是**trade-off**，即以牺牲一些其他方面的性能为代价，换取在某个方面极致的优化，从而**针对某个场景带来巨大的收益**。

    上述三种cache结构亦是如此，对于直接映射的方式，由于每个组内只有一个cache line，大大简化了cache查找和evict的逻辑，因此cache hit的时延可以很低，但同时由于只有一个cache line，cache miss率将大大提高。对于全相联结构，只要cache有空闲块，就可以存储数据，因此可以大大降低cache miss率，但同时也大大增加了查找和evict的复杂度，因此增加了访问时延。对于组相联结构，目标就是在二者之间寻找一个平衡。

### 如何处理写操作

cache处理写操作的流程比读取要复杂，因为写入操作涉及**数据的更改**，一旦涉及修改操作，就会带来各种一致性问题，因此cache需要合理的处理数据更改的时机和范围。同时还需要处理写miss的情况。我们在这里简要介绍一下有关写cache的一些问题和处理机制。

一般而言，对于写入操作，cache一般有两种处理机制，分别是：

- **write back（写回）**：即数据的修改只发生在当前这一级cache中，通常会引入一个**dirty**标记位，表示cache中的数据和下一级cache（或内存）中的数据**不一致**，只有在当前的cacheline被evict的时候才会将数据写回到下一级cache（或内存）。
- **write through（写直达）**：顾名思义，写入操作会同时将数据写入到当前cache和下一级cache（或内存中），因此二者的数据是同步的。

除了上述的两种策略，cache还需要确定如何处理write miss的情况，一般而言，也有两种方法：

- **write allocate（写分配）**：当发生cache miss时，需要访问下一级cache（或内存）将需要的cache line加载到当前cache中，然后再修改这个cache line中的内容
- **no write allocate（写不分配）**：当发生cache miss时，无需修改当前cache中的内容，直接写入下一级cache（或内存）

上述策略两两组合可以产生4种不同的写策略，但是一般常见的只有以下两种：

- **write back/write allocate**：即写回+写分配策略
- **write through/no write allocate**：即写直达+写不分配策略

大家可以自己思考一下为什么另外两种搭配方式不常见。🤔

有关write back/write allocate的写入流程如下（图源自[wiki: cache write policy](https://en.wikipedia.org/wiki/Cache_(computing))）：

![wbwa](../assets/images/Write-back_with_write-allocation.png)

有关write through/no write allocate的写入流程如下（图源自[wiki: cache write policy](https://en.wikipedia.org/wiki/Cache_(computing))）：

![wt](../assets/images/Write-through_with_no-write-allocation.png)

!!!note
    实际上，在现代的CPU中，几乎每一级cache都使用的是write back/write allocate策略，而write through策略只在早期AMD的CPU上的L1D cache使用过。

    write through的好处是简化了数据一致性的处理，但是会引发大量的总线流量，而访问内存又是一个比较慢的操作（相对于CPU来说），因此对于write through cache通常会额外引入一块write buffer，用来缓冲大量的写入流量。

    write back不会产生大量的总线流量，因此性能较好，但是会产生数据一致性问题，因此需要更复杂的机制来保证数据一致性。不过随着技术的发展，write back带来的收益已经超过write through带来的收益，因此write through已经逐渐淘汰了。

### Cache 相关统计量

![cache access](../assets/images/cachelab_cache_memory2.png)

#### Cache Hit

对于上面的图，如果程序需要Memory层的数据对象d，它会首先访问Cache，如发现Cache中恰好存在数据对象d（例如访问8），此时就叫Cache Hit（命中）。

当发生Cache Hit时，程序就不用访问主存了，直接从Cache读取目标数据即可。

#### Cache Miss

如果程序需要主存的数据对象d，它首先访问Cache，发现Cache中并没有存储数据对象d对应的数据块(block)，此时就叫Cache Miss (不命中)。

当发生Cache Miss时，Cache层会从主存中取出包含d的数据块，然后根据地址中的Set Index找到Cache中的对应Cache Set。如果这个Cache Set中还有空的Cache Line（即Valid Bit = 0），直接存入，并更新Tag字段和Valid位；否则，需要进行Cache Line替换，Cache Line替换需要覆盖一个块，所以也可以称为Cache Eviction。

#### Cache Eviction

上述Cache Line替换的过程是需要覆盖原有的一个块的，所以可以称为Cache Eviction。一个Cache Set中有多个Cache Line，替换时需要决定替换哪个Cache Line，这是由Cache替换策略决定的，例如常见的最近最少被使用策略(LRU策略)会替换掉一个访问时间距离现在最长的块。

### 多级cache组织方式
???todo
    感觉这里的组织逻辑不是很通顺，可以单独起一节来讲多级cache,包括后面两小节，需要具体优化@rouge3877

现代CPU中，CPU和内存的速度差距越来越大。因此CPU内部往往不会仅采用一级cache，而是使用多级cache结构，以**优化最坏情况（从内存访问）下的性能**。多级cache结构的组织又会引入很多的问题，我们在这里简单讲述一些重要的问题，其他的一些细节问题感兴趣的同学可以自行google。

### Memory Hierarchy

根据我们课程中的学习，不同存储技术的访问时间差别很大，但速度较快的技术每字节的成本要比速度慢的技术高，而且容量小。现代计算机系统往往采用存储器层次结构的方法，下图展示了一个典型的存储器层次结构。

![Memory Hierarchy](../assets/images/cachelab_memory_hierarchy.png)

存储器层次结构的中心思想是：对于每个k，位于k层的更快更小的存储设备作为位于k+1层的更大更慢的存储设备的cache。即每一层的cache的内容均来自于较低一层的数据对象。例如图中，L2 cache(SRAM)作为L3 主存的cache。

这样的体系结构，使得整体计算机的整体存储系统呈现出高速且大容量的整体特性。

### 多级cache的包含准则（inclusion policy）

!!!danger
    这一部分在课程中可能讲述较少，同时与实验内容**强相关**，因此强烈建议阅读本节。如果你有任何困惑，可以上piazza提问并讨论，或者自行google相关wiki和他人写的博客进行学习。

正如上述的内存层次结构所提到的，对于二级及以上的cache，一种常见的组织方式是较小且位于高层次的cache作为较大且位于低层次的cache的子集。即所有在高层次cache中存放的数据**一定**也存在于低层次中，但在低层次中拥有的数据则不一定在高层次中存在。

上述这种组织方式叫做**inclusive policy(包含性策略)**，这也是你需要在本次实验中实现的策略，当然还存在另外两种策略，分别是**exclusive policy**和**non-inclusive-non-exclusive(NINE)**策略，三种方式各有好坏，这些策略的集合通常叫做**inclusion policy**，即包含准则，这是设计实现多级cache时必须要确定的一个因素（你可以假设内存是一个最大的集合，包含所有数据）。

我们将简要介绍inclusive policy，如果你对其他策略感兴趣，可以自行查阅[wiki: cache inclusion policy](https://en.wikipedia.org/wiki/Cache_inclusion_policy)。

考虑一个二级缓存结构，这里借用[wiki: cache inclusion policy](https://en.wikipedia.org/wiki/Cache_inclusion_policy)上的一张图来表示：

![inclusive](../assets/images/Inclusive.png)

对于上述结构，如果要满足L2始终包括L1的所有数据这样一条性质的话，则在cache之间的访问需要满足以下要求：

- 如果L1 hit, 则直接返回
- 如果L1 miss, L2 hit, 则需要访问L2，并且将这个cache line复制到L1中
- 如果L1 需要evict一个cache line用来放置从L2得到cache line, 则evict过程不会对L2产生影响（实际上，还需要考虑写策略的影响，这里聚焦于inclusion policy，因此不考虑具体的写入策略）
- 如果L1和L2均miss，则需要访问内存，并且将cache line加载到**L2和L1中**
- 如果L2 需要而evict一个cache line，则L2 需要 **back invalidation（回溯失效）** L1，即在L1中寻找对应的cache line，并且将其evict。

大家可以自行想一下为什么对于**inclusive policy**，步骤4和5是必要的。😊

## 开始实验
???todo
    需要修改具体实验目录@blowinding

首先进入实验目录`cachelab-sp25`，然后运行`ls`查看文件：

```bash
$ cd cachelab-sp25
$ ls
cache-impl.c  csim.c           driver.py   test-trans.c   traces-data-intensive/
cachelab.c    csim-ref-partA*  Makefile    tracegen.c     traces-hard/
cachelab.h    csim-ref-partB*  test-csim*  traces-basic/  trans.c
```

你可以尝试运行`make`进行编译：

```bash
$ make
gcc -g -Wall -Werror -m64 -o csim csim.c cachelab.c cache-impl.c
gcc -g -Wall -Werror -m64 -O0 -c trans.c
gcc -g -Wall -Werror -m64 -o test-trans test-trans.c cachelab.c trans.o
gcc -g -Wall -Werror -m64 -O0 -o tracegen tracegen.c trans.o cachelab.c
```

此时再次运行`ls`，得到的输出如下：

```bash
$ ls
cache-impl.c  csim.c           Makefile      tracegen*               traces-hard/
cachelab.c    csim-ref-partA*  test-csim*    tracegen.c              trans.c
cachelab.h    csim-ref-partB*  test-trans*   traces-basic/           trans.o
csim*         driver.py        test-trans.c  traces-data-intensive/
```

下面是对于实验文件的详细解释：

- `cachelab.h/cachelab.c`：包含了本次实验的一些helper function
- `cache-impl.c`：cache模拟器的源文件，是在**Part A中唯一需要修改**的文件
- `csim.c`：cache模拟器的入口文件，助教们在这个文件中帮大家实现好了主要的处理逻辑
- `csim-ref-partA`：参考cache模拟器，可以与自己的实现进行比对，将在Part A中使用
- `test-csim`：用于Part A测试的文件
— `traces-*`：三个目录，用于存放在Part A中用于测试的文件
- `csim-ref-partB`：在Part B中使用的cache模拟器
- `trans.c`：Part B中你需要修改的文件
- `tracegen.c`：生成`tracegen`的源文件
- `tracegen`：Part B中用于生成trace的可执行程序
- `test-trans`：测试Part B部分的可执行文件
- `driver.py`：用于测试Part A + Part B总得分的脚本

## 评分方法

???todo
    修改评分策略@blowinding

- Part A 100分
- Part B 不计入总分

和往常一样，我们将最终用于评分的`driver.py`脚本也分发给了大家，大家可以用于快速自测分数，使用方法如下：

```shell
$ make
$ python3 driver.py
```

或者：

```bash
$ make
$ make grade
```

!!!note
    先`make`保证当前你的可执行文件已编译为你最新的提交版本

通常来说你会获得类似如下的输出：（这是一个满分输出）

```shell
$ make grade
Part A: Testing cache simulator
Running ./test-csim
Start testing basic traces...
Testcase                                     Lines     Result    Random    Score     
---------------------------------------------------------------------------------
traces-basic/l3evict.trace                   15        PASS      IGNORE    5/5       
traces-basic/mixed-2.trace                   90        PASS      IGNORE    5/5       
traces-basic/l1Dhit.trace                    4         PASS      IGNORE    5/5       
traces-basic/backinvalidation.trace          23        PASS      IGNORE    5/5       
traces-basic/mixed-1.trace                   40        PASS      IGNORE    5/5       
traces-basic/l1Devict.trace                  3         PASS      IGNORE    5/5       
traces-basic/l2evict.trace                   7         PASS      IGNORE    5/5       
traces-basic/l1missl2hit.trace               5         PASS      IGNORE    5/5       
traces-basic/l1Ihit.trace                    5         PASS      IGNORE    5/5       
traces-basic/l1Ievict.trace                  5         PASS      IGNORE    5/5       
traces-basic/mixed-3.trace                   128       PASS      IGNORE    5/5       
traces-basic/l1missl2missl3hit.trace         6         PASS      IGNORE    5/5       
---------------------------------------------------------------------------------
Total Score: 60 / 60
   12 passed,     0 failed,    12 total

Start testing data-intensive traces...
Testcase                                     Lines     Result    Random    Score     
---------------------------------------------------------------------------------
traces-data-intensive/multiply.trace         25347     PASS      IGNORE    3/3       
traces-data-intensive/add.trace              16451     PASS      IGNORE    3/3       
traces-data-intensive/convolve.trace         80397     PASS      IGNORE    3/3       
traces-data-intensive/sort.trace             8369      PASS      IGNORE    3/3       
traces-data-intensive/grep.trace             38328     PASS      IGNORE    3/3       
traces-data-intensive/inner_product.trace    16388     PASS      IGNORE    3/3       
traces-data-intensive/long.trace             267988    PASS      IGNORE    3/3       
traces-data-intensive/link_list.trace        49878     PASS      IGNORE    3/3       
traces-data-intensive/transpose.trace        6147      PASS      IGNORE    3/3       
traces-data-intensive/wc.trace               26311     PASS      IGNORE    3/3       
---------------------------------------------------------------------------------
Total Score: 30 / 30
   10 passed,     0 failed,    10 total

Start testing hard traces...
Testcase                                     Lines     Result    Random    Score     
---------------------------------------------------------------------------------
traces-hard/multiply.trace                   149382    PASS      IGNORE    1/1       
traces-hard/add.trace                        94710     PASS      IGNORE    1/1       
traces-hard/ls.trace                         56756     PASS      IGNORE    1/1       
traces-hard/convolve.trace                   1848393   PASS      IGNORE    1/1       
traces-hard/sort.trace                       45978     PASS      IGNORE    1/1       
traces-hard/grep.trace                       406467    PASS      IGNORE    1/1       
traces-hard/inner_product.trace              98329     PASS      IGNORE    1/1       
traces-hard/link_list.trace                  249719    PASS      IGNORE    1/1       
traces-hard/transpose.trace                  31990     PASS      IGNORE    1/1       
traces-hard/wc.trace                         404114    PASS      IGNORE    1/1       
---------------------------------------------------------------------------------
Total Score: 10 / 10
   10 passed,     0 failed,    10 total

Testing cache simulator done. Total scores: 100 / 100
Totally 2.720263 seconds passed.

Part B: Testing transpose function
Running ./test-trans -M 32 -N 32
Running ./test-trans -M 64 -N 64
Running ./test-trans -M 61 -N 67

Cache Lab summary:
                        Points   Max pts      Misses
Csim correctness         100.0       100
Trans perf 32x32          30.0        30         288
Trans perf 64x64          30.0        30        1108
Trans perf 61x67          40.0        40        1914
          Total points   100.0       100
```

如上分别是各个部分的组成，最后几行会输出你的总成绩。你可以通过如上方法快速得知你最终会得到多少的分数。

## 代码提交
???todo
    需要修改代码提交策略@blowinding

在 `cachelab-sp25` 目录（也就是你的实验目录）下执行 `make submit` 命令，会生成一个名为 `<userid>-handin.zip` 的压缩文件，这会将你的`cache-impl.c`压缩上交。（其中 `<userid>` 为`你的学号-ics`，也就是ICS Server中你的用户名）。

在[在线学习平台](http://class.xjtu.edu.cn/)上的作业模块中，将该文件作为附件提交即可。

!!!note
    提交网站可能会自动加上（2）等后缀以区分多次提交，这种情况无需担心，下载会自动以**最后一次提交**为准。如果你觉得不舒服，可以命名为例如`<userid>-cachelab-handin.zip`。文件命名不会影响到最后的成绩。

### 迟交
???todo
    需要修改迟交策略@blowinding

在超过原定的截止时间后，我们仍然接受同学的提交。此时，在lab中能获得的最高分数将随着迟交天数的增加而减少，具体服从以下给分策略：

- 4.14 - 4.20（含），每天扣除1%的分数，双休日不扣分，也就是最多扣除5%。
- 4.21 - 4.26（含），由于**概率论考试**，不扣分。
- 4.27（调休） - 5.11（含），其中：4.27 - 4.30（含）以及5.6 - 5.8（含）每天扣除2%的分数，也就是最多扣除14%。五一放假不扣分，5.9 **算法考试**，不扣分，5.10 - 5.11，双休日不扣分。
- 5.11及以后，每天扣除7%的分数，直至扣完

以上策略中超时不足一天的，均按一天计，自ddl时间开始计算。届时在线学习平台将开放迟交通道。

---

Copyright © 2026 XJTU ICS-TEAM

