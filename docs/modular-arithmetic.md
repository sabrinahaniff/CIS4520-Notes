# Modular Arithmetic & GCD

## Greatest Common Divisor (GCD)

### Definition

The **greatest common divisor (GCD)** of two integers $a$ and $b$ is the largest integer that divides both $a$ and $b$.

**Notation:** $\gcd(a, b)$

### Example

$$\gcd(29, 8)$$

**Finding factors:**
- Factors of 29: 1, 29
- Factors of 8: 1, 2, 4, 8
- Common factors: 1

$$\gcd(29, 8) = 1$$

---

## The Euclidean Algorithm

> **Goal:** Compute $\gcd(a, b)$ **fast** without factoring!

### Why It Works

The "block sizes that work" don't change when we replace the bigger number by the leftover (remainder).

### The Algorithm

**Repeat:**
1. Divide: $a = bq + r$
2. Replace: $(a, b) \to (b, r)$
3. Stop when $r = 0$

**Answer:** The last non-zero remainder is the GCD.

---

### Example: Find $\gcd(3959, 1177)$

$$3959 = 1177 \times 3 + 428$$
$$1177 = 428 \times 2 + 321$$
$$428 = 321 \times 1 + 107$$
$$321 = 107 \times 3 + 0$$

**Last non-zero remainder:** $\boxed{\gcd(3959, 1177) = 107}$

---

### Step-by-Step Process

| Step | Equation | $(a, b)$ |
|------|----------|----------|
| 1 | $29 = 8 \times 3 + 5$ | $(29, 8) \to (8, 5)$ |
| 2 | $8 = 5 \times 1 + 3$ | $(8, 5) \to (5, 3)$ |
| 3 | $5 = 3 \times 1 + 2$ | $(5, 3) \to (3, 2)$ |
| 4 | $3 = 2 \times 1 + 1$ | $(3, 2) \to (2, 1)$ |
| 5 | $2 = 1 \times 2 + 0$ | $(2, 1) \to (1, 0)$ |

$$\gcd(29, 8) = 1$$

---

## Modular Arithmetic

> **Idea:** You only care about the **remainder** after dividing by something.

### Definition

Let $a, b, n$ be integers with $n > 0$. We say:

$$a \equiv b \pmod{n}$$

if $n$ divides $(a - b)$.

**In other words:** $a$ and $b$ have the **same remainder** when divided by $n$.

---

### Examples

**Is $17 \equiv 5 \pmod{6}$?**

$$17 - 5 = 12$$
$$12 \div 6 = 2 \quad \text{(exactly)}$$

Since 6 divides 12, **yes**: $17 \equiv 5 \pmod{6}$ ✓

---

**Is $23 \equiv 7 \pmod{8}$?**

$$23 - 7 = 16$$
$$16 \div 8 = 2 \quad \text{(exactly)}$$

**Yes**: $23 \equiv 7 \pmod{8}$ ✓

---

### Computing $a \bmod n$

Find the **remainder** when $a$ is divided by $n$.

**Examples:**
- $17 \bmod 5 = 2$ (since $17 = 5 \times 3 + 2$)
- $100 \bmod 7 = 2$ (since $100 = 7 \times 14 + 2$)
- $-3 \bmod 5 = 2$ (since $-3 = 5 \times (-1) + 2$)

---

## Modular Addition

$$(a + b) \bmod n = ((a \bmod n) + (b \bmod n)) \bmod n$$

### Example

Compute $(17 + 23) \bmod 8$:

**Method 1:** Direct
$$17 + 23 = 40$$
$$40 \bmod 8 = 0$$

**Method 2:** Reduce first
$$17 \bmod 8 = 1$$
$$23 \bmod 8 = 7$$
$$(1 + 7) \bmod 8 = 8 \bmod 8 = 0$$

---

## Modular Multiplication

$$(a \times b) \bmod n = ((a \bmod n) \times (b \bmod n)) \bmod n$$

### Example

Compute $(17 \times 23) \bmod 8$:

**Method 1:** Direct
$$17 \times 23 = 391$$
$$391 \bmod 8 = 7$$

**Method 2:** Reduce first
$$17 \bmod 8 = 1$$
$$23 \bmod 8 = 7$$
$$(1 \times 7) \bmod 8 = 7$$

---

## Modular Exponentiation

To compute $a^n \bmod m$ efficiently:

### Fast Exponentiation (Square and Multiply)

**Idea:** Break down the exponent using binary representation.

$$a^{13} = a^8 \times a^4 \times a^1$$

(because $13 = 8 + 4 + 1$ in binary: $1101_2$)

### Example: Compute $7^{13} \bmod 11$

**Binary:** $13 = 1101_2$

| Power | Calculation | Result mod 11 |
|-------|-------------|---------------|
| $7^1$ | $7$ | $7$ |
| $7^2$ | $7^2 = 49$ | $5$ |
| $7^4$ | $5^2 = 25$ | $3$ |
| $7^8$ | $3^2 = 9$ | $9$ |

$$7^{13} = 7^8 \times 7^4 \times 7^1 \equiv 9 \times 3 \times 7 \equiv 189 \equiv 2 \pmod{11}$$

---

## Modular Inverse

### Definition

The **modular inverse** of $a$ modulo $n$ is a number $a^{-1}$ such that:

$$a \times a^{-1} \equiv 1 \pmod{n}$$

### When Does It Exist?

$a^{-1} \bmod n$ exists **if and only if** $\gcd(a, n) = 1$

(i.e., $a$ and $n$ are **coprime**)

### Example

Find $7^{-1} \bmod 11$:

We need: $7x \equiv 1 \pmod{11}$

**Testing:**
- $7 \times 1 = 7 \not\equiv 1$
- $7 \times 2 = 14 \equiv 3 \not\equiv 1$
- $7 \times 3 = 21 \equiv 10 \not\equiv 1$
- $7 \times 4 = 28 \equiv 6 \not\equiv 1$
- $7 \times 5 = 35 \equiv 2 \not\equiv 1$
- $7 \times 6 = 42 \equiv 9 \not\equiv 1$
- $7 \times 7 = 49 \equiv 5 \not\equiv 1$
- $7 \times 8 = 56 \equiv 1$ ✓

$$7^{-1} \equiv 8 \pmod{11}$$

---

## Extended Euclidean Algorithm

Finds integers $x$ and $y$ such that:

$$ax + by = \gcd(a, b)$$

This is used to compute **modular inverses** efficiently.

---

## Applications to Cryptography

### Why Modular Arithmetic Matters

1. **Keeps numbers bounded** (prevents overflow)
2. **Makes operations reversible** (with modular inverse)
3. **Foundation of RSA** (encryption/decryption uses mod $n$)
4. **Diffie-Hellman** key exchange
5. **Digital signatures**

---

## Key Takeaways

1. **Euclidean algorithm** computes GCD fast
2. **Modular arithmetic** = clock arithmetic (wrap around)
3. **Modular inverse** exists only when $\gcd(a, n) = 1$
4. **Fast exponentiation** uses binary representation
5. All modern crypto uses **modular arithmetic**

---

[[matrix-algebra|← Matrix Algebra]] | [[../02-Public-Key/rsa|RSA →]]
