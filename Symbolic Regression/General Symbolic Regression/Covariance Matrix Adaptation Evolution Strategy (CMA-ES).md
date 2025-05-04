CMA-ES is a optimization algorithm for finding the minimum of an unknown, possibly rugged, non-differentiable function. 
## Overview
We have an expression like $f(x) = a\cdot x^2 + b \cdot x + c$. We want to find the values of $a, b, c$ that make the expression fit some data well.
CMA-ES treats this like **searching a landscape**. Each point in the landscape is a guess for the constants. 