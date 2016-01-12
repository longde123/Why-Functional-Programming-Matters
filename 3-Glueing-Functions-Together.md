## functions整合

The first of the two new kinds of glue enables simple functions to be glued
together to make more complex ones. 

>
第一种整合方式是将简单的函数整合在一起，生成更复杂的函数。


It can be illustrated with a simple list processing problem - adding up the elements of a list. We define lists by

	listof X ::= nil | cons X (listof X)

which means that a list of Xs (whatever X is) is either nil, representing a list with no elements, or it is a cons of an X and another list of Xs. A cons represents a list whose first element is the X and whose second and subsequent elements are the elements of the other list of Xs.X here may stand for any type - for example, if X is “integer” then the definition says that a list of integers is either empty or a cons of an integer and another list of integers. Following normal practice, we will write down lists simply by enclosing their elements in square brackets, rather than by writing conses and nils explicitly. This is simply a shorthand for notational convenience. For example,

```
[]		means 	nill
[1]		means	cons 1 nill
[1,2,3]	means	cons 1 (cons2 (cons 3 nill))
```

The elements of a list can be added up by a recursive function sum. Sum must be defined for two kinds of argument: an empty list (nil), and a cons. Since the sum of no numbers is zero, we define 

	sum nil = 0
	
and since the sum of a cons can be calculated by adding the first element of the
list to the sum of the others, we can define

	sum (cons num list) = num + sum list
	
Examining this definition, we see that only the boxed parts below are specific
to computing a sum.

```
			  +---+
	sum nil = | 0 |
	          +---+
	                          +---+
	sum (cons num list) = num | + | sum list	
	                          +---+
```
This means that the computation of a sum can be modularised by glueing together a general recursive pattern and the boxed parts. This recursive pattern is conventionally called reduce and so sum can be expressed as 

	sum = reduce add 0
	
where for convenience reduce is passed a two argument function add rather than
an operator. Add is just defined by

	add x y = x + y
	
The definition of reduce can be derived just by parameterising the definition of
sum, giving

	(reduce f x) nil = x
	(reduce f x) (cons a l) = f a ((reduce f x) l)
	
Here we have written brackets around (reduce f x) to make it clear that it replaces sum. Conventionally the brackets are omitted, and so ((reduce f x) l) is written as (reduce f x l). A function of 3 arguments such as reduce, applied to only 2 is taken to be a function of the one remaining argument, and in general, a function of n arguments applied to only m(< n) is taken to be a function of the n − m remaining ones. We will follow this convention in future. Having modularised sum in this way, we can reap benefits by re-using the parts. The most interesting part is reduce, which can be used to write down a function for multiplying together the elements of a list with no further programming:

	product = reduce multiply 1
	
It can also be used to test whether any of a list of booleans is true

	anytrue = reduce or false
	
or whether they are all true
alltrue = reduce and true
One way to understand (reduce f a) is as a function that replaces all occurrences
of cons in a list by f, and all occurrences of nil by a. Taking the list [1,2,3] as
an example, since this means
cons 1 (cons 2 (cons 3 nil))
then (reduce add 0) converts it into
add 1 (add 2 (add 3 0)) = 6
and (reduce multiply 1) converts it into
multiply 1 (multiply 2 (multiply 3 1)) = 6
Now it is obvious that (reduce cons nil) just copies a list. Since one list can be
appended to another by consing its elements onto the front, we find
append a b = reduce cons b a
5
As an example,
append [1,2] [3,4] = reduce cons [3,4] [1,2]
= (reduce cons [3,4]) (cons 1 (cons 2 nil))
= cons 1 (cons 2 [3,4]))
(replacing cons by cons and nil by [3,4])
= [1,2,3,4]
A function to double all the elements of a list could be written as
doubleall = reduce doubleandcons nil
where doubleandcons num list = cons (2*num) list
Doubleandcons can be modularised even further, first into
doubleandcons = fandcons double
where double n = 2*n
fandcons f el list = cons (f el) list
and then by
fandcons f = cons . f
where “.” (function composition, a standard operator) is defined by
(f . g) h = f (g h)
We can see that the new definition of fandcons is correct by applying it to some
arguments:
fandcons f el = (cons . f) el
= cons (f el)
so fandcons f el list = cons (f el) list
The final version is
doubleall = reduce (cons . double) nil
With one further modularisation we arrive at
doubleall = map double
map f = reduce (cons . f) nil
where map applies any function f to all the elements of a list. Map is another
generally useful function.
We can even write down a function to add up all the elements of a matrix,
represented as a list of lists. It is
summatrix = sum . map sum
6
The map sum uses sum to add up all the rows, and then the left-most sum adds
up the row totals to get the sum of the whole matrix.
These examples should be enough to convince the reader that a little modularisation can go a long way. By modularising a simple function (sum) as a
combination of a “higher order function” and some simple arguments, we have
arrived at a part (reduce) that can be used to write down many other functions
on lists with no more programming effort. We do not need to stop with functions on lists. As another example, consider the datatype of ordered labelled
trees, defined by
treeof X ::= node X (listof (treeof X))
This definition says that a tree of Xs is a node, with a label which is an X, and
a list of subtrees which are also trees of Xs. For example, the tree
1 o
/ \
/ \
/ \
2 o o 3
| | |
o 4
would be represented by
node 1
(cons (node 2 nil)
(cons (node 3
(cons (node 4 nil) nil))
nil))
Instead of considering an example and abstracting a higher order function from
it, we will go straight to a function redtree analogous to reduce. Recall that
reduce took two arguments, something to replace cons with, and something to
replace nil with. Since trees are built using node, cons and nil, redtree must
take three arguments - something to replace each of these with. Since trees and
lists are of different types, we will have to define two functions, one operating
on each type. Therefore we define
redtree f g a (node label subtrees) =
f label (redtree’ f g a subtrees)
redtree’ f g a (cons subtree rest) =
g (redtree f g a subtree) (redtree’ f g a rest)
redtree’ f g a nil = a
Many interesting functions can be defined by glueing redtree and other functions
together. For example, all the labels in a tree of numbers can be added together
using
7
sumtree = redtree add add 0
Taking the tree we wrote down earlier as an example, sumtree gives
add 1
(add (add 2 0)
(add (add 3
(add (add 4 0) 0))
0))
= 10
A list of all the labels in a tree can be computed using
labels = redtree cons append nil
The same example gives
cons 1
(append (cons 2 nil)
(append (cons 3
(append (cons 4 nil) nil))
nil))
= [1,2,3,4]
Finally, one can define a function analogous to map which applies a function f
to all the labels in a tree:
maptree f = redtree (node . f) cons nil
All this can be achieved because functional languages allow functions which are
indivisible in conventional programming languages to be expressed as a combination of parts - a general higher order function and some particular specialising
functions. Once defined, such higher order functions allow many operations to
be programmed very easily. Whenever a new datatype is defined higher order
functions should be written for processing it. This makes manipulating the
datatype easy, and also localises knowledge about the details of its representation. The best analogy with conventional programming is with extensible
languages - it is as though the programming language can be extended with
new control structures whenever desired