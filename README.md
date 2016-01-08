# Why-Functional-Programming-Matters
函数式编程原始论文Why Functional Programming Matters 中文版翻译

This paper dates from 1984, and circulated as a Chalmers memo for many
years. Slightly revised versions appeared in 1989 and 1990 as [Hug90] and
[Hug89]. This version is based on the original Chalmers memo nroff
source, lightly edited for LaTeX and to bring it closer to the published versions,and with one or two errors corrected. Please excuse the slightly oldfashioned type-setting, and the fact that the examples are not in Haskell!

>
本文始于1984年，多年来以备忘录形式传播。稍微修改后的版本出现在1989年([Hug89])和1990([Hug90])年,本文基于原始备忘录。


#### 摘要
As software becomes more and more complex, it is more and more
important to structure it well. Well-structured software is easy to write,
easy to debug, and provides a collection of modules that can be re-used
to reduce future programming costs. Conventional languages place conceptual
limits on the way problems can be modularised. Functional languages
push those limits back. In this paper we show that two features of
functional languages in particular, higher-order functions and lazy evaluation,
can contribute greatly to modularity. As examples, we manipulate
lists and trees, program several numerical algorithms, and implement
the alpha-beta heuristic (an algorithm from Artificial Intelligence used in
game-playing programs). Since modularity is the key to successful programming,
functional languages are vitally important to the real world.

>
随着软件变得越来越复杂，良好的结构变得愈发重要。具备良好结构的软件具备如下特性：易编写、易调试、提供一系列的可重用模块方便后续的编程开发。传统语言的先天特性使得模块化开发具备很多限制。函数式编程则是解决这些限制的良方。在本论文中会特别说明函数式编程语言大大有利于模块化开发的两个特性：高阶函数和延迟执行。在例子中，我们会通过使用list和树，编写几个数值算法，实现α-β启发式程序(从人工智能中使用的一种算法游戏程序)。届时模块化则是编程成功的关键要素，而函数式编程则是重中之重。

## 简介
This paper is an attempt to demonstrate to the “real world” that functional
programming is vitally important, and also to help functional programmers
exploit its advantages to the full by making it clear what those advantages are.

>
本论文试图展示函数式编程的重要性，然后帮助程序员充分了解并利用函数式编程的优点。


Functional programming is so called because a program consists entirely of
functions. The main program itself is written as a function which receives the
program’s input as its argument and delivers the program’s output as its result.
Typically the main function is defined in terms of other functions, which in
turn are defined in terms of still more functions, until at the bottom level the
functions are language primitives. These functions are much like ordinary mathematical
functions, and in this paper will be defined by ordinary equations. Our
1
notation follows Turner’s language Miranda(TM) [Tur85], but should be readable
with no prior knowledge of functional languages. (Miranda is a trademark
of Research Software Ltd.)

>
函数式编程的定义是程序全部由函数组成。主程序是一个function(后文称main function)，接收程序的输入作为参数然后输出运算结果。通常来说，main function由一系列的其他function组成，而这些function又由其他的一系列function组成，依次类推，一直到底层语言的私有实现为止。这些function与普通的数学方程非常像，本文中将之定义为普通方程。

The special characteristics and advantages of functional programming are
often summed up more or less as follows. Functional programs contain no
assignment statements, so variables, once given a value, never change. More
generally, functional programs contain no side-effects at all. A function call can
have no effect other than to compute its result. This eliminates a major source
of bugs, and also makes the order of execution irrelevant - since no side-effect
can change the value of an expression, it can be evaluated at any time. This
relieves the programmer of the burden of prescribing the flow of control. Since
expressions can be evaluated at any time, one can freely replace variables by
their values and vice versa - that is, programs are “referentially transparent”.
This freedom helps make functional programs more tractable mathematically
than their conventional counterparts.

>
函数式编程通常有下面列举的特点和优势。函数式编程不包含变量赋值语句，变量值一旦给定，就不会有变化。简而言之，函数式编程没有副作用。一个函数调用除了计算出结果，没有其他影响。这就消除了一个主要的bug来源，且使执行顺序变得无关紧要：因为如果没有外在因素改变一个表达式的值，它的输出也应该是确定，那么它应该可以用在任何地方。这减轻了程序员处理流程控制的负担。当表达式的执行时间不受执行顺序束缚时，我们可以自由的替换这些表达式的值所代表的变量，反之亦然，这意味着程序是“referentially transparent”。这种自由性能使函数式编程相比传统编程方式,在理论上更加清晰易懂。





