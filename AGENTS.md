# FAIlib: Lean Formalization for Statistical Learning Theory

This file provides guidance to you when working with code in this repository.

A Lean 4 formalization of statistical learning theory built on Mathlib. The unified `FAIlib`
library combines the former SLT and FoML developments and is fully proved (zero `sorry`, `axiom`,
`admit`, and `native_decide`); any new code must compile cleanly (no warning or info messages).

## Build & Check

Pinned to Lean `v4.32.0` and Mathlib `v4.32.0` (`lean-toolchain` / `lakefile.lean`).

Build in parallel — Lake has **no `-j`/`--jobs` flag**; concurrency is the `LEAN_NUM_THREADS` env var (default: *all* hardware threads, which oversubscribes a CPU-capped node). Size it to the cores actually available with `nproc` (it respects this node's allotment — the machine may report more). Prefix each build, since a fresh shell won't inherit it:

```bash
nproc                                # print cores available to this process (e.g. 32; machine total may be higher)
LEAN_NUM_THREADS=$(nproc) lake build                              # whole FAIlib library
LEAN_NUM_THREADS=$(nproc) lake build FAIlib.Analysis.MetricEntropy.Basic
LEAN_NUM_THREADS=$(nproc) lake build FAIlib.Probability.Concentration.LogSobolev.TwoPoint
```

There is no test suite — a clean `lake build` is the verification. There is no `FAIlib.lean` root
file; `lakefile.lean` globs every submodule under `FAIlib/`.

## Architecture

`FAIlib/` has seven Mathlib-style layer-one directories and no root-level Lean modules:

1. `MeasureTheory`, `Topology`, and `LinearAlgebra` are foundational subject layers.
2. `Analysis` may depend on those foundational layers.
3. `Probability` may depend on the foundational and analysis layers.
4. `LearningTheory` may depend on probability and all earlier layers.
5. `Statistics` is the application layer and may depend on every earlier layer.

An import may stay within its layer or point to an earlier layer; it must never point upward. Put
new material under the mathematical subject that owns its definitions, not under a proof technique,
source repository, or workflow stage. See `ARCHITECTURE.md` for the detailed ownership policy.

## Conventions

- **No `sorry`/`axiom`/`admit`/`native_decide`.** The project is complete; new lemmas must be fully proved.
- **No warning or info messages** Do not add `_` to unused parameter to hide the warning message. You must remove completely.
- **File headers** follow Mathlib. New FAIlib-origin files use this Apache copyright block;
  migrated files retain their upstream attribution:
  ```
  /-
  Copyright (c) 2026 Yuanhe Zhang. All rights reserved.
  Released under Apache 2.0 license as described in the file LICENSE.
  Authors: Yuanhe Zhang, Jason D. Lee, Fanghui Liu
  -/
  ```
  FoML-origin files credit Kei Tsukamoto, Kazumi Kasaura, Naoto Onda, Yuma Mizuno, and Sho Sonoda.
  The header is followed by imports (`FAIlib.*` before `Mathlib.*`) and a `/-! # Title … -/`
  module docstring with `## Main definitions` / `## Main results`.
- **Naming** follows Mathlib: lowerCamelCase for defs, snake_case for theorems.

## New Development

When building new formalizations on top of `FAIlib`, the default is **reuse + merge**, not "new
file from scratch." Ground yourself over `FAIlib/` and `.lake/packages/mathlib/Mathlib`.

**1. Ground & reuse — never reprove or redefine what already exists.**
- Ground `FAIlib/` *first* for your subject, then Mathlib.
- Find what's already proved about a declaration you'll cite.
- Build on FAIlib's own infrastructure and foundational roots.

**2. Merge by default — a new file is a last resort.**
- Find the *owning* file (where the subject is defined, not just used).
- **Merge** a lemma into that file, next to the related lemmas. File size alone is *not* a reason to
  split, but mixed mathematical ownership is: split a file when its parts belong to different
  layers. A shared new definition goes in the qualified cluster's `Defs.lean` or `Basic.lean`, never
  in an unqualified root file.
- **New file** only when: it's a distinct pipeline stage importing exactly its predecessor
  (cf. `Statistics/Regression/LeastSquares/Linear/*`); a self-contained reusable theorem; or
  merging would force an upward or
  cross-cluster import that breaks the acyclic import DAG. A new subdirectory only for an
  anticipated 3+ file program. A new file must replicate the Apache header + module
  docstring, order imports (`FAIlib.*` before `Mathlib.*`), use one theme-named `namespace`,
  and import strictly at-or-below its tier (no upward edge).

**3. Verify** with `LEAN_NUM_THREADS=$(nproc) lake build FAIlib.<Module>` before moving on.
