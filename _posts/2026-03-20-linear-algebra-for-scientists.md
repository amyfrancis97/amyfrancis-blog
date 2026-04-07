---
layout: post
title: "Linear algebra for scientists - a coding-first guide"
description: "A plain-language, code-first walkthrough of the linear algebra behind machine learning. Based on Chapter 2 of the Deep Learning textbook - written for scientists who didn't come from the maths side."
standfirst: "Most machine learning tutorials assume you already know your eigenvectors from your eigenvalues. This one doesn't. Each concept is introduced in plain language, paired with runnable Python code, and grounded in examples from real scientific data."
category: tutorial
category_label: "Tutorials & Guides"
date: 2026-03-20
read_time: 25
---

This is a coding-first walkthrough of the linear algebra that underpins machine learning - based on Chapter 2 of [Deep Learning](https://www.deeplearningbook.org/) by Goodfellow, Bengio and Courville, but written for scientists coming from biology or
chemistry rather than maths or computer science.

Each section covers one concept: what it is (in plain language), why it matters for data science, and how to use it in Python. No assumed background beyond basic scientific data familiarity and some Python coding experience.


<div class="divider"></div>

## Why linear algebra shows up everywhere in machine learning

Most scientific datasets can be written as a matrix: rows are samples, columns are variables. Once your data is in that form, almost everything you do to it-fitting models, reducing dimensionality, filtering noise, finding patterns-can be expressed as matrix operations.

Linear algebra is the language that makes those operations precise and scalable:

- A linear model is just a matrix multiplication  
- Least-squares fitting is solving \(Ax = b\)  
- PCA is an eigendecomposition of a covariance matrix  
- Noise filtering is truncating an SVD  

Most machine learning algorithms can be viewed as choosing parameters that minimise some norm of the error.

The goal of this guide is not to teach abstract mathematics, but to show how these operations arise naturally when working with real scientific data, and how to use them directly in Python.

**Requirements:** `numpy`, `matplotlib`, `seaborn`

```bash
pip install numpy matplotlib seaborn
```

<div class="divider"></div>

## 1. Scalars, Vectors, Matrices and Tensors

Linear algebra is just a formal way of organising numbers. As a scientist you already work with all of these objects - the notation is the only unfamiliar part.

| Object | Description | Scientific example |
|--------|-------------|-------------------|
| **Scalar** | A single number | A temperature, a p-value, a rate constant |
| **Vector** | An ordered list of numbers | Measurements on one sample: [pH, Temp, Absorbance] |
| **Matrix** | A 2D table (rows x columns) | Samples x variables data table |
| **Tensor** | 3+ dimensional array | Time-series of 2D images (width x height x time) |

![Vectors and matrices illustrated](/assets/images/linear-algebra/01_vectors_matrices.png)

```python
import numpy as np

# A scalar
temperature = 37.2

# A vector: three measurements on one sample
sample = np.array([7.2, 22.1, 0.81])  # [pH, Temp(C), Absorbance]
print(f"pH: {sample[0]}, Temp: {sample[1]}, Abs: {sample[2]}")

# A matrix: 5 samples x 3 variables
data = np.array([
    [7.2, 22.1, 0.81],
    [6.8, 25.3, 0.63],
    [7.5, 19.8, 0.92],
    [6.5, 28.0, 0.55],
    [7.1, 21.5, 0.78],
])
print(f"Matrix shape: {data.shape}")  # (5, 3)

# Transpose: flip rows and columns
print(data.T.shape)  # (3, 5)
```

> **Transpose** flips a matrix across its diagonal, so rows become columns. If your data matrix is (samples x variables), its transpose is (variables x samples).

<div class="divider"></div>

## 2. Matrix Multiplication

Matrix multiplication is not element-wise. It combines rows of one matrix with columns of another to produce a new matrix. Geometrically, it *transforms space* - rotating, stretching, or shearing data. This comes up in coordinate changes, mixing variables, and representing linear models.

![Matrix multiplication as a geometric transformation](/assets/images/linear-algebra/02_matrix_multiplication.png)

```python
import numpy as np

# Define a transformation matrix
A = np.array([[2,   0.5],
              [0.3, 1.5]])

# Apply A to a set of 2D points (unit square corners)
square = np.array([[0, 1, 1, 0],
                   [0, 0, 1, 1]], dtype=float)

transformed = A @ square  # @ is Python's matrix multiply operator
print("Original points:\n", square.T)
print("Transformed points:\n", transformed.T)

# Dot product: how aligned are two vectors?
v1 = np.array([1.0, 2.0, 3.0])  # e.g. spectrum 1
v2 = np.array([1.1, 1.9, 3.1])  # e.g. spectrum 2
similarity = v1 @ v2
print(f"Dot product: {similarity:.3f}")
```

> Matrix multiplication is *not* commutative. \(AB \neq BA\) in general, just as rotate-then-translate and translate-then-rotate give different results.

<div class="divider"></div>

## 3. Solving Systems of Equations

A system of simultaneous equations like:

\[2x + y = 8\]
\[x - y = 1\]

can be written as \(\mathbf{Ax = b}\), where \(\mathbf{A}\) encodes the coefficients, \(\mathbf{x}\) the unknowns, and \(\mathbf{b}\) the outcomes. Writing it this way scales to thousands of equations with no change in notation. Relevant in practice for spectral deconvolution, calibration, and model fitting.

![Solving a linear system geometrically](/assets/images/linear-algebra/03_linear_system.png)

```python
import numpy as np

A = np.array([[2,  1],
              [1, -1]], dtype=float)
b = np.array([8, 1], dtype=float)

# Solve exactly (works when A is square and non-singular)
x = np.linalg.solve(A, b)
print(f"Solution: x = {x[0]:.2f}, y = {x[1]:.2f}")  # x=3.0, y=2.0

# Verify
print(f"Check Ax = {A @ x}  (should equal {b})")

# Real-world: spectral deconvolution
# A = extinction coefficients matrix (compounds x wavelengths)
# b = measured absorbance vector
# x = concentrations of each compound
```

<div class="divider"></div>

## 4. Identity and Inverse Matrices

The **identity matrix** \(I\) is the matrix equivalent of the number 1 - multiplying anything by it leaves it unchanged. It has 1s along the main diagonal and 0s everywhere else.

The **inverse** of a matrix \(A\), written \(A^{-1}\), is the matrix that undoes \(A\)'s transformation. Together they satisfy \(A^{-1}A = I\). This is how we analytically solve \(\mathbf{Ax = b}\): multiply both sides by \(A^{-1}\) to get \(\mathbf{x} = A^{-1}\mathbf{b}\).

Not every matrix has an inverse. If the columns of \(A\) are linearly dependent, the determinant is zero and no inverse exists - such a matrix is called **singular**.

![Identity matrix, inversion, and singularity](/assets/images/linear-algebra/04_identity_inverse.png)

```python
import numpy as np

I = np.eye(3)
print("3x3 Identity matrix:\n", I)

A = np.array([[3., 1.],
              [2., 2.]])

A_inv = np.linalg.inv(A)
print("\nA @ A_inv:\n", np.round(A @ A_inv, 3))  # identity

# Solve Ax = b using the inverse
b = np.array([5., 6.])
x = A_inv @ b
print(f"Solution x = {x}")

# Check if a matrix is singular
singular = np.array([[1., 2.],
                     [2., 4.]])   # second row = 2 x first row
print(f"\nDeterminant: {np.linalg.det(singular):.1f}")  # 0.0
```

<div class="callout">
  <div class="callout-label">Worth knowing</div>
  <p>In practice, computing \(A^{-1}\) explicitly and then multiplying is often numerically less stable than using <code>np.linalg.solve(A, b)</code> directly. The solve function uses more robust algorithms internally. Reserve explicit inversion for cases where you genuinely need the inverse matrix itself.</p>
</div>

<div class="divider"></div>

## 5. Linear Dependence and Span

The **span** of a set of vectors is the set of all points you can reach by scaling and adding those vectors together. Think of it as the "reachable space" given those directions.

Vectors are **linearly independent** if none of them can be written as a combination of the others. If one can, the set is **linearly dependent** and the redundant vector adds no new reachable space.

This matters for \(\mathbf{Ax = b}\): a solution exists only if \(\mathbf{b}\) lies within the column space (span of the columns of \(A\)), and a unique solution only exists if the columns are all linearly independent.

![Linear dependence and span illustrated](/assets/images/linear-algebra/05_linear_dependence.png)

```python
import numpy as np

# Two linearly independent vectors: span all of 2D space
v1 = np.array([2.0, 0.5])
v2 = np.array([0.5, 2.0])

target = np.array([1.5, 1.8])
A = np.column_stack([v1, v2])
coeffs = np.linalg.solve(A, target)
print(f"target = {coeffs[0]:.2f}*v1 + {coeffs[1]:.2f}*v2")

# Two linearly dependent vectors: span only a 1D line
v1 = np.array([1.0, 2.0])
v2 = np.array([2.0, 4.0])   # = 2 * v1 -- redundant

A_indep = np.column_stack([np.array([2., 0.5]), np.array([0.5, 2.])])
A_dep   = np.column_stack([v1, v2])
print(f"Rank of independent matrix: {np.linalg.matrix_rank(A_indep)}")  # 2
print(f"Rank of dependent matrix:   {np.linalg.matrix_rank(A_dep)}")    # 1
```

> **Rank** tells you the true dimensionality of a matrix's column space. If `np.linalg.matrix_rank(A) < A.shape[1]`, your variables are collinear and you should consider dimensionality reduction before fitting models.

<div class="divider"></div>

## 6. Norms

A norm measures the magnitude of a vector. Different norms treat distance differently, and the choice has real consequences for how models behave during fitting.

![The three main norm unit balls](/assets/images/linear-algebra/06_norms.png)

```python
import numpy as np

v = np.array([3.0, -4.0, 1.0])  # e.g. a residual vector

# L2 norm (Euclidean): standard geometric distance
l2 = np.linalg.norm(v)
print(f"L2 norm: {l2:.3f}")     # 5.099

# L1 norm (Manhattan / LASSO): sum of absolute values
l1 = np.linalg.norm(v, ord=1)
print(f"L1 norm: {l1:.3f}")     # 8.0

# L-inf norm (Max norm): largest absolute value
linf = np.linalg.norm(v, ord=np.inf)
print(f"L-inf norm: {linf:.3f}") # 4.0

# RMSD
residuals = np.array([0.1, -0.3, 0.2, 0.05, -0.15])
rmsd = np.sqrt(np.mean(residuals**2))
print(f"RMSD: {rmsd:.4f}")
```

| Norm | Formula | Prefers | Used in |
|------|---------|---------|---------|
| \(L^2\) | \(\sqrt{\sum x_i^2}\) | All residuals small | RMSD, Ridge regression |
| \(L^1\) | \(\sum \|x_i\|\) | Many exact zeros | LASSO, sparse models |
| \(L^\infty\) | \(\max \|x_i\|\) | Worst-case element | Robust optimisation |

<div class="divider"></div>

## 7. Special Matrix Structures

Some matrix structures come up constantly and have properties that make computation much cheaper.

```python
import numpy as np

# Diagonal matrix: non-zero only on the main diagonal
d = np.diag([2.0, 3.0, 0.5])
x = np.array([1.0, 2.0, 4.0])
print("d @ x =", d @ x)   # [2, 6, 2], each element scaled separately

# Symmetric matrix: A = A^T (covariance, distances)
np.random.seed(42)
X = np.random.randn(50, 3)
cov = X.T @ X / (50 - 1)
print("\nIs symmetric:", np.allclose(cov, cov.T))  # True

# Orthogonal matrix: rotation (inverse = transpose, very cheap)
theta = np.pi / 4  # 45 degree rotation
R = np.array([[np.cos(theta), -np.sin(theta)],
              [np.sin(theta),  np.cos(theta)]])
print("R @ R^T =\n", np.round(R @ R.T, 3))   # identity
```

> **Orthogonal matrices** have \(A^{-1} = A^\top\), so inverting them is just a transpose. Computationally, this is essentially free.

<div class="divider"></div>

## 8. Eigendecomposition

An eigenvector of a matrix \(A\) is a direction \(\mathbf{v}\) that, when \(A\) is applied to it, only stretches rather than rotating. The amount of stretching is the **eigenvalue** \(\lambda\):

\[A\mathbf{v} = \lambda\mathbf{v}\]

Breaking a matrix into its eigenvectors and eigenvalues reveals the natural structure of the data: the directions of greatest variance, the fundamental modes, the axes of a covariance ellipse.

![Eigendecomposition visualised on a 2D covariance matrix](/assets/images/linear-algebra/07_eigendecomposition.png)

```python
import numpy as np

cov = np.array([[3.0, 1.8],
                [1.8, 1.5]])

# Use eigh for symmetric matrices - more numerically stable
eigenvalues, eigenvectors = np.linalg.eigh(cov)

# Sort by descending eigenvalue
idx = np.argsort(eigenvalues)[::-1]
eigenvalues  = eigenvalues[idx]
eigenvectors = eigenvectors[:, idx]

print("Eigenvalues:", np.round(eigenvalues, 3))
print("Eigenvectors (columns):\n", np.round(eigenvectors, 3))

# Variance explained
variance_explained = eigenvalues / eigenvalues.sum() * 100
for i, (ev, ve) in enumerate(zip(eigenvalues, variance_explained)):
    print(f"PC{i+1}: eigenvalue={ev:.3f}, explains {ve:.1f}% of variance")
```

> Normal modes of vibration in a molecule are eigenvectors of the force constant (Hessian) matrix. Each mode oscillates independently at a characteristic frequency given by its eigenvalue. PCA in genomics, metabolomics, and ecology works the same way.

<div class="divider"></div>

## 9. Singular Value Decomposition (SVD)

SVD is a more general factorisation that works on *any* matrix, not just square ones. It writes \(A\) as a product of three matrices:

\[A = UDV^\top\]

where \(U\) and \(V\) are orthogonal and \(D\) is diagonal. The diagonal entries of \(D\) are the **singular values**, ranked by importance. Discarding small ones removes noise while keeping the signal.

![SVD: singular value spectrum and noise filtering](/assets/images/linear-algebra/08_svd.png)

```python
import numpy as np

# Simulate a noisy spectral data matrix
np.random.seed(0)
t = np.linspace(0, 4*np.pi, 80)
signal = np.outer(np.sin(t), np.cos(t*0.5)) + np.outer(np.cos(t*0.3), np.sin(t))
noisy  = signal + 0.5 * np.random.randn(*signal.shape)

U, s, Vt = np.linalg.svd(noisy)
print(f"Singular values (first 8): {np.round(s[:8], 2)}")

# Low-rank reconstruction: keep only the top k components
def reconstruct(U, s, Vt, k):
    return (U[:, :k] * s[:k]) @ Vt[:k, :]

k = 4  # keep top 4 (signal), discard the rest (noise)
denoised = reconstruct(U, s, Vt, k)

error = np.linalg.norm(signal - denoised) / np.linalg.norm(signal)
print(f"Relative error after rank-{k} reconstruction: {error:.3f}")

variance_captured = (s[:k]**2).sum() / (s**2).sum()
print(f"Variance captured: {variance_captured*100:.1f}%")
```

| Application | How SVD is used |
|-------------|-----------------|
| NMR / cryo-EM | Discard small singular values to filter noise |
| PCA | Right singular vectors of centred data = eigenvectors of covariance matrix |
| Data compression | Store only top-k singular values and vectors |
| Latent Semantic Analysis | Find hidden topics in document-term matrices |

<div class="divider"></div>

## 10. The Pseudoinverse

Real systems are rarely perfectly square. Either there are more measurements than unknowns (overdetermined - no exact solution) or more unknowns than measurements (underdetermined - infinitely many solutions). Standard inversion cannot handle either case.

The **Moore-Penrose pseudoinverse** \(A^+\) gives the best available answer: the least-squares solution for overdetermined systems, and the minimum-norm solution for underdetermined ones.

![Least-squares fit via pseudoinverse](/assets/images/linear-algebra/09_pseudoinverse.png)

```python
import numpy as np

# Overdetermined: 30 measurements, 2 unknowns (linear calibration)
np.random.seed(3)
x_data = np.linspace(0, 10, 30)
y_data = 2.1*x_data + 1.5 + np.random.randn(30)*2

# Build the design matrix
A = np.column_stack([x_data, np.ones_like(x_data)])

# Least-squares solution (equivalent to pseudoinverse)
slope, intercept = np.linalg.lstsq(A, y_data, rcond=None)[0]
print(f"Fitted: y = {slope:.3f}x + {intercept:.3f}")
print(f"True:   y = 2.100x + 1.500")

# Explicit pseudoinverse
A_plus = np.linalg.pinv(A)
params = A_plus @ y_data
print(f"Via pinv: slope={params[0]:.3f}, intercept={params[1]:.3f}")
```

> This is what linear regression computes. `numpy.linalg.lstsq` and `scipy.stats.linregress` both use the pseudoinverse internally. The same applies to polynomial fitting, multiple regression, and regularised models like Ridge and LASSO.

<div class="divider"></div>

## 11. Trace and Determinant

Two scalar quantities that summarise properties of a square matrix.

```python
import numpy as np

A = np.array([[4.0, 2.0],
              [1.0, 3.0]])

# Trace: sum of diagonal entries = sum of eigenvalues
tr = np.trace(A)
print(f"Trace: {tr}")  # 7.0

# Determinant: product of eigenvalues
# |det| measures how much the matrix expands or contracts volume
# det = 0 means the matrix is singular
det = np.linalg.det(A)
print(f"Determinant: {det:.2f}")  # 10.0

# Confirm: det = product of eigenvalues
evals = np.linalg.eigvals(A)
print(f"Product of eigenvalues: {np.prod(evals):.2f}")  # matches det

# Frobenius norm via trace
frob = np.sqrt(np.trace(A.T @ A))
print(f"Frobenius norm: {frob:.3f}")
```

> If the determinant of your covariance matrix is 0 (or very close to 0), your variables are perfectly collinear and you have redundant measurements. This is a signal that dimensionality reduction - PCA, SVD - is needed before fitting models.

<div class="divider"></div>

## 12. Principal Components Analysis (PCA)

PCA is where everything above comes together. It applies eigenvectors (to find directions of variation), orthogonality (to keep axes uncorrelated), and norms (to minimise reconstruction error) to a practical problem: given a high-dimensional dataset, find the lower-dimensional representation that loses the least information.

The derivation shows that the optimal encoding directions are the eigenvectors of \(X^\top X\) corresponding to the largest eigenvalues.

![PCA in action on a 2D correlated dataset](/assets/images/linear-algebra/10_pca.png)

```python
import numpy as np

# Simulate 2D correlated data (e.g. two correlated gene expression measurements)
np.random.seed(42)
cov = np.array([[4.0, 2.8],
                [2.8, 2.5]])
data = np.random.multivariate_normal([5, 3], cov, 200)

# Step 1: centre the data
mean = data.mean(axis=0)
X    = data - mean

# Step 2: compute the covariance matrix
C = X.T @ X / (len(X) - 1)

# Step 3: eigendecomposition
eigenvalues, eigenvectors = np.linalg.eigh(C)
idx          = np.argsort(eigenvalues)[::-1]
eigenvalues  = eigenvalues[idx]
eigenvectors = eigenvectors[:, idx]

# Variance explained
var_exp = eigenvalues / eigenvalues.sum() * 100
for i, ve in enumerate(var_exp):
    print(f"PC{i+1}: {ve:.1f}% of variance")

# Step 4: project data onto principal components
scores = X @ eigenvectors
print(f"Scores shape: {scores.shape}")

# Reconstruction from 1 PC
reconstructed = scores[:, :1] @ eigenvectors[:, :1].T + mean
error = np.sqrt(np.mean((data - reconstructed)**2))
print(f"Reconstruction RMSD (1 PC): {error:.3f}")

# In practice, use scikit-learn:
# from sklearn.decomposition import PCA
# pca = PCA(n_components=2).fit_transform(data)
```

| Field | Typical use |
|-------|-------------|
| Genomics / Transcriptomics | Reduce thousands of genes to 2-3 PCs; visualise sample clustering |
| Metabolomics | Remove batch effects; find co-variation groups |
| Ecology | Ordination |
| Analytical chemistry | QC; spot outlier samples |
| Structural biology | Conformational dynamics from MD simulations |

<div class="divider"></div>

## Quick reference

| Concept | What it does | Scientific relevance |
|---------|-------------|----------------------|
| Vector / Matrix | Organise data | Sample x feature tables |
| Transpose | Flip rows and columns | Common in covariance calculations |
| Matrix multiply | Transform / combine data | Linear models, coordinate changes |
| Identity matrix | The "do nothing" matrix | Baseline for defining inverses |
| Inverse \(A^{-1}\) | Undo a transformation; solve \(\mathbf{Ax=b}\) | Spectral deconvolution, calibration |
| Linear dependence | Redundancy among variables | Collinearity, rank, solvability |
| Norms | Measure vector size | RMSD, LASSO, Ridge |
| Eigenvectors / values | Natural axes and magnitudes | PCA, normal modes, covariance structure |
| SVD \(UDV^\top\) | Universal factorisation | Noise filtering, NMR/cryo-EM, compression |
| Pseudoinverse \(A^+\) | Best approximate solution | Least-squares fitting, linear regression |
| Determinant | Volume scaling factor | Detect collinearity; check invertibility |
| Trace | Sum of diagonal \(= \sum \lambda_i\) | Frobenius norm, matrix invariants |

*Based on Chapter 2 of [Deep Learning](https://www.deeplearningbook.org/) by Goodfellow, Bengio and Courville (MIT Press, 2016).*
