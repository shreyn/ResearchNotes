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
## Context-Free Grammar (CFG)
#### Why do we need syntax rules?
Not every arrangement of operators results in a valid expression
**Operators require a specific number of inputs (arity), and there must be clear rules for how these inputs are connected**
#### CFG Formal Definition
$$G = (\mathcal{V}, \Sigma, R, S)$$
where, 
- $\mathcal{V}$: finite set of non-terminal symbols (abstract placeholders like "Expression")
- $\Sigma$: finite set of terminal symbols (operators $\mathcal{O}$ and terminals $\mathcal{T}$)
- $R$: finite set of production rules, which specify how non-terminals can be replaced by sequences of terminals and non-terminals
- $S \in \mathcal{V}$: start symbol, representing a complete expression
#### Example
**First defining the CFG:** 
Operator set: $\mathcal{O} = \{+, \times, sin\}$
Terminals set: $\mathcal{T} = \{x, c\}$, where $x$ is a variable and $c$ is a constant.
Non-terminal set: $\mathcal{V} = \{E\}$, where $E$ stands for "Expression".
Start symbol: $S = E$.
Production rules $R$:
1. $E \rightarrow E + E$   (addition is binary, arity 2)
2. $E \rightarrow E \times E$   (multiplication is binary, arity 2)
3. $E \rightarrow sin(E)$   (sine is unary, arity 1)
4. $E \rightarrow x$    (variable, arity 0)
5. $E \rightarrow c$    (constant, arity 0)
**Deriving Expressions using the Grammar**
Lets use the above rules to derive
$$sin(x) + c$$
Derivation Steps:
	1. Start with start symbol $E$
	2. Apply rule (1): 
$$E \rightarrow E + E$$
	3. Replace the left $E$ using rule (3): $E \rightarrow sin(E)$:
$$sin(E) + E$$
	4. Replace the inner $E$ of sin using rule (4): $E \rightarrow x$:
$$sin(x) + E$$
	5. Replace the right $E$ using rule (5): $E \rightarrow c$:
$$sin(x) + c$$
**CFG is built based on the arity**.

**Expression Trees are built correctly because the grammar forces that the arity is correct.**
## Size of Search Space (Tree Enumeration)
We have formalized the search space structurally. The next question is how large is this space?

Understanding the size of the search space is critical because it impacts
- The feasibility of brute force search,
- The need for heuristic and stochastic methods (like GP),
- The design of complexity penalties to avoid exploring unnecessarily large regions.
#### Counting Binary Expression Trees
We consider the case where all operators are binary (arity 2).
A full binary tree is a tree where each internal node has exactly two children, and leaves are terminals (variables or constant).
**Catalan Number Formula**:
The number of distinct full binary trees with $n$ internal nodes and $n+1$ leaves is given by the $n$-th Catalan number:
$$C_n = \frac{1}{n+1} \binom{2n}{n}$$
As $n$ increases, $C_n$ grows roughly like $4^n / n^{3/2}$.

So far, Catalan numbers only count **unlabeled tree structures** (just the shape).
In SR, however, the internal nodes are labeled with operators, and leaves are labeled from terminals.
Therefore, the total number of labeled trees is:
$$N_n = C_n \cdot |\mathcal{O}^n| \cdot |\mathcal{T}^{n+1}|$$
- $C_n$: number of binary tree shapes with $n$ internal nodes,
- $|\mathcal{O}^n|$: choices of operators for the $n$ internal nodes,
- $|\mathcal{T}^{n+1}|$: choices of terminals for the $n+1$ leaves.
This shows just how the number of possible expression trees explodes combinatorially with $n. (and this is just with binary operators. Unary operators can chain indefinitely).
#### Why does this explosion matter for Search
The rapid growth in search space size means that:
- Exhaustive enumeration of trees is infeasible very quickly,
- Intelligent search strategies like GP are necessary,
- Complexity control is critical to make SR practical.

## Complexity Control
As we have seen, the number of possible expression trees grows super-exponentially with tree size due to tree structure (Catalan), operator and terminal labeling, and unary operators.
Without constraints, the search process can easily:
- Overfit the data by producing overly complex functions that memorize rather than generalize.
- Spend resources exploring unnecessarily large and complicated regions.
This motivates the need for complexity control.
#### How to Measure Complexity
- Tree Size
	- Total number of nodes (internal + leaves)
	- Larger trees tend to be more complex
- Tree Depth
	- Length of longest path from root to leaf 
	- Deep trees reflect nested operations
- Operator weights
	- Assign weights to different operators (ex. ^ may cost more than +)
	- Penalizes "harder" operations
- Subtree repetition
	- Measure how many subtrees repeat structurally
#### Complexity in the Fitness Function
There is regular Error metric, but also a complexity metric $Complexity(T)$.
The objective function becomes:
$$\mathcal{L}(T) = Error(T, D) + \lambda \cdot Complexity(T)$$
where $\lambda$ is a hyperparameter that controls the trade-off between accuracy and simplicity.
- Some systems dynamically adjust $\lambda$ during the search process (adaptive parsimony)
#### Alternative: Multi-Objective Approach
Instead of incorporating complexity into the fitness function, SR can be framed as a multi-objective optimization problem:
$$min(Error(T, D), Complexity(T))$$
Solutions are evaluated based on Pareto dominance.
## Exploring through the Space
We use [[Evolutionary Algorithm]]s to search through this space. 
#### Tree Mutation
Mutation occurs within a single individual (one tree). 
An important aspect of mutation is that it **changes part of the tree while keeping the overall expression syntactically valid.**
##### Common Strategies:
- Subtree Mutation
	- Replace a randomly chosen subtree with a newly generated random subtree 
	- Ex: replace $sin(x)$ with $x+c$
- Node Replacement (Operator Mutation)
	- Replace an operator at a randomly chosen internal node with another operator of the same arity. 
	- Ex: change $+$ to $\times$ at a certain node
- Constant Pertubation
	- Slightly change a constant value at a leaf node
	- Ex: change $c = 3.0$ to $3.1$
- Terminal Replacement
	- Replace one variable or constant with another terminal
	- Ex: change $x$ to $y$
##### How does mutation maintain syntax validity?
Mutation can replace or modify subtrees safely as long as:
- Any replacement operator has the same arity,
- The inserted subtree is itself valid (which is ensured if it is generated by the CFG)
So mutation should never produce invalid trees as long as these conditions are respected.
##### Subtree Mutation (most common)
The most widely used mutation strategy is subtree mutation.
1. Select a random node in the tree (internal or leaf)
2. If the node is:
	- internal, then its entire subtree is selected
	- leaf, then the leaf itself is selected
3. Generated a new random subtree (according to CFG and arity rules)
4. Replace the selected subtree with this new structure
Why is this powerful?
- It allows for small mutations (leaf replacement), as well as large mutations (if a deep internal node is replaced)
#### Tree Crossover
##### Subtree Crossover
1. Select two parent trees $T_1$ and $T_2$
2. Randomly select one crossover point (node) in each tree
3. Swap the selected subtrees between the two parent trees.
4. Produce one or two offspring trees (depending on whether you generate both swapped versions or just one)
##### Preservation of syntax validity
- Each swapped piece is a subtree, which, by definition, is already a valid expression,
- Arity is respected at each node, since entire subtrees are swapped, not partial fragments
