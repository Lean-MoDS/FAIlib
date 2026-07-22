<h1 align="center">FAIlib</h1>

<h4 align="center">Formal foundations for artificial intelligence in Lean 4</h4>

<p align="center">
  <a href="https://github.com/leanprover/lean4/releases/tag/v4.32.0"><img src="https://img.shields.io/badge/Lean-v4.32.0-blue?style=for-the-badge" alt="Lean v4.32.0"></a>
  <a href="./LICENSE"><img src="https://img.shields.io/badge/License-Apache%202.0-lightgrey?style=for-the-badge" alt="Apache 2.0"></a>
  <a href="https://lean-mods.github.io/FAIlib/"><img src="https://img.shields.io/badge/Website-FAIlib-255b94?style=for-the-badge" alt="FAIlib website"></a>
</p>

FAIlib is a standalone Lean 4 library of reusable formal foundations for artificial intelligence
and learning theory. Its current modules cover probability, high-dimensional statistics, empirical
processes, and the analytic and algebraic infrastructure these subjects require. It is built on
Mathlib and has no dependency on another project-local library.

The public source contains 89 modules under the independent `FAIlib.*` namespace and no `sorry`,
`axiom`, `admit`, or `native_decide`.

## Scope

| Layer | Module root | Contents |
| --- | --- | --- |
| Measure theory | `FAIlib.MeasureTheory.*` | Integral, convergence, and L1 infrastructure |
| Topology | `FAIlib.Topology.*` | Covering and packing numbers, separable suprema |
| Analysis | `FAIlib.Analysis.*` | Metric entropy, chaining, normed-space covering estimates |
| Linear algebra | `FAIlib.LinearAlgebra.*` | Singular values, variational principles, matrix perturbation |
| Probability | `FAIlib.Probability.*` | Concentration, entropy methods, Gaussian analysis, random matrices |
| Learning theory | `FAIlib.LearningTheory.*` | Empirical metrics, Rademacher complexity, uniform deviation |
| Statistics | `FAIlib.Statistics.*` | Localized least squares, regression, and minimax guarantees |

The dependency order is foundational measure theory, topology, and linear algebra; then analysis;
probability; learning theory; and statistics. See [ARCHITECTURE.md](./ARCHITECTURE.md) for the
ownership policy and [FILE_TREE.md](./FILE_TREE.md) for the complete module index.

## Selected results

- Dudley's entropy integral and truncated Dudley bounds for sub-Gaussian processes
- Efron–Stein, Hoeffding, McDiarmid, Gaussian Poincare, and Gaussian log-Sobolev inequalities
- Gaussian Lipschitz concentration, Hanson–Wright, and matrix Bernstein inequalities
- Singular-value decomposition, Courant–Fischer, Eckart–Young–Mirsky, Weyl, and Davis–Kahan
  perturbation results
- Symmetrization, Massart's lemma, Rademacher complexity, and uniform-deviation bounds
- Localized least-squares theory for linear and L1-constrained regression

Representative declarations include `dudley`, `truncated_dudley_entropy_bound`, `efronStein`,
`gaussian_lipschitz_concentration`, `hanson_wright_inequality`,
`RMT.matrix_bernstein_inequality_hdp_all`, `expectation_le_rademacher`, and
`master_error_bound`.

## Getting started

FAIlib is pinned to Lean and Mathlib `v4.32.0`.

```bash
# Optional: download the Mathlib build cache.
lake exe cache get

# Build every FAIlib module.
LEAN_NUM_THREADS=$(nproc) lake build

# Build an individual module.
LEAN_NUM_THREADS=$(nproc) lake build FAIlib.Probability.Process.Dudley
```

To use the `v4.32.0` release from another Lake project:

```lean
require «FAIlib» from git
  "https://github.com/Lean-MoDS/FAIlib.git" @ "v4.32.0"
```

Then import only the modules needed by the project:

```lean
import FAIlib.Probability.Concentration.HansonWright
import FAIlib.LearningTheory.UniformDeviation.Bounds
```

## Contributing

Read [CONTRIBUTING.md](./CONTRIBUTING.md) and the
[Code of Conduct](./CODE_OF_CONDUCT.md) before opening a change. New code must preserve the subject
ownership rules, source attribution, complete-proof policy, and warning-free build.

## Authors, copyright, and provenance

Files retain their original copyright and author headers. See [AUTHORS.md](./AUTHORS.md) for the
organizer and contributor lists and the repository's copyright, authorship, and co-authorship
policy; the individual source headers remain authoritative for file-level attribution.

## License

FAIlib is released under the [Apache License 2.0](./LICENSE). Copyright remains with the individual
holders identified in the source files.
