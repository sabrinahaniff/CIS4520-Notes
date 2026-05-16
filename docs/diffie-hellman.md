# Key Exchange & Diffie-Hellman

## The Key Distribution Problem

### The Challenge

**Question:** How do two parties establish a shared secret key over an insecure channel?

**Without meeting in person, how can Alice and Bob agree on a secret key when Eve is listening?**

**Traditional approach:**
- Meet in person
- Trusted courier
- Pre-shared keys

**These don't scale!**

---

## Diffie-Hellman Key Exchange

### The Breakthrough (1976)

**Whitfield Diffie** and **Martin Hellman** solved the key distribution problem!

**Idea:** Two parties can create a shared secret without ever transmitting it.

**Analogy:** Color mixing
- Alice has secret color (red)
- Bob has secret color (blue)
- They exchange mixed colors publicly
- Each adds their secret → same final color!

---

## How Diffie-Hellman Works

### Setup (Public Parameters)

**Everyone agrees on:**
- Large prime $p$
- Generator $g$ (primitive root modulo $p$)

These can be public and reused.

---

### The Protocol

**Alice's side:**
1. Choose secret $a$ (random, $1 < a < p-1$)
2. Compute $A = g^a \bmod p$
3. Send $A$ to Bob (public)

**Bob's side:**
1. Choose secret $b$ (random, $1 < b < p-1$)
2. Compute $B = g^b \bmod p$
3. Send $B$ to Alice (public)

**Alice computes:**
$$K = B^a \bmod p = (g^b)^a = g^{ab} \bmod p$$

**Bob computes:**
$$K = A^b \bmod p = (g^a)^b = g^{ab} \bmod p$$

**Result:** Both have $K = g^{ab} \bmod p$ (the shared secret!)

---

### Example

**Public parameters:**
- $p = 23$ (prime)
- $g = 5$ (generator)

**Alice:**
- Secret: $a = 6$
- Compute: $A = 5^6 \bmod 23 = 15625 \bmod 23 = 8$
- Send: $A = 8$

**Bob:**
- Secret: $b = 15$
- Compute: $B = 5^{15} \bmod 23 = 8$
- Send: $B = 8$

**Alice computes shared secret:**
$$K = B^a = 8^6 \bmod 23 = 262144 \bmod 23 = 2$$

**Bob computes shared secret:**
$$K = A^b = 8^{15} \bmod 23 = 2$$

**Shared secret:** $K = 2$ ✓

---

## Why Diffie-Hellman is Secure

### The Hard Problem: Discrete Logarithm

**Given:**
- $p, g, A = g^a \bmod p$

**Find:** $a$

This is the **discrete logarithm problem** - computationally hard for large $p$.

**Eve sees:** $p, g, A, B$  
**Eve needs:** $a$ or $b$ to compute $K$  
**Problem:** No efficient algorithm to find $a$ from $g^a \bmod p$

---

### Computational Diffie-Hellman (CDH)

**Even harder:** Given $g, g^a, g^b$, compute $g^{ab}$ without knowing $a$ or $b$.

**Security assumption:** CDH is hard.

---

## Diffie-Hellman Variants

### 1. Finite Field DH (Original)

Uses modular arithmetic over prime $p$.

**Key sizes:** 2048-3072 bits

---

### 2. Elliptic Curve DH (ECDH)

Uses elliptic curve points instead of modular exponentiation.

**Advantages:**
- **Smaller keys** (256-bit ECDH ≈ 3072-bit DH)
- **Faster** computations
- **Same security level**

**Protocol:**
- Alice: $A = aG$ (scalar multiplication)
- Bob: $B = bG$
- Shared: $K = aB = bA = abG$

**Most common today:** X25519 (Curve25519)

---

### 3. Post-Quantum Key Exchange

**Problem:** Quantum computers can break DH!

**Solutions:**
- Lattice-based (e.g., Kyber)
- Code-based
- Hash-based

---

## Man-in-the-Middle Attack

### The Vulnerability

**Diffie-Hellman provides no authentication!**

**Attack scenario:**

