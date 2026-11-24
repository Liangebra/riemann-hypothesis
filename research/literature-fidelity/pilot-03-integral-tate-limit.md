# Pilot 03 follow-up — finite-level to integral Tate-twist cohomology

Snapshot e61ceacb65699cf48728e7738c843b5ab8a5c1f2. Source-body audit and independent mathematical reconstruction, not a fresh elaboration or kernel dependency check. This checks the high-risk interface identified in pilot-03-main-consumption.md.

## 1. Exact mathematical scope

[Formalism/GaloisCohomology/Statement.lean:98–151](https://github.com/Liangebra/riemann-hypothesis/blob/e61ceacb65699cf48728e7738c843b5ab8a5c1f2/src/Formalism/GaloisCohomology/Statement.lean#L98-L151) defines D with S **equal to** the set of primes above l, not merely containing them. Its cyclotomic character chi factors through G=G_{F,S}. For integer e put

    R=Z_l, T=R(chi^(e+1)), A=(Q_l/Z_l)(chi^(-e)),
    T_r=T/l^r T, A_r=A[l^r].
    X=H²(G,T), Y=product_{v in S} H²(G_v,T),
    f:X->Y, K=ker f, C=coker f,
    B=Sha¹(G,A).

All cochains are continuous in the indicated coefficient topology. The A-module is discrete; the T-module is p-adic, not discrete. Assume l odd or F totally imaginary (PTCase). The aim is the LENGTH identities length(C)=length(H⁰(G,A)), length(K)=length(B), and local length(H²(G_v,T))=length(H⁰(G_v,A)). These are equalities in extended naturals, not a construction of a perfect integral pairing or an assertion of finiteness for every twist.

## 2. Actual reduction maps, not a presumed commutation with inverse limits

For a continuous rank-one R-representation, let rho_r be its genuine reduction. The source proves

    H²(G,T)/l^r H²(G,T) -> H²(G,T_r)

is bijective under H³(G,T_1)=0. The map is induced by pointwise reduction on cocycles.

* Kernel: lift a continuous primitive modulo l^r by a section of the finite discrete coefficient set. If z reduces to a coboundary, z-d b is pointwise divisible by l^r. Divide it by l^r; the quotient is continuous because multiplication by l^r is a closed embedding R->R (compact-to-Hausdorff). The quotient remains a cocycle by torsion-freeness. Thus [z] lies in l^r H². The converse is immediate. See [PadicIntTwoCocycleEqPowSmulOfReductionCoboundary.lean:58–95](https://github.com/Liangebra/riemann-hypothesis/blob/e61ceacb65699cf48728e7738c843b5ab8a5c1f2/src/Development/AbelianVarietyTheory/Proofs/FoundationProofs/PadicIntTwoCocycleEqPowSmulOfReductionCoboundary.lean#L58-L95) and [958–979](https://github.com/Liangebra/riemann-hypothesis/blob/e61ceacb65699cf48728e7738c843b5ab8a5c1f2/src/Development/AbelianVarietyTheory/Proofs/FoundationProofs/LengthEqLengthInvariantsOfTorsionLevels.lean#L958-L979).
* Surjectivity: lift a mod-l^r two-cocycle y to a continuous cochain Y. Write dY=l^r c by continuous division (see [169–194](https://github.com/Liangebra/riemann-hypothesis/blob/e61ceacb65699cf48728e7738c843b5ab8a5c1f2/src/Development/AbelianVarietyTheory/Proofs/FoundationProofs/LengthEqLengthInvariantsOfTorsionLevels.lean#L169-L194)). The equation d²=0 and torsion-freeness imply dc=0. Since H³(G,T_1)=0, iteratively lift primitives of the reductions of c_i to get c_i=d u_i+l c_{i+1}, with c_0=c and continuous integral cochains throughout. Set u=sum_{i>=0} l^i u_i. The uniform bound ||l^i u_i(g,h)||<=l^(-i) proves uniform summability and continuity, and the differential commutes with this sum because it is a finite linear combination of evaluations. Telescoping leaves remainder l^N c_N tending to zero. Thus du=c and z=Y-l^r u is an integral cocycle reducing to y. See [231–335](https://github.com/Liangebra/riemann-hypothesis/blob/e61ceacb65699cf48728e7738c843b5ab8a5c1f2/src/Development/AbelianVarietyTheory/Proofs/FoundationProofs/LengthEqLengthInvariantsOfTorsionLevels.lean#L231-L335) and [980–990](https://github.com/Liangebra/riemann-hypothesis/blob/e61ceacb65699cf48728e7738c843b5ab8a5c1f2/src/Development/AbelianVarietyTheory/Proofs/FoundationProofs/LengthEqLengthInvariantsOfTorsionLevels.lean#L980-L990).

This proves the quotient comparison without a general inverse-limit exactness claim, without assuming Mittag-Leffler for all cohomology groups, and without presupposing integral global duality. H³ vanishing only at r=1 is sufficient for the explicit iterative lift.

## 3. Separation and finite generation of integral H²

The reduction comparison alone does not establish l-adic separation of X. The source proves it independently, assuming finite H¹(G,T_s) for every s>0.

[PadicCoboundaryOfDivisible.lean:91–166](https://github.com/Liangebra/riemann-hypothesis/blob/e61ceacb65699cf48728e7738c843b5ab8a5c1f2/src/Development/AbelianVarietyTheory/Proofs/FoundationProofs/PadicCoboundaryOfDivisible.lean#L91-L166): if z represents a class in every l^r H², let E_s be the set of continuous one-cochain primitives of z mod l^s. E_s is nonempty. Its cardinality is finite because it is a translate of Z¹(G,T_s), and Z¹ is finite as an extension of finite H¹ by B¹, the image of finite T_s under d0. This is a finiteness statement about the solution set, not about all continuous maps G->T_s.

Consider the compact product R^G of arbitrary functions with the pointwise topology. For each r let F_r consist of functions whose reductions at s=1,...,r+1 belong to E_s. These sets are closed, since E_s is finite in the Hausdorff product of finite coefficient spaces and pointwise reduction is continuous. They are nested. They are nonempty because z-l^(r+1)w=d b for some continuous b. By compactness their intersection contains b. Every reduction of b is continuous, hence b is continuous in the p-adic topology; d b=z follows from equality of all reductions. This establishes intersection_r l^r X=0, without wrongly assuming a pointwise limit of continuous maps is continuous.

[1001–1018](https://github.com/Liangebra/riemann-hypothesis/blob/e61ceacb65699cf48728e7738c843b5ab8a5c1f2/src/Development/AbelianVarietyTheory/Proofs/FoundationProofs/LengthEqLengthInvariantsOfTorsionLevels.lean#L1001-L1018) applies this to H², uses reduction at r=1 for X/lX finite, then [534–556](https://github.com/Liangebra/riemann-hypothesis/blob/e61ceacb65699cf48728e7738c843b5ab8a5c1f2/src/Development/AbelianVarietyTheory/Proofs/FoundationProofs/LengthEqLengthInvariantsOfTorsionLevels.lean#L534-L556) invokes the complete-local-ring finite-generation lemma finite_of_isHausdorff_of_finite_quotient. Independent elementary justification of the latter in this setting: choose lifts x_1,...,x_m of a spanning set of X/lX. For each x in X recursively approximate x=sum_i a_i^(N)x_i mod l^N X, choosing coherent coefficients a_i^(N) by adding l^N times digits. Completeness of R gives coefficient limits a_i in R. The difference x-sum_i a_i x_i belongs to every l^N X, hence zero by separation. No completeness of X has to be assumed. The exact mathlib provider dependency was not independently traversed.

At the actual application, [1086–1112](https://github.com/Liangebra/riemann-hypothesis/blob/e61ceacb65699cf48728e7738c843b5ab8a5c1f2/src/Development/AbelianVarietyTheory/Proofs/FoundationProofs/LengthEqLengthInvariantsOfTorsionLevels.lean#L1086-L1112) and [1150–1190](https://github.com/Liangebra/riemann-hypothesis/blob/e61ceacb65699cf48728e7738c843b5ab8a5c1f2/src/Development/AbelianVarietyTheory/Proofs/FoundationProofs/LengthEqLengthInvariantsOfTorsionLevels.lean#L1150-L1190) transport local finiteness and H³ vanishing from local fields by a cyclotomic-compatible decomposition-group equivalence. The condition v|l is supplied by the equality S=Above, not by an unwarranted assertion about every place of a larger S. [1222–1236](https://github.com/Liangebra/riemann-hypothesis/blob/e61ceacb65699cf48728e7738c843b5ab8a5c1f2/src/Development/AbelianVarietyTheory/Proofs/FoundationProofs/LengthEqLengthInvariantsOfTorsionLevels.lean#L1222-L1236) obtains global finite H¹ at every r and H² at r=1 by localization into finite products and finite Sha kernels from ShaDual; global H³ mod l comes from finite_level. Hence the hypotheses of both lifting and separation are actually supplied. No integral duality is used to establish those hypotheses.

## 4. The Tate-dual coefficient and the filtration comparison

The finite Tate dual is A_r, not T_r with the original exponent. [678–730](https://github.com/Liangebra/riemann-hypothesis/blob/e61ceacb65699cf48728e7738c843b5ab8a5c1f2/src/Development/AbelianVarietyTheory/Proofs/FoundationProofs/LengthEqLengthInvariantsOfTorsionLevels.lean#L678-L730) constructs the torsion embedding R/l^r->Q_l/Z_l by sending a residue of a to a/l^r modulo R. It proves injectivity and exact image A[l^r]. [1058–1081](https://github.com/Liangebra/riemann-hypothesis/blob/e61ceacb65699cf48728e7738c843b5ab8a5c1f2/src/Development/AbelianVarietyTheory/Proofs/FoundationProofs/LengthEqLengthInvariantsOfTorsionLevels.lean#L1058-L1081) evaluates a linear functional on 1 to identify the rank-one dual and checks equivariance: chi * (chi^(e+1))^(-1)=chi^(-e). Thus the source does not confuse the two twists. The reduction and dual embeddings are explicit; no compatibility of arbitrary isomorphisms is needed.

A useful general elementary lemma, actually proved in [464–518](https://github.com/Liangebra/riemann-hypothesis/blob/e61ceacb65699cf48728e7738c843b5ab8a5c1f2/src/Development/AbelianVarietyTheory/Proofs/FoundationProofs/LengthEqLengthInvariantsOfTorsionLevels.lean#L464-L518), is as follows. If N_r is a decreasing separated filtration of an R-module X' and P_r is an increasing exhaustive filtration of an R-module Y', with finite equal cardinalities of X'/N_r and P_r for all sufficiently large r, then their R-lengths agree. If cardinalities are bounded, they attain a maximum on the tail. Inclusions on P_r and quotient maps on X'/N_r become bijections, so P_r=Y' and N_r=0 by exhaustion and separation. Both modules are finite with equal cardinality; over R their length equals log_l(cardinality). If either module is finite, the cardinalities are bounded by it. Otherwise both are infinite, hence both lengths are top because the residue field is finite. This is valid even for modules not finitely generated. It is a LENGTH comparison, not a proof that X' and Y' are isomorphic.

For the local H²-length formula take N_r=l^r X' and P_r=H⁰(G_v,A)[l^r]. Reduction gives X'/N_r=H²(G_v,T_r), and local Tate duality gives its cardinality as #H⁰(G_v,A_r). Equivariance gives the invariant identification with P_r. Apply the filtration lemma. This is precisely [1020–1042](https://github.com/Liangebra/riemann-hypothesis/blob/e61ceacb65699cf48728e7738c843b5ab8a5c1f2/src/Development/AbelianVarietyTheory/Proofs/FoundationProofs/LengthEqLengthInvariantsOfTorsionLevels.lean#L1020-L1042) and [1180–1190](https://github.com/Liangebra/riemann-hypothesis/blob/e61ceacb65699cf48728e7738c843b5ab8a5c1f2/src/Development/AbelianVarietyTheory/Proofs/FoundationProofs/LengthEqLengthInvariantsOfTorsionLevels.lean#L1180-L1190).

## 5. The kernel of localization cannot be reduced naively

Let f_r:X/l^r X->Y/l^r Y be the actual localization at finite level. The reduction squares commute on cochains (the code verifies by quotient induction, rfl, [1243–1263](https://github.com/Liangebra/riemann-hypothesis/blob/e61ceacb65699cf48728e7738c843b5ab8a5c1f2/src/Development/AbelianVarietyTheory/Proofs/FoundationProofs/LengthEqLengthInvariantsOfTorsionLevels.lean#L1243-L1263)). There is always

    C/l^r C ≅ coker f_r.

But the expected kernel comparison is only

    K/(K intersect l^r X) ≅ ker f_r

under the extra condition

    im f intersect l^r Y = l^r im f.

Counterexample to an unconditional assertion: f:R->R is multiplication by l, r=1. K=0 while ker f_1=R/lR. Even if reduction of X and Y is exact, reduction of the kernel need not be. The source has precisely the extra image-saturation hypothesis. See [358–420](https://github.com/Liangebra/riemann-hypothesis/blob/e61ceacb65699cf48728e7738c843b5ab8a5c1f2/src/Development/AbelianVarietyTheory/Proofs/FoundationProofs/LengthEqLengthInvariantsOfTorsionLevels.lean#L358-L420): it constructs the surjection by choosing an integral lift x of a finite-level kernel element and correcting x by l^r x_1 when f(x)=l^r f(x_1). The denominator is K intersect l^r X, not l^r K; no equality between these two is assumed.

Using the last two arrows of the nine-term sequence, [1265–1282](https://github.com/Liangebra/riemann-hypothesis/blob/e61ceacb65699cf48728e7738c843b5ab8a5c1f2/src/Development/AbelianVarietyTheory/Proofs/FoundationProofs/LengthEqLengthInvariantsOfTorsionLevels.lean#L1265-L1282) identifies #coker f_r with #H⁰(G,A_r); the finite dual over Z/l^r has equal cardinality. Hence length(C)=length(H⁰(G,A)), by the same filtration lemma. In particular C is a quotient of finitely generated Y.

## 6. The torsion Sha exhaustion is also not a naive direct limit

[765–891](https://github.com/Liangebra/riemann-hypothesis/blob/e61ceacb65699cf48728e7738c843b5ab8a5c1f2/src/Development/AbelianVarietyTheory/Proofs/FoundationProofs/LengthEqLengthInvariantsOfTorsionLevels.lean#L765-L891) defines P_r as the image of finite-level Sha¹(G,A_r) in B=Sha¹(G,A), in an explicit representative form: a class has a cocycle taking values in A[l^r] and local primitives also in A[l^r]. These two bounds are essential. The filtration is increasing and exhaustive: the cocycle has finite image because G is compact and A discrete, and there are finitely many chosen local primitives. A single exponent kills all of these finitely many elements.

The map Sha¹(G,A_r)->P_r is surjective by construction and the exact coefficient embedding. Injectivity requires a separate condition: whenever a global coboundary da with a in A can locally be represented by db_v with b_v in A[l^r], then da=db for a global b in A[l^r]. The proof extracts a global primitive from a class that maps to zero, applies this condition, and pulls back b along the injection A_r->A. This is not the assertion that Sha¹ commutes with arbitrary direct limits or that the finite-level maps are always injective. The code explicitly takes this as a hypothesis of its cardinality equality.

## 7. Both extra conditions hold eventually, for different reasons in the two twist cases

The crucial proof is [1347–1400](https://github.com/Liangebra/riemann-hypothesis/blob/e61ceacb65699cf48728e7738c843b5ab8a5c1f2/src/Development/AbelianVarietyTheory/Proofs/FoundationProofs/LengthEqLengthInvariantsOfTorsionLevels.lean#L1347-L1400).

Case e=0: A has trivial action. Coboundaries in A are zero, so the required bounded global primitive can be chosen as 0. By the cokernel calculation #C/l^r C=l^r for every r>0, because A_r is trivial cyclic. Since C is finitely generated, C is torsion-free. One can see this by the structure theorem over R; the actual proof [572–627](https://github.com/Liangebra/riemann-hypothesis/blob/e61ceacb65699cf48728e7738c843b5ab8a5c1f2/src/Development/AbelianVarietyTheory/Proofs/FoundationProofs/LengthEqLengthInvariantsOfTorsionLevels.lean#L572-L627) instead compares finite indices of l^k C with the preimage of l^(k+1) C under multiplication by l, proving the l-torsion lies in every l^k C and then applying Krull intersection. Consequently l^r[y]=0 in C implies [y]=0. If f(x)=l^r y then y=f(x_1), giving saturation. This does not cancel l^r in Y, which may have torsion.

Case e != 0: each local invariant group H⁰(G_v,A) is killed by some l^{c_v}. Indeed choose g in the decomposition group such that chi(g)^(-e) != 1, and the action equation says (chi(g)^(-e)-1)a=0 for an invariant a; write that nonzero scalar as a unit times l^{c_v}. The existence of g is proved in [ExistsDecompositionGroupCyclotomicCharacterZpowN.lean:56–105](https://github.com/Liangebra/riemann-hypothesis/blob/e61ceacb65699cf48728e7738c843b5ab8a5c1f2/src/Development/AbelianVarietyTheory/Proofs/FoundationProofs/ExistsDecompositionGroupCyclotomicCharacterZpowN.lean#L56-L105): 1+l has infinite order (integer powers cannot become 1 in characteristic zero); inertia over Q surjects onto the cyclotomic units, and a positive power of the chosen element fixes F by finite index. This works at l=2 as well. The inertia-surjectivity provider remains an independent check.

Take a uniform bound c across S. The local invariant groups are then finite. By the already proved local length formula, Y has finite length, hence is Artinian. Its descending chain l^rY stabilizes; its intersection is zero by finite generation/Krull intersection, so l^m Y=0 for some m. This is the actual [1323–1344](https://github.com/Liangebra/riemann-hypothesis/blob/e61ceacb65699cf48728e7738c843b5ab8a5c1f2/src/Development/AbelianVarietyTheory/Proofs/FoundationProofs/LengthEqLengthInvariantsOfTorsionLevels.lean#L1323-L1344) argument, independent of global Sha duality at the integral layer.

For r>=c+m, the saturation condition follows immediately: f(x)=l^r y=0. For the bounded primitive condition, choose one v in S (nonempty_S proves a prime over l exists). If da locally equals db_v then a-b_v is local invariant, killed by l^c. With l^r b_v=0, we have l^r a=0; take b=a. This explains exactly why the nonempty set of local places matters. There is no circular appeal to global integral duality.

Therefore, for sufficiently large r,

    # K/(K intersect l^r X)
      = # Sha²(G,T_r)
      = # Sha¹(G,A_r)
      = # P_r.

The middle equality and finiteness are exactly the numeric component of finite-level ShaDual. The filtration of K is separated as a submodule filtration induced from X; P_r exhausts B. Apply the general length lemma to obtain length(K)=length(B).

## 8. Consequence and audit verdict

The short exact sequences 0->K->X->im f->0 and 0->im f->Y->C->0 give length(X)+length(C)=length(K)+length(Y). Substitution of the above equalities yields the formula in [Formula.lean:57–95](https://github.com/Liangebra/riemann-hypothesis/blob/e61ceacb65699cf48728e7738c843b5ab8a5c1f2/src/Development/AutomorphicArithmetic/AutomorphicVarieties/GaloisLibrary/GaloisCohomologyLength/Formula.lean#L57-L95), with the global invariant length still expressed by the separate nuGal comparison. Dropping a nonnegative summand gives the upper bound without cancelling infinity.

No discrepancy found in the inspected finite-to-integral length argument. In particular, the previously identified risk of incompatible finite-level duality is resolved for the **length statement** by actual reduction maps, separation, and eventual saturation, rather than by requiring compatible duality isomorphisms. This is also why the numerical ShaDual fragment suffices here; exact trace naturality LF-GC009-NAT-01 is not needed by this consumer. No claim of a new integral perfect pairing.

Residual: inspect the local completion/cyclotomic equivalence provider, inertia-surjectivity provider, compactness of restricted quotient G and actual imported complete-local-ring finite-generation lemma; then trace nuGal and shaInv identifications and the remaining bounds through VWin. No fresh Lean build, no full kernel reachability, no claim of correctness of the complete RH proof.

This work was assisted by ChatGPT.
