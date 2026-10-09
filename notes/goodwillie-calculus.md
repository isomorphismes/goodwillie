# Goodwillie calculus: first reading notes

Reading date: 9 October 2026. This is a selective first pass through definitions,
theorem statements, examples, and bibliography, not a claim to have checked every
proof in the papers.

## What is being approximated?

Start with a construction that accepts a space and produces another space or a
spectrum. We want to understand how its output changes when the input is assembled
from pieces. One model of a spectrum uses pointed spaces whose successive loop
spaces recover the preceding spaces; this packages stable homotopy information.
A useful approximation should preserve that dependence on the input:
it must approximate the whole construction, including its maps.

A **homotopy functor** respects weak equivalences. Goodwillie's approximation
problem asks how much of such a functor can be recovered from constructions whose
response to assembling pieces has a bounded degree. This degree is expressed by
excision, rather than by the degree of a coordinate formula. [G1, §1; G2, §3]

## 1. Cubes say exactly which assembly is under discussion

An n-cube assigns an object to each subset of an n-element set, with compatible
maps for inclusions. A square has four objects; a 3-cube has eight.

A cube is **cartesian** when its initial object maps equivalently to the homotopy
limit of the other objects. It is **strongly cocartesian** when every square face
is a homotopy pushout. The word “strongly” matters once the cube has more than two
dimensions. [G2, §2, definition preceding Proposition 2.2]

A functor is **n-excisive** when it sends strongly cocartesian (n+1)-cubes to
cartesian cubes. Thus:

| Degree | Required behavior |
|---|---|
| 0 | Every map becomes an equivalence. |
| 1 | A homotopy pushout square becomes a homotopy pullback square. |
| 2 | Every strongly cocartesian 3-cube becomes a cartesian 3-cube. |

Lower degree implies higher-degree excision, just as a linear polynomial also has
degree at most two. [G2, Definition 3.1 and Proposition 3.2, p. 319]

Analyticity allows controlled failure of exact excision. In Goodwillie's
convention, ρ-analyticity supplies a constant q such that, for every n ≥ 1, a
strongly cocartesian (n+1)-cube with initial-edge connectivities kᵢ ≥ ρ+1 becomes
(∑kᵢ − nρ + q)-cartesian after applying F. Being k-cartesian means that the
comparison with the homotopy limit is k-connected. [G2, Definitions 4.1–4.2,
p. 321]

## 2. The first approximation enters stable homotopy theory

**Reduced** means that the one-point input has contractible output. Reduced and
1-excisive functors are called linear. Their behavior on finite spaces is encoded
by a spectrum. A **finitary** functor preserves filtered homotopy colimits;
this hypothesis extends the description to arbitrary spaces.
[G1, Definition 1.1 and Remarks 1.3, pp. 4–6]

For a reduced, stably excisive functor of pointed spaces, linearization can be
obtained by repeatedly suspending the input and looping the output:

$$
P_1F(Y)\simeq\operatorname*{hocolim}_{r\geq0}
\Omega^rF(\Sigma^rY).
$$

Here Σ suspends a pointed space, Ω forms its based loop space, and hocolim is the
homotopy colimit. For the identity functor the result is the stabilized space
Ω∞Σ∞Y. This makes the first approximation geometrically useful: it retains stable
information from the original space. [G1, introduction and §1]

The original application is pseudoisotopy theory. For Waldhausen's A-theory of
spaces, the coefficient spectrum at a chosen point x ∈ X is
∂ₓA(X) ≃ Σ∞(ΩₓX)₊. Here ΩₓX consists of loops based at x, and + adds a disjoint
basepoint. This describes the derivative at (X, x); assembling the whole
differential over X involves the free-loop fibration over X. [G1, Corollary 3.3,
p. 16; statement checked, geometric proof not checked]

## 3. The tower, its layers, and its convergence are distinct pieces of data

For homotopy functors from spaces over a fixed space Y to spaces or spectra,
Goodwillie constructs natural approximations PₙF. The map F → PₙF is universal
among maps to n-excisive functors, in the homotopy category of homotopy functors.
The compatible approximations form the tower

