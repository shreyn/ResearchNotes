## Overall Goal
Given:
- Public key $(e,n)$
- Ciphertext $c = m^e \mod n$
	Can we recover the message $m$ without knowing the private exponent $d$?
## Håstad's Broadcast Attack
Suppose:
- the same message $m$ is sent to $e$ recipients
- each recipient has their own RSA public key $(e,n_i)$, where $n_i$ is pairwise coprime
Then,
1. For each $i=1, \dots , e$, we have
$$c_i \equiv m^e \mod n_i$$
- This means, 
	- $m^e \equiv c_1 \mod n_1$
	- $m^e \equiv c_2 \mod n_2$
		$\vdots$
	- $m^e \equiv c_e \mod n_e$
2. By Chinese Remainder Theorem,
	- If $x \equiv a_i \mod n_i$ for $i = 1, \dots, e$, and the $n_i$ are pairwise coprime, then there exists a unique solution modulo $N = n_1 \cdot n_2 \cdot \dots \cdot n_e$
- Therefore, $\exists C \in \mathbb{Z}$ s.t. :
	- $C \equiv c_1 \mod n_1$
		$\vdots$
	- $C \equiv c_e \mod n_e$
- And since $\equiv$ is an equivalence relation, by transitivity and symmetry,
	- $C \equiv m^e \mod n_i$
- And by CRT, 
	- $C \equiv m^e \mod N$
- But since $0 \leq m^e \leq N$ (because $m < n_e$ for all $e$), the modulus doesn't reduce anything, so:
	- $C = m^e$
- To find the message, we take the root:
	- $m = \sqrt[e]{C}$
## (Side-Channel) Timing Attacks
### Square-and-Multiply
What is Square and Multiply?: [[RSA Overview#Square-and-Multiply (Binary Exponentiation) Algorithm]]
#### Overview
- RSA Decryption is done through $m = c^d \mod n$.
	- For speed, this is typically implemented using the square-and-multiply algorithm.
- For each bit in $d_i$, we square the current val, and if $d_i = 1$, also multiply by $c$. Therefore, **execution time depends on the number and positions of 1s in $d$**.
- The attacker can therefore recover the private exponent $d$ by measuring how long decryption takes for different known ciphertexts.
#### Example:
Let: 
- $p = 5, q = 11, n= pq = 55$. 
- $\phi(n) = (p-1)(q-1) = 40$
- $e = 3$. $gcd(3, 40) = 1$
- Compute $d$ s.t. $ed \equiv 1 \mod 40$
	- $3 \cdot 27 \equiv 1 \mod 40$, so $d = 27$.
Square-And-Multiply:
- $d = 27 = 11011$ (in binary)
	- Square (always square first)
	- Multiply (bit = 1)
	- Multiply (bit = 1)
	- nothing (bit = 0)
	- Multiply (bit = 1)
	- Multiply (bit = 1)
- So we expect 4 multiplications, each taking a longer time
- 