pl_ch11

Encapsulation 

Information

pl_ch10

environment pointer, dynamic/static link

pl_ch9

partimophism ??????

pl_ch8

loop guard/semant ??? guard

guard selection

pl_ch7

pl_ch6

dangling pointer

mark

pl_ch5

逻辑（声明式）编程语言

使用符号裸机的方式书写程序

使用逻辑推理过程来得到结果

声明式， 而不是过程式

只陈述结果，而没有详细的生成过程

命题：一个可能为真可能为假的逻辑表述

包含对象以及对象之间的关系

操作数（真值或假值）

操作（and or not nor）

标识符：一系列数字或字母，第一个是一个字母：例如：abc

如果 abc 是一个命题，那么“非abc“也是一个命题

符号逻辑：用来表示命题，用来表示命题之间的关系，描述一个新的命题是如何通过其他命题推导出来的



谓词演算

使用非逻辑对象的量化变量

允许使用包含变量的语句



对象表示：在命题中的对象由简单的术语表示：包括常量或者量化变量

常量 constant：一个代表对象的符号，举例：likes(bob,steak)这里 bob 和 steak 是常量

量化变量：一个符号在不同环境中可以代表不同的对象

e.g., ∀ X. P (For all X, P is true), where X is a
quantified variable; ∀ is a universal quantifier
• Different from variables in imperative languages



复合术语

原子命题：最简单的那些命题：likes(bob,trout)

包含复合术语，复合术语包含了两个部分Functor：一个表明关系的函数符号：

e.g., student(jon)
like(seth, OSX)
like(nick, windows)
like(jim, linux)

中的 student 和 like，就表明了关系，

第二个部分是参数列表（元组 tuple）

**命题的形式**

命题可以用两种方式陈述：

1.fact 事实：命题被认为是真实的

2.query 质疑：命题的真假还需要被进一步确定

##### 复合命题

拥有两个或两个以上的原子命题，命题之间由逻辑操作符相连

逻辑操作符：非，与，或

Resolution:
– process of inferring an proposition from given
propositions
– allows inferred propositions to be computed
from given propositions.
– e.g.,
• older(joanne, jake) Ì mother(joanne, jake)
• wiser(joanne, jake) Ì older(joanne, jake)
• wiser(joanne, jake) Ì mother(joanne, jake)

Unification



生命周期：

静态 static，在执行前就确定了地址，声明周期程序运行的整个时间一直存在

栈式动态存储绑定，例如显示声明的变量，非静态变量，函数中的参数在运行期间分配内存，所在函数结束后

显式堆动态存储 - c++中的 new malloc

隐式堆动态存储 - JavaScript， php 的变量都是存储在堆中的



数学表达式

operator associativity rules 对定了同等优先级的运算如何进行

加法减法，从左到右，求次方运算，从右到左

apl 这个语言所有的操作符优先级一样，所有的 operators associate 都是从左到右

ruby 的操作符用方法来定义：数学运算，关系运算，赋值运算符，数组索引，偏移，位运算；操作符可以重载

scheme lisp，所有的逻辑运算符和数学运算符都通过“子程序”显式调用：

- a + b * c is coded as (+ a (* b c))

操作数优先级：被括号括起来的先进行运算

函数式的副作用：改变双向参数或者非本地变量

Referential Transparency：拥有相同值的两个表达式可以互相替换，不会影响程序的表现

纯函数式语言是Referential Transparency的，因为他们没有变量

操作符重载：

Narrowing conversion类型转换会丢失掉一些信息，比如 floar to int

Widening conversion 和上面的相反 例如 int to float

类型转换模式：

混合模式转换，对于多种类型都有操作符

强制类型转换：隐式类型转换增加编译器去检查

显式类型转换：被称为 casting eg (int)angle，F# float(sum) 这个 fsharp 类似与函数调用

表达式中的错误：原因：数学限制例如除零，计算机限制例如越界

关系表达式 relational expressions 

ruby 的 == 会忽略类型，而 eql？会确保类型也一样才返回真

Short Circuit Evaluation : 表达式的结果不用计算全部的操作和操作数

例如 (13 * a) / (b + 1), 如果 a 是0，后面的就不计算了

ada 这个语言的赋值操作是:=

Compound Assignment Operators :  a += b

Unary Assignment Operators: a++

赋值语句被用作表达式 while(a = b) a 可以用作表达式

Multiple Assignments：(\$first, 、\$second, \$third) = (20, 30, 40);

函数式语言中的赋值

Identifiers in functional languages are only
names of values
• ML (MetaLanguage)
– Names are bound to values with val
val fruit = apples + oranges;

- If another val for fruit follows, it is a new,
  different identifier.

看不懂上面这个啥意思

In Fortran, C, Perl, and C++
– any numeric type value can be assigned to
any numeric type variable-–coercion freely
applied
• In Java and C#
– only widening assignment coercions are
allowed



选择语句：if else switch

Java example
if (sum == 0)
if (count == 0)
result = 0;
else result = 1;
• Which if gets the else?
• Java's static semantics rule:
– else matches with the nearest previous if



计数循环语句：循环变量，初始条件，结束条件，步长