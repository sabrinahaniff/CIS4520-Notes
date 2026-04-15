# CIS 4520: Cryptography Notes

Comprehensive notes for Cryptography course, converted from handwritten notes to Obsidian markdown.

> **Best viewed in [Obsidian](https://obsidian.md)** -  supports math rendering and wiki-links.  
> On GitHub, click the file names below to navigate.

---

## Table of Contents

### 1️ Foundations
- [Matrix Algebra in Cryptography](01-Foundations/matrix-algebra.md)
- [Modular Arithmetic & GCD](01-Foundations/modular-arithmetic.md)

### 2️ Public Key Cryptography
- [RSA Encryption](02-Public-Key/rsa.md)

### 3️ Classical Cryptography
- [Classical Ciphers](03-Classical-Ciphers/classical-ciphers.md)
  - Caesar, Vigenère, Substitution, OTP
  - Rail Fence, Playfair, Hill Cipher
  - Security analysis & frequency attacks

### 4️ Symmetric Cryptography  
- [AES & Block Cipher Modes](04-Symmetric-Crypto/aes.md)
  - AES structure & operations
  - Galois Fields GF(2⁸)
  - Block cipher modes (ECB, CBC, CTR, GCM)
  - Padding schemes

### 5️ Hash Functions
- [Cryptographic Hash Functions](05-Hash-Functions/hash-functions.md)
  - MD5, SHA-1, SHA-2, SHA-3
  - Properties & security requirements
  - Birthday paradox & collision attacks
  - HMAC & password hashing
  - Applications & best practices

### 6️ Digital Signatures
- [Digital Signatures & PKI](06-Digital-Signatures/signatures.md)
  - RSA signatures
  - DSA & ECDSA
  - Signature padding (PSS)
  - Certificate Authorities
  - Chain of trust
  - Nonce reuse attacks

### 7️ Key Exchange
- [Diffie-Hellman & Key Exchange](07-Key-Exchange/diffie-hellman.md)
  - Diffie-Hellman protocol
  - Discrete logarithm problem
  - ECDH (Elliptic Curve DH)
  - ElGamal encryption
  - Forward secrecy & ephemeral keys
  - Man-in-the-middle attacks
  - Authenticated key exchange (STS, TLS)
  - KDF & best practices

---

##  Quick Reference

### Key Concepts

**Matrix Operations:**
- Determinant: tells if a matrix is invertible
- Matrix multiplication: row × column (order matters!)
- Inverse matrix: $A^{-1}$ where $AA^{-1} = I$

**Modular Arithmetic:**
- $a \equiv b \pmod{m}$
- Used in all modern encryption

---

## How to Use These Notes

### Option 1: View on GitHub
- Click any `.md` file above to read
- GitHub supports LaTeX math rendering ✓

### Option 2: Clone and Use in Obsidian (Recommended)

1. **Clone this repo:**
   ```bash
   git clone https://github.com/sabrinahaniff/CIS4520.git
   ```

2. **Open in Obsidian:**
   - Download [Obsidian](https://obsidian.md)
   - Open folder as vault
   - Wiki-links and graph view work automatically!

---

##  Course Information

**Course:** CIS4520: Introduction to Cryptography  

**Topics Covered:**
- Mathematical Foundations (Matrix Algebra, Modular Arithmetic)
- Classical Ciphers (Caesar, Vigenère, Hill)
- Symmetric Encryption (DES, AES)
- Public Key Cryptography (RSA, Diffie-Hellman, ECC)
- Hash Functions (MD5, SHA)
- Digital Signatures
- Cryptographic Protocols

---

## Status

**Last updated:** April 15th, 2026

---

*Study notes for CIS 4520. Feel free to use for studying!*
