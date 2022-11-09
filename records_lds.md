# <center><font color=blue size=6> 1、链接脚本语法 </font></center>
<!-- # <center>$\color{red}{ 1、链接脚本语法}$</center> -->
---
<!-- fit -->

## 1.1）进程入口地址
ld有多种方法设置进程入口地址，按以下顺序执行(编号越前，优先级越高)
-   1、ld命令行的-e选项
-   2、链接脚本的ENTRY(SYMBOL)命令
-   3、如果定义了start符号，使用start符号值
-   4、如果存在.text section，则使用.text section的第一个字节的位置值
-   5、使用值0

例1：
-    ENTRY(SYMBOL): 将符号SYMBOL的值设置成入口地址


## 1.2）INCLUDE filename
 INCLUDE filename:包含其他名为filename的链接脚本
-   1、相当于c程序中的#include指令，用以包含另一个链接脚本。脚本搜索路径由-L选项指定，INCLUDE指令可以嵌套使用最大深度为10


## 1.3）INPUT(files)
 INPUT(files): 将括号内的文件作为链接过程的输入文件
-   1、ld首先在当前目录下寻找该文件，如果没有找到则由-L指定的搜索路径下搜索，
        file可以为-lfile形式，就像命令行的-l选项一样，
-   2、如果该命令出现在暗含的脚本内， 则该命令内的file在连接过程中的顺序由该暗含的脚本在命令行内的顺序决定


## 1.4）GROUP(files)
 GROUP(files): 指定需要重复搜索符号定义的多个输入文件
-   1、files必须是库文件，且file文件作为一组被ld重复扫描，直到不再有新的未定义的引用出现


## 1.5）OUTPUT(filename)
OUTPUT(filename): 定义输出文件的名字
-   1、同ld的-o选项，不过ld的-o选项优先级更高，所以它可以用来定义默认的输出文件名


## 1.6）SEARCH_DIR(path)
SEARCH_DIR(path): 定义搜索路径
-   1、同ld的-L选项，不过ld的-L指定的路径要比它定义的优先级高


## 1.7)STARTUP(filename)
STARTUP(filename): 指定filename为第一个输入文件
-   1、在链接过程中，每个输入文件是有顺序的，此命令设置文件filename为第一个输入文件

## 1.8)OUTPUT_FORMAT(BFDNAME)
OUTPUT_FORMAT(BFDNAME): 设置输出文件使用的BFD格式
-   1、同ld选项-o format BFDNAME，不过ld选项优先级更高
-   2、OUTPUT_FORMAT(DEFAULT,BIG,LITTLE):定义三种输出文件的格式(大小端)

---
# <center><font color=blue size=6>2、SECTIONS命令 </font></center>
---

## 2.1）SECTIONS概述
 - SECTIONS命令告诉ld 如何把输入文件的sections映射到输出文件的各个section；
 - 如何将输入section合并为输出section；
 - 如何把输出section放入到程序地址空间(VMA)和进程地址空间(LMA)；

 例1：
 ```javascript
    SECTIONS
    {
        SECTIONS-COMMAND
        SECTIONS-COMMAND
        ...
    }
```

## 2.2）SECTION-COMMAND
- SECTION-COMMAND有四种：
  -  1、ENTRY命令
  -  2、符号赋值语句
  -  3、一个输出section的描述(output section description)
  -  4、一个section叠加描述(overlay description)

> - <font color=blue> 如果整个链接脚本内没有SECTIONS命令，那么ld将所有同名输入section合并为一个输出section内，各输入section的顺序为他们被链接器发现的顺序。
> - 如果某输入section没有在SECTIONS命令内提到，那么该section将被直接拷贝成输出section </font>

### 2.2.1）输出section描述
```javascript
    SECTION [ADDRESS] [(TYPE)] : [AT(LMA)]
    {
        OUTPUT-SECTION-COMMAND
        OUTPUT-SECTION-COMMAND
        ...
    } [>REGION] [AT>LMA_REGION] [:PHDR HDR ...] [=FILEEXP]
```
> - []内的内容为可选项，一般不需要
> - SECTION: section名字
> - SECTION左右的空白、圆括号、冒号是必须的，换行符和其他空格是可选的


- 每个 $\color{blue}{OUTPUT-SECTION-COMMAND}$ 为以下四种之一:
    - 符号赋值语句
    - 一个输入section描述
    - 直接包含的数据值
    - 一个特殊的输出section关键字

- $\color{red}{输出section名字(SECTION)}$:
  - 必须符合输出文件格式要求比如：a.out格式的文件只允许存在.text、.data、和.bss section名。
  - 而有的格式只允许存在数字名字，那么此时应该用引号将所有名字内的数字组合在一起;
  - 另外，还有一些格式允许任何序列的字符存在于section名字内，此时如果名字内包含特殊字符(比如空格逗号等)，那么需要用引号将其组合在一起

