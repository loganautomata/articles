# Python基础篇

## 搭建VScode的Python编译环境

### Step1_DownLoad

[下载VScode编辑器]("https://code.visualstudio.com/download") 

[下载Python3解释器]("https://www.python.org/downloads/") 

下面这几个选项全选勾。不知道为甚么的话用了就知道了ヾ(ﾟ∀ﾟゞ) 

![Python-00-00.jpg](https://img.oss.logan.ren/2020/04/04/Python-00-00.jpg)

其余的步骤跟着安装应用程序走就完事。（PS：Python的安装路径如果是自定义的话，路径一定要记住，因为下面配置环境变量的时候就要用上了）

### Step2_配置环境变量

这件事情是第一次在我的Blog出现，但这件事并不会是最后一次出现，以后还会经常出现，Linux中更会经常用到，唯一的区别仅是没有图形化界面而已。所以在这里我会先详细讲述如何在Windows环境中配置。  

1.定义：环境变量其实就是一个Path，这个Path的目录下有一个或多个应用程序。
 
>例如：C:\Users\Logan\AppData\Local\Programs\Python\Python38-32，Python38-32是我们要启动的程序python.exe的上级目录，你将这个Path添加到环境变量后。然后打开PowerShell（或cmd）。这里推荐养成用PowerShell的习惯，为什么？因为它更“Power”。输入python你便会看到下面这幅图。

![Python-00-01.jpg](https://img.oss.logan.ren/2020/04/04/Python-00-01.jpg) 

你输入`python`系统其实并不知道它在那里，但系统会到你添加的环境变量的目录下挨个挨个的去找，当它去C:\Users\Logan\AppData\Local\Programs\Python\Python38-32这个目录下查找时便会找到python并且运行。如果你不添加的话你就必须输入完整的目录了，如下图。

![Python-00-02.jpg](https://img.oss.logan.ren/2020/04/04/Python-00-02.jpg)

>看到这里，你应该知道什么是环境变量了吧，接下来我会讲如何在Win10下快速添加环境变量。

2.Win10下添加环境变量。

>~~打开小娜，大喊环境变量，然后它就出来了~~，有一说一，这样挺好玩的，但一般要找什么东西的话，点开任务栏上的搜索按钮，输入要搜索的内容，eg：“环境变量”，然后点击系统属性上的环境变量，就可以开始设置了。接下来的设置分为两种情况，一种是设置用户变量，另一种是设置系统变量，就像它们的名字一样一个是当前用户的变量一个是系统全局变量，不管是哪个都要点开Path在Path变量中加入你python上级目录的地址，如果你和我一样安装python时未自定义安装目录，安装在Users\XXXXX下的话只能设置在用户变量中，如果是其他地方，你放在用户变量和系统变量下都行。

![Python-00-03.jpg](https://img.oss.logan.ren/2020/04/04/Python-00-03.jpg) 

![Python-00-04.jpg](https://img.oss.logan.ren/2020/04/04/Python-00-04.jpg) 

### Step3_建立PythonWorkSpace

新建一个Python的workspace文件夹，随便取个名字比如“PythonStudy”，然后在里面新建一个test.py文本，用VScode打开文件夹，文件夹，文件夹。不出意外，vscode会提醒你装插件Python,你安装就OK了，出了意外的话你点侧边栏最后一个到插件商店里去搜索Python，第一个就是它了，下载它，然后打开命令面板（ctrl+shift+p）输入"python:select interpreter"命令，回车，这个时候如果你前面设置好了环境变量的话就会在下面出现对应版本python解释器的选项，选择你的版本即可。然后F5调试，出现“select a debug configuration”默认第一个选调试当前文件夹即可。

![Python-00-05.jpg](https://img.oss.logan.ren/2020/04/04/Python-00-05.jpg)

>PS：现在VScode给我的感觉已经很成熟了，以前搭建C/C++环境时不配置launch.json......基本没法用，而现在用默认配置不管是python还是C初学已经足够了。~~绝不是因为懒，绝不是！！！~~。如果想要自己配置的话，在当前工作区新建.vscode文件夹，然后把别人作业里的配置文件抄好复制粘贴即可。

附：一定要记得下python插件，VScode有大量的插件，可以根据自己的需要下载。

![Python-00-06.jpg](https://img.oss.logan.ren/2020/04/04/Python-00-06.jpg)

over！！！

## 关键字

### True

布尔值一

### False

布尔值零

### None

表示该值是一个空对象，为空不为零。  

### as

`import turtle as t`取别名。  

### assert

`assert condition`condition为False时，抛出AssertError并且包含错误信息。  

### break

跳出循环

### continue

结束本轮循环，继续下一轮的循环。  

### class

类

### def

函数

### return

返回值，当函数没有显式return，默认返回None值。  

### del

删除变量而不删除数据（删除指针）。  

### and

与

### or

或

### not

非

### if

`if conditionIf`conditionIf为true时，执行后续语句。  

### elif

`elif conditionElif`conditionIf为false且conditionElif为true时，执行后续语句。  

### else

之前的判断条件都为false时执行后续语句。  

eg：

```Python
if expression1:
    statement(s)
elif expression2:
    statement(s)
elif expression3:
    statement(s)
else:
    statement(s)
```

### try

try中语句必须执行，其中的语句是否抛出异常作为判断标准。  

### except

try中的语句出现异常时执行except中的语句。  

### finally

finally中语句不管是否抛出异常都会执行。  

eg:  

```Python
try:
    clause
except:
    clause
else:
    clause
finally:
    clause
```

### raise

引发异常，单独一个 raise。该语句引发当前上下文中捕获的异常（比如在 except 块中），或默认引发 RuntimeError 异常。raise [exceptionName [(reason)]]引发指定类型的异常，同时附带异常的描述信息。  

### while

满足条件时循环。  

### for

循环。  

### in

判断数据是否在列表，数组等中。  

```Python
for l in list0
```

### import

引入库。  

### from

仅引入库中某一类。  

### global

全局变量。  

###　nonlocal

非局部变量，指向最近的嵌套作用域，可读但不可写。  

```Python
def a():
    i = 0
    def b():
        nonlocal i
        i=i+1
    b()
```

### is

判断两个变量（指针）是否指向同一个对象。  

### pass

空语句，占位，保证程序结构完整性何可读性。  

### with

with 语句适用于对资源进行访问的场合，确保不管使用过程中是否发生异常都会执行必要的“清理”操作，释放资源。  

```Python
with open("１.txt") as fileName:
```

### yeid

返回生成器

### lambda

匿名函数

## 标识符

命名规则：数字，下划线，英文字符，中文字符等皆可命名，但首字符不可为数字，不与关键字相同，且大小写敏感。

## Number

定义：包含int、float、bool、complex数据类型。  

```python
a + b
a - b
a * b
a / b #结果默认为浮点数
a % b
a // b #截尾取得一个整数
a ** b #乘方
```

PS：复数由实数部分和虚数部分构成，可以用a + bj,或者complex(a,b)表示， 复数的实部a和虚部b都是浮点型。  

## String

定义：Python中的字符串用单引号 ' 或双引号 " 或三单引号 ''' 括起来，同时使用反斜杠 \ 转义特殊字符。  

1.索引格式：变量[]。  
2.截取格式：变量[头下标:尾下标]（头下标缺失表示至开头，尾下标缺失表示至结尾）。  
3.从前往后数第一个数是0，从后往前数第一个数是-1。  
4.字符串前加上r或者R取消 \ 符号的转义。  
5.可使用 + 符号连接字符串，也可以使用 * 符号重复字符串eg：`str * 2`。  

PS：python中的字符串无法改变。  

### 字符串格式控制

定义：`"Hello {0} {1}".format("Logan", "Ren")`

|填充|对齐|宽度|
|:---:|:---:|:---:|
|用于填充的单个字符|左对齐 < ，右对齐 > ，居中对齐 ^ |输出宽度|

eg：`{0:*^10}`

|分隔符|精度|类型|
|:---:|:---:|:---:|
|千位分隔符 , |浮点数小数精度或字符串最大输出长度 . |b c d o x X e E f %|

b：二进制  
c：Unicode字符  
d：十进制  
0：八进制  
x：小写十六进制  
X：大写十六进制  
e：科学计数法e  
E：科学计数法E  
f：常规表示  
%：百分号表示  

eg：`{0:,.2f}`

## Tuple

定义：元组写在小括号 () 里，元素之间用逗号隔开。  

列表List与Stirng的索引，截取等都相同。  

PS：元组中只有一个元素时须在其后加上 , 符号，而空元组则不需要 , 符号eg；`tuple = ()`。元组中元素不可以改变。  

## List

定义：列表是写在方括号 [] 之间、用逗号分隔开的元素列表。  

列表List与Stirng的索引，截取等都相同，不同的主要有：  
1.Python 列表截取可以接收第三个参数，参数作用是截取的步长（每隔多少个字符截取一个元素）eg：list[ : : -1]，此时可以翻转字符串。  

PS：列表中元素的类型可以不相同，它支持数字，字符串甚至可以嵌套。列表中元素可以改变。  

## Set

定义：集合写在大括号 {} 里，元素之间用逗号隔开。基本功能是进行成员关系测试和删除重复元素。  

```python
a | b #并集
a & b #交集
a - b #差集
a ^ b #对称差
```

PS：创建一个空集合必须用 set() 而不是 { }，其中元素可以改变。  

## Dictionary

定义：字典写在大括号 {} 里，元素之间用逗号隔开，每个元素由键(key)和值(value)组成，key和value由 : 隔开。字典是无序的对象集合，字典当中的元素是通过键来存取的，健(key)必须为不可变类型（string，number，tuple）。  

PS：直接用 {} 构建空字典，通过`dictionary[key] = value`添加新的键值对。  

## 数据类型转换

|函数名|转换类型|
|:---:|:---:|
|int(x)|number to int|
|int(x, base)|string to int(base为string的进制类型)|
|float(x)|number or string to float|
|complex(r, i)|number to complex|
|complex(x)|stirng to complex|
|str(x)|object to string|
|repr(x)|object to expression|
|eval(x)|expression to object|
|tuple(x)|iterable(eg : list) to tuple|
|list(x)|tuple or string to list|
|set(x)|iterable to set (ps: 重复元素会被删除)|
|dict(x)|iterable or **kwarg... to dictionary|
|frozenset(x)|iterable to set(Invariable)|
|chr(i)|number(ASCII) to character|
|ord(c)|character to number|
|hex(x)|base 10 to base 16 string|
|oct(x)|base 10 to base 8 string|

## 数值运算函数

|函数名|描述|
|:---:|:---:|
|abs(x)|绝对值，x的绝对值|
|divmod(x, y)|商余，(x//y, x%y)，同时返回商和余数|
|pow(x, y[, z])|幂，(x**y%z)|
|round(x[, d])|四舍五入，保留d位小数，默认d为0|
|max($x_1, x_2 ... x_n$)|最大值|
|min($x_1, x_2 ... x_n$)|最小值|

## 字符串处理方法

|方法名|描述|
|:---:|:---:|
|str.lower()|返回全小写字符串副本|
|str.upper()|返回全大写的字符串副本|
|str.split(sep=None)|返回由sep分割后的list副本|
|str.count(sub)|返回sub子字符串在str中的出现次数|
|str.replace(old, new)|返回由new字符串替代old后生成的string副本|
|str.center(width[, fillchar])|返回根据width居中，根据fillchar填充生成的string副本|
|str.strip(chars)|返回去掉左右侧chars中字符生成的新string副本|
|str.join(iter)|返回在每个元素后加上iter后生成的新副本（最后一个元素除外，用以split）|

## 文件的打开和关闭

### open

<变量名> = open(<文件名>, <打开模式>)

文件名

文件路径,与Linux一致使用斜杠('/'), 反斜杠为转义符.

打开模式

| 模式名 | 描述                                                         |
| ------ | ------------------------------------------------------------ |
| 'r'    | 只读模式,如果文件不存在返回"FileNotFoundError".              |
| 'w'    | 覆盖写模式,如果文件存在则完全覆盖.                           |
| 'x'    | 创建写模式,如果文件存在则返回"FileExistsError".              |
| 'a'    | 追加写模式,如果文件不存在则创建文件,存在则在文件最后追加内容. |
| 'b'    | 二进制文件模式.                                              |
| 't'    | 文本文件模式.                                                |
| '+'    | 与'r', 'w', 'x', 'a'同时使用,表示在原有功能上增加同时读写功能. |

eg:file = open("./test.py", "wt+")

> PS:默认打开模式"rt"

### close

关闭文件

eg: file.close()

## 文件内容的读取

### read

<变量名>.read(size=-1)

默认读入文件全部内容, 若有参数size则读入前size长度.

### readline

<变量名>.readline(size=-1)

读入一行内容, 若有参数size则读入前size长度.

### readlines

<变量名>.readlines(hint=-1)

读入文件所有行内容,以每行内容为元素形成列表, 若有参数hint则读入前hint长度.

## 文件内容的写入

### write

<变量名>.write(str)

向文件中写入一个字符串.

### writelines

<变量名>.writelines(list)

将一个元素全为字符串的列表写入文件.

## 文件指针

### seek

<变量名>.seek(offset)

改变当前文件操作指针, 0-文件开头, 1-当前位置, 2-文件结尾.

## 获取时间函数

### time()

获取从1970年1月1日0点0分至今的时间段,单位为秒,浮点是返回.  
eg:

>1580977461.1635454  

### ctime()

返回一个可以由程序员读懂的字符串  
eg:

>Thu Feb  6 16:24:21 2020

### gmtime

返回一个可以由其它程序调用的结构体.  
eg:

>time.struct_time(tm_year=2020, tm_mon=2, tm_mday=6, tm_hour=8, tm_min=24, tm_sec=22, tm_wday=3, tm_yday=37, tm_isdst=0)
