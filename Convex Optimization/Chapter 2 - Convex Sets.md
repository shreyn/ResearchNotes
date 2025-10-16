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
**Affine hull**: 
Set of all affine combinations of points in $C \subseteq \mathbb{R}^n$ 
$$
\text{aff} \,C =  \{\theta_{1}x_{1} + \dots + \theta_{k}x_{k} | x_{1}, \dots, x_{k} \in C, \quad \theta_{1} + \dots + \theta_{k} = 1 \}
$$
- the smallest affine set that contains $C$
- if $S$ is an affine set, $C \subseteq S$, then aff $C \subseteq S$
### 2.1.3 Affine dimension and relative interior
**Affine Dimension**:
Dimension of the affine hull. 
- note: not consistent with other definitions of dimension
	- Ex: unit circle in $\mathbb{R}^2$. $\{ x \in \mathbb{R}^2 | x_{1}^2 + x_{2}^2 = 1 \}$
	- The affine hull is $\mathbb{R}^2$, so affine dim is 2. But the unit circle in $\mathbb{R}^2$ has dimension one.
- if the affine dimension of a set $C \subseteq \mathbb{R}^n$ is less than $n$, then the set lies in the affine set, so $\text{aff} C \neq \mathbb{R}^n$.

**Interior**: 
Imagine you are standing inside a region C. If you can move around slightly in every direction without stepping outside of C, then you are at an interior point of C.
$$
\text{int} C \{ x \in C | \exists r > 0, B(x,r) \subseteq C\}
$$
- $B(x,r) = \{ y \ | \ \lVert y-x \rVert \leq r \}$
	- A small ball centered at x, radius r.
- *You are not at the edge of the set, you still have some buffer zone of space around you*.
**But** in some sets, it is always a flat surface in some larger dimension.
- So if you were to try and move around in a 3D ball, while in a 2D line, you will obviously not be in the interior at any point.
- So $\text{int} C = \emptyset$ (it has no thickness)
Therefore... 

**Relative Interior**: 
Interior relative to $\text{aff} C$
$$
\text{relint} C = \{ x \in C | B(x,r) \cap \text{aff} C \subseteq C \}
$$
- $B(x,r) = \{ y \ | \  \lVert y-x \rVert \leq r\}$
	- the ball of radius $r \geq 0$, center $x$
- *It is the interior, but within the affine hull of a set*.
- **Relative boundary**: $\text{cl} C \setminus \text{relint} C$
	- $\text{cl} C$: closure of $C$
**Ex:**
In $\mathbb{R}^2$, a line:
$$
C = \{ (t,0) \ | \ 0 \leq t \leq 1 \}
$$
- Interior: none, since any ball around a point includes points above and below the line
- Affine hull: $x$-axis
- Relative Interior: points strictly between 0 and 1: $\text{relint} \ C = \{ (t,0) \ | \ 0 < t <1 \}$
- Relative boundary: endpoints (0,0), (1,0)
### 2.1.4 Convex Sets
**Convex Set**: 
If the line segment between any two points in $C$ lies in $C$$$
\theta x_{1} + (1-\theta)x_{2} \in C
$$
- $\theta \in [0,1]$
- In other words: every point in the set can be seen by every other point, along an unobstructed straight path that lies in the set.
![[Pasted image 20251009171327.png]]
**Convex Combination**: 
Affine combination, but **coefficients are also nonegative**!
$$
\theta_{1}x_{1} + \dots+\theta_{k}x_{k}
$$
- $\theta_{1} + \dots + \theta_{k} =1$
- $\theta_{i} \geq 0$
- Kind of like a mixture / weighted average of the points

**Convex Hull**: 
Set of all convex combinations of points in $C$
$$
\text{conv} \ C = \{ \theta_{1}x_{1} + \dots + \theta_{k}x_{k} \ | \ x_{i} \in C, \theta_{i} \geq 0, \theta_{1} + \dots + \theta_{k} = 1 \}
$$
- smallest convex set that contains $C$
	- So if $B$ is a convex set that contains $C$, then $\text{conv} \ C \subseteq B$.