```
Alice              Darth              Bob
  |                  |                 |
  |--- A = g^a ----> |                 |
  |                  |--- A' = g^d --->|
  |                  |<--- B' = g^d ---|
  |<--- B = g^d -----|                 |
  |                  |                 |
```

**Result:**
- Alice thinks she shares $K_1$ with Bob (actually with Darth)
- Bob thinks he shares $K_2$ with Alice (actually with Darth)
- Darth can decrypt, read, re-encrypt everything!

---

### Mitigation: Authentication

**Solutions:**

1. **Digital Signatures**
   - Sign $g^a$ and $g^b$
   - Requires public keys

2. **Pre-Shared Keys**
   - Authenticate with existing key

3. **Certificates**
   - Use PKI to verify identities

4. **Station-to-Station (STS) Protocol**
   - Combines DH with signatures

---

## ElGamal Encryption

### Overview

**ElGamal** uses Diffie-Hellman for **encryption** (not just key exchange).

**Based on:** DH key exchange idea

---

### Key Generation

1. Choose large prime $p$
2. Choose generator $g$
3. Choose private key $x$ (random, $1 < x < p-1$)
4. Compute public key $y = g^x \bmod p$

**Public key:** $(p, g, y)$  
**Private key:** $x$

---

### Encryption

**Alice encrypts message $M$ for Bob:**

1. Get Bob's public key $(p, g, y)$
2. Choose random $k$ (ephemeral key, $1 < k < p-1$)
3. Compute:
   - $C_1 = g^k \bmod p$ (hint for Bob)
   - $C_2 = M \cdot y^k \bmod p$ (masked message)

**Ciphertext:** $(C_1, C_2)$

---

### Decryption

**Bob decrypts using private key $x$:**

1. Compute $s = C_1^x \bmod p$
2. Compute $M = C_2 \cdot s^{-1} \bmod p$

---

### Why It Works

$$s = C_1^x = (g^k)^x = g^{kx} = (g^x)^k = y^k \bmod p$$

Therefore:
$$M = C_2 \cdot s^{-1} = M \cdot y^k \cdot (y^k)^{-1} = M$$

---

### Example

**Bob's keys:**
- $p = 23, g = 5$
- Private: $x = 6$
- Public: $y = 5^6 \bmod 23 = 8$

**Alice encrypts $M = 12$:**
- Choose $k = 3$
- $C_1 = 5^3 \bmod 23 = 10$
- $C_2 = 12 \cdot 8^3 \bmod 23 = 12 \cdot 512 \bmod 23 = 6144 \bmod 23 = 6$

**Ciphertext:** $(10, 6)$

**Bob decrypts:**
- $s = 10^6 \bmod 23 = 18$
- $s^{-1} = 18^{-1} \bmod 23 = 9$
- $M = 6 \cdot 9 \bmod 23 = 54 \bmod 23 = 12$ ✓

---

## Forward Secrecy

### The Concept

**Forward secrecy** (Perfect Forward Secrecy - PFS): Compromising long-term keys doesn't compromise past sessions.

**Without PFS:**
- If private key is stolen, attacker can decrypt **all past traffic**
- Recorded ciphertext becomes vulnerable

**With PFS:**
- Each session uses **ephemeral keys** (temporary)
- Session keys are destroyed after use
- Past sessions remain secure even if long-term key is compromised

---

### Ephemeral Diffie-Hellman (DHE/ECDHE)

**How it works:**

1. Generate **new DH keypair** for each session
2. Authenticate using long-term keys (certificates)
3. Destroy ephemeral keys after session

**TLS with ECDHE:**
- Each HTTPS connection uses new ECDH keys
- Old sessions can't be decrypted even if server key leaks

**Standard practice:** Modern TLS requires ECDHE

---

## Key Derivation Functions (KDF)

### The Problem

Raw Diffie-Hellman output isn't suitable as an encryption key:
- May have patterns
- Not uniformly distributed
- Wrong size

---

### HKDF (HMAC-based KDF)

**Standard KDF** for deriving keys from DH shared secret.

