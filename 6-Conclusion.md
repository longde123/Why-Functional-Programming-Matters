6 Conclusion
In this paper, we’ve argued that modularity is the key to successful programming. Languages which aim to improve productivity must support modular
programming well. But new scope rules and mechanisms for separate compilation are not enough - modularity means more than modules. Our ability to
decompose a problem into parts depends directly on our ability to glue solutions
together. To assist modular programming, a language must provide good glue.
Functional programming languages provide two new kinds of glue - higher-order
functions and lazy evaluation. Using these glues one can modularise programs
in new and exciting ways, and we’ve shown many examples of this. Smaller
and more general modules can be re-used more widely, easing subsequent programming. This explains why functional programs are so much smaller and
easier to write than conventional ones. It also provides a target for functional
programmers to aim at. If any part of a program is messy or complicated, the
programmer should attempt to modularise it and to generalise the parts. He
should expect to use higher-order functions and lazy evaluation as his tools for
doing this.
Of course, we are not the first to point out the power and elegance of higherorder functions and lazy evaluation. For example, Turner shows how both can
be used to great advantage in a program for generating chemical structures
[Tur81]. Abelson and Sussman stress that streams (lazy lists) are a powerful
tool for structuring programs [AS86]. Henderson has used streams to structure
functional operating systems [P.H82]. The main contribution of this paper is
to assert that better modularity alone is the key to the power of functional
languages.
It is also relevant to the present controversy over lazy evaluation. Some
believe that functional languages should be lazy, others believe they should
not. Some compromise and provide only lazy lists, with a special syntax for
constructing them (as, for example, in SCHEME [AS86]). This paper provides
further evidence that lazy evaluation is too important to be relegated to secondclass citizenship. It is perhaps the most powerful glue functional programmers
possess. One should not obstruct access to such a vital tool.
Acknowledgements
This paper owes much to many conversations with Phil Wadler and Richard
Bird in the Programming Research Group at Oxford. Magnus Bondesson at
Chalmers University, Goteborg pointed out a serious error in an earlier version of
one of the numerical algorithms, and thereby prompted development of many of
the others. This work was carried out with the support of a Research Fellowship
from the UK Science and Engineering Research Council.
22
References
[AS86] H. Abelson and G.J. Sussman. Structure and Interpretation of Computer Programs. MIT Press, Boston, 1986.
[Hug89] J. Hughes. Why Functional Programming Matters. Computer Journal, 32(2), 1989.
[Hug90] John Hughes. Why Functional Programming Matters. In D. Turner,
editor, Research Topics in Functional Programming. Addison Wesley,
1990.
[MTH90] R. Milner, M. Tofte, and R. Harper. The Definition of Standard ML.
MIT Press, 1990.
[oD80] United States Department of Defense. The Programming Language
Ada Reference Manual. Springer-Verlag, 1980.
[P.H82] P.Henderson. Purely Functional Operating Systems. 1982.
[Tur81] D. A. Turner. The Semantic Elegance of Applicative Languages. In
Proceedings 1981 Conference on Functional Languages and Computer
Architecture, Wentworth-by-the-Sea, Portsmouth, New Hampshire,
1981.
[Tur85] D. A. Turner. Miranda: A non-strict language with polymorphic
types. In Proceedings 1985 Conference on Functional Programming
Languages and Computer Architecture, pages 1–16, Nancy, France,
1985.
[Wir82] N. Wirth. Programming in Modula-II. Springer-Verlag, 1982.