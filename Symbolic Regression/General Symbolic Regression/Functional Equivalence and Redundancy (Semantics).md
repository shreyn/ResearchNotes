## Motivation
We search over a very large set of valid functions. 
However, multiple different expression trees can represent the exact same mathematical function.
This means that the search space contains significant redundancy. 
- Ex: Tree 1: $f(x) = x + 0$. Tree 2: $f(x) = x$. These are structurally different, but they define the same exact function.
Two levels of equivalence:
- Syntactic equivalence 
	- Tree structure is the same
	- Ex: $x + y$ vs. $y + x$
- Semantic equivalence:
	- Functions produce the same outputs for all inputs
	- Ex: $sin^(x) = cos^2(x) = 1$
In SR, **semantic equivalence** is more important since we care about what the function does, not how it is written.

Semantic equivalence is challenging because it is in general undecidable over arbitrary functions.
- Some identities are known (basic algebraic simplifications, trig identities)
- But there is no general algorithm to prove equivalence for all pairs of arbitrary expressions
