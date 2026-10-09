# Existing mathematics and implementations worth reusing

Research date: 2026-10-09. This is a first source audit for `isomorphisms/goodwillie`, not a completed formalization or a choice of implementation language.

## Finding

We can reuse substantial mathematics immediately. The closest verified implementation found in this pass is **Cubical Agda's homotopy theory library**: it contains actual homotopy pushouts, cofibers, joins, sequential colimits, the Blakers–Massey theorem, the Freudenthal suspension theorem, prespectra, spectra, and fibers of spectrum maps. **agda-unimath** also provides particularly relevant code for coherent cubes, including rotations, reflections, and pasting.

The Stacks Project and Kerodon serve a different purpose: both are mathematical textbooks with precise linked statements and proofs. Their web interfaces and source repositories do not make their mathematics an executable or proof-assistant-checked library. No complete, reusable Goodwillie Taylor-tower implementation was verified in this pass. That is a bounded finding about the sources inspected, not a claim that nobody has formalized the subject.

“Implemented” below means that the named declaration or proof was read in upstream source. No Agda or Lean compilation, extraction, numerical computation, or graphics integration was run for these notes.

## 1. The Stacks Project: reusable statements and dependency links

The project's [own description](https://stacks.math.columbia.edu/about) calls it an open-source textbook and reference work on algebraic stacks and their foundations. It also explains that tags permanently identify results. Those tags give us useful stable citations:

