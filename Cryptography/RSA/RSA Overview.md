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
by Euler's Theorem, as long as $\gcd(m,n) = 1$

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
## Proof / Rationale behind these steps
### Key Generation
#### Overall Goal
We want to construct a public key $(e,n)$ and a private key $(d,n)$ s.t.:
- **we first raise the message $m$ by $e$, send the ciphertext to the recipient, then raise that ciphertext again by $d$, and by $\mod n$, it returns back $m$.**
$$(m^e)^d \equiv m \mod n \Rightarrow m^{ed} \equiv m \mod n$$
#### Choose Two Large Primes $p$ and $q$, Compute $n$
We use the $p, q$ to form $n = pq$, which is the modulus for both encryption and decryption. 
<u>RSA Assumption</u>:The security of RSA relies on the assumption that it's hard to factor $n$ back into $p$ and $q$. 
Once someone knows $p$ and $q$, they can compute $\phi(n)$, and break RSA.
The reason we use two primes is that it **gives us a composite number $n$ whose totient is still easy to compute if you know the factors**.
#### Compute Euler's Totient (Proof)
We use the fact that $p,q$ are primes, so $$\phi(n) = \phi(pq) = (p-1)(q-1)$$
Euler's Theorem says that if $\gcd(m,n) = 1$, then $m^{\phi(n)}\equiv 1 \mod n$. This implies any multiple of $m^{\phi(n)}$ is equiv to 1 mod n, so
$$m^{k\cdot \phi(n)} \equiv 1 \mod n$$
So if we choose $e, d$ s.t. 
$$ed \equiv 1 \mod \phi(n)$$
, implies 
$$ed = 1 + k\phi(n), \hspace{0.5em} k \in \mathbb{Z}$$
Substituting this into $m^{ed}$, 
$$m^{ed} = m^{1 + k\phi(n)} = m \cdot (m^{\phi(n)})^k$$
By Euler's Theorem:
$$m^{\phi(n)} \equiv 1 \mod n \Rightarrow (m^{\phi(n)})^k \equiv 1^k = 1 \mod n$$
So:
$$m^{ed} = m \cdot (m^{\phi(n)})^k = m \cdot 1 \equiv m \mod n$$
Therefore, 
$$m^{ed} \equiv m \mod n$$
NOTE: This only applies if $\gcd(m,n) = 1$ (as required by Euler's Theorem. There are ways to ensure this in practice.
#### Choose Encryption Exponent $e$
Choose a number $e$ s.t.:
- $1 < e < \phi(n)$
- $\gcd(e, \phi(n)) = 1$
$e$ will be the exponent in the encryption function $c = m^e \mod n$.
Therefore, it must be invertible $\mod \phi(n)$, so that decryption is possible.
#### Compute Decryption Exponent $d$
Solve for $d$ s.t.:
$$e \cdot d \equiv 1 \mod \phi(n)$$
This means that
$$d = e^{-1} \mod \phi(n)$$
$d$ basically undoes the encryption.
We use Extended EA to compute this $d$.
### Encryption
Given a plaintext message $m \in \mathbb{Z}$, we compute:
$$c = m^e \mod n$$
This works since we are applying a transformation to $m$ that is:
- easy to compute (fast modular exponentiation)
- hard to reverse without $d$ (computing $d$ requires factoring $n$, which is computationally infeasible if $n$ is large)
	- Why? EXPLORE THIS
### Decryption
As shown above, raising the ciphertext by the $d$ private key mod n returns back the original message $m$.
### Notes
1. $\gcd(m,n) = 1$ matters:
	- Euler's Theorem requires it
	- In practice, padding schemes ensure messages satisfy this.
2. What if $m \geq n$?
	- We must ensure $m < n$. In real systems, messages are often broke into blocks smaller than $n$.
3. String encoding
	- RSA doesn't encrypt raw strings, so messages are converted to integers via encoding (ex. ASCII)



