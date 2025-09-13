## 4.1 Modular Arithmetic
### Congruence Modulo $n$
Two integers are **congruent modulo $n$** if: 
$$a \equiv b\mod n$$
- means $n | (a-b)$
#### Equivalence Classes
Congruence Modulo is an equivalence relation (R, S, T), so it **partitions integers**.
Ex: $n = 5$
- 0: ..., -5, 0, 5, ...
- 1: ..., -4, 1, 6, ...
**Integers modulo** $n$:
$$\mathbb{Z}_n = \{0,1,2, \dots, n-1\}$$
### Addition and Multiplication in $\mathbb{Z}_n$
Just add/multiply, divide by $n$, take remainder.
#### Modular additive inverse
For any $a \in \mathbb{Z}_n$, there exists an element $b$ s.t.$$a + b \equiv 0 \mod n$$
- $b$ is the **additive inverse** of $a$, since $b = (n-a) \mod n$
- Ex: for $n=7$, the additive inverse of $5$ is 2, since $5 + 2 = 7 \equiv 0 \mod 7$
#### Modular multiplicative inverse
For any $a \in \mathbb{Z}_n$, the **multiplicative inverse** $a^{-1}$ is an element satisfying:
$$a \cdot a^{-1} \equiv 1 \mod n$$
- Ex: for $n = 7$, $3^{-1}= 5$ since $3 \cdot 5 = 15 \equiv 1 \mod 7$
**Existence of Multiplicative Inverse Theorem**:
An element $a \in \mathbb{Z}_n$ has a mult. inverse iff $\gcd(a,n) = 1$.
- $a$ is coprime to $n$
- if $n$ is prime, every element has an inverse.
**How to find Multiplicative Inverses**:
$$a \cdot a^{-1} \equiv 1 \mod n $$
is equivalent to solving
$$a \cdot x + n \cdot y = 1$$
- Linear Diophantine equation
##### Extended Euclidean Algorithm
- Same as euclidean algo, but also in reverse to solve for $x$
## 4.2 Fermat's Little Theorem
If $p$ is prime, and $a \in \mathbb{Z}$  s.t. $\gcd(a,p) = 1$ , then:
$$a^{p-1} \equiv 1 \mod p$$
which is equivalent to:
$$a^p \equiv a \mod p$$
- Proof:
	- Since $p$ is prime, the residues modulo $p$ is $\{1, 2, \dots, p-1\}$.
		- Every number is coprime to $p$
	- Multiple this set by $a$, reduce modulo $p$.
	- Since $gcd(a,p) = 1$, multiplying by $a$ doesn't create duplicates modulo $p$
		- Each result is still between 1 and $p-1$. 
		- So $\{a, 2a, 3a, \dots, a(p-1)\}$ is just a rearrangement of the original set mod p
	- Now multiple both sets. Since they are both just permutations, the two products are congruent mod p
		- $(1a)(2a)(3a) \dots (a\cdot (p-1))  = a^{p-1} \cdot (1 \cdot 2 \dots (p-1)) \equiv (1 \cdot 2 \dots (p-1)) \mod p$
	- Since $(1 \cdot 2 \dots (p-1))$ is not divisible by $p$ (none of the terms are multiples of p), cancel out of modulo $p$
		- Therefore, $a^{p-1} \equiv 1 \mod p$
## 4.3 Euler's Theorem
If $n > 1$ and $\gcd(a,n)=1$, then:
$$a^{\phi(n)} \equiv 1 \mod n$$
, where $\phi(n)$ is **Euler's totient function** (number of integers up to $n$ that are coprime to $n$)

Connection to Fermat's Little Theorem:
- If $n$ is prime, then $\phi(n) = n-1$.
- Then Euler's theorem reduces to FLT.

Example:
- Let $n = 10$. 
	- Coprime numbers: $\{1, 3, 7, 9\}$, so $\phi(10) = 4$.
- Check with $a = 3$ (gcd(3, 10) = 1):
	- $3^4 = 81 \equiv 1 \mod 10$
- Check with $a = 7$:
	- $7^4 =  2401 \equiv 1 \mod 10$
## 4.4 Chinese Remainder Theorem
Suppose you have a series of modular equations.
CRT says that *if the moduli are all pairwise coprime, then there exists a unique solution modulo the product of the moduli*.

General Statement:
Let $n_1, n_2, \dots, n_k$ be pairwise coprime. Then the system
$$x \equiv a_1 \mod n_1,\quad x \equiv a_2 \mod n_2, \quad\dots, \quad x \equiv a_k \mod n_k$$
has a unique solution module $N = n_1 \cdot n_2 \dots n_k$.

### Intuition
- Imagine two clocks, one with $m$ hours, one with $n$ hours. 
- They only show the same time once every $m\cdot n$ hours. 
- That is why the solution is unique modulo $m\cdot n$
