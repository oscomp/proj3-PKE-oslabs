# proj3-PKE-oslabs

## 基于代理内核PKE的操作系统实验设计

----------


## 项目描述 ##

当前普通（985和211）高校计算机专业的大学本科学生学习《操作系统原理》课程的过程中，课程实验往往面临着两难的现状：做内核实验（如uCore、xv6等）难度高、工作量大；做一些应用的设计又导致难度太低，对操作系统原理的知识点理解不准确。为此，本项目的目标是希望设计一组**面向普通大学本科生的操作系统实验**。 这个实验的目标是，将操作系统的**知识点进行分解，先找到合适的（利用到操作系统知识点所支持的）应用，完成代理内核（即一个“刚刚好”的内核）的设计，并最终将应用在RISC-V的模拟环境（我们倾向于使用Spike）上运行起来**。这一组操作系统实验的设计特点是：**以典型应用驱动，带动内核的开发，随着应用的不断复杂化，最终获得一个功能较为完善的操作系统**。

经过2021年的实验课程建设，我们开发和完善了实验代码（riscv-pke，见https://gitee.com/hustos/riscv-pke ）和实验文档（见https://gitee.com/hustos/pke-doc ），同时，该实验在头歌平台上也得到了部署（见https://www.educoder.net/paths/9peq8avu ）。其中，头歌平台提供了代码仓库管理和docker环境，学生能够在不在本地装虚拟机和交叉编译器的情况下，在网页上完成实验。 riscv-pke目前包含了3组（涵盖了中断处理、内存管理和进程管理），共9个基础实验；同时，在每个组提供了2个挑战实验，也就是共6个挑战实验。其中，基础实验有代码上的依赖关系，但挑战实验互不依赖，可独立进行。**基础实验和挑战实验可以依学生的水平和学生对课程内容的要求，有选择性地进行组合**。riscv-pke的代码量非常小：为了完成这些实验，学生需要阅读的代码量约为500行--1500行，需要输入的代码量约为1行--50行。

riscv-pke的设计采用了以下思路：

- 在目标机方面，我们选择了支持RV64G指令集的RISC-V机器，为了方便部署和调试，我们推荐使用Spike模拟器，实验尽量利用Spike模拟器所提供的HTIF（Host-Target InterFace）来屏蔽底层硬件的具体操纵细节，将实验环节的设计重点放在操作系统的核心概念（如虚实转换、资源管理与调度）的实现上。Spike模拟器的本质是一个ISA（指令集架构）模拟器，其代码量非常小（只有1--2万行）。**实际上，我们也是希望提供机会给学生去了解一个小巧的模拟器的设计，并进一步激发激发学生对计算机底层硬件组成的兴趣**。
- 在软件编程语言方面，我们使用C语言和RISC-V汇编语言，保证riscv-pke实验的一致性。同时，**我们直接瞄准64位平台**（不考虑16位或32位），因为我们认为这也会是学生将来参加科研或工作所主要使用和接触的平台，且在可预见的未来不大可能会看到更高位宽平台（如128位）的流行。

通过2021年的教学实践，我们在华中科技大学卓越工程师1901班、校际交流1901班和校际交流1902班进行了实践。其中在卓越工程师1901班我们是将所有（共9个）基础实验，以及较低难度的挑战（3组实验中的第一个挑战）作为《操作系统原理》教学的课内实验，将3组实验中较高难度的第二个挑战做为课设提供给学生；在校际交流班，我们将所有9个基础实验作为《操作系统原理》教学的课内实验内容，将所有6个挑战实验作为课设提供给学生。

从教学实践的效果来看，卓越工程师班的25名同学，其中只有1名同学没有完成所有9个基础实验（缺1个）；仅完成基础实验的同学有6名；完成所有基础实验+1个挑战实验的同学有3名；完成所有基础实验+2个挑战实验的同学有4名；完成所有基础实验+3个挑战实验的同学有11名。两个校际交流班共有60名同学，完成所有基础实验的有57名，仅有2名同学完成了6个基础实验。以上数据因为考虑到隐私的保护，无法给出原始数据，感兴趣的老师可以单独联系我（email:zyshao@hust.edu.cn）。以上数据显示，riscv-pke实验的设计基本上完成了2021年所定下来的，**面向普通高校本科学生的操作系统实验**的设计初衷，实践上也具备较好的区分度。华中科技大学的操作系统课程设计因为是在2022年的春季展开，课设的实践数据将在2022年5月份更新。



