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
- But since $0 \leq m^e \leq N$, the modulus doesn't reduce anything, so:
	- $C = m^e$
- To find the message, we take the root:
	- $m = \sqrt[e]{C}$
