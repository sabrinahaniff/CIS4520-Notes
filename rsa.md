# RSA Cryptography

## Public Key Cryptography

### The Big Idea

In **public key cryptography**, there are **two keys**:

1. **Public Key** - Shared with everyone (used to **encrypt**)
2. **Private Key** - Kept secret (used to **decrypt**)

### How It Works

**Scenario:** Alice wants to send Bob a secret message.

1. Bob publishes his **public key**
2. Alice **encrypts** with Bob's public key
3. Bob **decrypts** with his **private key**

> **Key insight:** Anyone can encrypt, but only Bob can decrypt!

---

## Why RSA?

**Problem with symmetric crypto:** How do you share the secret key securely?

**RSA solution:** 
- Encryption key is **public** (anyone can encrypt)
- Decryption key is **private** (only recipient can decrypt)
- Keys are mathematically related but **computationally hard** to derive one from the other

---

## RSA Key Generation

### Step 1: Choose Two Large Primes

Pick two large prime numbers $p$ and $q$.

**Example:** $p = 61, q = 53$

### Step 2: Compute $n = p \times q$

$$n = p \times q$$

**Example:** $n = 61 \times 53 = 3233$

This is the **modulus** for both keys.

### Step 3: Compute $\phi(n) = (p-1)(q-1)$

Euler's totient function:

$$\phi(n) = (p - 1)(q - 1)$$

**Example:** $\phi(3233) = 60 \times 52 = 3120$

### Step 4: Choose Public Exponent $e$

Pick $e$ such that:
- $1 < e < \phi(n)$
- $\gcd(e, \phi(n)) = 1$ (e and $\phi(n)$ are coprime)

Common choice: $e = 65537 = 2^{16} + 1$

**Example:** Let's use $e = 17$ (simpler for demo)

Check: $\gcd(17, 3120) = 1$ ✓

### Step 5: Compute Private Exponent $d$

Find $d$ such that:

$$e \times d \equiv 1 \pmod{\phi(n)}$$

In other words, $d$ is the **modular inverse** of $e$ modulo $\phi(n)$.

**Example:** Find $d$ where $17d \equiv 1 \pmod{3120}$

Using Extended Euclidean Algorithm:
$$d = 2753$$

Verify: $17 \times 2753 = 46801 = 15 \times 3120 + 1$ ✓

---

## RSA Keys

### Public Key

$$(n, e)$$

**Example:** $(3233, 17)$

### Private Key

$$(n, d)$$

**Example:** $(3233, 2753)$

**Share:** Public key $(n, e)$ — anyone can use it  
**Keep secret:** Private key $(n, d)$ — only you have it

---

## RSA Encryption

### Encryption Formula

Given plaintext message $M$ (as a number):

$$C = M^e \bmod n$$

where:
- $C$ = ciphertext
- $M$ = plaintext
- $e$ = public exponent
- $n$ = modulus

### Example

**Plaintext:** $M = 123$  
**Public key:** $(n, e) = (3233, 17)$

$$C = 123^{17} \bmod 3233$$

Using fast exponentiation:
$$C = 855$$

---

## RSA Decryption

### Decryption Formula

Given ciphertext $C$:

$$M = C^d \bmod n$$

where:
- $M$ = plaintext (recovered)
- $C$ = ciphertext
- $d$ = private exponent
- $n$ = modulus

### Example

**Ciphertext:** $C = 855$  
**Private key:** $(n, d) = (3233, 2753)$

$$M = 855^{2753} \bmod 3233$$

$$M = 123$$ ✓

**Success!** We recovered the original message.

---

## Why RSA Works

### The Math

$$C = M^e \bmod n$$
$$M = C^d \bmod n$$

Substituting:
$$M = (M^e)^d \bmod n = M^{ed} \bmod n$$

### Euler's Theorem

If $\gcd(M, n) = 1$, then:

$$M^{\phi(n)} \equiv 1 \pmod{n}$$

Since $ed \equiv 1 \pmod{\phi(n)}$, we can write:

$$ed = 1 + k\phi(n)$$

for some integer $k$.

Therefore:
$$M^{ed} = M^{1 + k\phi(n)} = M \cdot (M^{\phi(n)})^k \equiv M \cdot 1^k \equiv M \pmod{n}$$