通过2021年的教学实践，也暴漏出riscv-pke实验的一些问题：

- 由于Spike模拟器的小巧，导致其功能距离全系统模拟器（如QEMU）还有相当差距，缺乏较为完善的设备模拟，使得在Spike+riscv-pke的基础上很难设计操作系统设备管理、文件系统方面的实验。现有的实验4（https://gitee.com/hustos/pke-doc/blob/master/chapter6_device.md ）是在PYNQ FPGA开发板上进行的，受限于实验环境，其普及性较差。
- 挑战实验的选择还是偏少，可能的话应该给学生更多的选择，以达到更好的区分度。



2022年操作系统比赛，我们拟在现有的riscv-pke基础上进一步扩展和完善riscv-pke，具体来说，有以下两个方向（将在“预期目标”中详细列举）：

- 新增挑战实验
- 设备实验



PKE现有的文档和源码：  

- （文档）：https://gitee.com/hustos/pke-doc
- （代码）：https://gitee.com/hustos/riscv-pke

## 所属赛道 ##

2022全国大学生操作系统比赛的“OS功能设计”赛道



## 参赛要求 ##

- 以小组为单位参赛，最多3人一个小组，且小组成员是来自同一所高校的本科生（2021年春季学期或之后本科毕业的大一~大四的学生）
- 如学生参加了多个项目，参赛学生选择一个自己参加的项目参与评奖
- 请遵循“2022全国大学生操作系统比赛”的[章程](https://mp.weixin.qq.com/s/yxunoM7MwXJ3EYt4aZCNmw)和[技术方案](https://mp.weixin.qq.com/s/9UUDcTguBCh2UedXlpATXQ)要求

### 项目导师

邵志远

- email: zyshao@hust.edu.cn

### 难度

初等~中等

## 特征 ##

- 采用软件模拟器，如Spike或QEMU，模拟支持RV64G指令集的RISC-V目标机器（不鼓励物理机器）
- 使用C语言/内嵌RISC-V汇编实现
- 为进一步阅读/修改类UNIX操作系统，如xv6、UCore、Linux内核等，打下基础
- 可参考更高级的教学类操作系统（如UCore、xv6等）的实现，但要作品要体现riscv-pke在设计上的“刚刚好”的思想，做到代码的极简化

## 参考文档 ##

- https://gitee.com/hustos/pke-doc
- 请认真阅读参考文档，里面的实验文档可以作为作品的例子。

### License ###

GPL-3 license

## 预期目标 ##

**所提交的作品应至少包括以下内容**：

- **输入应用**（最好找到贴合实际，且具有代表性的应用，因为代理内核的开发是应用驱动的）
- **设计任务**（对实验所要达到的目标有较为准确、详细的描述）
- **实验文档**（MD格式的实验指导。实验指导要求尽量详细，能够引导读者的理解，符合普通高校本科生的知识储备）
- **（挑战实验）标答**（对于挑战实验，需要提供DOC或者PDF格式的挑战解析）

**注意：下面的内容是建议内容，不要求必须全部完成。选择本项目的同学也可与导师联系，提出自己的新想法，如导师认可，可加入预期目标**


在现有riscv-pke课程实验的基础上，进一步完成如下目标：

### 第一题：增加挑战实验 ###

- 增加riscv-pke的挑战实验，挑战的内容可以是非法指令截获、系统调用、物理内存管理、缺页异常的处理、进程的封装、多进程支持、信号量的实现等。
- 新增实验应考虑参考现有的挑战实验的设计，设计这些实验环节的挑战部分，挑战实验的设计需要激励学生完成一定量的自学内容（不一定仅限于课本知识）。

### 第二题：riscv-pke设备管理 ###

- 可以考虑切换模拟器（如用QEMU），但也可以在Spike上做简单的IO扩展。如果是在Spike上做IO扩展，最好能考虑进一步贡献给riscv社区！
- 实验的设计并体现操作系统的基础概念，如设备独立性、设备虚拟分配等。