**Two phases:**

1. **Extract:** Create pseudo-random key (PRK)
   $$\text{PRK} = \text{HMAC}(\text{salt}, \text{DH-secret})$$

2. **Expand:** Generate output keys of desired length
   $$\text{Key} = \text{HMAC}(\text{PRK}, \text{info} || \text{counter})$$

**Result:** Strong encryption keys from DH output

---

## Authenticated Key Exchange

### The Goal

Combine **key exchange** with **authentication**.

**Requirements:**
- Establish shared secret
- Authenticate both parties
- Resist man-in-the-middle attacks

---

### Station-to-Station (STS) Protocol

**Combines DH with digital signatures:**

1. Alice → Bob: $g^a$
2. Bob → Alice: $g^b$, $\text{Sign}_B(g^a || g^b)$
3. Alice → Bob: $\text{Sign}_A(g^a || g^b)$

**Key:** $K = g^{ab}$

**Security:** Signatures prevent MITM

---

### TLS Handshake (Simplified)

**Modern TLS 1.3 uses authenticated ECDHE:**

1. **ClientHello:** Supported ciphers, ECDHE public key
2. **ServerHello:** Chosen cipher, ECDHE public key, **certificate**
3. **Certificate Verify:** Server signs handshake with private key
4. **Finished:** MAC of handshake messages

**Result:**
- Shared secret from ECDHE
- Server authenticated via certificate
- Client optionally authenticated

---

## Practical Considerations

### Choosing Parameters

**Finite Field DH:**
- **p:** 2048-bit minimum, 3072-bit recommended
- **g:** Standard generator (often 2 or 5)
- Use standardized groups (RFC 3526)

**Elliptic Curve DH:**
- **Curve25519** (most common)
- **P-256** (NIST curve)
- **P-384** (higher security)

---

### Performance

**Comparison (approximate):**

| Operation | DH-2048 | ECDH-256 |
|-----------|---------|----------|
| Key gen | Slow | Fast |
| Shared secret | Slow | Fast |
| Security | High | High |
| Key size | 2048 bits | 256 bits |

**Winner:** ECDH for performance

---

### Common Implementations

**Libraries:**
- OpenSSL (supports DH, ECDH)
- libsodium (Curve25519)
- BoringSSL (Google's fork)

**Protocols:**
- TLS 1.3 (ECDHE mandatory)
- SSH (supports multiple methods)
- IPsec/IKE (DH groups)
- Signal Protocol (X3DH)

---

## Security Best Practices

### 1. Use Ephemeral Keys

Always generate fresh keys per session (DHE/ECDHE).

### 2. Authenticate

Never use plain DH - always authenticate!

### 3. Use KDF

Don't use raw DH output - derive keys properly.

### 4. Choose Strong Parameters

- Minimum 2048-bit DH
- Use well-known curves for ECDH
- Don't roll your own parameters

### 5. Implement Constant-Time

Avoid timing side-channels.

### 6. Check for Zero/Small Subgroups

Validate received public keys.

---

## Attacks on Diffie-Hellman

### 1. Small Subgroup Attack

**Problem:** Weak generator limits key space

**Defense:** Validate public keys, use safe primes

### 2. Logjam Attack (2015)

**Problem:** Reused 512-bit DH parameters

**Defense:** Use 2048+ bit parameters

### 3. Quantum Attacks

**Problem:** Shor's algorithm breaks DL in polynomial time

**Defense:** Transition to post-quantum algorithms

---

## Key Takeaways

1. **Diffie-Hellman** enables secure key exchange over insecure channels
2. **Security** relies on discrete logarithm problem
3. **ECDH** is faster and uses smaller keys than traditional DH
4. **Always authenticate** - plain DH vulnerable to MITM
5. **Ephemeral keys** provide forward secrecy
6. **KDF** derives proper encryption keys from DH output
7. **TLS 1.3** uses authenticated ECDHE by default
8. **Post-quantum** algorithms needed for quantum threat

---

[[../06-Digital-Signatures/signatures|← Digital Signatures]] | [[README|Main Index →]]
