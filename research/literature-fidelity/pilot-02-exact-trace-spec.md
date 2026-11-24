# LF-GC009-NAT-01 — exact compact trace: mathematical proof specification

Snapshot 513a2af7e483b2770847d53561e947ec8f970a57. The mathematical strengthening below is reconstructed from the inspected maps and standard local class field theory. It is NOT a new compiled Lean theorem. The existing proof of perfectness needs only multiplication by a unit, so this is an optional naturality improvement, not a defect in the nine-term theorem. Previous notes: pilot-02-mackey-trace.md and pilot-02-kummer-cup.md.

## Statement and notation

Let F be a number field, S a finite set of finite places containing all primes over n, n>0, with n odd or F totally imaginary. Let F_S be the maximal extension unramified outside S at finite places, G=Gal(F_S/F), L/F a finite subextension (not necessarily Galois), U=Gal(F_S/L), and S_L the places of L over S. All n-th roots of unity lie in F_S. Write D=mu_n(F_S), and use the actual coefficient identification with the roots over the extension field.

Assume the degree-three global vanishing and the trace maps supplied by the previously inspected providers. Let e be the actual arithmetic Mackey compact isomorphism from the L-family to the U-Mackey family. The target identity is

    T_F o compactCores o e = T_L

on H_c^3(G_{L,S_L},mu_n), with T_F and T_L the gsCompactInv maps defined by local invariant sums, taking values in Z/n. This is stronger than the unit comparison in BaseLevel.lean. If the traces are compared on the opposite side of e, use its inverse accordingly.

## 1. Ordinary Mackey trace-on-summand identity

For each v choose phi:H=G_{F_v}->G. Fix x representing a double coset U x phi(H) and let H_x=phi^{-1}(x^{-1} U x). The source defines the H_x-action on D by h acting as x phi(h) x^{-1}. Let D_x denote this module; D_v is D with the H-action by phi.

The map

    k_x:D_x -> Res_{H_x} D_v,  a |-> x^{-1} a

is equivariant: x^{-1}(x phi(h) x^{-1}a)=phi(h)x^{-1}a. No assumption that phi is injective is required.

Use the coinduced-function convention f(ug)=u f(g). The summand inclusion I_x sends an H_x-equivariant F:H->D_x to the unique U-equivariant function on G supported on U x phi(H) whose value at x phi(h) is F(h). This is the inclusion defined using mackeyLift. The trace on Coind_U^G Res D is

    Tr_G(f)=sum_{gU in G/U} g f(g^{-1}).

Restrict it to H and evaluate on I_x F. The only potentially nonzero summands have g^{-1} in U x phi(H), equivalently gU in phi(H)x^{-1}U/U. The map

    H/H_x -> phi(H)x^{-1}U/U, h H_x |-> phi(h)x^{-1}U

is a bijection: equality of the two right coset expressions is equivalent to h_2^{-1}h_1 in H_x, and surjectivity follows from the description of the target. Consequently

    Tr_G(I_x F)
      = sum_{h H_x} phi(h) x^{-1} F(h^{-1})
      = Tr_H(Coind(k_x) F).

