# vbs基本语法！

一个简单语言，她也是有自己最最基本的语法的！并且最基本的东西往往是相通的，只不过体现的形式变了而已！

比如：变量，数组，函数，操作符，条件分支，循环分支，这些都是一门语言，最基本的要素。

## 变量

1. 所有单引号后面的内容都被解释为注释。

2. 在VBScript中，变量的命名规则遵循标准的命名规则，需要注意的是：**在VBScript中对变量、方法、函数和对象的引用是不区分大小写的**。在申明变量时，要显式地申明一个变量，需要使用关键字Dim来告诉VBScript你要创建一个变量，并将变量名称跟在其后。申明多个同类型变量，可以用逗号分隔。注意：VBScript中不允许在申明变量的时候同时给变量赋值。但是允许在一行代码内同时对两个变量进行赋值，中间用冒号分隔。

3. 你可以使用OptionExplicit来告诉宿主变量必须先声明后使用。

4. **VBScript在定义时只有一种变量类型，在实际使用中需要使用类型转换函数来将变量转换成相应的变量类型。**

   * Cbool函数将变量转换成布尔值。
   * cbyte函数将变量转换为0到255之间的整数。
   * Ccur函数、Cdbl函数和Csng函数将变量转换为浮点数值，前者只精确到小数点后四位，后两者要更加精确，数值的范围也要大的多。
   * Cdate函数将变量转换为日期值。
   * Cint函数和Clng函数将变量转换为整数，后者的范围比前者要大的多。
   * Cstr函数将变量转换为字符串。

## 数组

数组的定义与变量非常类似，只需要在变量后描述这个数组的个数和维数。需要注意的是：数组的下标总是从0开始，而以数组定义中数值减一结束。也就是说你以要定义一个有十个数据的数组，将这样书写代码：dIm array（9），同样，当你要访问第五个元素时，实际的代码是array(4)。当然，你可以通过不指定数组的个数和维数来申明动态数组。等到数组的个数和维数固定后，使用关键字redim来改变数组。注意，在改变数组的大小时，数组的数据会被破坏，使用关键字preserve来保护数据。例如：

```vbscript
Redim preserve  array(个数,维数)

Dim array(9,2)
array(0,0)=11
array(0,1)=22
array(0,2)=33
MsgBox array(0,0)
MsgBox array(0,1)
MsgBox array(0,2)
```

这样想来，还是更喜欢C语言。最简洁有效的语言！

## 操作符

在VBScript运算符中，加减乘除都是我们常用的符号，乘方使用的是^ ，取模使用的Mod。

在比较操作符中，等于、小于、大于、小于等于、大于等于都与我们常用的符号是一致的，而不等于是小于和大于连用。

逻辑运算符为：和操作—>AND ，非操作—>NOT ，或操作—>OR。

你可以使用操作符 + 和操作符& 来连接字符串，一般使用&操作符。

另外还有一个比较特殊的操作符Is用来比较对象，例如按钮对象，如果对象是同一类型，结果就是真，如果对象不是同一类型，结果就是假。

## 条件判断

条件语句主要有if……then语句和select  case语句两种形式。

```vbscript
Dim myvaraint
myvaraint = 5
If myvaraint = 2 Then
MsgBox myvaraint
ElseIf myvaraint=3 Then
MsgBox ("myvaraint=")&myvaraint
Else MsgBox ("my=")&myvaraint
End if
```

这里vbs的劣势就体现出来了。=竟然既可以应用于赋值，也可以用于比较！难道比较不应该是==吗？另外，&明显是取地址，然而，可能vbs根本就是取地址的功能！

```vbscript
Dim color,myavar
Sub changebackgroud(color)
myvar = LCase(color)
Select Case myvar          
Case "red" document.bgcolor = "red"
Case "green" document.bgcolor = "green"
Case "blue" document.bgcolor = "blue"
  Case Else MsgBox "anther color"
End Select 
End Sub
```

## 循环控制

循环控制语句有：

1. for……next循环
2. for……each循环
3. do……while循环
4. do……until循环
5. while循环

在使用循环控制语句前，首先要对循环条件进行判断，如果循环次数是有固定次数的，那么使用For……next循环，其结构为：





## 函数

常用的过程有三种：

* 一种为函数，给调用者返回值。
* 一种为子程序，无返回值。
* 还有一种叫事件的特殊子程序，用的比较少。

函数的基本定义方法为：

Function 函数名称（参数列表）

函数代码

函数名称＝某值 ‘用来返回值

end function

子程序一些都类似，不过没有返回值

注意：

尽管在定义子程序的时候，参数列表要加括号，但在调用子程序的时候，参数列表不加括号，**括号只在函数中使用**。另外，子程序不能在表达式中使用。

而函数只能出现在赋值语句的右边，或者表达式中，函数不能直接使用，如果必须直接使用函数，则必须用call语句调用，并取消返回值。

从这点讲，子程序就跟Python中的方法有点像，比如print。

而函数，在vbs中必须要有返回值，切必须在赋值语句的右边！