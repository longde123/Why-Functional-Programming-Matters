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


Such a catalogue of “advantages” is all very well, but one must not be surprised
if outsiders don’t take it too seriously. It says a lot about what functional
programming is not (it has no assignment, no side effects, no flow of control) but
not much about what it is. The functional programmer sounds rather like a medieval
monk, denying himself the pleasures of life in the hope that it will make
him virtuous. To those more interested in material benefits, these “advantages”
are not very convincing.

>
即使函数式编程的这些优点都棒极了，但是如果局外人不待见它，我们也不必感到惊讶。这些优点说了很多函数式编程的不同之处（没有变量赋值，无副作用，没有流程控制），但是没有具体说明函数式编程到底是啥。使用函数式编程的程序员听起来像是古代苦行僧，通过压制自己的欲望来使其看起来超脱于世。对那些不见兔子不撒鹰的人，上述函数式编程的特性并不具备足够的吸引力。

Functional programmers argue that there are great material benefits - that
a functional programmer is an order of magnitude more productive than his
conventional counterpart, because functional programs are an order of magnitude
shorter. Yet why should this be? The only faintly plausible reason one
can suggest on the basis of these “advantages” is that conventional programs
consist of 90% assignment statements, and in functional programs these can be
omitted! This is plainly ridiculous. If omitting assignment statements brought
such enormous benefits then FORTRAN programmers would have been doing it
for twenty years. It is a logical impossibility to make a language more powerful
by omitting features, no matter how bad they may be.

>
函数式程序员会反驳，说函数式编程能带来巨大的实际好处，比如使用函数式编程比传统方式的开发效率高一个数量级，因为函数式编程的代码量本身就比传统的编程方式少一个数量级。然而为何差距如此之大呢？一个比较让人信服的理由是，传统编程90%的代码是由赋值语句组成，而函数式编程没有。这显然很荒谬。如果能省掉赋值语句，搞FORTRAN的人早二十年就开始干了。通过省略语言特性来使语言更加强大，无论这个语言多糟糕，在逻辑上就不可能。


Even a functional programmer should be dissatisfied with these so-called
advantages, because they give him no help in exploiting the power of functional
languages. One cannot write a program which is particularly lacking in assignment
statements, or particularly referentially transparent. There is no yardstick
of program quality here, and therefore no ideal to aim at.

>
甚至有函数式程序员本身就对这些特性不满意，因为它对开发函数式语言没啥帮助。缺少赋值语句或referentially transparent，咱写不出程序。没有代码质量标准，而且没有目标。


Clearly this characterisation of functional programming is inadequate. We
must find something to put in its place - something which not only explains the
power of functional programming, but also gives a clear indication of what the
functional programmer should strive towards.

>
明显，仅仅只有这些函数式编程的描述是不够的。我们必须找到它的应用场景-不仅仅是说一说而已，还需要一个清晰的指导。