This is a coefficient-level equality of H-morphisms, not merely a counting argument. Applying cohomology and using the inverse Shapiro map in the definition of corestriction proves, on the d-th summand of the local Mackey isomorphism, that the local trace is ordinary corestriction after k_x. Summing over d gives the full transfer identity. Source ingredients: [MackeyDefs.lean:66–116](https://github.com/Liangebra/riemann-hypothesis/blob/513a2af7e483b2770847d53561e947ec8f970a57/src/GroundTheory/ProfiniteCohomology/MackeyDefs.lean#L66-L116), [MackeyFormula.lean:205–253](https://github.com/Liangebra/riemann-hypothesis/blob/513a2af7e483b2770847d53561e947ec8f970a57/src/GroundTheory/ProfiniteCohomology/MackeyFormula.lean#L205-L253), [ShapiroDefs.lean:94–144](https://github.com/Liangebra/riemann-hypothesis/blob/513a2af7e483b2770847d53561e947ec8f970a57/src/GroundTheory/ProfiniteCohomology/ShapiroDefs.lean#L94-L144), [MackeyFormula/CompactSupport.lean:135–207](https://github.com/Liangebra/riemann-hypothesis/blob/513a2af7e483b2770847d53561e947ec8f970a57/src/GroundTheory/ProfiniteCohomology/MackeyFormula/CompactSupport.lean#L135-L207), [Shapiro.lean:357–365](https://github.com/Liangebra/riemann-hypothesis/blob/513a2af7e483b2770847d53561e947ec8f970a57/src/GroundTheory/ProfiniteCohomology/Shapiro.lean#L357-L365). The new finite coset reindexing equality has not been elaborated.

## 2. Coefficient transport is the inverse-conjugated embedding, not the original embedding

Write iota_v for the chosen embedding of the global algebraic closure into Omega_v. The actual embedding of L at this double coset is j_x(l)=iota_v(x^{-1}l), interpreting x via the restricted field. The source extends j_x to an embedding jhat:L_w->Omega_v and chooses a ring equivalence

    theta:Omega_v -> Omega_w

over L_w via jhat. The subgroup H_x consists of automorphisms fixing j_x(L); equivalently it fixes jhat(L_w), because it fixes the dense subfield and preserves the local valuation. Thus the fixed field Omega_v^{H_x} is jhat(L_w) and the group equivalence to G_{L_w} is sigma |-> theta sigma theta^{-1}. The actual local embedding over L is iota_w=theta o iota_v o lift(x)^{-1}. The lift of x agrees on F_S, so for every zeta in D the equality

    iota_w(zeta) = theta(iota_v(x^{-1} zeta))

holds independently of the lift chosen. The inverse of gsRootsOfUnityIso identifies the same elements in the common maximal restricted field. This identifies the coefficient map k_x from Section 1 exactly with the arithmetic localization maps after theta. There is no free choice of a primitive root or arbitrary scalar in this coefficient comparison.

Source: [BaseChangeMackey.lean:103–179](https://github.com/Liangebra/riemann-hypothesis/blob/513a2af7e483b2770847d53561e947ec8f970a57/src/GroundTheory/ProfiniteCohomology/RestrictedRamification/BaseChangeMackey.lean#L103-L179) (closure equivalence and group conjugation), [235–270](https://github.com/Liangebra/riemann-hypothesis/blob/513a2af7e483b2770847d53561e947ec8f970a57/src/GroundTheory/ProfiniteCohomology/RestrictedRamification/BaseChangeMackey.lean#L235-L270) (embedding and subgroup), [384–467](https://github.com/Liangebra/riemann-hypothesis/blob/513a2af7e483b2770847d53561e947ec8f970a57/src/GroundTheory/ProfiniteCohomology/RestrictedRamification/BaseChangeMackey.lean#L384-L467) (lift, embedding, local group and global map), [BaseChangeModules.lean:175–192](https://github.com/Liangebra/riemann-hypothesis/blob/513a2af7e483b2770847d53561e947ec8f970a57/src/GroundTheory/ProfiniteCohomology/RestrictedRamification/BaseChangeModules.lean#L175-L192), [BaseDefs.lean:140–190](https://github.com/Liangebra/riemann-hypothesis/blob/513a2af7e483b2770847d53561e947ec8f970a57/src/GroundTheory/ProfiniteCohomology/RestrictedRamification/BaseDefs.lean#L140-L190) (roots-to-local-units and invariant), and [BaseTransportGS.lean:444–483](https://github.com/Liangebra/riemann-hypothesis/blob/513a2af7e483b2770847d53561e947ec8f970a57/src/GroundTheory/ProfiniteCohomology/RestrictedRamification/BaseTransportGS.lean#L444-L483).

## 3. Invariance of the normalized Brauer invariant under the field equivalence

A field isomorphism between the two presentations of L_w preserving the local field valuation induces a conjugation isomorphism on the absolute Galois groups and an equivariant units isomorphism. The normalized Brauer invariant is unchanged. To see this without assuming the desired global identity: every local Brauer class is inflated from a finite unramified layer of degree m. The field isomorphism carries that layer to the corresponding degree-m unramified layer, carries the normalized valuation to the normalized valuation (both send a uniformizer to 1), and identifies arithmetic Frobenius by its action on the residue field. The cohomological character class obtained from valuation therefore has the same value on Frobenius. Inflation and the invariant formula for finite layers prove the claim for arbitrary classes. This argument also proves independence of the chosen closure equivalence; it uses the arithmetic Frobenius convention, not a geometric Frobenius sign change.

Compare [Sharifi, Algebraic Number Theory, ch. 9, Definition 9.1.14 and Theorem 9.1.15](https://www.math.ucla.edu/~sharifi/notes/algnum-ch09.html). The actual source [LocalClassFormation/InvariantDefs.lean:61–112](https://github.com/Liangebra/riemann-hypothesis/blob/513a2af7e483b2770847d53561e947ec8f970a57/src/GroundTheory/ProfiniteCohomology/LocalClassFormation/InvariantDefs.lean#L61-L112) defines the unramified layer using a primitive root of order |k|^m-1; the layer has degree m and its invariant uses unramifiedInv. [InvariantSplit.lean:257–310](https://github.com/Liangebra/riemann-hypothesis/blob/513a2af7e483b2770847d53561e947ec8f970a57/src/GroundTheory/ProfiniteCohomology/LocalClassFormation/InvariantSplit.lean#L257-L310) supplies inflation representation, compatibility of the invariant with it, and bijectivity. Thus a plausible Lean lemma can reduce transport of brauerInv to finite unramified levels and comparison of valuation and Frobenius. The transport of induced valuation instances and the unramifiedInv naturality under this ring equivalence have NOT been compiled or located as an existing bundled lemma.

[BaseLocal.lean:214–243](https://github.com/Liangebra/riemann-hypothesis/blob/513a2af7e483b2770847d53561e947ec8f970a57/src/GroundTheory/ProfiniteCohomology/RestrictedRamification/BaseLocal.lean#L214-L243) identifies gsLocalInvRoots with the local roots-of-unity invariant after the actual coefficient map. [LocalTateDuality/InjectiveCohomologyMapKummerSeqOpenFTwo.lean:269–278](https://github.com/Liangebra/riemann-hypothesis/blob/513a2af7e483b2770847d53561e947ec8f970a57/src/GroundTheory/ProfiniteCohomology/LocalTateDuality/InjectiveCohomologyMapKummerSeqOpenFTwo.lean#L269-L278) proves the exact normalized corestriction identity on nested open subgroups of a fixed absolute Galois group. Combining it with the field/roots compatibility above gives, in standard mathematical notation,

    inv_v(cor_{H_x}^H (k_x)_*(e_w xi_w)) = inv_w(xi_w).

Here e_w is the arithmetic local comparison of Section 2. There is no extension-degree factor in this formula. The proof of localInvRootsOfUnity_cores itself uses the already normalized full Brauer invariant; see pilot-02-kummer-cup.md.

## 4. Assemble the compact identity without assuming global duality

Let delta_F and delta_L denote local-to-compact in degree two. By [MackeyFormula/ConeHomotopy.lean:123–139](https://github.com/Liangebra/riemann-hypothesis/blob/513a2af7e483b2770847d53561e947ec8f970a57/src/GroundTheory/ProfiniteCohomology/MackeyFormula/ConeHomotopy.lean#L123-L139), the homotopy-corrected Mackey cone map commutes with its shifted local inclusion, and hence with the cohomology connecting arrow. The homotopy correction is zero on (0,b). Inverse Mackey therefore has the inverse local square. Coefficient trace commutes with local-to-compact by naturality. The arithmetic comparison e uses the same maps on the local components.

Section 1 gives the sum of local corestrictions on the right of the square. Thus for xi in the product of H² at places w over S,

    compactCores(e(delta_L xi))
      = delta_F((sum_{w|v} cor_{w/v} xi_w)_v),

where the right-hand corestrictions include the just-described coefficient/field transports. Apply T_F and inject Z/n into Q/Z. Sections 2–3 give

    i_n T_F(compactCores(e(delta_L xi)))
      = sum_v sum_{w|v} inv_v(cor_{w/v} xi_w)
      = sum_w inv_w(xi_w)
      = i_n T_L(delta_L xi).

Both trace formulas are [BaseDefs.lean:353–382](https://github.com/Liangebra/riemann-hypothesis/blob/513a2af7e483b2770847d53561e947ec8f970a57/src/GroundTheory/ProfiniteCohomology/RestrictedRamification/BaseDefs.lean#L353-L382). By global H³(mu_n)=0 at L, delta_L is onto, and i_n is injective. Therefore the desired equality follows in ordinary mathematics. This proof does not require compact duality, surjectivity of compact corestriction, or inversion of [L:F].

## 5. Suggested Lean implementation order and acceptance criteria

1. Generic trace-on-Mackey-summand morphism: construct k_x, prove the right-coset equivalence explicitly (phi may have a kernel), and reindex the finite trace. Then prove the local cohomological version via Shapiro.
2. Arithmetic comparison: prove the local roots coefficient square using gsLocalEmbeddingOver_apply and gsAutLift_inv_apply; prove invariance of brauerInv under the valued field/closure equivalence and apply localInvRootsOfUnity_cores. Avoid choosing unrelated isomorphisms D≃Z/n.
3. Compact transfer square: reuse inr_mapOfHomotopy and homologyδ_naturality_of_inr and coefficient naturality; obtain exact trace equality by surjectivity of delta_L.
4. Only after elaboration and axiom checks, optionally simplify BaseLevel's proof. Keep the existing valid unit-scaling proof until that point. Include a case with l dividing [L:F], and nonnormal finite L/F when testing the generic construction.

This is a mathematically closed proof specification with standard local field inputs and a source-grounded implementation plan, not evidence that a new Lean declaration has been accepted. No production files changed, and no fresh compilation is claimed.

This work was assisted by ChatGPT.
