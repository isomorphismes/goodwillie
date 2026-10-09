# Existing diagram software worth studying for Goodwillie notes

Checked 2026-10-09. This is a source and documentation review, with selected source files inspected. No listed program was built or executed. The proposed connections to this repository are inferences, not reports of an existing Goodwillie implementation.

## What the quantum reference contributes

The relevant name is **ZX-calculus**. Its reusable ideas include named generators with boundaries, composition by joining matching boundaries, explicit permitted transformations, and a visible record of how a diagram changed. Those ideas can inform the interface without making ZX notation the language of this project.

### People and texts behind the earlier discussion

The relevant authors include **Bob Coecke, Ross Duncan, and Aleks Kissinger**. **Dusko Pavlovic** and **Jamie Vicary** also appear in the related categorical account of bases. The following primary records make the references precise:

| Source | What this pass establishes |
| --- | --- |
| Bob Coecke and Aleks Kissinger, [*Picturing Quantum Processes: A First Course in Quantum Theory and Diagrammatic Reasoning*](https://www.cambridge.org/core/books/picturing-quantum-processes/1119568B3101F3A685BE832FEEC53E52), Cambridge University Press, 2017. | Book identity and publisher record checked; a chapter-by-chapter reading remains open. |
| Bob Coecke and Ross Duncan, [*Interacting Quantum Observables: Categorical Algebra and Diagrammatics*](https://arxiv.org/abs/0906.4725). | Author record and abstract checked; a primary source for the ZX connection. |
| Bob Coecke, Dusko Pavlovic, and Jamie Vicary, [*A new description of orthogonal bases*](https://arxiv.org/abs/0810.0812). | Author record and abstract checked; a source for expressing basis structure through categorical operations. |

The spelling “Dusko Pavlovic” follows the linked public author records. No claim to have read the complete book or these two papers is made by listing them here.

The existing software already supplies substantial diagram infrastructure. The best first reading depends on the question:

| Question for this project | Most useful checked reference | What the reference actually implements |
| --- | --- | --- |
| How can a higher-dimensional diagram retain mathematical boundaries while being manipulated and rendered? | [homotopy.io](https://github.com/homotopy-io/homotopy-rs) | Finitely presented semistrict higher-category diagrams, generator signatures, typechecking, contraction and expansion, and two- and three-dimensional displays; four-dimensional diagrams can appear as movies of three-dimensional geometry. |
| How can someone apply rules, revisit earlier steps, and publish a readable derivation? | [ZXLive](https://github.com/zxcalc/zxlive) | Editing and proof modes, ZX rewriting through PyZX, a rewrite history, saved proofs, custom rules, and TikZ proof export. |
| How can an abstract diagram be evaluated in another mathematical setting? | [DisCoPy](https://github.com/discopy/discopy) | Typed objects and boxes, sequential and tensor composition, rewriting, drawing, and functors into chosen interpretations, including functions, matrices, and quantum circuits. |
| How can the diagram remain independent of its coordinates? | [Catlab.jl](https://github.com/AlgebraicJulia/Catlab.jl) | Boxes, ordered input/output ports and wires, categorical composition, and a distinction between the combinatorial diagram, its layout, and its rendered drawing. |
| How can user-specified algebraic rules produce checked diagrammatic steps? | [Chyp](https://github.com/akissinger/chyp) | Generators, sequential/parallel composition, rules and checked rewrite chains for symmetric monoidal categories, using monogamous acyclic hypergraphs. |
| What earlier work already addressed variable-size patterns and branching derivations? | [Quantomatic](https://github.com/zxcalc/quantomatic) | String-graph rewriting, repeatable subdiagram patterns called !-boxes, and derivation editing. Its repository is archived and its README says it is no longer maintained. |
| Which reference already calculates and simplifies actual ZX diagrams? | [PyZX](https://github.com/zxcalc/pyzx) | ZX graph representations, quantum-circuit import and extraction, automatic simplification, and graph visualization. |

Sources: [homotopy.io paper](https://arxiv.org/abs/2402.13179), [ZXLive introduction](https://zxlive.readthedocs.io/en/latest/gettingstarted.html), [DisCoPy README](https://github.com/discopy/discopy), [Catlab wiring-diagram basics](https://algebraicjulia.github.io/Catlab.jl/stable/generated/wiring_diagrams/wiring_diagram_basics/), [Chyp README](https://github.com/akissinger/chyp), [Quantomatic paper](https://arxiv.org/abs/1503.01034), and [PyZX README](https://github.com/zxcalc/pyzx).

## Concrete things already implemented

### homotopy.io: boundaries and higher-dimensional display

The tutorial constructs categories, functors, and natural transformations as generators of successive dimensions. A new higher-dimensional generator must have source and target with matching lower-dimensional boundaries: the tutorial explicitly demonstrates the globularity check and an invalid construction. Directed and invertible generators are distinguished. This is directly relevant to avoiding a picture whose apparent joins hide an ill-formed mathematical composition. [Tutorial](https://github.com/homotopy-io/homotopy-rs/blob/master/TUTORIAL.md).

The inspected `typecheck` entry point first checks dimensions, then checks the diagram against its signature. Its error types distinguish unknown generators, ill-typed diagrams, impermissible orientation of directed generators, and illegal non-manifold behavior of dualisable generators. This is a selected source review, not a soundness audit. [Inspected source](https://github.com/homotopy-io/homotopy-rs/blob/c51f0cd7625ff2e994b247089c6d64ba7c28be1a/homotopy-core/src/typecheck.rs).

The separate core and graphics crates make useful study targets. The core exposes diagrams, rewrites, signatures, typechecking, layout, projection and meshes; the graphics crate exposes SVG, TikZ, STL and Manim modules. Presence of an export module does not establish that every dimension can be exported successfully. The README describes Manim support as experimental. [Core module inventory](https://github.com/homotopy-io/homotopy-rs/blob/master/homotopy-core/src/lib.rs), [graphics module inventory](https://github.com/homotopy-io/homotopy-rs/blob/master/homotopy-graphics/src/lib.rs).

**Inference for this repository:** this is the closest checked precedent for manipulating coherent higher-dimensional diagrams and displaying their slices. An interpretation of Goodwillie constructions in its chosen higher-category model would still have to be specified and justified.

### ZXLive and PyZX: keep every transformation explainable

ZXLive's documented proof mode restricts edits to rewrite operations and shows the resulting history. A completed derivation can be saved, revisited and exported. Its custom-rule interface can introduce a rule directly or obtain one from a finished derivation, and supports phase parameters. Therefore the provenance of a rule matters: a user-entered axiom and a derived result must remain distinguishable. [Proof workflow](https://zxlive.readthedocs.io/en/latest/gettingstarted.html), [custom rules](https://zxlive.readthedocs.io/en/latest/adding-rewrites.html).

The inspected `zxlive/proof.py` represents a proof using its initial graph and a list of rewrites. A rewrite records its display name, rule name, resulting graph, and optionally grouped rewrites. This is a concrete reusable record structure. The inspected portion establishes serialization and history management; it does not establish an independent proof kernel that rechecks all saved steps. [Inspected source](https://github.com/zxcalc/zxlive/blob/47ffbaef615ca294daf69bef37f86fd4c37f5b94/zxlive/proof.py). The TikZ exporter preserves the sequence with named rewrite steps. [Export documentation](https://zxlive.readthedocs.io/en/latest/tikz_export.html).

PyZX's graph abstraction retains vertex kinds, phase data, boundaries and edge kinds. Phases are represented in units of π. The default graph backend may simplify newly added parallel edges using ZX rules; a multigraph backend keeps them explicitly. That difference matters: a generic diagram representation must not silently simplify edges using quantum-specific identities. [Graph documentation](https://pyzx.readthedocs.io/en/stable/graph.html), [graph base class located in source](https://github.com/zxcalc/pyzx/blob/c2b209a0623ec9f18d358baf7b80c46e628e3d39/pyzx/graph/base.py).

### DisCoPy and Catlab: syntax, interpretation and drawing

DisCoPy's `Ty` represents a sequence of object types; each generating `Box` has a domain and codomain. Its diagrams have composable layers. The project also provides distinct diagram families for additional categorical structure and functorial evaluation. These are useful examples of selecting a theory explicitly rather than granting every diagram the same identities. [README and theory overview](https://github.com/discopy/discopy), [monoidal implementation](https://github.com/discopy/discopy/blob/main/discopy/monoidal.py).

Catlab's basic wiring-diagram example explicitly builds boxes with input/output sequences and connects particular ports. Its layout vignette first creates a purely combinatorial diagram, then obtains a Graphviz or series-parallel layout, then renders the same layout with Compose.jl or TikZ. This supplies a working precedent for preserving one mathematical object across multiple drawings. [Wiring-diagram basics](https://algebraicjulia.github.io/Catlab.jl/stable/generated/wiring_diagrams/wiring_diagram_basics/), [layouts versus drawings](https://algebraicjulia.github.io/Catlab.jl/stable/generated/graphics/layouts_vs_drawings/).

Catlab's own scope statement says it does not produce formal proof certificates and is not a proof assistant. Its typed symbolic and categorical computations should be described accordingly. [Project scope](https://algebraicjulia.github.io/Catlab.jl/stable/).

### Chyp and Quantomatic: rules supplied by a theory

Chyp's declarative language specifies generators and equations, then checks that each claimed rewrite follows from the given rule. Its README explicitly distinguishes the implemented symmetric-monoidal rewriting from additional Frobenius rewriting planned for the future. It also qualifies its soundness guarantees relative to an LCF-style proof kernel. A prover's name alone is insufficient evidence about the trusted base. [Chyp README](https://github.com/akissinger/chyp).

Quantomatic's current README points to ZXLive and Chyp as newer related projects, while explicitly saying neither is a direct successor. Its 2015 paper remains a useful account of the earlier system. [Current README](https://github.com/zxcalc/quantomatic), [paper](https://arxiv.org/abs/1503.01034).

## A lawful spider move, and what a Goodwillie cube needs

Take two Z-spiders, each with one input and one output, with phases π/2 and π/4. In the usual qubit interpretation, such a spider preserves the amplitude of the zero basis vector and multiplies the amplitude of the one basis vector by its phase factor. Composing the two multiplies those factors:

`exp(iπ/2) × exp(iπ/4) = exp(3iπ/4)`.

The resulting diagram can therefore use one Z-spider of phase 3π/4. More generally, connected spiders of the same kind fuse and their phases add modulo 2π. The displayed contraction expresses an equality of the represented linear maps, with the chosen scalar convention. [John van de Wetering, sections 3.1 and 4.1](https://arxiv.org/pdf/2012.13966).

For a Goodwillie square, the four corners represent spaces, spectra or other objects in the chosen source category. The edges represent maps. In a homotopy-coherent presentation, a face also carries a specified homotopy between the two boundary composites. The fact that the boundary can be drawn does not establish that the square is a homotopy pushout or homotopy pullback. Those are additional mathematical properties, expressed through comparison maps and universal properties in the relevant homotopy theory. See the [Goodwillie notes and primary references G1–G3](goodwillie-calculus.md), and [Kerodon Tag 0106](https://kerodon.net/tag/0106).

For a finite cubical example, label a corner by the subset of directions already traversed: ∅, {first}, {second}, and {first, second}. With three directions the indexing set has eight subsets. This indexing, the maps, and the coherence data should survive rotation of the displayed cube unchanged. Goodwillie-specific definitions and references should accompany this example in the main notes; it is not a claimed translation from ZX-calculus.

## Proposed representation, as an inference

A useful design would retain the following separate pieces of information:

1. **The mathematical diagram:** indexing shape, objects, maps, sources and targets, and any explicit coherence data.
2. **The selected theory:** the meaning of composition and tensor-like operations, the allowed equations, their hypotheses, and whether a step is an equality, equivalence or specified higher cell.
3. **Evidence for a step:** the named rule or theorem, its instantiation, the matched boundary, before/after objects, and whether the rule was assumed or derived.
4. **The drawing:** positions, paths, surfaces, colors, camera and slice choices. A change here should preserve the first three unless an explicit mathematical edit was requested.

This proposal combines patterns already visible in the checked systems. It is not a recommendation to adopt Python, Julia or Rust for the user's project. The existing implementations can supply reference designs and examples while the repository's own language and rendering choices remain to be decided in its established context.

## What was not found in the checked scope

An exact-name GitHub code search for `Goodwillie` across `homotopy-io/homotopy-rs`, `zxcalc/pyzx`, `zxcalc/zxlive`, `zxcalc/quantomatic`, `discopy/discopy`, `AlgebraicJulia/Catlab.jl`, and `akissinger/chyp` returned no results on 2026-10-09. Together with the inspected scope descriptions, this supports **no Goodwillie-calculus implementation identified in this checked set**. It does not establish that none exists elsewhere, under different terminology, or in unindexed code.

The remaining mathematical work includes the actual choices of source and target homotopy theories, homotopy (co)limits, excision conditions, polynomial approximations and their tower maps, layers and derivatives, and any applicable convergence claims. The diagram packages above are evidence that much of the visual and compositional machinery has been built; they are not evidence that these Goodwillie constructions have been formalized there.

## License inventory

This records the checked project files, not a full dependency or redistribution audit.

| Project | Checked license | Direct file or explicit project statement |
| --- | --- | --- |
| homotopy.io source | BSD 3-Clause | [LICENSE](https://github.com/homotopy-io/homotopy-rs/blob/master/LICENSE) |
| homotopy.io documentation | CC BY 4.0 | [README license section](https://github.com/homotopy-io/homotopy-rs#license) |
| PyZX | Apache 2.0 | [LICENSE](https://github.com/zxcalc/pyzx/blob/master/LICENSE) |
| ZXLive | Apache 2.0 | [LICENSE](https://github.com/zxcalc/zxlive/blob/master/LICENSE) |
| DisCoPy | BSD 3-Clause | [LICENSE](https://github.com/discopy/discopy/blob/main/LICENSE) |
| Catlab.jl | MIT | [LICENSE](https://github.com/AlgebraicJulia/Catlab.jl/blob/main/LICENSE) |
| Chyp | Apache 2.0 | [LICENSE](https://github.com/akissinger/chyp/blob/master/LICENSE) |
| Quantomatic | GPL version 3 text | [docs/LICENSE.txt](https://github.com/zxcalc/quantomatic/blob/stable/docs/LICENSE.txt); also stated as GPLv3 in its paper |

## Inspection depth and exact source anchors

| Source | What was actually inspected |
| --- | --- |
| homotopy.io | README, tutorial boundary construction, paper abstract, license, core/graphics module inventories, and first 140 lines of `homotopy-core/src/typecheck.rs` at `c51f0cd7625ff2e994b247089c6d64ba7c28be1a`. |
| ZXLive | README, getting-started/custom-rules/TikZ documentation, first 150 lines of `zxlive/proof.py`, and package metadata at `47ffbaef615ca294daf69bef37f86fd4c37f5b94`. |
| PyZX | README and graph documentation; code search located `pyzx/graph/base.py` at `c2b209a0623ec9f18d358baf7b80c46e628e3d39`; the whole file was not read. |
| DisCoPy | README, main documentation index, module page for `discopy/monoidal.py`, paper metadata and license; no runtime or full-module correctness review. |
| Catlab.jl | README, scope page, wiring-diagram and layouts-versus-drawings vignettes, license; code search located `src/graphics/GraphvizWiringDiagrams.jl` and `docs/literate/graphics/layouts_vs_drawings.jl` at `692bd5e9d4b792a01bc507a59a19c2311b321359`. |
| Chyp | README's implemented calculus, generators, rewrite checking, tactics and soundness qualification; repository license badge. |
| Quantomatic | Current README/archive notice, paper sections on derivations/architecture plus acknowledgments/bibliography, and beginning of `docs/LICENSE.txt`; no current engine code audit. |
| ZX semantics | John van de Wetering's survey, selected text and visual inspection of pages 9 and 19 for spider meaning and fusion; not the whole 93-page survey. |

All unpinned branch and documentation links can move. The full hashes above identify only the selected source anchors; they are not test or build receipts.

See [acknowledgments and bibliography coverage](../ACKNOWLEDGMENTS.md).

