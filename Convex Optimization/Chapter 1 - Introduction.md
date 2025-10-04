## 1.1 Mathematical Optimization
Has the form: minimize $f_0(x)$, subject to $f_i ≤ b_i, i = 1, \dots, m$.
- **optimization variable:** vector $x = (x_1, \dots, x_n)$
-  **objective function**: $f_0 : \mathbb{R}^n \rightarrow \mathbb{R}$ 
- **constraint functions**: functions $f_i: \mathbb{R}^n \rightarrow \mathbb{R}$
- **limits / bounds**: constants $b_1, \dots, b_m$
- **optimal vector**: $x^*$ s.t. it has the smallest objective value among all vectors that satisfy the constraints
	- *A kind of $\sup$ of the set of valid vectors*
**Convex optimization**: objective and constraint functions are convex $$f_i(\alpha x + \beta y) \leq \alpha f_i(x) + \beta f_i (y)$$
- $x, y \in \mathbb{R}^n, \forall \alpha, \beta \in \mathbb{R}_{\geq 0}$, with $\alpha + \beta = 1$ 
- Note: **linear programs** are a subset of convex optimization problems (equality versus inequality)
### 1.1.1 Applications
A solution to an optimization problem represents a *choice* that has a *minimum cost / maximum utility*, among all *choices that meet the requirements*.
- Portfolio Optimization: want to find the best way to invest some budget, across a series of assets, with a measure of risk. 
- Data Fitting: want to find a model that best fits some observed data, with some limits on the params, and with a measure of error.
- Decision making: widely used in engineering, automatic control systems, network design, finance, supply chain. 
- **Embedded Optimization:** automatically makes real-time choices, with little / no human intervention.
### 1.1.2 Solving optimization problems
**Solution method**: algorithm that computes a solution of the problem, given a particular problem from the class (instance of the problem).
- In general, optimization problems are difficult to solve. 
- Some problems, however, can be solved reliably. 
## 1.2 Least-squares and Linear Programming
### 1.2.1 Least-squares problems
A **least-squares** problem is an optimization problem with no constraints ($m= 0$), and an objection which is the sum of squares of terms of the form $a_i^T x - b_i$:
$$\text{minimize} \hspace{1em} f_0(x)= || Ax-b||^2_2 = \sum^k_{i=1} (a_i^T x - b_i)^2$$
- $A \in \mathbb{R}^{k \times n}$ $(k \geq n)$, $a_i^T$ are the rows of A, $x \in \mathbb{R}^n$ is the optimization variable.
- *Basically, want to pick an $x$ s.t. $Ax = b$ is as close as possible*
	- each row $a_i^T$ represents one linear equation
	- $b$ is the observed outputs
	- $x$ are the coefficients we want to find
- $Ax$ is the projection of $b$ onto the column space of $A$, and the solution $x^*$ is the one that makes $Ax$ the closest possible vector
**Solving least-squares problem**
Solve
$$(A^TA)x = A^T b$$
so the solution is $x = (A^TA)^{-1}A^Tb$
- Time complexity: $n^2 k$
**Using least-squares**
- To recognize an optimization problem as least squares, check that the *objective function is quadratic, positive semidefinite*
- **Weighted least-squares**
$$\sum^k_{i=1}w_i(a_i^T x - b_i)^2$$
	- $w_1, \dots, w_k$ are positive
	- Weights are used to influence a solution
### 1.2.2 Linear Programming
Objection and constraint functions are linear:
$$\text{minimize} \hspace{1em} c^Tx$$
$$\text{subject to} \hspace{1em} a_i^T x \leq b_i, \hspace{1em} i = 1,\dots, m $$
- Complexity: $n^2 m$
## 1.3 Convex Optimization
**Convex optimization**: objective and constraint functions are convex $$f_i(\alpha x + \beta y) \leq \alpha f_i(x) + \beta f_i (y)$$
- $x, y \in \mathbb{R}^n, \forall \alpha, \beta \in \mathbb{R}_{\geq 0}$, with $\alpha + \beta = 1$ 
Using convex optimization is difficult.
- If you can formulate a practical problem as a convex optimization problem, then you have solved the problem
## 1.4 Nonlinear Optimization
Objective or constraint functions are not linear, and not known to be convex. 
- No effective methods for solving the general nonlinear problem
### 1.4.1 Local Optimization
We compromise, and instead of finding an $x$ that minimizes over all feasible points, we find a point that is **locally optimal**. 
- minimizes objective function among feasible points around it, but not guaranteed to have lower objective value that other feasible points
Local optimization methods are widely used. 
Some disadvantages:
- requires an initial guess for the optimization variable
- Little info is provided about how far from global optimal the local solution is
- Sensitive to param values
So local optimization is more of an art than a science. 
**Interesting tradeoff:**
- formulating a practical problem as a nonlinear optimization problem is easy, but solving it is hard
- Formulating a problem into convex optimization is hard, but solving is easy
### 1.4.2 Global Optimization
The true global solution is found, but the compromise is efficiency.
- worst case complexity grows exponentially with the problem size


