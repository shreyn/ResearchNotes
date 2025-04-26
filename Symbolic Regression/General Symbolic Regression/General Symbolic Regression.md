## What is SR?
- In general regression, we assume we know the form of $f$ (linear, polynomial, etc.), and the goal is to find parameters.
	- Ex: $y = ax + b$, and we tune $a, b$ to minimize error between predictions and actual $y$
- Symbolic Regression removes this assumption that the structure of the function $f$ is known
	- Searches simultaneously for both the structure of the equation and the parameters within the structure.
	- Therefore, search space is space of all valid symbolic mathematical expressions.
- Formal Problem Statement:
	- Given:
		- Input-Output data $D$
		- A set of valid operators $O$ (ex. $+, -, \times, sin, cos,$ etc)
		- A set of terminals (variables $x_i$ and constants)
	- Find:
		- An expression $f$ built from $O$ and terminals, s.t. error is minimized
## [[SR Search Space - Expression Trees]]
- Mathematical expressions can be represented as trees:
	- nodes are operators
	- leaves are variables / constants
- Ex: $f(x) = sin(x) + 3x$
	- Tree form:
		- + {sin{$x$}, $\times\{3, x\}$}
- The set of all such trees generated from $O$ and the terminals forms the search space.
- To search through this space, we use a type of [[Evolutionary Algorithm]] called Genetic Programming (GP), where the individuals in the population are expression trees.
## Objective Function
- The fitness of a candidate express $f$ is evaluated using a loss function that includes a complexity penalty:
$$L(f) = Error(f, D) + \lambda \cdot Complexity(f)$$
- where $Complexity$ measures tree size, depth, or number of operators, and $\lambda$ controls the trade-off between accuracy and simplicity

## Why SR?
- Interpretability: Human readable functions, not black box models like most ML
- Scientific discovery
- Extrapolation: May recover the true underlying relationship
## Challenges
- Search Space explosion ([[SR Search Space - Expression Trees]]): number of possible expression trees grows super-exponentially with depth and operator set size.
- Non-differentiable: discrete and non-smooth, so classical gradient methods for optimization don't apply
- [[Functional Equivalence and Redundancy (Semantics)]]: different expressions can represent the same function
	- Ex: $sin^2(x) + cos^2(x) = 1$