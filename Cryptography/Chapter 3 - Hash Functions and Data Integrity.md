## 3.1 Hash Function Properties
**Def**: A hash function is a deterministic function that takes an input of arbitrary length and outputs a fixed length string.
$$H: \{0,1\}^* \rightarrow \{0,1\}^n$$
- Must be efficient and deterministic
### Why do we hash things?
- Fingerprinting a message: instead of storing and entire file, just store the short hash.
- Integrity checking: if someone sends a file, hash it, and compare it with sender's hash. If altered, the hash will be different
- Passwords: store the hash of a password, not the password itself. On login, hash the entered password and compare
- Digital signatures: instead of signing an entire document, sign just its hash
- Merkle trees: efficiently verify the integrity of a part of a dataset
### What makes a hash function Cryptographic?
A cryptographic hash function (CHF) must satisfy some key security properties:
1. **Preimage Resistance**: Given a hash output $h$, it should be computationally infeasible to find any input $x$ s.t. $H(x) = h$
	- If you know the hash, you shouldn't be able to recover the original input
	- Use case: password storage: if you store H(password), and if someone sees that, they shouldn't be able to find the password.
2. **Second Preimage Resistance**: Given an input $x_1$, it should be infeasible to find a different input $x_2 \neq x_1$ s.t. $H(x_1) = H(x_2)$.
	- You can't fake a different message with the same hash as a real one.
	- Use Case: digital signatures: You sign a document with a hash. No one can forge a different document with the same signature
3. **Collision Resistance**: It should be infeasible to find any pair $x_1 \neq x_2$ s.t. $H(x_1) = H(x_2)$.
	- Stronger version of second preimage resistance (you aren't given either input, you just have to find *any* collision)
	- Note: pigeonhole principle says that collisions must exist (input space is infinite, output space is finite)
		- But what matters is that its "hard" to find them
### Birthday Paradox: Finding collisions are easy
Finding a collision is much easier than finding a preimage.
**Birthday Paradox**: There are 365 days in a year. How many people do you need in a room before theres a 50% chance that two people share a birthday?
- To find prob. of at least one match, do 1 - prob. of no two share a birthday
$$Pr[\text{at least one match}] = 1 - Pr[\text{no two share a birthday}]$$
Suppose there are $q$ people.
- First person can have any birthday: $365/365 = 1$
- Second must avoid the first person's birthday: $364/365$, so on
- So prob that all $q$ birthdays are unique:$$Pr[\text{no collision}] = \prod^{q-1}_{i=0} \left (1 - \frac{i}{365}\right)$$
- When does this fall before 0.5?
	- q = 23: prob = 0.492 --> below 50%
Therefore, the answer is **23 people.** 
### Application to Hash functions
Just like birthdays, a hash function maps a large set of inputs to a fixed number of outputs.
$$H: \{0,1\}^* \rightarrow \{0,1\}^n$$
This means there are exactly $2^n$ possible outputs.
If you hash random inputs, you will get a collision eventually. Birthday bound tells how many inputs you need before a collision becomes likely.
**To find a collision, you only need about:**
$$q = 1.2 \cdot 2^{n/2}$$
- Not $2^n$. Why?
- Because we are looking for **any** pair that collide, not targeting a specific output
Therefore, collision attacks are strictly easier:
- Preimage: 2^n
- Second preimage: 2^n
- Collision: $2^{n/2}$
Hash output size must be $\geq 2 \times$ desired security level
- Need a 256-bit output to get a 128-bit collision resistance
## 3.2 Classic Constructions
How are real world cryptographic hash functions built?
- We compress the message gradually, one block at a time
### Compression Function
Fixed-length, deterministic function:
$$f: \{0,1\}^{n+b} \rightarrow \{0,1\}^n$$
- $n$: size of internal state and final hash output 
- $b$: size of each message block
- So $n+b \rightarrow n$ (compression!)
### Merkle-Damgård Iterated Construction

