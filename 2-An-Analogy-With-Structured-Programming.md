## 类比结构化编程

It is helpful to draw an analogy between functional and structured programming.
In the past, the characteristics and advantages of structured programming have
been summed up more or less as follows. Structured programs contain no goto
statements. Blocks in a structured program do not have multiple entries or exits.
Structured programs are more tractable mathematically than their unstructured
counterparts. These “advantages” of structured programming are very similar in
spirit to the “advantages” of functional programming we discussed earlier. They
are essentially negative statements, and have led to much fruitless argument
about “essential gotos” and so on.

>
类比一下结构化编程和函数式编程是有必要的。在过去，结构化编程的特点和优势或多或少被总结如下。结构化程序不包含goto语句。结构化程序的代码块没有多个入口和出口。结构化程序比非结构化的程序更加清晰易懂。这些优点与之前函数编程宣称的优点非常类似。这本质上是消极的声明，导致很多“针对goto”的毫无结果的争论等等。


With the benefit of hindsight, it is clear that these properties of structured
programs, although helpful, do not go to the heart of the matter. The most important difference between structured and unstructured programs is that structured programs are designed in a modular way. Modular design brings with
it great productivity improvements. First of all, small modules can be coded
quickly and easily. Secondly, general purpose modules can be re-used, leading to
faster development of subsequent programs. Thirdly, the modules of a program
can be tested independently, helping to reduce the time spent debugging.

>
事后看来,很明显,这些结构化程序的特性,虽然有用，但是并没有指向问题的核心.结构化和非结构化程序最重要的却别在于，结构化程序是以模块化方式设计。模块化设计带来了巨大生产力的提高。首先，小模块写起来快而且容易。其次，通用模块可以被重用，促进后续项目更加快速的开发。第三，程序模块可以独立测试，帮助减少调试时间。


The absence of gotos, and so on, has very little to do with this. It helps with
“programming in the small”, whereas modular design helps with “programming
in the large”. Thus one can enjoy the benefits of structured programming in
FORTRAN or assembly language, even if it is a little more work.

>
规避goto，以及类似的原则，起的作用并不大。这些教条有利于微观上的编程（细节），而模块化编程有利于宏观方向（整体）。因此我们可以在FORTRAN或汇编语言中享受结构化编程得好处，


It is now generally accepted that modular design is the key to successful programming,and languages such as Modula-II [Wir82], Ada [oD80] and Standard
ML [MTH90] include features specifically designed to help improve modularity.
However, there is a very important point that is often missed. When writing
a modular program to solve a problem, one first divides the problem into subproblems,then solves the sub-problems and combines the solutions. The ways
in which one can divide up the original problem depend directly on the ways
in which one can glue solutions together. Therefore, to increase ones ability
to modularise a problem conceptually, one must provide new kinds of glue in
the programming language. Complicated scope rules and provision for separate
compilation only help with clerical details; they offer no new conceptual tools
for decomposing problems.



One can appreciate the importance of glue by an analogy with carpentry.
A chair can be made quite easily by making the parts - seat, legs, back etc. -
and sticking them together in the right way. But this depends on an ability
to make joints and wood glue. Lacking that ability, the only way to make a
chair is to carve it in one piece out of a solid block of wood, a much harder
task. This example demonstrates both the enormous power of modularisation
and the importance of having the right glue.


>
现在，人们已经接受了模块化设计是成功的程序设计范式，而且有些语言包含了方便模块化的设计。但是有非常重要的一点容易被忽视。当我们写一个模块化程序来解决问题时，首先是将问题分解成小问题，然后各个击破，最后合并起来形成一个完整的解决方案。分解问题的解决方式直接依赖于如何将方案整合起来。因此，要提高模块化的概念，传统的编程语言必须在语言中提供一有效的模块化整合方式。复杂的规则和提供单独的范围编译只能帮助处理文本细节，它们并没有提供任何新的概念工具用于分解问题。


Now let us return to functional programming. We shall argue in the remainder
of this paper that functional languages provide two new, very important
kinds of glue. We shall give many examples of programs that can be modularised
in new ways, and thereby greatly simplified. This is the key to functional
3 programming’s power - it allows greatly improved modularisation. It is also the
goal for which functional programmers must strive - smaller and simpler and
more general modules, glued together with the new glues we shall describe.


>
回到函数式编程。本文中，为函数式编程提供了两种新的，重要的模块化整合方式。它们能极大的提高模块化。我们将提供一些程序例子来感受函数式编程的威力。
