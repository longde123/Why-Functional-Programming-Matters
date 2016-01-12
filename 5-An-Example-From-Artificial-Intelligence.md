5 An Example from Artificial Intelligence
We have argued that functional languages are powerful primarily because they
provide two new kinds of glue: higher-order functions and lazy evaluation. In
this section we take a larger example from Artificial Intelligence and show how
it can be programmed quite simply using these two kinds of glue.
The example we choose is the alpha-beta “heuristic”, an algorithm for estimating how good a position a game-player is in. The algorithm works by
looking ahead to see how the game might develop, but avoids pursuing unprofitable lines.
Let game-positions be represented by objects of the type “position”. This
type will vary from game to game, and we assume nothing about it. There must
be some way of knowing what moves can be made from a position: assume that
there is a function
moves: position -> listof position
that takes a game-position as its argument and returns the list of all positions
that can be reached from it in one move. Taking noughts and crosses (tic-tactoe) as an example,
| | X| | |X| | |
-+-+- -+-+- -+-+- -+-+-
moves | | = [ | | , | | , |X| ]
-+-+- -+-+- -+-+- -+-+-
| | | | | | | |
| | O| | |O|
-+-+- -+-+- -+-+-
moves |X| = [ |X| , |X| ]
-+-+- -+-+- -+-+-
| | | | | |
15
This assumes that it is always possible to tell which player’s turn it is from a
position. In noughts and crosses this can be done by counting the noughts and
crosses, in a game like chess one would have to include the information explicitly
in the type “position”.
Given the function moves, the first step is to build a game tree. This is a
tree in which the nodes are labelled by positions, such that the children of a
node are labelled with the positions that can be reached in one move from that
node. That is, if a node is labelled with position p, then its children are labelled
with the positions in (moves p). A game tree may very well be infinite, if it
is possible for a game to go on for ever with neither side winning. Game trees
are exactly like the trees we discussed in section 2 - each node has a label (the
position it represents) and a list of subnodes. We can therefore use the same
datatype to represent them.
A game tree is built by repeated applications of moves. Starting from the
root position, moves is used to generate the labels for the sub-trees of the root.
Moves is then used again to generate the sub-trees of the sub-trees and so on.
This pattern of recursion can be expressed as a higher-order function,
reptree f a = node a (map (reptree f) (f a))
Using this function another can be defined which constructs a game tree from
a particular position
gametree p = reptree moves p
For an example, look at figure 1. The higher-order function used here (reptree) is
analogous to the function repeat used to construct infinite lists in the preceding
section.
The alpha-beta algorithm looks ahead from a given position to see whether
the game will develop favourably or unfavourably, but in order to do so it must
be able to make a rough estimate of the value of a position without looking
ahead. This “static evaluation” must be used at the limit of the look-ahead, and
may be used to guide the algorithm earlier. The result of the static evaluation
is a measure of the promise of a position from the computer’s point of view
(assuming that the computer is playing the game against a human opponent).
The larger the result, the better the position for the computer. The smaller the
result, the worse the position. The simplest such function would return (say)
+1 for positions where the computer has already won, -1 for positions where
the computer has already lost, and 0 otherwise. In reality, the static evaluation
function measures various things that make a position “look good”, for example
material advantage and control of the centre in chess. Assume that we have
such a function,
static: position -> number
Since a game-tree is a (treeof position), it can be converted into a (treeof number) by the function (maptree static), which statically evaluates all the positions
in the tree (which may be infinitely many). This uses the function maptree defined in section 2.
16
| |
-+-+-
gametree | |
-+-+-
| |
| |
-+-+-
= | |
-+-+-
| |
/ | \
/ | \
/ | \
/ | \
/ | \
/ | \
X| | |X| | |
-+-+- -+-+- -+-+-
| | | | |X|
-+-+- -+-+- -+-+-
| | | | | |
/|\ /|\ /\
... ... / \
/ \
/ \
O| | |O|
-+-+- -+-+-
|X| |X|
-+-+- -+-+-
| | | |
/|\ /|\
... ...
Figure 1: An Example of a Game-Tree.
17
Given such a tree of static evaluations, what is the true value of the positions
in it? In particular, what value should be ascribed to the root position? Not
its static value, since this is only a rough guess. The value ascribed to a node
must be determined from the true values of its subnodes. This can be done by
assuming that each player makes the best moves he can. Remembering that a
high value means a good position for the computer, it is clear that when it is
the computer’s move from any position, it will choose the move leading to the
sub-node with the maximum true value. Similarly, the opponent will choose the
move leading to the sub-node with the minimum true value. Assuming that the
computer and its opponent alternate turns, the true value of a node is computed
by the function maximise if it is the computer’s turn and minimise if it is not:
maximise (node n sub) = max (map minimise sub)
minimise (node n sub) = min (map maximise sub)
Here max and min are functions on lists of numbers that return the maximum
and minimum of the list respectively. These definitions are not complete because
they recurse for ever - there is no base case. We must define the value of a node
with no successors, and we take it to be the static evaluation of the node (its
label). Therefore the static evaluation is used when either player has already
won, or at the limit of look-ahead. The complete definitions of maximise and
minimise are
maximise (node n nil) = n
maximise (node n sub) = max (map minimise sub)
minimise (node n nil) = n
minimise (node n sub) = min (map maximise sub)
One could almost write down a function at this stage that would take a position
and return its true value. This would be:
evaluate = maximise . maptree static . gametree
There are two problems with this definition. First of all, it doesn’t work for
infinite trees. Maximise keeps on recursing until it finds a node with no subtrees
- an end to the tree. If there is no end then maximise will return no result. The
second problem is related - even finite game trees (like the one for noughts and
crosses) can be very large indeed. It is unrealistic to try to evaluate the whole
of the game tree - the search must be limited to the next few moves. This can
be done by pruning the tree to a fixed depth,
prune 0 (node a x) = node a nil
prune n (node a x) = node a (map (prune (n-1)) x)
(prune n) takes a tree and “cuts off” all nodes further than n from the root. If
a game tree is pruned it forces maximise to use the static evaluation for nodes
at depth n, instead of recursing further. Evaluate can therefore be defined by
evaluate = maximise . maptree static . prune 5 . gametree
18
which looks (say) 5 moves ahead.
Already in this development we have used higher-order functions and lazy
evaluation. Higher order functions reptree and maptree allow us to construct
and manipulate game trees with ease. More importantly, lazy evaluation permits
us to modularise evaluate in this way. Since gametree has a potentially infinite
result, this program would never terminate without lazy evaluation. Instead of
writing
prune 5 . gametree
we would have to fold these two functions together into one which only constructed the first five levels of the tree. Worse, even the first five levels may be
too large to be held in memory at one time. In the program we have written,
the function
maptree static . prune 5 . gametree
only constructs parts of the tree as maximise requires them. Since each part can
be thrown away (reclaimed by the garbage collector) as soon as maximise has
finished with it, the whole tree is never resident in memory. Only a small part
of the tree is stored at a time. The lazy program is therefore efficient. Since
this efficiency depends on an interaction between maximise (the last function in
the chain of compositions) and gametree (the first), it could only be achieved
without lazy evaluation by folding all the functions in the chain together into
one big one. This is a drastic reduction in modularity, but it is what is usually
done. We can make improvements to this evaluation algorithm by tinkering
with each part: this is relatively easy. A conventional programmer must modify
the entire program as a unit, which is much harder.
So far we have only described simple minimaxing. The heart of the alphabeta algorithm is the observation that one can often compute the value of maximise or minimise without looking at the whole tree. Consider the tree:
max
/ \
/ \
/ \
/ \
min min
/ \ / \
/ \ / \
1 2 0 ?
Strangely enough, it is unnecessary to know the value of the question mark
in order to evaluate the tree. The left minimum evaluates to 1, but the right
minimum clearly evaluates to something less than or equal to 0. Therefore the
maximum of the two minima must be 1. This observation can be generalised
and built into maximise and minimise.
The first step is to separate maximise into an application of max to a list of
numbers; that is, we decompose maximise as
19
maximise = max . maximise’
(Minimise is decomposed in a similar way. Since minimise and maximise are
entirely symmetrical we shall discuss maximise and assume that minimise is
treated similarly). Once decomposed in this way, maximise can use minimise’
rather than minimise itself, to discover which numbers minimise would take
the minimum of. It may then be able to discard some of the numbers without
looking at them. Thanks to lazy evaluation, if maximise doesn’t look at all of
the list of numbers, some of them will not be computed, with a potential saving
in computer time.
It is easy to “factor out” max from the definition of maximise, giving
maximise’ (node n nil) = cons n nil
maximise’ (node n l) = map minimise l
= map (min . minimise’) l
= map min (map minimise’ l)
= mapmin (map minimise’ l)
where mapmin = map min
Since minimise’ returns a list of numbers, the minimum of which is the result of
minimise, (map minimise’ l) returns a list of lists of numbers. Maximise’ should
return a list of the minima of those lists. However, only the maximum of this
list matters. We shall define a new version of mapmin which omits the minima
of lists whose minimum doesn’t matter.
mapmin (cons nums rest) =
= cons (min nums) (omit (min nums) rest)
The function omit is passed a “potential maximum” - the largest minimum seen
so far - and omits any minima which are less than this.
omit pot nil = nil
omit pot (cons nums rest) =
= omit pot rest, if minleq nums pot
= cons (min nums) (omit (min nums) rest), otherwise
Minleq takes a list of numbers and a potential maximum, and returns true if the
minimum of the list of numbers is less than or equal to the potential maximum.
To do this, it does not need to look at all the list! If there is any element in the
list less than or equal to the potential maximum, then the minimum of the list
is sure to be. All elements after this particular one are irrelevant - they are like
the question mark in the example above. Therefore minleq can be defined by
minleq nil pot = false
minleq (cons num rest) pot = true, if num<=pot
= minleq rest pot, otherwise
Having defined maximise’ and minimise’ in this way it is simple to write a new
evaluator:
20
evaluate = max . maximise’ . maptree static . prune 8 . gametree
Thanks to lazy evaluation, the fact that maximise’ looks at less of the tree
means that the whole program runs more efficiently, just as the fact that prune
looks at only part of an infinite tree enables the program to terminate. The
optimisations in maximise’, although fairly simple, can have a dramatic effect
on the speed of evaluation, and so can allow the evaluator to look further ahead.
Other optimisations can be made to the evaluator. For example, the alphabeta algorithm just described works best if the best moves are considered first,
since if one has found a very good move then there is no need to consider worse
moves, other than to demonstrate that the opponent has at least one good reply
to them. One might therefore wish to sort the sub-trees at each node, putting
those with the highest values first when it is the computer’s move, and those
with the lowest values first when it is not. This can be done with the function
highfirst (node n sub) = node n (sort higher (map lowfirst sub))
lowfirst (node n sub) = node n (sort (not.higher) (map highfirst sub))
higher (node n1 sub1) (node n2 sub2) = n1>n2
where sort is a general purpose sorting function. The evaluator would now be
defined by
evaluate = max . maximise’ . highfirst . maptree static .
prune 8 . gametree
One might regard it as sufficient to consider only the three best moves for the
computer or the opponent, in order to restrict the search. To program this, it
is only necessary to replace highfirst with (taketree 3 . highfirst), where
taketree n = redtree (nodett n) cons nil
nodett n label sub = node label (take n sub)
Taketree replaces all the nodes in a tree with nodes with at most n subnodes,
using the function (take n) which returns the first n elements of a list (or fewer
if the list is shorter than n).
Another improvement is to refine the pruning. The program above looks
ahead a fixed depth even if the position is very dynamic - it may decide to look
no further than a position in which the queen is threated in chess, for example.
It is usual to define certain “dynamic” positions and not to allow look-ahead
to stop in one of these. Assuming a function “dynamic” that recognises such
positions, we need only add one equation to prune to do this:
prune 0 (node pos sub) = node pos (map (prune 0) sub),
if dynamic pos
Making such changes is easy in a program as modular as this one. As we
remarked above, since the program depends crucially for its efficiency on an
interaction between maximise, the last function in the chain, and gametree, the
first, it can only be written as a monolithic program without lazy evaluation.
Such a program is hard to write, hard to modify, and very hard to understand