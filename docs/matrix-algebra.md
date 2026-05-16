# Matrix Algebra in Cryptography

## Why Matrices in Crypto?

Matrices are used to:
- **Mix information** in a structured way
- **Hide patterns** in data
- **Build ciphers** (e.g., Hill cipher)
- **Formalize systems of equations**

**Think of a matrix as a table of numbers used to store and combine equations.**

**Key insight:** Matrices let us represent multiple equations at once, solve them, and check whether solutions exist.

---

## What is a Matrix?

A **matrix** is a rectangular array of numbers arranged in rows and columns.

### Notation

An $m \times n$ matrix has:
- $m$ rows
- $n$ columns

### Example

$$A = \begin{bmatrix} 1 & -3 & 5 \\ -6 & 1 & 7 \end{bmatrix}$$

This is a $2 \times 3$ matrix (2 rows, 3 columns).

---

## Square Matrices

A **square matrix** has the same number of rows and columns.

### Example

$$A = \begin{bmatrix} 1 & 5 & 7 \\ 2 & -3 & 4 \\ 0 & 6 & -2 \end{bmatrix}$$

This is a $3 \times 3$ square matrix.

**Why they matter:** Square matrices are the only matrices that can have inverses, which is crucial for encryption/decryption.

---

## Matrix Multiplication

### Why Order Matters

Matrix multiplication is **not** entry-by-entry. It is **row × column**.

### The Rule

If $A$ is $m \times n$ and $B$ is $n \times p$, then $AB$ is $m \times p$.

**Important:** The number of columns in the first matrix must equal the number of rows in the second matrix.

### Example

$$\begin{bmatrix} 2 & 3 \\ 1 & 4 \end{bmatrix} \begin{bmatrix} 5 & 7 \\ 6 & 8 \end{bmatrix} = \begin{bmatrix} 2(5)+3(6) & 2(7)+3(8) \\ 1(5)+4(6) & 1(7)+4(8) \end{bmatrix}$$

$$= \begin{bmatrix} 10+18 & 14+24 \\ 5+24 & 7+32 \end{bmatrix} = \begin{bmatrix} 28 & 38 \\ 29 & 39 \end{bmatrix}$$

### Matrix Multiplication Algorithm

To compute entry $(i, j)$ in the product $AB$:
1. Take row $i$ from matrix $A$
2. Take column $j$ from matrix $B$
3. Multiply corresponding elements
4. Sum the products

---

## Systems of Equations Become Matrix Equations

A system of linear equations can be written as a single matrix equation.

### Example

System:

$$\begin{cases} 2x + 3y = 7 \\ x + 4y = 10 \end{cases}$$

Matrix form:

$$\begin{bmatrix} 2 & 3 \\ 1 & 4 \end{bmatrix} \begin{bmatrix} x \\ y \end{bmatrix} = \begin{bmatrix} 7 \\ 10 \end{bmatrix}$$

Written as: $A\vec{x} = \vec{b}$

---

## Determinants

### What is a Determinant?

The **determinant** tells you:
- Whether a matrix is **invertible**
- Whether a system of equations has:
  - **One unique solution** (det ≠ 0)
  - **Infinitely many solutions** (det = 0, consistent)
  - **No solution** (det = 0, inconsistent)

### Computing Determinant of 2×2 Matrix

For $A = \begin{bmatrix} a & b \\ c & d \end{bmatrix}$:

$$\det(A) = |A| = ad - bc$$

### Example

$$A = \begin{bmatrix} 2 & 3 \\ 1 & 4 \end{bmatrix}$$

$$\det(A) = (2)(4) - (3)(1) = 8 - 3 = 5$$

Since $\det(A) \neq 0$, matrix $A$ is invertible.

---

## Matrix Inverse

### Definition

If $A$ is a square matrix with $\det(A) \neq 0$, then there exists a matrix $A^{-1}$ (the inverse) such that:

