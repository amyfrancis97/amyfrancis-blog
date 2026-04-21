---
layout: post
title: "Linear algebra for scientists - a coding-first guide"
description: "A plain-language, code-first walkthrough of the linear algebra behind machine learning. Based on Chapter 2 of the Deep Learning textbook - written for scientists who didn't come from the maths side."
standfirst: "Most machine learning tutorials assume you already know your eigenvectors from your eigenvalues. This one doesn't. Each concept is introduced in plain language, paired with runnable Python code, and grounded in examples from real scientific data."
category: tutorial
category_label: "Tutorials & Guides"
date: 2026-03-20
read_time: 35
---

This is a coding-first walkthrough of the linear algebra that underpins machine learning - based on Chapter 2 of [Deep Learning](https://www.deeplearningbook.org/) by Goodfellow, Bengio and Courville, but written for scientists coming from biology or
chemistry rather than maths or computer science.

Each section covers one concept: what it is (in plain language), why it matters for data science, and how to use it in Python. No assumed background beyond basic scientific data familiarity and some Python coding experience.


<div class="divider"></div>

## Why linear algebra shows up everywhere in machine learning

Most scientific datasets can be written as a matrix: rows are samples, columns are variables. Once your data is in that form, almost everything you do to it-fitting models, reducing dimensionality, filtering noise, finding patterns-can be expressed as matrix operations.

Linear algebra is the language that makes those operations precise and scalable:

- A linear model is just a matrix multiplication  
- Least-squares fitting is solving $Ax = b$  
- PCA is an eigendecomposition of a covariance matrix  
- Noise filtering is truncating an SVD  

Most machine learning algorithms can be viewed as choosing parameters that minimise a loss function — often based on norms of the error, but sometimes a likelihood, cross-entropy, or margin. The linear algebra stays the same regardless.

Gradients are vectors, and optimisation algorithms repeatedly apply matrix operations to update parameters. Understanding the underlying geometry is what makes debugging and adapting these models tractable.

The goal of this guide is not to teach abstract mathematics, but to show how these operations arise naturally when working with real scientific data, and how to use them directly in Python.

Throughout, it helps to hold three views of data in mind simultaneously — as a **table of numbers**, as **vectors pointing in space**, and as **transformations that move things around**. Each section approaches the same ideas from one of these angles. By the end, they should feel like the same thing described three different ways.

The running dataset is the [breast cancer Wisconsin dataset](https://scikit-learn.org/stable/datasets/toy_dataset.html#breast-cancer-dataset) — 569 tumour biopsies, each described by 30 cell nucleus measurements. It's real clinical data with clear structure, and it illustrates every concept here naturally.

**Requirements:** `numpy`, `matplotlib`, `seaborn`, `scikit-learn`

```bash
pip install numpy matplotlib seaborn scikit-learn
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
from sklearn.datasets import load_breast_cancer

# Load the dataset: 569 tumour biopsies, 30 cell nucleus measurements each
bc = load_breast_cancer()
X, y = bc.data, bc.target          # y: 0 = malignant, 1 = benign
feature_names = bc.feature_names

print(f"Data matrix shape: {X.shape}")          # (569, 30)
print(f"Features: {feature_names[:5]} ...")
print(f"Classes: {bc.target_names}")             # ['malignant', 'benign']

# One sample as a vector: 30 measurements from a single biopsy
sample = X[0]
print(f"\nSample 0 (malignant): {np.round(sample[:5], 2)} ...")

# Transpose: (569 x 30) -> (30 x 569)
print(f"Transposed shape: {X.T.shape}")
```

> **Transpose** flips a matrix across its diagonal, so rows become columns. If your data matrix is (samples x variables), its transpose is (variables x samples).

<div class="divider"></div>

## 2. Matrix Multiplication

Matrix multiplication is not element-wise. It combines rows of one matrix with columns of another: each element of the output is a dot product between a row of $A$ and a column of $B$. That mental model is worth internalising — it makes shape mismatches much easier to debug. Geometrically, it *transforms space* — rotating, stretching, or shearing data. This comes up in coordinate changes, mixing variables, and representing linear models.

![Matrix multiplication as a geometric transformation](/assets/images/linear-algebra/02_matrix_multiplication.png)

```python
import numpy as np
from sklearn.datasets import load_breast_cancer
from sklearn.preprocessing import StandardScaler

bc = load_breast_cancer()
X = bc.data[:, :2]   # radius_mean, texture_mean for illustration

# Standardise: subtract mean, divide by std — a matrix operation
scaler = StandardScaler()
X_std = scaler.fit_transform(X)
print(f"Original:     mean={X.mean(axis=0).round(2)}, std={X.std(axis=0).round(2)}")
print(f"Standardised: mean={X_std.mean(axis=0).round(2)}, std={X_std.std(axis=0).round(2)}")

# Matrix multiplication: rotate the standardised data 45 degrees
theta = np.pi / 4
R = np.array([[np.cos(theta), -np.sin(theta)],
              [np.sin(theta),  np.cos(theta)]])
X_rotated = X_std @ R.T
print(f"Rotated shape: {X_rotated.shape}")
```

> Matrix multiplication is *not* commutative. $AB \neq BA$ in general, just as rotate-then-translate and translate-then-rotate give different results.

<div class="divider"></div>

## 3. Solving Systems of Equations

A system of simultaneous equations like:

$$2x + y = 8$$
$$x - y = 1$$

can be written as $\mathbf{Ax = b}$, where $\mathbf{A}$ encodes the coefficients, $\mathbf{x}$ the unknowns, and $\mathbf{b}$ the outcomes. Writing it this way scales to thousands of equations with no change in notation. Relevant in practice for spectral deconvolution, calibration, and model fitting.

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

The **identity matrix** $I$ is the matrix equivalent of the number 1 - multiplying anything by it leaves it unchanged. It has 1s along the main diagonal and 0s everywhere else.

The **inverse** of a matrix $A$, written $A^{-1}$, is the matrix that undoes $A$'s transformation. Together they satisfy $A^{-1}A = I$. This is how we analytically solve $\mathbf{Ax = b}$: multiply both sides by $A^{-1}$ to get $\mathbf{x} = A^{-1}\mathbf{b}$.

Not every matrix has an inverse. If the columns of $A$ are linearly dependent, the determinant is zero and no inverse exists - such a matrix is called **singular**.

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
  <p>In practice, computing $A^{-1}$ explicitly and then multiplying is often numerically less stable than using <code>np.linalg.solve(A, b)</code> directly. The solve function uses more robust algorithms internally. Reserve explicit inversion for cases where you genuinely need the inverse matrix itself.</p>
</div>

<div class="divider"></div>

## 5. Linear Dependence and Span

The **span** of a set of vectors is the set of all points you can reach by scaling and adding those vectors together. Think of it as the "reachable space" given those directions.

Vectors are **linearly independent** if none of them can be written as a combination of the others. If one can, the set is **linearly dependent** and the redundant vector adds no new reachable space.

This matters for $\mathbf{Ax = b}$: a solution exists only if $\mathbf{b}$ lies within the column space (span of the columns of $A$), and a unique solution only exists if the columns are all linearly independent.

![Linear dependence and span illustrated](/assets/images/linear-algebra/05_linear_dependence.png)

```python
import numpy as np
from sklearn.datasets import load_breast_cancer

bc = load_breast_cancer()
X = bc.data

# radius_mean and perimeter_mean are nearly perfectly correlated
r = np.corrcoef(X[:, 0], X[:, 2])[0, 1]  # features 0 and 2
print(f"Correlation (radius vs perimeter): {r:.4f}")  # ~0.998

# Check rank: with 30 features, rank should be 30 if all independent
print(f"Data matrix rank: {np.linalg.matrix_rank(X)}")

# A matrix built from two nearly identical columns has rank 1
A_dep = np.column_stack([X[:, 0], X[:, 2]])   # radius ~ perimeter
print(f"Rank of [radius, perimeter]: {np.linalg.matrix_rank(A_dep)}")  # 2, but barely
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
| $L^2$ | $\sqrt{\sum x_i^2}$ | All residuals small | RMSD, Ridge regression |
| $L^1$ | $\sum \|x_i\|$ | Many exact zeros | LASSO, sparse models |
| $L^\infty$ | $\max \|x_i\|$ | Worst-case element | Robust optimisation |

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

> **Orthogonal matrices** have $A^{-1} = A^\top$, so inverting them is just a transpose. Computationally, this is essentially free.

<div class="divider"></div>

## 8. Eigendecomposition

An eigenvector of a matrix $A$ is a direction $\mathbf{v}$ that, when $A$ is applied to it, only stretches rather than rotating. The amount of stretching is the **eigenvalue** $\lambda$:

$$A\mathbf{v} = \lambda\mathbf{v}$$

Breaking a matrix into its eigenvectors and eigenvalues reveals the natural structure of the data: the directions of greatest variance, the fundamental modes, the axes of a covariance ellipse.

![Eigendecomposition visualised on a 2D covariance matrix](/assets/images/linear-algebra/07_eigendecomposition.png)

```python
import numpy as np
from sklearn.datasets import load_breast_cancer
from sklearn.preprocessing import StandardScaler

bc = load_breast_cancer()
X = StandardScaler().fit_transform(bc.data)

# Covariance matrix of the standardised 30-feature dataset
C = X.T @ X / (len(X) - 1)   # (30 x 30)

eigenvalues, eigenvectors = np.linalg.eigh(C)
idx = np.argsort(eigenvalues)[::-1]
eigenvalues  = eigenvalues[idx]
eigenvectors = eigenvectors[:, idx]

print("Top 5 eigenvalues:", np.round(eigenvalues[:5], 3))

variance_explained = eigenvalues / eigenvalues.sum() * 100
cumulative = np.cumsum(variance_explained)
for i in range(5):
    print(f"PC{i+1}: {variance_explained[i]:.1f}% variance  (cumulative: {cumulative[i]:.1f}%)")
```

> Normal modes of vibration in a molecule are eigenvectors of the force constant (Hessian) matrix. Each mode oscillates independently at a characteristic frequency given by its eigenvalue. PCA in genomics, metabolomics, and ecology works the same way.

<div class="divider"></div>

## 9. Singular Value Decomposition (SVD)

SVD is a more general factorisation that works on *any* matrix, not just square ones. It writes $A$ as a product of three matrices:

$$A = UDV^\top$$

where $U$ and $V$ are orthogonal and $D$ is diagonal. The diagonal entries of $D$ are the **singular values**, ranked by importance. Discarding small ones removes noise while keeping the signal.

![SVD: singular value spectrum and noise filtering](/assets/images/linear-algebra/08_svd.png)

```python
import numpy as np
from sklearn.datasets import load_breast_cancer
from sklearn.preprocessing import StandardScaler

bc = load_breast_cancer()
X = StandardScaler().fit_transform(bc.data)   # (569 x 30)

U, s, Vt = np.linalg.svd(X, full_matrices=False)
print(f"Singular values (first 8): {np.round(s[:8], 2)}")
print(f"U: {U.shape}, s: {s.shape}, Vt: {Vt.shape}")

# How many components to capture 90% of variance?
variance_ratio = s**2 / (s**2).sum()
cumulative = np.cumsum(variance_ratio)
k90 = np.searchsorted(cumulative, 0.90) + 1
print(f"\nComponents needed for 90% variance: {k90}")

# Low-rank reconstruction
def reconstruct(U, s, Vt, k):
    return (U[:, :k] * s[:k]) @ Vt[:k, :]

X_5 = reconstruct(U, s, Vt, 5)
error = np.linalg.norm(X - X_5, 'fro') / np.linalg.norm(X, 'fro')
print(f"Relative reconstruction error (k=5): {error:.3f}")
print(f"Variance captured (k=5): {cumulative[4]*100:.1f}%")
```

| Application | How SVD is used |
|-------------|-----------------|
| NMR / cryo-EM | Discard small singular values to filter noise |
| PCA | Right singular vectors of centred data = eigenvectors of covariance matrix |
| Data compression | Store only top-k singular values and vectors |
| Latent Semantic Analysis | Find hidden topics in document-term matrices |

> In practice, PCA is almost always computed via SVD of the centred data matrix rather than eigendecomposition of the covariance matrix — it is more numerically stable and avoids squaring the condition number. `sklearn.decomposition.PCA` does this internally.

<div class="divider"></div>

## 10. The Pseudoinverse

Real systems are rarely perfectly square. Either there are more measurements than unknowns (overdetermined - no exact solution) or more unknowns than measurements (underdetermined - infinitely many solutions). Standard inversion cannot handle either case.

The **Moore-Penrose pseudoinverse** $A^+$ gives the best available answer: the least-squares solution for overdetermined systems, and the minimum-norm solution for underdetermined ones.

![Least-squares fit via pseudoinverse](/assets/images/linear-algebra/09_pseudoinverse.png)

```python
import numpy as np

# Overdetermined: 30 measurements, 2 unknowns (linear calibration)
np.random.seed(3)
x_data = np.linspace(0, 10, 30)
y_data = 2.1*x_data + 1.5 + np.random.randn(30)*2

# Build the design matrix
# The column of ones represents the intercept (bias term) - this generalises to any number of predictors
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

<div class="callout">
  <div class="callout-label">Key intuition: projection</div>
  <p>Geometrically, least-squares fitting is a projection. When you solve $\mathbf{Ax \approx b}$, you're finding the point in the column space of $A$ closest to $\mathbf{b}$ — i.e. projecting $\mathbf{b}$ onto that space. This same idea connects the pseudoinverse, linear regression, and PCA. Everything is a projection of something onto a lower-dimensional subspace.</p>
</div>

```python
import numpy as np

# Project vector v onto vector u
u = np.array([3.0, 1.0])
v = np.array([2.0, 2.5])
proj = (v @ u) / (u @ u) * u
print(f"Projection of v onto u: {np.round(proj, 3)}")
# Residual: component of v perpendicular to u
residual = v - proj
print(f"Residual (perpendicular): {np.round(residual, 3)}")
print(f"Orthogonal check (should be ~0): {np.round(residual @ u, 10)}")
```

<div class="callout">
  <div class="callout-label">Conditioning and numerical stability</div>
  <p>If the singular values of $A$ span many orders of magnitude — say, from $10^6$ down to $10^{-3}$ — the matrix is ill-conditioned. Small measurement noise can then cause large changes in the solution. Check with <code>np.linalg.cond(A)</code>: a condition number above $10^6$ is a warning sign. This is exactly why regularisation methods like Ridge and LASSO exist — they deliberately constrain the solution to avoid instability from small singular values.</p>
</div>

```python
import numpy as np

# Well-conditioned vs ill-conditioned
A_good = np.array([[3., 1.], [1., 3.]])
A_bad  = np.array([[1., 1.], [1., 1.001]])  # nearly singular

print(f"Condition number (good): {np.linalg.cond(A_good):.1f}")
print(f"Condition number (bad):  {np.linalg.cond(A_bad):.2e}")
```

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

# Determinant: product of eigenvalues (for square matrices)
# |det| measures how much the matrix expands or contracts volume
# sign of det encodes orientation: negative = reflection has occurred
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

If you ask "which directions preserve the most variance after projecting the data?", the answer is an eigenvalue problem. The derivation shows that the optimal encoding directions are the eigenvectors of $X^\top X$ corresponding to the largest eigenvalues.

![PCA in action on a 2D correlated dataset](/assets/images/linear-algebra/10_pca.png)

```python
import numpy as np
from sklearn.datasets import load_breast_cancer
from sklearn.preprocessing import StandardScaler

bc = load_breast_cancer()
X_raw = bc.data
y     = bc.target   # 0 = malignant, 1 = benign

# Step 1: standardise (centre and scale — critical when features have different units)
scaler = StandardScaler()
X = scaler.fit_transform(X_raw)

# Step 2: covariance matrix
C = X.T @ X / (len(X) - 1)

# Step 3: eigendecomposition
eigenvalues, eigenvectors = np.linalg.eigh(C)
idx         = np.argsort(eigenvalues)[::-1]
eigenvalues = eigenvalues[idx]
eigenvectors= eigenvectors[:, idx]

# Step 4: project onto principal components
scores = X @ eigenvectors

var_exp = eigenvalues / eigenvalues.sum() * 100
print(f"PC1: {var_exp[0]:.1f}% variance | PC2: {var_exp[1]:.1f}% variance")

# Malignant vs benign separation on PC1
mal_pc1 = scores[y == 0, 0]
ben_pc1 = scores[y == 1, 0]
print(f"PC1 mean (malignant): {mal_pc1.mean():.2f}")
print(f"PC1 mean (benign):    {ben_pc1.mean():.2f}")
print("PC1 alone separates the two classes by {:.2f} standard deviations".format(
    abs(mal_pc1.mean() - ben_pc1.mean()) / scores[:, 0].std()))

# In practice:
# from sklearn.decomposition import PCA
# pca = PCA(n_components=2)
# scores = pca.fit_transform(X)
```

| Field | Typical use |
|-------|-------------|
| Genomics / Transcriptomics | Reduce thousands of genes to 2-3 PCs; visualise sample clustering |
| Metabolomics | Remove batch effects; find co-variation groups |
| Ecology | Ordination |
| Analytical chemistry | QC; spot outlier samples |
| Structural biology | Conformational dynamics from MD simulations |

<div class="divider"></div>

<div class="callout">
  <div class="callout-label">Common mistakes</div>
  <p><strong>Shape confusion:</strong> data matrices are typically (n_samples, n_features) but many operations expect (n_features, n_samples) — always check <code>.shape</code> before multiplying.</p>
  <p><strong>Using * instead of @:</strong> <code>*</code> in NumPy is element-wise; <code>@</code> is matrix multiplication. They give different results and both run without error.</p>
  <p><strong>Skipping centring before PCA:</strong> without centring, the first principal component often just reflects which samples have the highest absolute values, not the structure of variation.</p>
  <p><strong>Trying to invert a singular or near-singular matrix:</strong> if <code>np.linalg.det(A)</code> is near zero or <code>np.linalg.cond(A)</code> is large, use <code>lstsq</code> or <code>pinv</code> instead of <code>inv</code>.</p>
  <p><strong>Eigendecomposition on non-symmetric matrices:</strong> use <code>np.linalg.eigh</code> (not <code>eig</code>) for covariance matrices — it is faster, more stable, and guarantees real-valued eigenvalues.</p>
</div>

<div class="divider"></div>

## Quick reference

| Concept | What it does | When to use it |
|---------|-------------|----------------------|
| Vector / Matrix | Organise data | Any time data is samples x features |
| Transpose | Flip rows and columns | Covariance: $X^\top X$; shape fixes |
| Matrix multiply | Transform / combine data | Linear models, coordinate changes; use @ not * |
| Inverse $A^{-1}$ | Undo a transformation | Only when you need the inverse itself; otherwise use `solve` |
| Linear dependence | Redundancy among variables | If rank < n_features, reduce dimensions before fitting |
| Norms | Measure vector size | L2 for smooth solutions (Ridge); L1 for sparsity (LASSO) |
| Eigenvectors / values | Natural axes and magnitudes | PCA, normal modes; use `eigh` for symmetric matrices |
| SVD $UDV^\top$ | Universal factorisation | Noise filtering, PCA, any rank-deficient system |
| Pseudoinverse $A^+$ | Best approximate solution | Overdetermined or underdetermined systems; regression |
| Determinant | Volume scaling; orientation | Near-zero = collinear variables; negative = reflection |
| Trace | Sum of eigenvalues | Total variance; Frobenius norm |

*Based on Chapter 2 of [Deep Learning](https://www.deeplearningbook.org/) by Goodfellow, Bengio and Courville (MIT Press, 2016).*

<div class="divider"></div>

<div class="callout">
  <div class="callout-label">Up next in this series</div>
  <p><strong>Probability and information theory for scientists — Chapter 3 of the Deep Learning book.</strong> The natural follow-on: once you can represent and transform data with linear algebra, the next question is how to reason about uncertainty in it. Chapter 3 covers probability distributions, expectations, the chain rule, conditional independence, and information-theoretic ideas like entropy and KL divergence — all the foundations that underpin probabilistic models, Bayesian inference, and the loss functions used to train neural networks. Same format as this post: Python throughout, breast cancer dataset as a running example, and the maths introduced only when it earns its place. Subscribe below to be notified when it goes up.</p>
</div>