$$
\cdots\longrightarrow P_3F\longrightarrow P_2F
\longrightarrow P_1F\longrightarrow P_0F.
$$

For pointed outputs, its nth layer is DₙF ≝ hofib(PₙF → Pₙ₋₁F). It is
n-homogeneous: n-excisive with contractible (n−1)-excisive approximation.
[G3, Theorems 1.8 and 1.13, Definition 1.15, Proposition 1.17]

Convergence means F(X) ≃ holimₙ PₙF(X). The original Theorem 1.13 assumes
ρ-analyticity and a (ρ+1)-connected structural map X → Y; its proof uses an
integer connectivity k > ρ. Those hypotheses must accompany any convergence
claim. Existence of the tower alone does not establish convergence. [G3, p. 665]

## 4. A derivative remembers permutations of its inputs

The readable overview [AC, §2] gives the following formulation. For a finitary
homotopy functor F from pointed spaces to spectra, the homogeneous layer is

$$
D_nF(X)\simeq
\bigl(\partial_nF\mathbin{\wedge}(\Sigma^\infty X)^{\wedge n}\bigr)_{h\Sigma_n}.
$$

The derivative ∂ₙF is a spectrum with an action of the symmetric group Σₙ. The
symbol ∧ denotes smash product. The subscript hΣₙ denotes homotopy orbits, with
the group acting both on the derivative and by permuting the n inputs. For
space-valued outputs, apply Ω∞ to the expression. For spectrum inputs, omit Σ∞.
Without finitariness, the simple coefficient formula is safely stated on finite
inputs. [AC, Theorem 2.3 and Example 2.4]

This coefficient spectrum and its action replace the scalar coefficient and
factorial appearing in a Taylor monomial. Dropping the action loses mathematical
information. The overview is a useful entrance to the subject; original theorem
statements remain the reference for exact hypotheses. [AC, §§1–2]

## 5. A completely explicit stable example

The following is a worked calculation using the preceding definitions, rather
than a computation quoted from one of the papers.

Let A and B be spectra. In spectra, finite coproducts and products agree; write
their common direct sum as A ⊕ B. Consider the quadratic functor

F(X) ≝ X ∧ X.

Smash product distributes over direct sums, so

$$
F(A\oplus B)\simeq
(A\wedge A)\oplus(A\wedge B)\oplus
(B\wedge A)\oplus(B\wedge B).
$$

The maps which erase B or erase A induce

F(A ⊕ B) → F(A) ⊕ F(B).

Under the displayed decomposition, this is the projection onto the two pure
terms. Its homotopy fibre is therefore

$$
\operatorname{cr}_2F(A,B)\simeq
(A\wedge B)\oplus(B\wedge A).
$$

For a reduced spectrum-valued functor, this fibre is its second **cross-effect**.
It records the part that needs both inputs. Keeping both terms records the two
orders in which the inputs occupy the two smash factors.

This cross-effect is already linear separately in A and B: smashing with a fixed
spectrum preserves colimits, and stable pushout squares are also pullback
squares. The general method is to take a cross-effect and then linearize in every
variable. Goodwillie proves that the result recovers the multilinear functor
associated to the nth layer. [G3, Theorem 6.1, pp. 693–694]

Let 𝕊 denote the sphere spectrum, the unit for smash product. Evaluating the
bilinear expression at A = B = 𝕊 gives

∂₂F ≃ 𝕊 ⊕ 𝕊,

with Σ₂ exchanging the two summands. This is the regular Σ₂ coefficient. On
general diagonal inputs, its action also accounts for interchanging smash
factors. The homogeneous-functor classification gives

D₂F ≃ F,  P₁F ≃ 0,  P₂F ≃ F.

One can check the reconstruction directly: the homotopy orbits of the regular
coefficient, tensored with X ∧ X and its permutation action, recover X ∧ X.

The third cross-effect vanishes. To see the combinatorial reason, expand
(A ⊕ B ⊕ C) ∧ (A ⊕ B ⊕ C). Each summand has two slots, so each uses at most two
of A, B, C. The total fibre defining cr₃ removes all terms that survive when any
one input is erased. No term needs all three inputs, leaving zero. This is an
explicit calculation for this functor; vanishing cross-effects alone should not
be promoted to an unrestricted test for Goodwillie excision in arbitrary
categories.

