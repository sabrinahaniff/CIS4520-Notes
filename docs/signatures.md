# Digital Signatures

## What is a Digital Signature?

A **digital signature** is the cryptographic equivalent of a handwritten signature.

**Purpose:**
- **Authentication** - Proves who sent the message
- **Non-repudiation** - Sender can't deny sending it
- **Integrity** - Detects if message was modified

**Analogy:** Like signing a legal document, but mathematically verifiable.

---

## How Digital Signatures Work

### Signing Process

1. **Hash** the message: $h = H(m)$
2. **Encrypt hash** with private key: $s = \text{Sign}_{SK}(h)$
3. **Send** message $m$ and signature $s$

### Verification Process

1. **Hash** the received message: $h' = H(m)$
2. **Decrypt signature** with public key: $h = \text{Verify}_{PK}(s)$
3. **Compare:** If $h = h'$ → signature valid ✓

---

## Why Sign the Hash?

**Don't sign the message directly!**

**Reasons:**
1. **Efficiency** - Hash is small (256 bits), message could be gigabytes
2. **RSA limitations** - Can only sign data smaller than modulus
3. **Security** - Hash ensures integrity

---

## RSA Signatures

### Key Generation

Same as RSA encryption:
1. Choose primes $p, q$
2. Compute $n = pq$
3. Choose $e$ (public exponent)
4. Compute $d = e^{-1} \bmod \phi(n)$ (private exponent)

**Keys:**
- **Public key:** $(n, e)$ - for verification
- **Private key:** $(n, d)$ - for signing

---

### Signing

$$s = H(m)^d \bmod n$$

where:
- $m$ = message
- $H$ = hash function (e.g., SHA-256)
- $d$ = private key
- $s$ = signature

---

### Verification

$$h = s^e \bmod n$$

Check: $h \stackrel{?}{=} H(m)$

**If equal:** Signature is valid ✓  
**If different:** Signature is invalid or message was tampered ✗

---

### Example

**Key generation:**
- $p = 61, q = 53$
- $n = 3233$
- $e = 17, d = 2753$

**Signing message** "HELLO":
1. Hash: $H(\text{"HELLO"}) = 123$ (simplified)
2. Sign: $s = 123^{2753} \bmod 3233 = 855$
3. Signature: $s = 855$

**Verification:**
1. Receive: message "HELLO" and signature $s = 855$
2. Compute: $h = 855^{17} \bmod 3233 = 123$
3. Check: $H(\text{"HELLO"}) = 123$ ✓
4. **Valid!**

---

## Why RSA Signatures Work

### Encryption vs Signing

**Encryption:**
$$C = M^e \bmod n \quad (\text{public key})$$
$$M = C^d \bmod n \quad (\text{private key})$$

**Signing (reverse order):**
$$s = H(m)^d \bmod n \quad (\text{private key})$$
$$h = s^e \bmod n \quad (\text{public key})$$

### The Math

Since:
$$(H(m)^d)^e \equiv H(m)^{de} \equiv H(m) \pmod{n}$$

Only the person with private key $d$ can create valid signature!

---

## DSA (Digital Signature Algorithm)

### Overview

**DSA** is a signature scheme specifically designed for signing (not encryption).

**Used in:** US government, SSH keys, some TLS

### Parameters

**Domain parameters** (shared by everyone):
- $p$ - large prime (2048+ bits)
- $q$ - prime divisor of $p-1$ (256 bits)
- $g$ - generator

**Keys:**
- Private key: $x$ (random, $0 < x < q$)
- Public key: $y = g^x \bmod p$

---

### DSA Signing

**Input:** Message $m$

1. Choose random $k$ where $0 < k < q$
2. Compute $r = (g^k \bmod p) \bmod q$
3. Compute $s = k^{-1}(H(m) + xr) \bmod q$

**Signature:** $(r, s)$

**CRITICAL:** $k$ must be **truly random** and **never reused!**

---

### DSA Verification

**Input:** Message $m$, signature $(r, s)$, public key $y$

1. Compute $w = s^{-1} \bmod q$
2. Compute $u_1 = H(m) \cdot w \bmod q$
3. Compute $u_2 = r \cdot w \bmod q$
4. Compute $v = (g^{u_1} y^{u_2} \bmod p) \bmod q$

**Check:** $v \stackrel{?}{=} r$

If equal → valid ✓

---

## ECDSA (Elliptic Curve DSA)

### Overview

**ECDSA** is DSA using elliptic curve cryptography.

**Advantages over DSA/RSA:**
- **Smaller keys** - 256-bit ECDSA ≈ 3072-bit RSA
- **Faster** operations
- **Same security** with less computation

**Used in:** Bitcoin, Ethereum, TLS, SSH

---

### Elliptic Curve Basics

**Curve equation:**
$$y^2 = x^3 + ax + b$$

**Point addition:** Special geometric operation

**Scalar multiplication:**
$$kP = P + P + \cdots + P \quad (k \text{ times})$$

**Hard problem:** Given $P$ and $Q = kP$, find $k$ (discrete log)

---

### ECDSA Signing

**Input:** Message $m$, private key $d$

1. Choose random $k$
2. Compute point $(x_1, y_1) = kG$ (where $G$ is base point)
3. Compute $r = x_1 \bmod n$
4. Compute $s = k^{-1}(H(m) + dr) \bmod n$

**Signature:** $(r, s)$

**CRITICAL:** Never reuse $k$!

---

### ECDSA Verification

**Input:** Message $m$, signature $(r, s)$, public key $Q$