| Topic | Verified source | What we can reuse |
|---|---|---|
| Ordinary pullbacks | [Tag 001U: Fibre products](https://stacks.math.columbia.edu/tag/001U) | The universal property, cartesian squares, and basic composition/base-change arguments. |
| Ordinary pushouts | [Tag 0025: Pushouts](https://stacks.math.columbia.edu/tag/0025) | The dual universal property and cocartesian-square language. |
| Limits and colimits | [Tag 002D](https://stacks.math.columbia.edu/tag/002D) | Diagram, cone, and universal-property foundations. |
| Derived categories | [Tag 05RR](https://stacks.math.columbia.edu/tag/05RR) | Localizing the homotopy category of complexes in an abelian category at quasi-isomorphisms. |
| Sequential homotopy colimits in a triangulated category | [Tag 0A5K: Derived colimits](https://stacks.math.columbia.edu/tag/0A5K) | The mapping-telescope construction through a distinguished triangle, with its stated hypotheses and nonuniqueness. |
| Simplicial sets | [Tag 0174](https://stacks.math.columbia.edu/tag/0174) | Simplicial-set foundations. |
| Simplicial homotopies | [Tag 019K](https://stacks.math.columbia.edu/tag/019K) | A homotopy represented by a map out of the product with the simplicial interval. |

Two boundaries matter. An ordinary categorical pushout need not model a homotopy pushout. Also, the sequential homotopy-colimit construction in Tag 0A5K is stated inside a triangulated category; it is not a general implementation of homotopy-coherent limits and colimits of spaces. We should keep those distinctions visible when drawing the same square-shaped picture in different settings.

The Stacks bibliography does contain [Thomas G. Goodwillie and Stephen Lichtenbaum's paper on the h-topology](https://stacks.math.columbia.edu/bibliography/ph), used in [Tag 0DBC](https://stacks.math.columbia.edu/tag/0DBC). That occurrence of Goodwillie's name concerns a different piece of mathematics and should not be treated as evidence that the project develops his calculus of functors.

The source project uses the GNU Free Documentation License; see its [contribution guidance](https://stacks.math.columbia.edu/contribute). These notes use links and original summaries.

## 2. Kerodon: a closer reference for the homotopy squares themselves

[Kerodon](https://kerodon.net/about), maintained by Jacob Lurie, is explicitly an online textbook on categorical homotopy theory modeled on the Stacks Project. It is especially useful for the distinction just made:

- [Tag 0106: Homotopy Pullback and Homotopy Pushout Squares](https://kerodon.net/tag/0106).
- [Tag 010K: Homotopy Pullback Squares](https://kerodon.net/tag/010K).
- [Tag 03G3: Examples of Pullback and Pushout Squares](https://kerodon.net/tag/03G3), which connects the homotopy-pullback condition for Kan complexes with a pullback square in the ∞-category of spaces.

These are verified reference results, not imported proof objects. The first practical use is to make our captions, hypotheses, and proposed operations agree with an explicit mathematical statement before attempting a formalization.

## 3. Cubical Agda: the strongest directly relevant formalized starting point

Source snapshot: [`agda/cubical` at c37782bb4e382dc41035e0970409a0c17b55e6ce](https://github.com/agda/cubical/tree/c37782bb4e382dc41035e0970409a0c17b55e6ce).

The inspected [`cubical.agda-lib`](https://github.com/agda/cubical/blob/c37782bb4e382dc41035e0970409a0c17b55e6ce/cubical.agda-lib) enables `--safe --cubical`. The [README](https://github.com/agda/cubical/blob/c37782bb4e382dc41035e0970409a0c17b55e6ce/README.md) gives the library/Agda version compatibility table; use a matched pair if we proceed. The [license](https://github.com/agda/cubical/blob/c37782bb4e382dc41035e0970409a0c17b55e6ce/LICENSE) is MIT with explicitly marked exceptions for individual files.

### Exact reusable pieces

| File and verified declarations | Mathematical content | Relevance to this repository |
|---|---|---|
| [`Cubical/HITs/Pushout/Base.agda`](https://github.com/agda/cubical/blob/c37782bb4e382dc41035e0970409a0c17b55e6ce/Cubical/HITs/Pushout/Base.agda): `Pushout`, `Pushout→`, `cofib`, `symPushout` | Native higher inductive gluing, induced maps between gluing constructions, cofibers, and symmetry. | We can work with a specified gluing path and induced maps instead of pretending a square's layout supplies its mathematics. |
| [`Cubical/HITs/Join/Base.agda`](https://github.com/agda/cubical/blob/c37782bb4e382dc41035e0970409a0c17b55e6ce/Cubical/HITs/Join/Base.agda): `join`, `join∙` | Joins of types and pointed joins. | Relevant ingredients for constructions using joins; not by itself Goodwillie's approximation construction. |
| [`Cubical/HITs/SequentialColimit/Base.agda`](https://github.com/agda/cubical/blob/c37782bb4e382dc41035e0970409a0c17b55e6ce/Cubical/HITs/SequentialColimit/Base.agda): `SeqColim`, `FinSeqColim`, `realiseSequenceMap` | A sequential colimit with paths joining each stage to the next, a finite version, and induced maps. | Appropriate structure for displaying successive stages while remembering how they attach. |
| [`Cubical/Homotopy/BlakersMassey.agda`](https://github.com/agda/cubical/blob/c37782bb4e382dc41035e0970409a0c17b55e6ce/Cubical/Homotopy/BlakersMassey.agda): `BlakersMassey□.toPullback`, `BlakersMassey□.isConnected-toPullback` | A comparison from the original gluing data into a homotopy pullback, and a connectivity theorem for that map under the stated hypotheses. | A substantial theorem about how a homotopy pushout approximates a pullback, directly relevant to excision. |
| [`Cubical/Homotopy/Freudenthal.agda`](https://github.com/agda/cubical/blob/c37782bb4e382dc41035e0970409a0c17b55e6ce/Cubical/Homotopy/Freudenthal.agda): `isConnectedσ`, `FreudenthalEquiv` | Connectivity of the map from a pointed type into the loop space of its suspension, and the corresponding equivalence after truncation. | A concrete, already formalized route into stabilization and approximation. |
| [`Cubical/Homotopy/Prespectrum.agda`](https://github.com/agda/cubical/blob/c37782bb4e382dc41035e0970409a0c17b55e6ce/Cubical/Homotopy/Prespectrum.agda): `GenericPrespectrum`, `Prespectrum`, `SuspensionPrespectrum` | Pointed spaces linked to loop spaces of their successors; a suspension prespectrum. | A verified starting representation for successive stable data. |
| [`Cubical/Homotopy/Spectrum.agda`](https://github.com/agda/cubical/blob/c37782bb4e382dc41035e0970409a0c17b55e6ce/Cubical/Homotopy/Spectrum.agda): `Spectrum`, `isSpectrumMap`, `_→Sp_` | A prespectrum whose structure maps are equivalences, plus maps compatible up to pointed homotopy. | Actual spectra and maps, with explicit compatibility. This file alone does not establish every desired categorical property of spectra. |
| [`Cubical/Homotopy/Spectrum/Fiber.agda`](https://github.com/agda/cubical/blob/c37782bb4e382dc41035e0970409a0c17b55e6ce/Cubical/Homotopy/Spectrum/Fiber.agda): `FiberSp` | Constructs a spectrum from the levelwise pointed fibers of a spectrum map and supplies the required loop equivalences. | Relevant to studying the information in the fiber of a map between approximation stages, once those stages and maps have been constructed. |

The Blakers–Massey comparison is especially worth understanding visually. From a span `B ← A → C`, form the homotopy pushout. The homotopy pullback of the two inclusions consists of a point of `B`, a point of `C`, and a path between their images in the glued type. The comparison sends a point of `A` to its two images and its designated gluing path. The theorem controls the connectivity of this comparison. A picture could distinguish the original gluing paths from other paths that become possible after gluing, while an attached statement records the theorem's hypotheses.

Use the source's connectivity convention when specializing its theorem: its `HLevel` indexing should not be silently substituted into a textbook's shifted connectivity convention.

The [earlier HoTT-Agda Blakers–Massey formalization](https://github.com/HoTT/HoTT-Agda/blob/master/theorems/homotopy/BlakersMassey.agda) also exists and was inspected. The Cubical source explicitly credits it and the [2016 paper](https://arxiv.org/abs/1605.03227). It gives a useful proof lineage, but its `--without-K --rewriting` setup is not the same library foundation as Cubical Agda.

## 4. agda-unimath: coherent cubes already have operations

Source snapshot: [`UniMath/agda-unimath` at d994fa9e57772eeecec84692371559e4ba9ac331](https://github.com/UniMath/agda-unimath/tree/d994fa9e57772eeecec84692371559e4ba9ac331).

The particularly useful file is [`src/foundation/commuting-cubes-of-maps.lagda.md`](https://github.com/UniMath/agda-unimath/blob/d994fa9e57772eeecec84692371559e4ba9ac331/src/foundation/commuting-cubes-of-maps.lagda.md). Given six face homotopies, `coherence-cube-maps` defines the further homotopy expressing their compatibility around the cube. The source supplies 120° and 240° rotations, three reflections, and vertical pasting of coherent cubes. These operations transform the mathematical data. They do not implement a mesh or a camera controller.

The code makes the face homotopies and cube filling explicit, and provides transformations that retain their compatibility. This is a concrete source to study when choosing how our diagrams should carry mathematical information. It remains a three-dimensional coherence construction, not proof that arbitrary-dimensional excision cubes or Taylor towers are finished.

The foundation differs from Cubical Agda. Its [`agda-unimath.agda-lib`](https://github.com/UniMath/agda-unimath/blob/d994fa9e57772eeecec84692371559e4ba9ac331/agda-unimath.agda-lib) uses `--without-K`. Its [`pushouts.lagda.md`](https://github.com/UniMath/agda-unimath/blob/d994fa9e57772eeecec84692371559e4ba9ac331/src/synthetic-homotopy-theory/pushouts.lagda.md) explicitly postulates the standard pushout and eliminator data; subsequent results reason from those assumptions. Its [optional rewriting module](https://github.com/UniMath/agda-unimath/blob/d994fa9e57772eeecec84692371559e4ba9ac331/src/synthetic-homotopy-theory/rewriting-pushouts.lagda.md) adds strict computation rules on point constructors. This is meaningful formal mathematics with stated assumptions, but it should not be described as the same computational foundation as native cubical higher inductive types. In particular, that module's “rewriting” means Agda computation rules, not ZX spider rewriting.

The [project](https://github.com/UniMath/agda-unimath/blob/d994fa9e57772eeecec84692371559e4ba9ac331/README.md) is a community formalization library and has an [MIT license](https://github.com/UniMath/agda-unimath/blob/d994fa9e57772eeecec84692371559e4ba9ac331/LICENSE.md).

## 5. Lean/mathlib: substantial classical foundations, not an automatic language choice

Source snapshot: [`leanprover-community/mathlib4` at a02a63c2100af866343c74550041885f8f39f099](https://github.com/leanprover-community/mathlib4/tree/a02a63c2100af866343c74550041885f8f39f099).

Three relevant implementations were inspected:

- [`AlgebraicTopology/ModelCategory/Basic.lean`](https://github.com/leanprover-community/mathlib4/blob/a02a63c2100af866343c74550041885f8f39f099/Mathlib/AlgebraicTopology/ModelCategory/Basic.lean) defines `HomotopicalAlgebra.ModelCategory` with finite limits and colimits, the weak-equivalence axioms, lifting properties, and factorizations. It also provides a constructor using weak factorization systems.
- [`AlgebraicTopology/SimplicialSet/KanComplex.lean`](https://github.com/leanprover-community/mathlib4/blob/a02a63c2100af866343c74550041885f8f39f099/Mathlib/AlgebraicTopology/SimplicialSet/KanComplex.lean) defines Kan complexes and proves horn filling. The selected filler `liftOfKanComplex` is explicitly `noncomputable`; that declaration is not an executable filling algorithm for a visualization.
- [`Algebra/Homology/DerivedCategory/Basic.lean`](https://github.com/leanprover-community/mathlib4/blob/a02a63c2100af866343c74550041885f8f39f099/Mathlib/Algebra/Homology/DerivedCategory/Basic.lean) constructs the derived category of an abelian category by localization and equips it with a triangulated structure. This gives substantial algebraic foundations, with explicit universe and localization choices.

These results provide reference implementations and possible proof reuse if we deliberately select Lean. They do not imply that a Lean dependency is necessary for the user's Agda or Idriç exploration. Translation of definitions or proofs across these foundations would itself be work. The inspected mathlib source uses [Apache 2.0](https://github.com/leanprover-community/mathlib4/blob/a02a63c2100af866343c74550041885f8f39f099/LICENSE).

## 6. Rational models as a further reading lead

Ben Walter's [Rational Homotopy Calculus of Functors](https://arxiv.org/abs/math/0603336) is a 2005 Brown dissertation, posted to arXiv in 2006. Its abstract describes Taylor-tower models for differential graded vector spaces, differential graded Lie algebras, and differential graded coalgebras, including models of homotopy pushouts and pullbacks.

This may offer concrete algebraic examples for a later computational exploration. **Only the record and abstract were checked in this pass**; neither the 134-page thesis nor an associated code implementation has been audited. “Computationally simple models” in a mathematical text is not evidence that an executable library is already available.

## What remains unverified, and the first useful experiment

Default-branch code searches for `Goodwillie` in `agda/cubical`, `mathlib4`, `UniMath/UniMath`, and `HoTT/HoTT-Agda`, and for `excisive` in those repositories plus `agda-unimath`, returned no results in this pass. Current directory trees and the specific source files above were also inspected. This does not rule out relevant code under different terminology, in other branches, or in other projects.

Before calling a future library “a Goodwillie implementation,” check that it supplies the actual chosen functor category, homotopy-coherent cubes, the definition of n-excision, the approximation construction and maps between stages, and the relevant theorem that these approximations have the promised property. The availability of pushouts, spectra, or a theorem mentioning connectivity separately does not establish that entire result.

A small first Agda experiment could import an existing pushout construction, instantiate its gluing data, and expose the comparison to a homotopy pullback. A second view could display the cube's face homotopies and a chosen filling, drawing on the verified coherent-cube operations. The displayed arrows, faces, selected theorem hypotheses, and proof status should have explicit meanings. Whether the scene is camera-rotatable is a presentation choice; a camera rotation alone proves no mathematical equivalence.

For Idriç, these same source declarations are useful specifications and language-design examples. Any proposed Idriç types should be marked as a sketch until checked by the user's actual compiler; an Agda proof does not silently become an Idriç proof.

See [acknowledgments and bibliography coverage](../ACKNOWLEDGMENTS.md).

