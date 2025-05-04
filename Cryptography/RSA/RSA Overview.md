RSA is in the category of asymmetric cryptography (public and private keys).
- uses a public key for encryption, and a private key for decryption
- Anyone can encrypt, but only the owner can decrypt
## Key Generation
We want to generate two keys:
- public key $(e, n)$: used for encryption
- private key $(d,n)$: used for decryption
We do this through the following steps:
1. Choose two large prime numbers
	- Let $p$ and $q$ be distinct large primes
	- Then compute $n = p\cdot q$
2. Compute Euler's Totient Function
	- $\phi(n) = \phi(pq) = (p-1)(q-1)$ 
	- We do this because according to Euler's Theorem, 
		- If $\gcd(m,n) = 1$, then $m^{\phi(n)} \equiv 1 \mod n$ 
3. Choose a public exponent $e$
	- Pick an integer $e$ s.t.:
		- $1 < e < \phi(n)$
		- $\gcd(e, \phi(n)) = 1$ (ie. $e$ and $\phi(n)$ are coprime) 
4. Compute private exponent $d$
	- This is the modular inverse of $e$ modulo $\phi(n)$, meaning:
		- $d \cdot e \equiv 1 \mod \phi(n)$
	- This is computed using Reverse Euclidean Algorithm
At this point, we have:
- public key $(e,n)$
- private key (d, n)
## RSA Encryption
Suppose someone wants to send you a message $m \in \mathbb{Z}$. They do:
$$c = m^e \mod n$$
Where:
- $m$: original message (as an integer < n)
- $c$: ciphertext
- $e$: public exponent
- $n$: public modulus
## RSA Decryption
You, the owner of the private key, compute:
$$m = m^{e^d} = c^d \mod n$$
Because,
$$m^{ed} \equiv m \mod n$$
by Euler's Theorem, as long as $\gcd(m,n) = 1$. 
## Why Decryption works
We've built a $d$ s.t.:
$$d \cdot e \equiv 1 \mod \phi(n) \Rightarrow d \cdot e = 1 + k\cdot \phi(n)$$
Then:
$$c^d = m^{e^d} = m^{ed} = m^{1 + k\phi(n)} = m \cdot (m^{\phi(n)})^k$$ And since $m^{\phi(n)} \equiv 1 \mod n$, we get:
$$m \cdot (1)^k \equiv m \mod n$$
So decryption recovers the original message.
## Example
1. Choose two distinct primes
	- $p = 5$
	- $q = 11$
	- $n = pq = 5\cdot 11 = 55$
2. Compute Euler's Totient
	- $\phi(n) = \phi(55) = (p-1)(q-1) = (5-1)(11-1) = 40$
3. Choose a public exponent $e$
	- We need $1 < e < \phi(n) = 40$ s.t. $\gcd(e, 40) = 1$
	- Let's pick $e = 3$. $\gcd(3, 40) = 1$
4. Compute private exponent $d$
	- We want a $d$ s.t.:
		- $e \cdot d \equiv 1 \mod \phi(n)$ (ie. $d$ is the modular inverse of $e\mod \phi(n)$)
	- So we want:
		- $d \cdot e \equiv 1 \mod 40$
		- $d = 27$ (from extended EA)
Keys:
- public key: $(e,n) = (3,55)$
- private key: $(d,n) = (27,55)$

Suppose someone wants to send the message:
$$m = 12$$
To encrypt it using the public key $(e = 3, n = 55)$, they compute:
$$c = m^e \mod n = 12^e \mod 55 = 1728 \mod 55$$
$\Rightarrow c = 23$

You use your private key $(d = 27, n = 55)$ to decrypt:
$$m = c^d \mod n = 23^{27} \mod 55$$
You recover the original message $m = 12$.
