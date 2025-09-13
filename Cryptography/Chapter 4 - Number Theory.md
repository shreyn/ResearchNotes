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
- c