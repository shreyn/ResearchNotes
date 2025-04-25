## Expression Trees Definition
We define the search space of SR as the set of all trees that are valid under a specified operator set $\mathcal{O}$ and terminal set $\mathcal{T}$.

An expression tree $T$ is a finite, ordered tree such that:
- Each internal node $v$ is labeled by an **operator** $o \in \mathcal{O}$, where each operator $o$ has an associated [[#Arity of Operators]] $arity(o) \in \mathbb{N}$. The number of children of $v$ is $arity(o)$. 
- Each leaf node is labeled by a **terminal** $t \in \mathcal{T}$, where:
	- $\mathcal{T} = \mathcal{X} \cup \mathcal{C}$, with $\mathcal{X}$ being variables and $\mathcal{C} \subseteq \mathbb{R}$ being constants
## Evaluation Function induced by a Tree
Given an expression tree $T$, it defines a function $f_T: \mathbb{R}^d \rightarrow \mathbb{R}$.

The value of $f_T(x)$ at any input $x$ is obtained by recursively evaluating the tree from its leaves up to its root. 
## Arity of Operators
#### Definition
The **arity** of an operator refers to the number of inputs that the operator requires.

Given an operator set $\mathcal{O}$, the arity function $arity : \mathcal{O} \rightarrow \mathbb{N}$ is defined for each $o \in \mathcal{O}$ by $arity(o) = k$
- the operator $o$ takes exactly $k$ arguments

Examples:
- Addition (+): arity(+) = 2, since it takes 2 arguments
- Sine (sin): arity(sin) = 1, sine it takes 1 argument
#### Arity and Tree Structure
The arity of an operator directly determines the number of children that a node in the tree must have. 
Therefore, arity is a **"degree constraint"**
- For a node $v$ labeled by operator $o$, its outdegree (number of children) satisfies $deg^+(v) = arity(o)$.
- For terminals (constants and variables), $arity(t) = 0, deg^+(v) = 0$. (also known as nullary operators)
#### Multi-arity operators
While most SR systems use unary and binary operators (arity of 1 or 2), **multi-arity operators** are possible:
- Ex: $mean(x_1, x_2, \dots, x_n)$ has arity $n$.
- In practice, to keep the tree representation simple, such operators are either:
	- unrolled into binary trees (ex: $(x_1 + x_2) / 2),
	- or handled explicitly
- 