$$AA^{-1} = A^{-1}A = I$$

where $I$ is the **identity matrix**.

### Identity Matrix

$$I = \begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix} \quad \text{(for 2×2)}$$

$$I = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix} \quad \text{(for 3×3)}$$

**Property:** $AI = IA = A$ for any matrix $A$

---

## Inverse of 2×2 Matrix

For $A = \begin{bmatrix} a & b \\ c & d \end{bmatrix}$:

$$A^{-1} = \frac{1}{\det(A)} \begin{bmatrix} d & -b \\ -c & a \end{bmatrix}$$

$$A^{-1} = \frac{1}{ad - bc} \begin{bmatrix} d & -b \\ -c & a \end{bmatrix}$$

### Example

Find $A^{-1}$ for $A = \begin{bmatrix} 2 & 3 \\ 1 & 4 \end{bmatrix}$

**Step 1:** Compute determinant

$$\det(A) = (2)(4) - (3)(1) = 5$$

**Step 2:** Apply formula

$$A^{-1} = \frac{1}{5} \begin{bmatrix} 4 & -3 \\ -1 & 2 \end{bmatrix} = \begin{bmatrix} 4/5 & -3/5 \\ -1/5 & 2/5 \end{bmatrix}$$

**Verification:**

$$AA^{-1} = \begin{bmatrix} 2 & 3 \\ 1 & 4 \end{bmatrix} \begin{bmatrix} 4/5 & -3/5 \\ -1/5 & 2/5 \end{bmatrix} = \begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix} = I \quad \checkmark$$

---

## Solving $A\vec{x} = \vec{b}$ Using Inverse

If $A$ is invertible, then:

$$A\vec{x} = \vec{b}$$

$$A^{-1}(A\vec{x}) = A^{-1}\vec{b}$$

$$(A^{-1}A)\vec{x} = A^{-1}\vec{b}$$

$$I\vec{x} = A^{-1}\vec{b}$$

$$\vec{x} = A^{-1}\vec{b}$$

### Example

Solve:

$$\begin{bmatrix} 2 & 3 \\ 1 & 4 \end{bmatrix} \begin{bmatrix} x \\ y \end{bmatrix} = \begin{bmatrix} 7 \\ 10 \end{bmatrix}$$

We found $A^{-1} = \begin{bmatrix} 4/5 & -3/5 \\ -1/5 & 2/5 \end{bmatrix}$

$$\begin{bmatrix} x \\ y \end{bmatrix} = \begin{bmatrix} 4/5 & -3/5 \\ -1/5 & 2/5 \end{bmatrix} \begin{bmatrix} 7 \\ 10 \end{bmatrix}$$

$$= \begin{bmatrix} (4/5)(7) + (-3/5)(10) \\ (-1/5)(7) + (2/5)(10) \end{bmatrix}$$

$$= \begin{bmatrix} 28/5 - 30/5 \\ -7/5 + 20/5 \end{bmatrix} = \begin{bmatrix} -2/5 \\ 13/5 \end{bmatrix}$$

**Solution:** $x = -\frac{2}{5}, \quad y = \frac{13}{5}$

---

## Application to Cryptography

### The Hill Cipher

The **Hill cipher** uses matrix multiplication to encrypt messages.

**Encryption:** $C = PK \pmod{26}$
- $P$ = plaintext matrix
- $K$ = key matrix
- $C$ = ciphertext matrix

**Decryption:** $P = CK^{-1} \pmod{26}$

**Requirements:**
- $K$ must be invertible
- $\det(K) \neq 0 \pmod{26}$
- $\gcd(\det(K), 26) = 1$

---

## Key Takeaways

1. **Matrices organize information** systematically
2. **Determinant determines invertibility** (crucial for encryption)
3. **Matrix inverse enables decryption** (reverse the encryption)
4. **Order matters** in matrix multiplication
5. **Square matrices** are needed for inverse operations

---

[← Back to Main](README.md)
