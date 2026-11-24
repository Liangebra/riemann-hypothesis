# Pilot 02 follow-up — primary references, proof reconstruction and normalization

This note refines the second batch at the same revision a28ad7ec8b9f5ff4ec448fe210d3ac57aacfffe1. No Lean build or new kernel export has been run.

## GC-007: stronger literature source

Milne, Arithmetic Duality Theorems (2006), [I.2.6, p.30, PDF p.38](https://wstein.org/edu/2010/582e/refs/milne-arithmetic_duality_theorems.pdf#page=38), proves exact annihilation for unramified coefficients whose torsion order is prime to the residue characteristic. The finite module D=Hom(M,(F_v^sep)^×) takes values in roots of unity of order dividing l^r. Since l differs from the residue characteristic, these roots lie in the maximal unramified extension and are units. Consequently Milne's M^d=Hom(M,(O_v^ur)^×) agrees with M^D in this setting. Inflation-restriction converts the quotient-group H^1 on both sides into the kernel of restriction to inertia, which is the project's definition of H1ur. This removes reliance on a lecture-note source for the basic orthogonality theorem; the identification of the project decomposition group and its inertia with the local field group remains to be checked.

## GC-006: conventional proof from Poitou–Tate, and correspondence with the Lean proof

For clarity the following derivation uses finite groups, additive notation, and cardinalities. Let D=M^D, H=H^1(G_S,M), H'=H^1(G_S,D), V=product_{v in S} H^1(F_v,M), and V' the corresponding product for D. Let f:H→V and g:H'→V' be localization. Let A=im(f), B=im(g), and L=product L_v. The sum of local Tate pairings is perfect. Poitou–Tate exactness gives B=A^perp (using both exactness and the symmetry of the dual pairing).

The Selmer sizes are |ker f| |A∩L| and |ker g| |B∩L^perp|. As B∩L^perp=(A+L)^perp, perfectness gives |B∩L^perp|=|V|/|A+L|. Also |A+L| |A∩L|=|A||L|. Hence

    |Sel(M,L)| / |Sel(D,L^perp)| = |H| |L| / (|ker g| |V|).       (1)

Poitou–Tate duality gives |ker g|=|Sha^2(M)|. The tail of its exact sequence yields

    |H^2(G_S,M)| |H^0(G_S,D)| = |Sha^2(M)| product_v |H^2(F_v,M)|. (2)

Use the classical global Euler characteristic (Milne I.5.1), in the form
    |H| archH0Card = |H^0(G_S,M)| |H^2(G_S,M)| m^[F:Q].
Use the local Euler characteristic (I.2.8), multiplying over v in S:
    |V| = (product_v |H^0(F_v,M)|)(product_v |H^2(F_v,M)|) m^[F:Q].
Here sum_{v|l}[F_v:Q_l]=[F:Q]; at the remaining finite places l is invertible and the local Euler characteristic is one.

Substitute these expressions into (1) and (2) and cancel the nonzero cardinalities. The resulting identity is exactly GreenbergWilesAt in the project. This establishes a general-number-field derivation from the published Milne theorems, independently of the lecture notes; Wiles Prop. 1.6 remains the historical Q-case source.

The body of [greenbergWiles_of_poitouTate](https://github.com/leanprover-community/riemann-hypothesis/blob/a28ad7ec8b9f5ff4ec448fe210d3ac57aacfffe1/src/Development/AutomorphicArithmetic/ThetaCorrespondence/GaloisRepresentations/GreenbergWiles.lean#L805-L865) was now read. It follows the same mathematical structure:
- poitouTate_instance supplies the normalized exact sequence and Sha duality;
- locPerfect and sum_perfect supply the perfect product pairing;
- range_localization_eq_annihilator and card_comap_mul_card_ker_mul_card_eq implement the finite-group calculation in (1);
- card_sha2_mul_card_local_h2 supplies the tail count (2);
- hGEC supplies the global Euler characteristic;
- sum_locDeg_eq_finrank and localEuler_places combine the finite local Euler characteristics;
- arith combines the cardinality identities.

This is a source-level correspondence between the conventional proof and the actual provider, not a newly compiled formal proof. In particular, the provider is not merely a named hypothesis returned unchanged.

## GC-009: normalized Poitou–Tate

[PoitouTateNormalised](https://github.com/leanprover-community/riemann-hypothesis/blob/a28ad7ec8b9f5ff4ec448fe210d3ac57aacfffe1/src/Development/AbelianVarietyTheory/PublishedTheorems/GaloisCohomology₂/PoitouTateNormalised.lean#L54-L71) restricts to l-primary coefficients, r>0, and l≠2 or F totally complex. This last condition removes real-place positive-degree cohomology and modified local terms in the exact sequence. The proposition quantifies a primitive l^r-th root ζ and supplies a common family of local invariant maps for all admissible choices of S, local places, coefficient modules and actions.

The actual definitions in [PoitouTateNineTermTransgressionZeroApply.lean](https://github.com/leanprover-community/riemann-hypothesis/blob/a28ad7ec8b9f5ff4ec448fe210d3ac57aacfffe1/src/Development/AbelianVarietyTheory/PublishedTheorems/GaloisCohomology₂/PoitouTateNineTermTransgressionZeroApply.lean#L194-L220) were inspected. NineTermExact has precisely the nine nonzero-position terms of Milne [I.4.10](https://wstein.org/edu/2010/582e/refs/milne-arithmetic_duality_theorems.pdf#page=65), with finite local products and duals Hom_R(-,R). For an l^r-torsion finite group its Pontryagin dual can be identified with this R-linear dual after identifying the l^r-torsion of Q/Z with R. The localization maps are explicit restrictions and the three transgression maps are sums of local cup-product evaluations. The two connecting maps are asserted to exist, not independently identified as the specific classical maps. ShaDual states finite orders and the existence of linear isomorphisms to the corresponding duals in both directions; it does not assert the canonical global pairing of Milne. Higher cohomological vanishing follows from I.4.10(c) under the stated archimedean restriction.

The normalization must be assessed separately. [IsNormalised](https://github.com/leanprover-community/riemann-hypothesis/blob/a28ad7ec8b9f5ff4ec448fe210d3ac57aacfffe1/src/Development/AbelianVarietyTheory/PublishedTheorems/GaloisCohomology₂/PoitouTateNineTermTransgressionZeroApply.lean#L72-L81) asks the local invariant to send a cocycle of the form κ(g)χ(h) to 1 in Z/l^r, where κ encodes a chosen root of a uniformizer and χ is an unramified additive character with specified Frobenius value. The order of the Kummer factor and unramified character matters: swapping degree-one cup factors may change the sign after identifying the coefficient modules. To validate the normalized statement against the classical invariant one must check the exact arithmetic/geometric Frobenius convention and cup-product convention. A simultaneous sign change of all local invariant maps preserves exactness of the global sequence, but does not by itself validate the designated normalization. We therefore record a provisional narrowMatch, not an exactMatch or an allegation of error. The proof provider is PositivityTheory.Lit1.PTN.poitouTateNormalised_holds, referenced by the literature assembly.

This work was assisted by ChatGPT.
