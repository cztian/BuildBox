# 讨论时间：2020/3/3

1.讨论成员：隆林涛、蔡赵天、郝毓、段彦君  
  讨论内容：1. 确定各部门工作开展进展；2. 统计小组个人疑问并解答；  

1. 确定各部门工作开展进展
隆林涛：现在开始分享各项工作的进展情况。  
蔡赵天：编译通过，连接通过。  
段彦君：测试不通过  
郝毓：其原因是因为该功能还没有加上，正在研发。  
蔡赵天：这是网上某个模块化编译器的框图。可以供借我们参考。

2. 统计小组个人疑问并解答  
个人疑问：`int main (int argc, char* argv[], char* envp[])` ，这三个参数分别代表什么？  
团队讨论：`argc`是`argv`的个数。  
`envp`是环境变量，最后一个是零指针结束。  
比如，运行命令“`ls -l /`”，`argc`是`3`，一共三个参数。  
第一个，就是`argv[0]`，这个参数是程序自身的文件名。  
第二个，即`argv[1]`，是“`"-l"`”。  
第三个，是“`"/"`”。  
`envp`里面是各种环境变量。  
第一次讨论结束。

# 讨论时间：2020/4/10
讨论成员：蔡赵天，隆林涛，段彦君，郝毓  
本次讨论主要围绕对软件开发进度及其需求分析进行讨论，于软件需求规格说明书的任务进度报告后，本组开始进行以下讨论。  

1. 参考资料：
https://github.com/dspinellis/unix-history-repo（借鉴研发部分资料所需材料）  
https://github.com/weiss/original-bsd/blob/master/old/lex/libln/yyless.c（编译流程实例）  
http://progynova.f3322.net:44446/C%3A/Users/Administrator/Desktop/test/lex  
UNIX History

2. 编译的流程：
对单个文件进行预处理，编译；然后对所有后缀为“`.c`”文件进行以上操作，将生成的所有目标文件加上标准库文件，连接在一起，就是最终的可执行文件。  

3. 根据部分程序推出问题：
对78版的《C编程语言》及1990年的ISO C90标准的写法探讨，对函数以及变量的声明（declaration）展开讨论  
例：`yyless(x)`（见下图）中的参数类型、函数返回类型，经讨论并得出于78版（即第一版）的《C编程语言》书中写道，变量、函数都默认为`int`型，也就是早期的C语言，经常不区分变量类型。  
例如`int`和`int*`，一个里面存的是整数，另一个里面存的是指向某个整数的地址。这两种类型，在早期的C语言中经常不区分。为了区分，经常在函数名称及变量的后面加上类型。  
再比如`sprint`函数，声明了`s`的类型是`char*`。并发现了这种写法带来的缺陷，语法限制不严格，也不容易找出错误。有可能把指针误当成整数传进函数，也有可能整数当成指针传进来。而其中未写返回类型的，就是`int`类型。  
```C
/*	@(#)yyless.c	4.2	01/09/85	*/

yyless(x)
{
extern char yytext[];
register char *lastch, *ptr;
extern int yyleng;
extern int yyprevious;
lastch = yytext+yyleng;
if (x>=0 && x <= yyleng)
	ptr = x + yytext;
else
	ptr = (char *) x;
while (lastch > ptr)
	yyunput(*--lastch);
*lastch = 0;
if (ptr >yytext)
	yyprevious = *--lastch;
yyleng = ptr-yytext;
}
```
注：  
我们合力开发软件，使其让其符合ISO 9899:1990标准，以方便程序的可移植性。
