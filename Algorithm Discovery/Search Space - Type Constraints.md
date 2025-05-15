## Program Representation
In [[General Symbolic Regression]], the object being evolved is a numeric function $f: \mathbb{R}^n \rightarrow \mathbb{R}$, typically represented as a tree composed of operators and variables.

In algorithm discovery, however, we evolve full programs instead. 
We represent algorithms as **Abstract Syntax Trees (ASTs)**.
- Each node is a primitive (if, for, +, swap)
- Leaves are terminals (constants, variables)
The AST must be **typed**, with each nod
e having a declared return type and input types.
## Type System
In order to enforce syntactic correctness, we define a **type system** for each primitive node:
- Arity (number of inputs)
- Input type
- Return type
Example Type Signatures:
- +: Arity=2. Input Types=Expr,Expr. Return Type=Expr.
- if: Arity=3. Input Types=Bool, Statement, Statement. Return Type=Statement
## AST Construction Rules
To construct a valid syntax tree:
1. Select a return type $r$ as the root goal (ex: Statement)
2. Sample a primitive $p$ that has return type $r$.
3. For each child in arity($p$), recursively build a subtree that returns type $r_i$, which is the i-th input type of $p$.
4. Leaves are terminals of appropriate return type.
5. 
