## 2.1 Affine and Convex Sets
### 2.1.1 Lines and line segments
Suppose $x_1 \neq x_2$ are two points in $\mathbb{R}^n$. Points of the form $y = \theta x_1 + (1-\theta) x_2$, where $\theta \in \mathbb{R}$, form the line passing through $x_1, x_2$.
- $\theta$ = 0 : $y = x_2$, $\theta = 1$: $y = x_1$
Another way:
$$y = x_2 + \theta(x_1-x_2)$$
- $y$ is the sum of the base point $x_2$ and the direction $x_{1}-x_{2}$, scaled by the param $\theta$.
	- So $\theta$ gives the fraction of the way from $x_{2}$ to $x_{1}$
If $\theta \in [0,1]$, it is a line segment between $x_{1}, x_{2}$.
### 2.1.2 Affine Sets
A set $C \subseteq \mathbb{R}^n$ is **affine** if the line through any two distinct points in C lies in C.
$$
\forall x_{1}, x_{2} \in C, \theta \in \mathbb{R}, \quad \theta x_{1} + (1-\theta)x_{2}\in C
$$
- C contains the linear combination of any two points in C, *with the coefficients summing to 1*
Can be generalized to more than two points.
**Affine Combination**: $\theta_{1}x_{1}+\dots + \theta_{k}x_{k}=1$
- Using induction, can show that an affine set contains every affine combination of its points
$$
x_{1}, \dots , x_{k} \in C, \quad \theta_{1} + \theta_{k} = 1 \qquad \implies \theta_{1}x_{1} + \dots + \theta_{1}x_{k} \in C
$$
**A decomposition**: 
If $C$ is a affine set, and $x_{0} \in C$, then the set
$$
 V =C - x_{0} = \{x-x_{0} | x \in C\}
$$
is a subspace (closed under addition and scalar mult)
Therefore, the set $C$ can be expressed as:
$$
 C = V+ x_{0} = \{v + x_{0} | v \in V\}
$$
- a subspace plus an offset.
Note: the choice of $x_{0}$ doesn't matter. 
Dimension of C is the dimension of the subspace V
**In other words: every affine set is just a linear subspace shifted by some offset point**.
- constructing $V$ is just bringing the affine set back to the origin, so it is a subspace
- Captures the shape of C, but centered at origin
**Affine hull**: set of all affine combinations of points in $C \subseteq \mathbb{R}^n$ 
$$
\text{aff} \,C =  \{\theta_{1}x_{1} + \dots + \theta_{k}x_{k} | x_{1}, \dots, x_{k} \in C, \quad \theta_{1} + \dots + \theta_{k} = 1 \}
$$
- the smallest affine set that contains $C$
- if $S$ is an affine set, $C \subseteq S$, then aff $C \subseteq S$
- 