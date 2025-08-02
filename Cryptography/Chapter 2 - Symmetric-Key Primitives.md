## 2.1 Pseudorandom Generators (PRGs)
In symmetric cryptography, secure systems need lots of randomness. However, generating long truly-random strings is impractical. Instead, we use a short random seed, and "stretch" it into a longer string that looks random to any adversary.

Let $G: \{0,1\}^n \rightarrow \{0,1\})^{l(n)}$ be a deterministic function, where:
- $n$ is seed length (input size)
- $l(n) > n$ is the output length (expansion)
- $G$ is efficiently computable (polynomial time)
**Def:** A function $G$ is a PRG if:
1. Expansion: $l(n) > n$
2. Pseudorandomness: No probabilistic polynomial time (PPT) adversary $A$ can distinguish the output of $G$ from a truly random string of length $l(n)$ with more than negligible advantage.
	- Basically, you can't tell that it was created from a smaller string
### Next-Bit Test (Yao's Characterization)
Suppose someone gives you a long string $y \in \{0,1\}^m$. You know the string was either:
- chosen uniformly at random from $\{0,1\}^m$, or
- computed as $G(s)$ for a random seed $s \in \{0,1\}^n$, $n \leq m$
Can you look at the first $i$ bits, and guess the next bit $y_{i+1}$ with better than 50% accuracy?
- If you can, then $y$ is not truly random, you are detecting structure
**Yao's Theorem**: A generator $G$ is pseudorandom iff no PPT algo can predict the next bit of $G(s)$ with some advantage.
- Why is this powerful? This is a **local test** for pseudorandomness
	- Don't have to distinguish the entire string form uniform
$$\text{"can't distinguish output from uniform"} \Leftrightarrow \text{"can't predict the next bit given prior bits"}$$
### One-Way Functions (OWFs)
Can we really build a secure PRG?
Relies on the bedrock of cryptography: **Some functions are easy to compute but hard to invert.**
**Def:** A function $f:\{0,1\}^n \rightarrow \{0,1\}^*$ is one-way if:
- $f$ is easy to compute (poly-time)
- But hard to invert: no adversary can find a preimage $x$ from $y = f(x)$ easily.
Key Theorem:
- If one-way functions exist, then pseudorandom generators exist
$$\text{OWFs} \Leftrightarrow \text{PRGs}$$
Proof of how to construction a PRG from an OWF..
## 2.2 Stream Ciphers
Main idea: use a PRG to generate a keystream, then XOR it with the plaintext.
Let:
- $G: \{0,1\}^n \rightarrow \{0,1\}^m$ be a secure PRG
- $k \in \{0,1\}^n$ is the secret key
- $m\in \{0,1\}^m$ is the plaintext message
- $c \in \{0,1\}^m$ is the ciphertext
Then:
$$Enc_k(m) = G(k) \oplus m = c$$
$$Dec_k = G(k) \oplus c = m$$
- just like the one-time pad, but with a pseudorandom keystream instead of a truly random one
Correctness:
$$Dec_k(c) = G(k) \oplus c = G(k) \oplus (G(k)\oplus m ) = m$$
- since $x \oplus x = 0$, and $0 \oplus m = m$
### IND-CPA security
"indistinguishability under Chosen Plaintext Attack"
- since the PRG is secure, the ciphertext $c = G(k) \oplus m$ is also indistinguishable from random
**Keystream Reuse is BAD!**
- Suppose we encrypt two messages $m_1, m_2$ with the same key $k$.
$$c_1 = G(k) \oplus m_1, \hspace{1em} c_2 = G(k) \oplus m_2$$
- Then an attacker who intercepts the ciphertexts can compute:
$$c_1 \oplus c_2 = (G(k) \oplus m_1) \oplus (G(k)\oplus m_2) = m_1 \oplus m_2$$
- The XOR of two plaintexts leaks a bunch of info
- Called a **two-time pad attack**
## 2.3 Block Ciphers
Block cipher is a pseudorandom permutation (PRP). This is a keyed function that acts like a random bijection on fixed-length blocks.
**Def**: A block cipher is a function:
$$E : K \times \{0,1\}^n \rightarrow \{0,1\}^m$$
where:
- $K$ is the key space (ex. 128 bit keys)
- $\{0,1\}^n$ is the block space (128 bit blocks)
- For each key $k \in K$, the function $E_k(x):= E(k,x)$ is:
	- Efficiently computable
	- Invertible: exists a function $D_k$ s.t. $D_k(E_k(x)) = x$
	- Pseudorandom: $E_k$ is indistinguishable from a uniform random permutation by an adversary