### What this example lets a picture explain

Give the input labels different colours. Draw ordered pairs in a square grid.
The diagonal blocks contain the pure terms; the two off-diagonal blocks are the
second cross-effect. Erasing either colour removes both mixed blocks. Swapping
the two input slots exchanges them. A three-colour version has no term using all
three colours because a pair has only two positions.

That is a proposed visual explanation of distributivity, cross-effects, and
symmetry. Its exact content can be specified before choosing any rendering or
diagram-rewriting package.

## 6. Relation to ordinary calculus and manifold calculus

For the ordinary function square(t) ≝ t², the mixed difference is

square(a+b) − square(a) − square(b) + square(0) = 2ab.

At a = 2 and b = 3 it is 25 − 4 − 9 = 12. The two ordered mixed smash terms above
provide a useful structural companion to the two products ab and ba. This is an
analogy plus an elementary calculation, not an identification of real
differentiation with homotopy functor calculus.

Manifold calculus changes the input problem. Michael Weiss studies contravariant
functors on open subsets of a fixed manifold, such as U ↦ Emb(U, N). Restricting
an embedding to a smaller open set provides the maps. Polynomial approximation
then organizes information about small configurations of pieces inside a
manifold. Weiss explicitly distinguishes this from being a special case of
Goodwillie's homotopy functor calculus. [W, introduction, pp. 68–69]

For repository exploration, first decide whether the object changing is a
homotopy type, a collection of open pieces of a manifold, or an ordinary numerical
input. Their useful pictures can resemble one another while their mathematical
claims require different definitions.

## Primary sources and reading depth

These links identify sources; no paper or book is copied into this note.

| Key | Source | Pass completed |
|---|---|---|
| G1 | Thomas G. Goodwillie, **Calculus I: The First Derivative of Pseudoisotopy Theory**, K-Theory **4** (1990), 1–27. [DOI](https://doi.org/10.1007/BF00534191); [institutional PDF](https://www.sas.rochester.edu/mth/sites/doug-ravenel/otherpapers/goodwillie-calc1.pdf). | Introduction and selected §1 definitions/constructions read; geometric proof not checked. The printed paper verifies volume 4. |
| G2 | Thomas G. Goodwillie, **Calculus II: Analytic Functors**, K-Theory **5** (1992; volume often cited 1991/92), 295–332. [DOI](https://doi.org/10.1007/BF00535644); [institutional PDF](https://www.sas.rochester.edu/mth/sites/doug-ravenel/otherpapers/goodwillie-calc2.pdf). | Selected cube definitions and §§3–4 statements read; connectivity proofs not checked. |
| G3 | Thomas G. Goodwillie, **Calculus III: Taylor Series**, Geometry & Topology **7** (2003), 645–711. [DOI](https://doi.org/10.2140/gt.2003.7.645); [arXiv record](https://arxiv.org/abs/math/0310481); [PDF](https://arxiv.org/pdf/math/0310481). | Selected statements around Theorems 1.8, 1.13, 3.5, 6.1, explanatory passages, and the complete bibliography on pp. 710–711 read; complete proofs not checked. |
| AC | Gregory Arone and Michael Ching, **Goodwillie Calculus**, arXiv:1902.00803 (2019 preprint). [Record](https://arxiv.org/abs/1902.00803); [PDF](https://arxiv.org/pdf/1902.00803). | Selected §§1–2 statements and manifold-calculus discussion inspected. The complete bibliography has not been audited. |
| W | Michael Weiss, **Embeddings from the point of view of immersion theory: Part I**, Geometry & Topology **3** (1999), 67–101. [DOI](https://doi.org/10.2140/gt.1999.3.67); [record](https://arxiv.org/abs/math/9905202); [PDF](https://arxiv.org/pdf/math/9905202). | Abstract and introductory definitions inspected; later results and the separately listed erratum were not reviewed. |

See [acknowledgments and bibliography coverage](../ACKNOWLEDGMENTS.md).