### 2.1.5 Cones
**Cone**: 
A set $C$ is a cone / nonnegative homogenous, if for every $x \in C,  \ \theta \geq 0$, then $\theta x \in C$.
- *If for a point x in C, any positive multiple of x is also in C*
- *You can stretch a vector outward (away from origin), and it stays in the set*.
- 
**Convex Cone**: 
If it is convex and a cone: $\theta_{1}x_{1}+ \theta_{2}x_{2}\in C$, for $\theta_{1}, \theta_{2} \geq 0$.
- forms a 2D pie slice, with the apex 0, and edges ($\theta_{1} \ \text{or} \  \theta_{2} = 0$) passing through $x_{1}, x_{2}$
![[Pasted image 20251009171300.png]]
- *Take any two vectors in the cone, add any nonnegative weighted combo of them, you stay in the cone*.
**Conic Combination**: 
Convex combination (all nonnegative), but doesn't have to sum to 1:
$$
\theta_{1}x_{1} + \dots + \theta_{k}x_{k}
$$
- $\theta_{i}\geq 0$
*Convex combo is a bounded region between points, Conic combo is an unbounded region spreading from the origin.*

**Ex:**
Two vectors in $\mathbb{R}^2$:
- $x_{1} = (1,0), \ \ x_{2}= (0,1)$.
	- Convex combo: points in the line segment between them
	- Conic combo: all points in first quadrant ($\theta_{1}, \theta_{2} \geq 0$ of any size)

**Key Theorem**: 
$$
\text{A set } \ C \ \text{is a convex cone} \iff \text{it contains all conic combinations of its points}
$$
- so def of convex cone is: closed under all positive scaling and positive combinations.

**Conic Hull**:
Set of all conic combinations of points in $C$.
$$
\text{cone}(C) = \{ \theta_{1}x_{1} + \dots + \theta_{k}x_{k} \ | \ x_{i} \in C, \theta_{i} \geq 0 \}
$$
- smallest convex cone that contains $C$.
- Quadrant from two rays
- ![[Pasted image 20251009172530.png]]

### Summary:
- Affine sets: every line through points stays inside the set (extends to infinity)
	- Ex: lines and planes are affine
	- A square / triangle is not affine, since connecting points may extend outside
- Convex: region between points (line segment between them)
	- triangle is convex
	- A donut is not (space in the middle)
- NOTE: **All affine sets are convex!**

## 2.2 Important Examples
Some simple examples:
- $\emptyset$, a point, all of $\mathbb{R}^n$ are affine and convex subsets of $\mathbb{R}^n$
- Any line is affine
- A line segment is convex, but not affine
### 2.2.1 Hyperplanes and halfspaces
#### Hyperplane:
$$
\{ x \ | \ a^Tx = b \}
$$
- normal vector $a$ tells the direction of the plane (perp to plane) 
- $b$ tells how far the plane is from the origin
- Ex:
	- $a = [0, 0, 1]$, $b = 3$.
	- Then $a^T = b \implies 0x + 0y + 1z = 3\implies z = 3$
	- This is a horizontal plane $z = 3$
Another representation:
$$
\{ x \ | \ a^T (x-x_{0}) = 0 \}
$$
Another representation;
$$
x_{0} + a^\perp
$$
- *Hyperplane is made of an offset $x_{0}$ and all vectors orthogonal to the normal vector $a$*.
- ![[Pasted image 20251015155411.png]]
#### Halfspaces:
A hyperplane divides $\mathbb{R}^n$ into two halfspaces.
$$
\{ x \ | \ a^T x \leq b \}
$$
![[Pasted image 20251015155612.png]]
- Notes: **halfspaces are convex, but not affine.**
Another representation:
$$
\{ x \ | \ a^T(x-x_{0}) \leq 0 \}
$$
- *The halfspace is a point/vector $x_{0}$ and all vectors that are obtuse/right angle with the normal vector $a$.*
- ![[Pasted image 20251015155903.png]]
### 2.2.2 Euclidean balls and ellipsoids
#### Euclidean ball
$$
B(x_{c}, r) = \{ x \ | \ \lVert x-x_{c} \rVert _{2} \leq r \} = \{ x \ | \ (x-x_{c})^T (x-x_{c}) \leq r^2 \}
$$
- *So contains all points within distance $r$ of center $x_{c}$*.
**Ball is convex!**
#### Ellipsoid
$$
E = \{  x \ | \ (x-x_{c})^T P^{-1}(x-x_{c}) \leq 1 ) \}
$$
- $x_{c}$ is center
- $P$ is a PSD matrix that tells how far the ellipsoid extends in each direction from $x_{c}$
	- length of axes are $\sqrt{ \lambda_{i} }$
- A general form of a ball!
	- $P = r^2 I$
**Ellipsoid is convex!**

### 2.2.3 Norm balls and norm cones
Just a general form of the euclidean ball?
### 2.2.4 Polyhedra
**Def**: solution set of a finite number of linear equalities and inequalities:
$$
\mathbf{P} = \{  x \ | \ a_{j}^T x \leq b_{j}, \ j \}
$$