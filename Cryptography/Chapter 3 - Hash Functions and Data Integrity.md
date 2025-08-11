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
**How to build a compression function**:
We want something that is:
- deterministic and efficient
- collision-resistant
- works on fixed-length inputs
- outputs fixed-length
We can build it using simpler primitives like block ciphers.
#### Davies-Meyer Construction
Let $E_k(x)$ be a block cipher:
- $k \in \{0,1\}^n$ : key
- $x \in \{0,1\}^n$ : plaintext block
We will *misuse* the block cipher to mix a message block and a state:
Define the compression function $f$ as:
$$f(h,m) = E_m(h) \oplus k$$
Where:
- $h$: current hash state
- $m$: message block
- $E_m(h)$: encrypt state using **message block as key**
- final result is ciphertext (hash state with message as key) XOR original state
Why is this good?
- not invertible, even though the block cipher is
- cipher is pseudorandom, so $f$ behaves like a random function
- naturally binds the message and the state together 
### Merkle-Damgård Iterated Construction
**Input**:
- message $M$ (arbitrary length)
- compression function $f$
- block size $b$ bits
- output length $n$ bits
- initial state: constant value (IV), often all zeros of fixed standard value
**Procedure**:
1. Pad ([[#Padding]]) the message so that the total length is a multiple of $b$ bits
2. Split the padded message into $m_1, m_2, ..., m_t$, each of length $b$
3. Initialize $H_0 = IV$
4. Loop:
$$H_i = f(H_{i-1}, m_i)$$
	- Each round takes the previous hash state $H_{i-1}$, and new block $m_i$
5. Final output: $H(M) = H_i$
#### Padding
Why pad the message? We need it to be a multiple of $b$, so we can split it into clean blocks.
**Procedure**:
1. Append a single $1$ bit to the message
2. Append $0$ bits until the length is congruent to $b-64 \mod b$
	- This leaves 64 bits of space at the end of the last block
3. Append a 64-bit representation of the original message length (in bits)
This ensures the last block always contains the original message length.
**Why include the length?**
- if an attacker knows the final state $H_t$, they can continue the chain by appending more blocks and computing further $H_{t+1}, ...$ (without knowing the original message!)
- By appending the length, you "lock in" the messages' original size, so the attacker can't add more blocks. 
## 3.3 Merkle-Damgård Security Properties
*If the compression function is collision-resistant, does the overall Merkle-Damgård hash stay secure?*
**Proof: contrapositive.** 
Assume compression function is collision resistant, and an adversary found a collision in the hash function ($H(M) = H(M'), M \neq M'$). WTS there is a collision in compression function.
- Case 1: $M, M'$ are the same length.
	- Both messages produce the same number of blocks
	- If $M \neq M'$, then some blocks must differ. So $m_i \neq m_i'$ for some $i$
	- But since the previous hash states are the same until round $i$, $f(H_{i-1}, m_i) = f(H_{i-1}, m_i')$
	- Therefore, there is a collision in the compression function, which contradicts the assumption. 
- Case 2: $M, M'$ are different lengths
	- We prevent adding more blocks to the message by the padding scheme
	- We append the bit-length of the original message at the end of the padding, so even if the message contents are similar, the length will be different
	- So the final block will be different, so different output
#### Limitations
Second preimage attacks:
- If the message $M$ is very long $2^k$ blocks, then a second preimage can be found much faster than brute force time $2^n$. 
	- For a long message, there will be many repeated intermediate states, so it is easier to splice in a different message that leads to the same final hash
- Basically, MD compression is **state-limited** (doesn't scale with message length)
## 3.5 HMAC
Hash functions give integrity: If A sends B a file and the hash, B can recompute the hash and check if it matches (knows the file hasn't changed).
***BUT:*** If Eve intercepts the file and hash, can:
- Replace file with own file
- Compute new hash
- Send both to Bob
Bob's check will pass, since *nothing proves who computed the hash*.
We need **message authentication**.
*Can come back later...*
## 3.6 Merkle Trees
We might need to convince someone that a small piece of a larger dataset is authentic, **without sending the whole dataset**.
Idea:
- Beforehand, run all the data through a process that creates a short fingerprint of the entire thing (**root hash**)
- Later, when someone asks for just one piece,
	- Give them that piece + a short chain of hashes (a **proof**)
	- They can use those hashes to rebuild the root hash without needing the rest of the data
	- If it matches the published root hash, they know the piece is genuine
Why this works:
- the hash function makes it impractical to change the data without changing the root hash
- the "short chain" is possible since we arrange the data in a tree (only send hashes along the path from that data piece to the root)
- Instead of downloading everything, only download $log_2(N)$ hashes
Analogy:
- Lego pyramid, where top brick is root hash
	- Every brick below is built from the two bricks under it
- To prove one bottom brick is in the pyramid,
	- Don't need to bring the whole pyramid
	- Just bring that brick + the matching bricks along the way up to the top
	- Anyone can rebuild your path to the top brick and check if it matches the known pyramid top.
- *Any change to the bottom brick will change the root hash, since **Merkle trees bind all leaves together via hashing***.
### Structure of a Merkle Tree
Start with $N$ data blocks (leaves):
$$D_1, D_2, \cdots , D_N$$
1. Hash each block: $H_1 = hash(D_1), H_2 = hash(D_2), \cdots$
2. Pair them up and hash the pair:
$$P_{12} = hash(H_1 || H_2)$$
$$P_{3,4} = hash(H_3 || H_4)$$
3. Keep combining up until one hash remains (root)
$$Root = hash(something || something)$$
**This root hash is the fingerprint of the whole dataset**. 
- You can see how changing one block anywhere in the dataset will flow up the tree and change the root hash
### How a Merkle Proof works
Suppose you want to prove a block is in the dataset.
You send:
- The actual block 
- The authentication path (hashes needed to rebuild the root)
Verifier does:
- Hash the block, keep hashing upwards along the path
- Compare the calculated root hash to the published root hash
**NOTE**: the file isn't actually a tree (just some bits).
- *Merkle tree is something we construct*
	- Taking the bits, splitting into chunks, hashing chunks, pairing, hashing, repeat till root
## 3.7 Applications of Hash Functions
1. Password Storage
	- Websites don't store your password, they store the hash of the password
	- On login, you type your password. They hash it and compare to their stored hash
2. Git / Version control
	- Every commit in Git is identified with a hash of the files in the commit and the metadata
3. Blockchain
	- Bitcoin block headers contain a SHA-256 hash of the previous block's header
	- Transactions are organized in a Merkle tree
	- Proof of mining is: "Find a nonce so that SHA256(block header) < target"
4. Digital signatures
	- When you sign a document, you don't sign the entire file, you hash it first
5. File integrity checks
	- Download pages provide SHA-256 checksums
6. Peer-to-Peer Networks (BitTorrent)
	- later?