### Pseudorandom Permutation (PRP)
This is a function that is:
- Deterministic
- Bijective (invertible)
- Efficient
- Indistinguishable from random permutation
**So basically the same as PRF, but with the requirement of invertibility.**
A PRG takes a small input, and converts to a long output (not invertible).
A Block Cipher takes a fixed size input, outputs a fixed size output (invertible)
- basically it is a pseudorandom shuffle of the $2^n$ possible n-bit strings.
- The key is used to inverse it back.
*How do we build such a permutation? Or, what kind of structure gives us invertibility + pseudorandomness?*
### Feistel Networks
This turns a non-invertible function $F$ into an invertible one by splitting the input, using $F$ on part of it, and then swapping.
- "encrypt half, modify the other half, repeat"
#### Algorithm:
Let us say the input $x$ is a bitstring of length $2n$. Split into two $n$-bit halves:
$$x = (L_0, R_0)$$
One Feistel round transforms this pair using the round key $k_i$ and a round function $F$:
$$L_1 = R0$$
$$R_1= L_0 \oplus F(R_0,k_i)$$
- NOTE: only one half goes through $F$
Repeat this for multiple rounds, chaining outputs into inputs (L0--> R1, R0-->L1, ..)
After $r$ rounds:
$$(L_r, R_r) = Feistel_k^r(L_0,R_0)$$
- Each round uses a different subkey $k_i$, derived from the main key.
#### Why is this Invertible?
Lets reverse one round:
Given $(L_1,R_1)$, we can recover:
$$L_1 = R_0$$
$$R_1 \oplus F(L_1, k_i) = L_0$$
- **So even if $F$ is not invertible, the whole Feistel round is.**
This is why Feistel is cool! can encrypt and decrypt using the same function, just reversing the order of the keys.
#### Example
2-round Feistel cipher:
- Block size: 8 bits --> split into two 4-bit halves
- Input block: 8 bits total
- Round keys: $k_1 = 1100$, $k_2 = 0110$
- Round function: $F(x,k) = (x\oplus k )$ (bitwise XOR)
- Therefore, $F: \{0,1\} \times \{0,1\}^4 \rightarrow \{0,1\}^4$
Initial plaintext block: 
$$P = 10110110 \rightarrow L_0 = 1011, R_0 = 0110$$
Round 1:
- Using key $k_1 = 1100$,
$$L_1 = R_0 = 0110$$
$$R_1 = L_0 \oplus F(R_0, k_1) = 1011 \oplus (0110 \oplus 1100) = 1011 \oplus 1010 = 0001$$
Round 2: 
$$L_2 = R_1 = 0001$$
$$R_2 = L_1 \oplus F(R_1, k_k) = 0110 \oplus (0001 \oplus 0110) = 0110 \oplus 0111 = 0001$$
Therefore, final ciphertext:
$$C = L_2, R_2 = 0001 0001$$
Decryption:
Do the reverse of the above, will get back 10110110.
### Substitution-Permutation Networks (SPNs)
Another way to construct a block cipher (PRP).
**Def**: SPN is a block cipher construction that repeatedly applies two types of transformations:
1. **Substitution (S-box)**: Replace small pieces of the state with nonlinear lookups (adds **confusion**)
2. **Permutation (P-box)**: Rearrange or mix the bits to spread influence (adds **diffusion**)
Each SPN round looks like:
- Input block --> Key Mixing (XOR with round key) --> Substitution Layer --> Permutation Layer --> repeat
#### Substitution Layer (S-boxes)
- Apply a small, fixed nonlinear mapping to parts of the state (like 4-8 bits at a time)
- Stored as lookup tables
- Nonlinear (essential for resisting linear/differential cryptanalysis)
#### Permutation Layer (P-boxes)
- Rearranges or linearly mixes bits across state
- Goal is to spread local changes globally
#### Key Mixing
At each round,
- A round key (derived from main key) is XORed into the state
- Prevents attackers from isolating structure
- Done before or after S-boxes
#### Why is this invertible?
1. Substitution: The lookup table pairs are like 0111 -> 1000. It is clearly a bijection, since we can go $S^{-1}(1000) = 0111$
2. Permutation: Reordering the bits from the original position to the new position can be reverted easily
3. Key Mixing (XOR with key):  XOR has its own inverse since $(x \oplus k) \oplus k = x$
Therefore, the full SPN is invertible.
## 2.4 Modes of Operation
Block ciphers have a limitation: only work on fixed-size blocks (e.g. 128 bits, etc). 
Real world message are arbitrary length. 
*So how do we encrypt messages longer than one block?*
### Naive Use: ECB Mode
ECB: Electronic Codebook Mode
Simply split the message into blocks, and encrypt each block. 
Fast and parallelizable, but has a flaw:
- **The same plaintext block always encrypts to the same ciphertext block**
	- Leaks patterns, violates IND-CPA
- ECB penguin
### Cipher Block Chaining (CBC)
Basically, the encryption of the current block depends on the encryption of the previous block. The first block is a random vector.
#### Encryption
$$C_0 = IV$$
$$C_1=E_k(M_1 \oplus C_0)$$
$$\vdots$$
$$C_i=E_k(M_i\oplus C_{i-1})$$
#### Decryption
$$M_i = D_k(C_i)\oplus C_{i-1}$$
This works because:
$$C_i = E_k(M_i\oplus C_{i-1}) \Rightarrow M_i \oplus C_{i-1} = D_k(C_i) \Rightarrow M_i = D_k(C_i) \oplus C_{i-1}$$
- IV adds randomness, so encrypting the same message twice yields different ciphertexts
- Not parallelizable (each block depends on the previous)
### Counter Mode (CTR)
The problem with CBC is that it is sequential (can't encrypt $M_3$ until you've encrypted $M_2$)
- This is slow
- CTR solves this by turning a block cipher into a stream cipher
CTR doesn't encrypt the plaintext directly.
- Generates a keystream using the block cipher and a counter
- XORs the keystream with the plaintext (like a stream ciphter)
#### Encryption
- Nonce: value that is unique for each encryption under the same key. Doesn't need to be secret, but shouldn't repeat
	- Ensures that the same message encrypted twice produces different ciphertexts
- Counter: integer that increments by 1 for each block. 
	- Add the nonce with this counter
	- Ensures each block input is unique.
1. For each block index $i$, compute:
$$Keystream_i = E_k(Nonce || i)$$
2. XOR with the plaintext:
$$C_i = M_i \oplus Keystream_i$$
#### Decryption
1. Regenerate the same keystream using the nonce and the counter
2. XOR the ciphertext
$$M_i = C_i \oplus E_k(Nonce||i)$$
CTR is IND-CPA secure if:
- the block cipher is secure
- the nonce is never reused with the same key
Advantages:
- Each keystream block is independent



