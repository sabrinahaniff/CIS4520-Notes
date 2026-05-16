# Classical Ciphers

## Introduction

**Classical cryptography** refers to encryption methods used before computers. These ciphers are:
- **Symmetric** (same key for encryption and decryption)
- **Character-based** (operate on letters, not bits)
- **Historically important** but mostly **insecure today**

---

## Caesar Cipher

### How It Works

**Shift** each letter in the plaintext by a fixed number of positions in the alphabet.

**Key:** $K$ = shift amount (0-25)

### Encryption

$$C = (P + K) \bmod 26$$

where:
- $C$ = ciphertext letter position
- $P$ = plaintext letter position
- $K$ = key (shift amount)

### Decryption

$$P = (C - K) \bmod 26$$

Shift the ciphertext letter **backward** by $K$ positions.

---

### Example

**Plaintext:** HELLO  
**Key:** $K = 3$

**Encryption:**
- H (7) → (7 + 3) mod 26 = 10 → K
- E (4) → (4 + 3) mod 26 = 7 → H
- L (11) → (11 + 3) mod 26 = 14 → O
- L (11) → (11 + 3) mod 26 = 14 → O
- O (14) → (14 + 3) mod 26 = 17 → R

**Ciphertext:** KHOOR

---

### Security

**Completely insecure!**

- Only 26 possible keys
- **Brute force:** Try all 26 shifts in seconds
- **Frequency analysis:** Letter 'E' is most common in English

---

## Vigenère Cipher

### How It Works

Use a **keyword** to create multiple Caesar shifts.

**Key:** A word (e.g., "KEY")  
Each letter of the key determines the shift for the corresponding plaintext letter.

### Encryption

$$C_i = (P_i + K_i) \bmod 26$$

where:
- $K_i$ = key letter at position $i$ (repeats cyclically)

### Example

**Plaintext:** HELLO  
**Key:** KEY (repeated: KEYKE)

| Plaintext | H | E | L | L | O |
|-----------|---|---|---|---|---|
| Key | K (10) | E (4) | Y (24) | K (10) | E (4) |
| Shift | +10 | +4 | +24 | +10 | +4 |
| Ciphertext | R | I | J | V | S |

**Ciphertext:** RIJVS

---

### Security

**More secure than Caesar, but still breakable:**

1. **Kasiski examination** - find repeated patterns to determine key length
2. **Frequency analysis** - once key length is known, break each Caesar cipher separately
3. **Index of coincidence** - statistical method to find key length

---

## Substitution Cipher

### How It Works

Each plaintext letter maps to a **different** ciphertext letter (one-to-one mapping).

**Key:** A permutation of the alphabet

### Example

**Key mapping:**
```
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓
Q W E R T Y U I O P A S D F G H J K L Z X C V B N M
```

**Plaintext:** HELLO  
**Ciphertext:** ITSSG

---

### Security

**Key space:** $26!$ ≈ $4 \times 10^{26}$ (huge!)

**But still vulnerable to:**
- **Frequency analysis** - 'E' is most common, 'TH' is common bigram
- **Pattern analysis** - double letters (LL, SS) reveal structure
- **Known plaintext attacks**

---

## One-Time Pad (OTP)

### How It Works

Use a **random key** that is:
- As long as the message
- Used only once
- Truly random

### Encryption

$$C = P \oplus K$$

(XOR each bit of plaintext with corresponding key bit)

### Example

**Plaintext:** HELLO (in binary)  
**Key:** Random bits (same length)  
**Ciphertext:** Plaintext XOR Key

---

### Security

**Perfectly secure!** (Shannon proved this)

**Why it's perfect:**
- Every ciphertext is equally likely
- No frequency analysis possible
- No patterns to exploit

**Why we don't use it:**
- Key must be as long as the message
- Key must be truly random
- Key can only be used once
- Key distribution is hard

---

## Rail Fence Cipher

### How It Works

Write the plaintext in a **zigzag pattern** across multiple "rails," then read off row by row.

### Example (2 rails)

**Plaintext:** HELLOWORLD

```
H . L . O . O . L .
. E . L . W . R . D
```

**Ciphertext:** HLOOLELDWR

### Example (3 rails)

**Plaintext:** HELLOWORLD

```
H . . . O . . . L .
. E . L . W . R . D
. . L . . . O . . .
```

