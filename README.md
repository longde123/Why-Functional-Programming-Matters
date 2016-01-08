# Why-Functional-Programming-Matters
函数式编程原始论文Why Functional Programming Matters 中文版翻译

This paper dates from 1984, and circulated as a Chalmers memo for many
years. Slightly revised versions appeared in 1989 and 1990 as [Hug90] and
[Hug89]. This version is based on the original Chalmers memo nroff
source, lightly edited for LaTeX and to bring it closer to the published versions,and with one or two errors corrected. Please excuse the slightly oldfashioned type-setting, and the fact that the examples are not in Haskell!

>
本文始于1984年，多年来以备忘录形式传播。稍微修改后的版本出现在1989年([Hug89])和1990([Hug90])年,本文基于原始备忘录。


#### Abstract
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