---

## Security of RSA

### The Hard Problem

**Easy:** Given $(n, e)$, compute $C = M^e \bmod n$

**Hard:** Given $(n, e, C)$, find $M$ without knowing $d$

### Why It's Hard

To find $d$, you need to:
1. Factor $n$ into $p$ and $q$
2. Compute $\phi(n) = (p-1)(q-1)$
3. Find $d = e^{-1} \bmod \phi(n)$

**Factoring large numbers is computationally infeasible!**

For 2048-bit RSA:
- $n$ has ~600 digits
- Best known algorithms take billions of years

---

## Choosing Secure Parameters

### Key Size

- **Minimum:** 2048 bits
- **Recommended:** 3072-4096 bits
- **Insecure:** 1024 bits or less

### Prime Selection

$p$ and $q$ must be:
- **Large** (1024+ bits each)
- **Random**
- **Not too close** to each other
- **Actually prime** (use primality testing)

---

## Primality Testing

> **Problem:** RSA needs large primes. We cannot test primality by dividing up to $\sqrt{n}$ — too slow for big numbers!

### Solution: Probabilistic Tests

Instead of checking every divisor, we test if a number "behaves like a prime."

- If it **fails** → definitely composite
- If it **passes** many tests → probably prime

---

## Fermat Primality Test

### Fermat's Little Theorem

If $p$ is prime and $\gcd(a, p) = 1$, then:

$$a^{p-1} \equiv 1 \pmod{p}$$

### The Test

1. Pick a random $a$ where $1 < a < n$
2. Compute $a^{n-1} \bmod n$
3. **If NOT 1** → $n$ is composite
4. **If 1** → $n$ is *maybe* prime

### Why It's Probabilistic

Some composite numbers pass the Fermat test. These are called **Carmichael numbers**.

**Example:** 561 is composite but passes Fermat test for all $a$ coprime to 561.

---

## Miller-Rabin Test

### Improvement Over Fermat

- More reliable than Fermat
- Still probabilistic
- Much harder to fool

### How It Works

Write $n - 1 = 2^r \times d$ where $d$ is odd.

For random $a$:
1. Compute $x = a^d \bmod n$
2. If $x = 1$ or $x = n-1$ → probably prime
3. Square $x$ repeatedly $r-1$ times
4. If you ever get $n-1$ → probably prime
5. Otherwise → definitely composite

### Accuracy

Run Miller-Rabin $k$ times with different random values of $a$:
- Probability of error: $< 4^{-k}$
- With $k = 40$ → error probability $< 2^{-80}$ (negligible)

---

## RSA in Practice

### Textbook RSA Problems

**Never use raw RSA!** It has vulnerabilities:

1. **Deterministic** - same message always produces same ciphertext
2. **Malleable** - can manipulate ciphertexts
3. **No integrity** - attacker can modify messages

### Padding Schemes (OAEP)

Add randomness and structure before encryption:

$$C = (M \oplus G(r) \| r \oplus H(M \oplus G(r)))^e \bmod n$$

This makes RSA:
- **Non-deterministic** (random for each encryption)
- **Semantically secure**
- **Resistant to attacks**

---

## Common RSA Attacks

### Small Exponent Attack

If $e$ is small (e.g., $e = 3$) and message is short:
- Ciphertext might be smaller than $n$
- Can recover $M$ by taking cube root

**Defense:** Use padding (OAEP)

### Common Modulus Attack

If two users share the same $n$ but different $e, d$ pairs:
- Attacker can decrypt without private key

**Defense:** Never reuse modulus

### Timing Attacks

Measure how long decryption takes to learn about private key.

**Defense:** Use constant-time algorithms

---

## Key Takeaways

1. **RSA uses two keys** - public for encryption, private for decryption
2. **Security relies on factoring** being hard
3. **Key generation** requires large random primes
4. **Primality testing** uses probabilistic algorithms (Miller-Rabin)
5. **Never use textbook RSA** - always use padding (OAEP)
6. **Key size matters** - minimum 2048 bits

---

[[../01-Foundations/modular-arithmetic|← Modular Arithmetic]] | [Next: Digital Signatures →]
