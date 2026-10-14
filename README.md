<!-- solve-problem:cb7ba67c-81c8-4bd0-8669-e54013b49253 -->
# Polynomial-bit separation for sums of square roots

A [Tangle Solve problem](https://solve.tangle.tools/problems/cb7ba67c-81c8-4bd0-8669-e54013b49253).

## What is the problem?

Given integers \(a_1, \dots, a_k\) with \(|a_i| \le N\), consider the signed sum
\[
S = \sum_{i=1}^k \sigma_i \sqrt{a_i}, \quad \sigma_i \in \{-1, +1\}
\]
(and note \(\sqrt{0} = 0\) contributes nothing, so assume \(a_i \ge 1\)). It is classical that if \(S \ne 0\), then \(S\) cannot be arbitrarily small — the square roots of distinct squarefree integers are linearly independent over \(\mathbb{Q}\), so a nonzero sum is irrational and hence nonzero as a real number. The question is *quantitative*: prove a lower bound
\[
|S| \ge 2^{-\mathrm{poly}(k, \log N)}
\]
for every such nonzero \(S\), where \(\mathrm{poly}\) is some fixed polynomial (e.g., \( (k \log N)^{O(1)} \)).

The constraint is that the bound must depend polynomially on both \(k\) and \(\log N\) (i.e., on the *bit length* of the input). Note that with all \(a_i \le N\), there are at most \(\sqrt{N}\) distinct squarefree parts, so \(k\) values of \(\sqrt{a_i}\) span a space of dimension at most \(\min(k, \sqrt{N})\); the interesting regime is \(k \le N^{1/2}\), since trivial arguments (see Warnings) handle \(k\) close to \(\sqrt{N}\).

## Why does it matter?

Deciding \(\sum \sigma_i\sqrt{a_i} \overset{?}{>} 0\) is the bottleneck primitive in exact geometric computing: comparing sums of distances arises in Euclidean shortest paths, Delaunay/Voronoi predicates, minimum-weight triangulation, and exact arithmetic on segment lengths. Currently, exact comparison is done symbolically (algebraic-number arithmetic) or via separation bounds that may need \(2^{\Theta(k)}\) or worse bits of precision. A polynomial-bit lower bound would show that \(O(\mathrm{poly})\)-precision floating point suffices, collapsing symbolic comparison to numerically easy computation.

## What's already known?

- **General radical separation bounds.** Burnikel–Fleischer–Mehlhorn–Schirra (2000) and earlier work (Mignotte, Sarma–Zippel) give bounds of the form \(|S| \ge 2^{-O(k \log N)}\) in some regimes but the general bounds degrade — worst-case bounds known from degree/height arguments on the product polynomial (each \(\sqrt{a_i}\) is a root of \(x^2 - a_i\); the product over sign patterns has degree \(2^k\)) give roughly \(2^{-O(2^k \log N)}\), i.e., exponential in \(k\) only, not polynomial. The gap between \(2^{-O(2^k)}\) and the conjectured \(2^{-\mathrm{poly}(k)}\) is the whole problem.
- **Lattice-based attacks.** Cheng–Meng–Sun–Chen (arXiv:0905.4487) apply LLL-style lattice reduction to bound sums of square roots, relating the problem to how well \((\sqrt{a_1},\dots,\sqrt{a_k})\) can be approximated by integer relations. They achieve improvements in special cases but not the general polynomial bound.
- **Diophantine approximation angle.** The problem is equivalent to a statement about simultaneous approximation of square roots — related to (but weaker than) open questions about Littlewood-type and subspace-type bounds. Known results (e.g., from the Subspace Theorem) are non-effective or give bounds exponential in \(k\).
- **Counting/heuristic evidence.** There are roughly \(2^k\) sign patterns and sums lie in an interval of size \(k\sqrt{N}\); a random-model argument suggests typical minimum gap \(\approx k\sqrt{N}/2^k\), i.e., \(2^{-O(k)}\) — polynomial in the conjectured sense. No adversarial construction beating this heuristic is known; the conjecture is that nothing beats it by more than polynomial factors.

## What would count as solving it?

1. **Main goal (lower bound):** a proof that every nonzero signed sum satisfies \(|S| \ge 2^{-(k\log N)^{C}}\) for an explicit absolute constant \(C\), with the proof effective (an algorithm to compute \(S\) to that precision).
2. **Complement (upper bound):** a construction of \(a_1,\dots,a_k \le N\) and signs with \(|S| \le 2^{-c\,k}\) (for infinitely many \(k, N\)), showing the exponent must be at least linear in \(k\). Partial versions of this may be known; verify.
3. **Weaker but publishable milestones:** a bound \(2^{-\mathrm{poly}(k)}\) independent of \(N\) being super-logarithmic; or a polynomial bound restricted to structured inputs (e.g., all \(a_i\) squarefree and coprime, or \(k \le (\log N)^{C}\)).
4. A refutation — an explicit family with \(|S| \le 2^{-2^{\Omega(k)}}\) — would also resolve the problem (negatively) and be highly significant.

## How to start

1. Read Cheng–Meng–Sun–Chen (arXiv:0905.4487) first: it is short, self-contained, and frames the problem via lattice reduction; understand exactly where their approach loses the polynomial bound (approximation-factor blowup of LLL, dimension dependence).
2. Read Burnikel et al. (2000) for the exact statement of the classical separation bound \(2^{-O(2^k \log N)}\) obtained by the product-polynomial/root-counting argument — this is the baseline you must beat.
3. Formulate the problem as one of Diophantine approximation: \(|S|\) small means \(\sum \sigma_i\lfloor\sqrt{a_i} \cdot 2^t\rfloor\) nearly cancels; small \(|S|\) forces near-integer relations among scaled irrational parts. Try subspace-theorem-flavored or \(p\)-adic (valuation-theoretic) arguments: expand products and use linear independence over \(\mathbb{Q}\) of squarefree radicals with norm/trace arguments to extract lower bounds.
4. Main obstacle: the algebraic degree is \(2^k\), and all standard algebraic lower bounds (resultants, norm bounds) carry the degree in the exponent. Any successful approach must exploit the *structure* (nested square roots of small integers, tensor-product Galois group \((\mathbb{Z}/2)^m\)) rather than generic algebraic-number tools.

## Key references

- C. Burnikel, R. Fleischer, K. Mehlhorn, S. Schirra, "A Strong and Easily Computable Separation Bound for Arithmetic Expressions Involving Radicals," *Algorithmica* 27 (2000), 87–99. Establishes the standard (degree-based) separation bounds for expressions with square roots — the baseline general bound this problem aims to improve. (Searchable by title; no stable link recorded.)
- J. Cheng, J. Meng, X. Sun, L. Chen, "Bounding Sum of Square Roots via Lattice Reduction," arXiv:0905.4487 (2009), https://arxiv.org/abs/0905.4487. Attacks exactly this separation question using lattice basis reduction and identifies where current techniques fall short of polynomial bits.
- (verify) Related background on exact geometric predicates: the LEDA/CGAL exact-real arithmetic literature; also classical work by Mignotte on separation of algebraic numbers — useful context but not directly cited in the census record.

## Warnings

- **Trivial regime:** when \(k > \sqrt{N}\), the \(a_i\) must repeat squarefree parts, and grouping terms gives an integer sum of \(m \le \sqrt{N}\) radicals; the problem is only interesting for \(k \le N^{1/2}\). Don't "solve" it in the large-\(k\) regime by accident.
- **Zero sums exist:** e.g., \(\sqrt{4} = 2\), or \(\sqrt{2}+\sqrt{8}-3\sqrt{2}=0\) after simplification. The bound applies only to nonzero sums; define "nonzero" after reducing to distinct squarefree parts.
- The heuristic pigeonhole/counting argument suggests a lower bound near \(2^{-O(k)}\) but proves nothing adversarially — many have tried it; don't present it as a proof.
- Distinguish clearly between bounds of the form \(2^{-\mathrm{poly}(k)\cdot \log N}\) (wanted) and \(2^{-2^k \cdot \mathrm{poly}(\log N)}\) (known); papers in this area state bounds in inconsistent normalizations (bit complexity vs. value), so normalize before comparing.
- Beware of conflating this with the *computational* question "given the sum, decide its sign" — the separation bound is exactly what makes that decidable in polynomial precision; they are equivalent, but state results in the bound form.

---
*`ALGO-000157` · unverified*

## Acceptance guidance

1. **Main goal (lower bound):** a proof that every nonzero signed sum satisfies \(|S| \ge 2^{-(k\log N)^{C}}\) for an explicit absolute constant \(C\), with the proof effective (an algorithm to compute \(S\) to that precision).
2. **Complement (upper bound):** a construction of \(a_1,\dots,a_k \le N\) and signs with \(|S| \le 2^{-c\,k}\) (for infinitely many \(k, N\)), showing the exponent must be at least linear in \(k\). Partial versions of this may be known; verify.
3. **Weaker but publishable milestones:** a bound \(2^{-\mathrm{poly}(k)}\) independent of \(N\) being super-logarithmic; or a polynomial bound restricted to structured inputs (e.g., all \(a_i\) squarefree and coprime, or \(k \le (\log N)^{C}\)).
4. A refutation — an explicit family with \(|S| \le 2^{-2^{\Omega(k)}}\) — would also resolve the problem (negatively) and be highly significant.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/cb7ba67c-81c8-4bd0-8669-e54013b49253?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000157.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000157)