**Ciphertext:** HOLELLWRDO

---

### Security

**Very weak:**
- Only a few possible configurations
- Easy to recognize zigzag patterns
- Provides almost no security

---

## Playfair Cipher

### How It Works

Uses a **5×5 grid** of letters (I and J share a cell).

**Key:** A keyword fills the grid, then remaining letters

### Grid Construction

**Keyword:** MONARCHY

```
M O N A R
C H Y B D
E F G I/J K
L P Q S T
U V W X Z
```

### Encryption Rules

Encrypt **pairs of letters** (digraphs):

1. **Same row:** Shift right (wrap around)
2. **Same column:** Shift down (wrap around)
3. **Rectangle:** Swap with letter in same row

### Example

**Plaintext:** HELLO (pairs: HE, LL, O)

- HE → Same row? No. Same column? No. Rectangle → HF
- LL → Same letter → Insert X → LX → QS
- O → Odd letter → Add X → OX → ...

**Ciphertext:** (complex example)

---

### Security

**Stronger than simple substitution:**
- Operates on digraphs (26² = 676 possibilities)
- Frequency analysis is harder

**Still breakable:**
- Digraph frequency analysis
- Common pairs: TH, HE, AN

---

## Hill Cipher

### How It Works

Uses **matrix multiplication** modulo 26.

**Key:** An invertible $n \times n$ matrix

### Encryption

$$C = KP \bmod 26$$

where:
- $K$ = key matrix
- $P$ = plaintext vector
- $C$ = ciphertext vector

### Example (2×2)

**Key matrix:**
$$K = \begin{bmatrix} 3 & 3 \\ 2 & 5 \end{bmatrix}$$

**Plaintext:** HE = [7, 4]

$$C = \begin{bmatrix} 3 & 3 \\ 2 & 5 \end{bmatrix} \begin{bmatrix} 7 \\ 4 \end{bmatrix} = \begin{bmatrix} 33 \\ 34 \end{bmatrix} \equiv \begin{bmatrix} 7 \\ 8 \end{bmatrix} \pmod{26}$$

**Ciphertext:** HI

---

### Decryption

$$P = K^{-1}C \bmod 26$$

**Requirements:**
- $\det(K) \neq 0 \bmod 26$
- $\gcd(\det(K), 26) = 1$

---

### Security

**Stronger against frequency analysis:**
- Operates on blocks
- Diffusion through matrix multiplication

**Vulnerable to:**
- **Known plaintext attack** - if you know $P$ and $C$, can solve for $K$
- Requires chosen plaintext to break completely

---

## Comparison of Classical Ciphers

| Cipher | Key Space | Security | Speed | Notes |
|--------|-----------|----------|-------|-------|
| Caesar | 26 | Very weak | Fast | Brute force trivial |
| Vigenère | $26^k$ | Weak | Fast | Kasiski/frequency breaks it |
| Substitution | $26!$ | Weak | Fast | Frequency analysis breaks it |
| OTP | Infinite | Perfect | Fast | Key management impossible |
| Playfair | Large | Moderate | Medium | Digraph frequency vulnerable |
| Hill | Very large | Moderate | Medium | Known plaintext attack |
| Rail Fence | Small | Very weak | Fast | Pattern recognition breaks it |

---

## Why Classical Ciphers Failed

1. **Small key spaces** (except OTP and Hill)
2. **Frequency analysis** works on all except OTP
3. **Pattern preservation** - structure leaks information
4. **No confusion/diffusion** (except Hill cipher)
5. **Known plaintext attacks** effective

---

## Lessons Learned

Modern cryptography learned from classical failures:

1. **Large key spaces** are necessary (but not sufficient)
2. **Confusion** - ciphertext should look random
3. **Diffusion** - change one bit → changes many bits
4. **Avalanche effect** - small input change → large output change
5. **Computational security** vs. information-theoretic security

---

## Kerckhoffs's Principle

> **The security should rely on the key, not on keeping the algorithm secret.**

**Why this matters:**
- Algorithms get reverse-engineered
- "Security through obscurity" always fails
- Public algorithms get more scrutiny and testing

**Modern application:**
- AES algorithm is public
- RSA algorithm is public
- Security relies entirely on key secrecy

---

[[../02-Public-Key/rsa|← RSA]] | [[../04-Symmetric-Crypto/aes|AES →]]
