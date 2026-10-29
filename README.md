# Sygal Lean Calculus

Clifford-**first** calculus vocabulary in Lean 4 — same fashion as
[`sygal-lean-geo`](https://github.com/Akash-dvd/sygal-lean-geo): thin
bookkeeping + denotations + later certificates; **no CAS in Lean**.

| | |
|--|--|
| **Role** | Formal language for geometric / Clifford calculus (grades, pseudoscalar, vector derivative, analytic identities) |
| **Not** | A computer-algebra kernel; not Mathlib’s complex-analysis stack itself |
| **Discovery** | Python / Sygal (and related notebooks) — formulas locked in a vocab contract before Lean thickens |
| **Status** | Empty scaffold — design only; no Lake package yet |
| **Remote** | [Akash-dvd/sygal-lean-calculus](https://github.com/Akash-dvd/sygal-lean-calculus) |

---

## Contents

1. [Motivation](#motivation)
2. [What Mathlib has (and lacks)](#what-mathlib-has-and-lacks)
3. [Design thesis](#design-thesis)
4. [Proposed layers](#proposed-layers)
5. [Relation to elliptic / genus 1](#relation-to-elliptic--genus-1)
6. [Workflow](#workflow)
7. [Sibling repos](#sibling-repos)

---

## Motivation

Mathlib analysis is strong over **ℂ** and growing for differential forms on
manifolds. It is **not** organized as Hestenes-style geometric calculus:
multivector fields, pseudoscalar as the plane’s `I`, vector derivative, Clifford
analytic functions.

This folder is the intended home for that missing language — parallel to how
`sygal-lean-geo` holds chart + symmetry vocab for Euclidean geometry instead of
dumping System E or raw coordinates into Lean.

---

## What Mathlib has (and lacks)

| Layer | Mathlib today | Style |
|-------|---------------|--------|
| Clifford **algebra** | Yes — `CliffordAlgebra`, exterior algebra; ℂ/ℍ as special cases ([docs](https://leanprover-community.github.io/mathlib4_docs/Mathlib/LinearAlgebra/CliffordAlgebra/Basic.html), [lean-ga](https://github.com/pygae/lean-ga)) | Algebraic |
| Geometric **calculus** (∇, multivector fields, GA identities) | **No** first-class library | — |
| Differential forms | Growing (alternating maps, exterior derivative, manifolds) | Exterior / manifold first |
| Complex analysis | Strong | Analytic / meromorphic over **ℂ** |
| Elliptic / Weierstrass | Partial — e.g. `PeriodPair.weierstrassP` | **ℂ + lattices** |
| Modular forms | Substantial | Upper half-plane / slash actions |

**Clifford algebra ≠ Clifford-first calculus.** Pseudoscalar-as-`I`, “replace ℂ
by a 2D Clifford plane,” GA-analytic functions — those would live here (or
upstream later), not as a Mathlib import today.

---

## Design thesis

### Goal

A **coherent** calculus representation: grades, products, pseudoscalar, and a
small set of derivative / analytic moves — equational, composable, certificate-
friendly.

### Lean (no CAS)

| Need | Form |
|------|------|
| Language | Multivector grades, `I` (pseudoscalar), geometric product / contractions |
| Meaning | Lean defs on top of Mathlib `CliffordAlgebra` (and sandhi where useful) |
| Coherence | Equivalence lemmas (e.g. plane spanned by `{1,I}` ↔ ℂ when bridging) |
| Use | Certificates = finite trees of calculus moves; Lean **checks** |

### Optional: minimize ℂ in the core

Prefer stating 2D identities with a unit pseudoscalar `I` (`I² = −1`) instead of
building the core on `Complex`. Keep an **explicit bridge** to Mathlib ℂ so
Weierstrass / modular forms remain usable later.

### Discipline (same as geo)

1. Python owns **discovery** of formulas.
2. A `VOCABULARY.md` (to be added) is the **contract**.
3. Lean owns **meaning + equivalence lemmas**, not expand/search.
4. Failure mode: pasting coordinate calculus or ad-hoc ℂ lemmas and calling it
   geometric calculus.

---

## Proposed layers

Order of growth (do not jump to elliptic first):

| Layer | Content |
|-------|---------|
| **0 — Algebra** | Re-export / thin wrappers on Mathlib `CliffordAlgebra`; grade projection; reverse; contractions (tie to `sygal-lean-sandhi` where seams matter) |
| **1 — Pseudoscalar** | Oriented plane / volume element `I`; duality `X ↦ X I`; 2D as `span{1,I}` |
| **2 — Fields** | Multivector-valued fields on ℝⁿ (or GA manifolds later) |
| **3 — Vector derivative** | ∇ and basic product rules / identities (vocab-sized, not a CAS) |
| **4 — Analytic** | Clifford-analytic / monogenic-style statements as lemmas when locked in Python |
| **5 — Bridges** | Optional `≃` to exterior forms and to Mathlib `ℂ` |
| **6 — Genus 1** | Elliptic integrals / torus applications *after* 0–4; or consume Mathlib ℘ via the ℂ bridge |

---

## Relation to elliptic / genus 1

Two compatible paths — pick per goal:

| Path | When |
|------|------|
| **Mathlib ℂ** | Need Weierstrass / modular forms quickly — use existing analysis; bridge from Clifford core |
| **GA-native genus 1** | Research target later — elliptic structure stated in Clifford language; almost empty in Lean today |

Do **not** block the Clifford calculus kernel on genus 1.

---

## Workflow

| Step | Where | What |
|------|--------|------|
| 1 | This repo (later) | Thin Lean vocab + denotations |
| 2 | Python / Sygal | Discover identities (CAS / rewrite) |
| 3 | Lean certificates | Formalize selected proofs using vocab lemmas |

Trusted artifact = Lean. Search stays outside.

---

## Sibling repos

| Repo | Role |
|------|------|
| [`sygal-lean-sandhi`](https://github.com/Akash-dvd/sygal-lean-sandhi) | Contraction / seam theorems |
| [`sygal-lean-geo`](https://github.com/Akash-dvd/sygal-lean-geo) | Euclidean chart vocabulary (pattern to copy) |
| [`sygal-lean-problems`](https://github.com/Akash-dvd/sygal-lean-problems) | Geometry certificates |
| [`Sygal`](https://github.com/Akash-dvd/sygal) | Language / operators (Python discovery) |

---

## Next steps (when implementation starts)

1. `lakefile.toml` + depend on Mathlib (and optionally Sandhi).
2. `VOCABULARY.md` — Clifford calculus contract (mirror geo).
3. Layer 0–1 modules only (`Algebra`, `Pseudoscalar`).
4. First certificate: a small identity already verified in Python.