1. Compute $w = s^{-1} \bmod n$
2. Compute $u_1 = H(m) \cdot w \bmod n$
3. Compute $u_2 = r \cdot w \bmod n$
4. Compute point $(x_1, y_1) = u_1 G + u_2 Q$
5. Check: $r \stackrel{?}{=} x_1 \bmod n$

---

## Comparison of Signature Schemes

| Scheme | Key Size | Signature Size | Speed | Security |
|--------|----------|----------------|-------|----------|
| RSA-2048 | 2048 bits | 2048 bits | Slow sign, fast verify | High |
| RSA-3072 | 3072 bits | 3072 bits | Slower | Very high |
| DSA-2048 | 2048 bits | 320 bits | Medium | High |
| ECDSA-256 | 256 bits | 512 bits | Fast | High (≈ RSA-3072) |

**Trend:** Moving toward ECDSA for efficiency.

---

## Critical Security Issue: Nonce Reuse

### The PlayStation 3 Hack (2010)

Sony's PS3 signing keys were compromised because:

**Problem:** They **reused the same $k$ value** in ECDSA!

**Attack:**
- Two signatures $(r_1, s_1)$ and $(r_2, s_2)$ with same $k$
- Since $r_1 = r_2$, attacker can solve for $k$
- Once $k$ is known, private key $d$ can be recovered!

$$k = \frac{H(m_1) - H(m_2)}{s_1 - s_2} \bmod n$$

$$d = \frac{sk - H(m)}{r} \bmod n$$

**Lesson:** **NEVER** reuse nonces in signatures!

---

## Signature Padding (RSA-PSS)

### Problem with Textbook RSA

**Textbook RSA signature:**
$$s = H(m)^d \bmod n$$

**Vulnerabilities:**
- Deterministic (no randomness)
- Malleable (can manipulate)
- Vulnerable to chosen-message attacks

---

### RSA-PSS (Probabilistic Signature Scheme)

Adds **randomness** and **structure** before signing.

**Process:**
1. Hash message: $h = H(m)$
2. Generate random salt
3. Apply padding scheme with hash and salt
4. Sign the padded value

**Benefits:**
- **Non-deterministic** (different signature each time)
- **Provably secure**
- **Prevents forgery**

**Current standard:** Always use PSS padding for RSA signatures!

---

## Certificate Authorities & PKI

### The Trust Problem

**Question:** How do you know a public key belongs to who it claims?

**Without trust:** Man-in-the-middle attacks possible

---

### Digital Certificates

A **certificate** binds a public key to an identity.

**Contains:**
- Subject name (e.g., "google.com")
- Subject public key
- Issuer (Certificate Authority)
- Validity period
- **Signature from CA**

**Format:** X.509

---

### Certificate Authority (CA)

A **trusted third party** that signs certificates.

**Process:**
1. Website generates key pair
2. Sends public key + identity to CA
3. CA verifies identity
4. CA signs certificate: $\text{Cert} = \text{Sign}_{CA}(h)$
5. Website uses certificate

**Verification:**
1. Browser receives certificate
2. Verifies CA signature using CA's public key
3. If valid → trust the website's public key

---

### Chain of Trust

**Root CAs:** Pre-installed in browsers/OS

**Intermediate CAs:** Signed by root CAs

**End-entity certificates:** Signed by intermediate CAs

```
Root CA (self-signed)
    ↓ signs
Intermediate CA
    ↓ signs
Website Certificate (google.com)
```

**Verification:** Follow chain up to trusted root.

---

## Common Attacks on Signatures

### 1. Key-Only Attack

Attacker has:
- Public key
- No message-signature pairs

**Goal:** Forge a signature

**Defense:** Proper signature scheme (RSA-PSS, ECDSA)

---

### 2. Known-Message Attack

Attacker has:
- Public key
- Some valid message-signature pairs

**Defense:** Hash randomization, PSS padding

---

### 3. Chosen-Message Attack

Attacker can get signatures on chosen messages.

**Example:** Blind signatures, multiple legitimate uses

**Defense:** Secure signature scheme with proof

---

### 4. Hash Collision Attack

If attacker finds $m_1$ and $m_2$ where $H(m_1) = H(m_2)$:
- Get signature on $m_1$
- Signature also valid for $m_2$!

**Defense:** Use collision-resistant hash (SHA-256, SHA-3)

---

## Applications of Digital Signatures

### 1. Software Distribution

Sign software packages to prove authenticity.

**Example:** App stores, OS updates

---

### 2. Email (S/MIME, PGP)

Sign emails to prove sender and detect tampering.

---

### 3. Code Signing

Prove code hasn't been modified.

**Example:** Driver signing, kernel modules

---

### 4. Blockchain/Cryptocurrency

Every transaction is signed.

**Bitcoin:** ECDSA signatures prove ownership.

---

### 5. Legal Documents

Electronic signatures with legal validity.

**Standards:** eIDAS (EU), ESIGN Act (US)

---

### 6. TLS/SSL

Server signs handshake to prove identity.

---

## Key Takeaways

1. **Digital signatures** provide authentication, integrity, and non-repudiation
2. **Always sign the hash**, never the message directly
3. **RSA** can be used for signatures (reverse of encryption)
4. **DSA** is designed specifically for signatures
5. **ECDSA** offers same security with smaller keys
6. **Never reuse nonces** (k values) in DSA/ECDSA
7. **Use PSS padding** for RSA signatures
8. **Certificate Authorities** establish trust in public keys
9. **Chain of trust** connects certificates to root CAs

---

[[../05-Hash-Functions/hash-functions|← Hash Functions]] | [Next: Key Exchange →]
