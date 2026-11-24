# Local Galois cohomology: literature fidelity audit (pilot 01)

Source snapshot: [a28ad7ec8b9f5ff4ec448fe210d3ac57aacfffe1](https://github.com/leanprover-community/riemann-hypothesis/tree/a28ad7ec8b9f5ff4ec448fe210d3ac57aacfffe1). Mathlib pin: 31ce06eaadf4a200c0e1407c5f10953cf606b8f8. Related upstream [issue #39](https://github.com/leanprover-community/riemann-hypothesis/issues/39). This is an independent working note, not a proposed upstream documentation PR.

## Scope and evidence

Three proposition declarations, not three new proofs, are reviewed. The source statements and the listed definitions were read at the pinned revision. The mathematical implications below are conventional mathematical arguments; neither a fresh Lean compilation nor a new kernel dependency export was performed. The labels are candidate Literature Links relations (not schema-validated annotations). The actual theorem providers and use in the maintained tree remain to be checked. The initial inventory in [issue #39](https://github.com/leanprover-community/riemann-hypothesis/issues/39) lists these three among its unreviewed entries; it does not establish their full semantics.

## 1. Common semantics

Let l be prime, K a finite extension of Q_l inside its algebraic closure, R = Z/l^r Z with r>0, and M a finite discrete R-module with continuous G_K action.

- [StatementLoc.lean](https://github.com/leanprover-community/riemann-hypothesis/blob/a28ad7ec8b9f5ff4ec448fe210d3ac57aacfffe1/src/Formalism/GaloisCohomology/StatementLoc.lean) defines GK as the K-algebra automorphisms of PadicAlgCl l. At the pinned Mathlib revision [Complex.lean](https://github.com/leanprover-community/mathlib4/blob/31ce06eaadf4a200c0e1407c5f10953cf606b8f8/Mathlib/NumberTheory/Padics/Complex.lean), PadicAlgCl is AlgebraicClosure Q_l, not its completion. It is algebraic and algebraically closed over K, so it is a separable closure in characteristic zero.
- [KrullTopology.lean](https://github.com/leanprover-community/mathlib4/blob/31ce06eaadf4a200c0e1407c5f10953cf606b8f8/Mathlib/FieldTheory/KrullTopology.lean#L101-L140) supplies the Krull topology on the algebra automorphism group. This is the intended source-level instance; no fresh #synth was run.
- [SeparatedDvrSecondCohomologyMem.lean](https://github.com/leanprover-community/riemann-hypothesis/blob/a28ad7ec8b9f5ff4ec448fe210d3ac57aacfffe1/src/ReciprocityTheory/GaloisValuations/SeparatedDvrSecondCohomologyMem.lean) defines ContAction as continuity of the evaluation action, H0 as invariants, H1 and H2 as quotients of continuous inhomogeneous cocycles by coboundaries, and Hn from continuous maps on (Fin n → G), the standard alternating inhomogeneous differential, and its kernel/image quotient. In degree 1 the cocycle equation is f(gh)=f(g)+g f(h); in degree 2 it is g f(h,k)-f(gh,k)+f(g,hk)-f(g,h)=0. Under a continuous action the image of the differential is contained in the cocycles, so the comap in the quotient has the usual interpretation. The types Fin n → G carry the finite product topology.
- All coefficients are killed by l^r. The additive group of M is finite l-primary, including the trivial group. Thus every element order divides l^r and |M| is an l-power. No freeness of M over R is assumed.

## 2. GC-001: local Euler characteristic

Lean: [LocalEulerCharK](https://github.com/leanprover-community/riemann-hypothesis/blob/a28ad7ec8b9f5ff4ec448fe210d3ac57aacfffe1/src/Formalism/GaloisCohomology/LocalTateDualityK.lean#L90-L94).
Literature: J. S. Milne, *Arithmetic Duality Theorems*, 2nd ed., 2006, I.2.8, [printed p.31, PDF p.39](https://wstein.org/edu/2010/582e/refs/milne-arithmetic_duality_theorems.pdf#page=39). The theorem gives the inverse index of m O_K in O_K for the Euler characteristic, where m=|M|.

Writing m=l^a and d=[K:Q_l], O_K is free of rank d over Z_l. Consequently [O_K:m O_K]=l^(ad)=m^d. The Euler characteristic is |H^0| |H^2| / |H^1|. Finiteness and the presence of zero in each group allow multiplication by the nonzero denominator. The resulting identity is precisely |H^1|=|H^0| |H^2| m^d, as in the Lean proposition with Nat.card and Module.finrank.

Candidate relation: narrowMatch. The literature covers finite modules beyond the l-primary case and more general local fields. This is not a statement that every formal definition has been proved equivalent in Lean. The cardinality comparison requires the finiteness result; Nat.card of an infinite type is not its usual infinite cardinality.

## 3. GC-002: local finiteness and vanishing

Lean: [LocalFinitenessK](https://github.com/leanprover-community/riemann-hypothesis/blob/a28ad7ec8b9f5ff4ec448fe210d3ac57aacfffe1/src/Formalism/GaloisCohomology/LocalTateDualityK.lean#L74-L79).
Sources: R. Sharifi, [Iwasawa Theory, Appendix A, Remark A.0.2 and Theorem A.0.3](https://math.ucla.edu/~sharifi/notes/iwasawa-app-a.html); B. Conrad, [Etale cohomology of algebraic number fields](https://math.stanford.edu/~conrad/BSDseminar/Notes/L4.pdf), discussion of local duality and cohomological dimension, referencing Serre, Galois Cohomology, II §5.2.

For finite discrete l-primary modules over a p-adic local field, continuous cohomology is finite; cohomological dimension 2 gives H^n=0 for n>=3. The Lean proposition lists finiteness only for H0,H1,H2 and Subsingleton Hn for n>=3. Since these quotients are modules with zero, subsingleton is equivalent to the zero group. Finiteness of the higher groups follows as well.

Source caution: the sentence immediately before Theorem I.2.8 in the consulted Milne PDF says vanishing for r>=2, inconsistent with its own H^2 term and local duality. This is an apparent typographical error; the vanishing boundary is independently corroborated by the above sources. It must not be propagated into a literature annotation.

Candidate relation: narrowMatch relative to the general local-field finiteness and dimension statement. Exact published theorem locator in Serre still needs checking before a final annotation.

## 4. GC-003: local Tate duality

Lean: [LocalTateDualityK](https://github.com/leanprover-community/riemann-hypothesis/blob/a28ad7ec8b9f5ff4ec448fe210d3ac57aacfffe1/src/Formalism/GaloisCohomology/LocalTateDualityK.lean#L80-L89).
Literature: Milne, I.2.3, [printed p.28, PDF p.36](https://wstein.org/edu/2010/582e/refs/milne-arithmetic_duality_theorems.pdf#page=36), and I.2.5(b), p.29; Sharifi, [Appendix A, Lemma A.0.1 and Theorem A.0.3](https://math.ucla.edu/~sharifi/notes/iwasawa-app-a.html).

The source's dual is Hom_Z(M,(K^sep)^×). For l^r-torsion M its image lies in μ_{l^r}. Choosing a primitive l^r-th root identifies the additive R with μ_{l^r}, giving Hom_R(M,R)(1) as the Cartier dual; additive homomorphisms between these R-modules are R-linear automatically. No assumption that M is a free R-module is needed.

In [GalCohDefs.lean](https://github.com/leanprover-community/riemann-hypothesis/blob/a28ad7ec8b9f5ff4ec448fe210d3ac57aacfffe1/src/ReciprocityTheory/Hypotheses/GalCohDefs.lean), TDual is Module.Dual R M, charRep acts by the character, and twDual acts on f by g·f=χ(g) f∘ρ(g^{-1}). evalPair is evaluation; the three cup maps are lifts of the cocycle pairings, with the degree-(1,1) pairing taken in the dual-first order. [ContinuousConstSMulQuotient.lean](https://github.com/leanprover-community/riemann-hypothesis/blob/a28ad7ec8b9f5ff4ec448fe210d3ac57aacfffe1/src/ReciprocityTheory/GaloisValuations/ContinuousConstSMulQuotient.lean) implements the cup maps on cocycle quotients.

The Kummer sequence and Hilbert 90 identify H^2(K,μ_{l^r}) with Br(K)[l^r]; the local invariant identifies this with (1/l^r)Z/Z, hence R. This explains the Nonempty linear equivalence in the Lean proposition (existence, rather than a distinguished invariant map). Perfectness in degrees (0,2), (1,1), and (2,0) gives the asserted Function.Bijective conditions, including the flipped degree-(1,1) map: every additive homomorphism into the l^r-torsion target is R-linear, and a perfect pairing of finite groups identifies either factor with the dual of the other.

Critical remaining check: the declaration uses cycK defined via Mathlib's cyclotomicCharacter followed by reduction mod l^r. Its identification with the action on roots of unity is expressed by the nearby CycKSpec; the proof of that specification and its use must still be traced. The existence of that proposition alone does not prove the identification. Check exact cochain cup formula and the actual provider/consumer chain before promoting the candidate to a reviewed annotation.

Candidate relation: narrowMatch, conditional on the outstanding identification checks. This formal interface uses finite l-primary coefficients over finite Q_l extensions, and does not assert a canonical invariant isomorphism.

## 5. Review gates

1. Pin source and literature edition and locate the precise statement (done for GC-001; source located for GC-002/003).
2. Read project definitions and distinguish direct observations from mathematical derivations (pilot done; cyclotomic and instance checks remain).
3. Locate actual providers and distinguish proof dependencies from mere imported or carried hypotheses (pending).
4. Independently review mathematical comparison and, if practical, verify a Lean bridge on pinned dependencies (pending).
5. Produce Literature Links annotations with verified document hash/location and run the official validator (pending).
6. Coordinate with maintainers before an upstream contribution. No changes to proof code in this research branch.

This work was assisted by ChatGPT.
