# Pilot 03 — uniform constants from Lemma K to cyclotomic tower windows

Snapshot 60cce957f0e89bb4659801e06ccf7970f16c1b44. Static declaration-body inspection and independent reconstruction; no new Lean build. Follows pilot-03-integral-tate-limit.md. Distinguish an absolute exponent in Lemma K from parameters of individual cyclotomic towers and thresholds for a fixed collection of fields.

## 1. The actual bound and its quantifier

[LemmaK.lean:54–63](https://github.com/Liangebra/riemann-hypothesis/blob/60cce957f0e89bb4659801e06ccf7970f16c1b44/src/Development/AutomorphicArithmetic/AutomorphicVarieties/GeneratorCountTuples/LemmaK.lean#L54-L63) defines HasMuQ(l,L) as a primitive l-th root in L for odd l, or a primitive fourth root for l=2. With n=[L:Q], D=|disc L| and integer e>=1, write

    B_C(e,n,D) = exp((2en)^C log(2D)).

LemmaK(C) asserts FINITENESS and log #H²(G_{L,S},Z_l(e+1)) <= B_C(e,n,D), for every prime l, totally complex number field L with HasMuQ, every chosen algebraic closure and LData (S exactly the primes over l), and every e>=1. The logarithm on the LEFT matters: this is not a bound of B_C on the cardinality itself. The asserted existence is outside all of these universal quantifiers. No assertion for e=0, arbitrary number fields or arbitrary S is made.

[LemmaKOfSteps.lean:189–220](https://github.com/Liangebra/riemann-hypothesis/blob/60cce957f0e89bb4659801e06ccf7970f16c1b44/src/Development/AutomorphicArithmetic/AutomorphicVarieties/ModularityArgument/ClassCounting/LemmaKOfSteps.lean#L189-L220) makes the composition explicit: STors -> SNum (along with SFew, SCount, SSha, SLayer) -> ShaK -> SFinal. The upper bound on the integral cohomology length from the preceding audit is used only in SFinal. The much deeper class-counting providers cannot be inferred from that length formula.

## 2. The source actually gives a numerical exponent for the arithmetic bound

[LemmaKOfLitN.lean:400–515](https://github.com/Liangebra/riemann-hypothesis/blob/60cce957f0e89bb4659801e06ccf7970f16c1b44/src/Development/AutomorphicArithmetic/AutomorphicVarieties/ModularityArgument/ThresholdArguments/DenseThresholdCases/LemmaKOfLitN.lean#L400-L515) contains torsAll, which explicitly chooses C0=24 BEFORE introducing L,e,N,m. Its input KT.tors_le supplies finiteness and a log bound in terms of orbit counts and another numerical factor. orbnum (379ff) bounds the orbit count by (2D n N)^(12(m+1)(nN)^3 N). The factor 3^(dd*N) is bounded by exp(2 n N²); exponent_le absorbs the resulting expression

    12(m+1)(nN)^3 N log(2DnN) + 2(m+1)nN² + 2m

into (2en)^24 log(2D), for 1<=N<=4e+6 and m<=2e+1. All the numerical assumptions are given explicitly and the base is positive. STors specializes N=4e+6 at [LemmaKOfLitN.lean:519–523](https://github.com/Liangebra/riemann-hypothesis/blob/60cce957f0e89bb4659801e06ccf7970f16c1b44/src/Development/AutomorphicArithmetic/AutomorphicVarieties/ModularityArgument/ThresholdArguments/DenseThresholdCases/LemmaKOfLitN.lean#L519-L523). The actual arithmetic/geometric proof of KT.tors_le and the orbit-counting provider must still be reviewed; this section establishes the source's explicit exponent choice and final numerical reduction, not the entire underlying class-counting theorem.

[NumThree.lean:314–473](https://github.com/Liangebra/riemann-hypothesis/blob/60cce957f0e89bb4659801e06ccf7970f16c1b44/src/Development/AutomorphicArithmetic/AutomorphicVarieties/ModularityArgument/ClassCounting/NumThree.lean#L314-L473) chooses C2=5(max C0 9 + 2)+11 before introducing l,L,E. [LemmaKOfLitN.lean:907–974](https://github.com/Liangebra/riemann-hypothesis/blob/60cce957f0e89bb4659801e06ccf7970f16c1b44/src/Development/AutomorphicArithmetic/AutomorphicVarieties/ModularityArgument/ThresholdArguments/DenseThresholdCases/LemmaKOfLitN.lean#L907-L974) chooses CK=max C2 0 + 9 before introducing e,n,D. Consequently following these explicit witnesses gives

    C0=24, C1=26, C2=141, CK=150.

This is a witness obtained by reading the numerical constructions, NOT a newly elaborated Lean theorem LemmaK 150. The existing theorem exports an existential; extraction of this exact numeral as a declaration would require refactoring or replaying its witness construction. Other uses can enlarge CK (e.g. max CK 1). These constants are deliberately coarse.

## 3. Why the auxiliary cyclotomic extension does not destroy uniformity

[LemmaKOfLitN.lean:1617–1675](https://github.com/Liangebra/riemann-hypothesis/blob/60cce957f0e89bb4659801e06ccf7970f16c1b44/src/Development/AutomorphicArithmetic/AutomorphicVarieties/ModularityArgument/ThresholdArguments/DenseThresholdCases/LemmaKOfLitN.lean#L1617-L1675) chooses m=ceil(log_l(4E+4)), L'=layF_l(L,m), d=l^m, n'=dn. The ceiling estimate gives 4E+4<=d<=l(4E+4). Since L contains the specified root of unity, l<=n+1 (odd l by cyclotomic degree, l=2 by n>=1). Thus d<=(n+1)(4E+4). L' is totally complex, has maximal root order w=l^c, and w<=2n' by the cyclotomic degree bound. The extension is large enough for the strict inequalities needed in SFew, both 2E+1<w and 2E+1<r_2(L'). No field-dependent choice of an exponent C is made in this step.

[LemmaKOfLitN.lean:1573–1600](https://github.com/Liangebra/riemann-hypothesis/blob/60cce957f0e89bb4659801e06ccf7970f16c1b44/src/Development/AutomorphicArithmetic/AutomorphicVarieties/ModularityArgument/ThresholdArguments/DenseThresholdCases/LemmaKOfLitN.lean#L1573-L1600) proves D'<=D^d N^n' for a cyclotomic extension of root order N, directly from the relative different. If z is the generating integral root and f its relative minimal polynomial, X^N-1=f g. Evaluate the derivative at z and multiply by z: N=f'(z)g(z)z. Since f'(z) belongs to the different ideal, so does N. The absolute norm of the different divides N^n'. The discriminant tower formula gives the bound, using ideal containment in the correct direction. This does not require a monogenic ring of integers. The layer's chosen N=l^(cZero(L)+m) is at most w, hence D'<=D^d w^n'.

[LemmaKOfLitN.lean:1602–1615](https://github.com/Liangebra/riemann-hypothesis/blob/60cce957f0e89bb4659801e06ccf7970f16c1b44/src/Development/AutomorphicArithmetic/AutomorphicVarieties/ModularityArgument/ThresholdArguments/DenseThresholdCases/LemmaKOfLitN.lean#L1602-L1615) shows shaInv(L,l,E)<=shaInv(L',l,E) using the SAME endomorphism g from a cyclotomic descent representation. Its two descriptions are lengths of coker(g-1) and coker(g^(l^m)-1). The factorization g^a-1=(g-1)(1+...+g^(a-1)) gives image containment and the quotient surjection in the correct direction. This is not naive injectivity of restriction on Selmer groups in an extension of l-power degree; such restriction need not be injective. The substantive descent provider exists_descends remains to inspect.

[NumThree.lean:190–286](https://github.com/Liangebra/riemann-hypothesis/blob/60cce957f0e89bb4659801e06ccf7970f16c1b44/src/Development/AutomorphicArithmetic/AutomorphicVarieties/ModularityArgument/ClassCounting/NumThree.lean#L190-L286) independently controls the effect of the field enlargement on the numerical bound. Let x=2En>=2 and H=log(2D). From the degree bounds d<=8x and n'<=4x², get 2En'<=x^5. From w<=2n' and the discriminant estimate obtain

    log(2D') <= log 2 + d log D + n' log w
             <= x^11 H.

For C>=0 this yields B_C(E,n',D')<=B_(5C+11)(E,n,D). In particular it matters that C is nonnegative when bounding the base of the real power. SNum supplies that condition. No omitted discriminant factor or field-dependent constant is required in this numerical descent.

## 4. Counting at the enlarged field and passage back

The proof of SNum instantiates SFew with k=E-1, so 2k+3=2E+1 and 4k+10=4E+6; the layer inequalities give all needed strict bounds. SCount produces a product with (l^nu)^(r_2(L')), and the source cancels that factor only after proving strict positivity. The resulting bound on #EqHom is T1*T2*l^X with T1=tc(L',4E+6,2E+1), T2=tc(L',4E+6,2E), and X=(b+v_l(E))*gens(L')+c+v_l(E). Crucially it is independent of the layer parameter nu and the extension F in the SSha hypothesis. Thus SSha gives finite shaInv(L',l,E)=s and l^s <= T1*T2*l^X. Positivity of the product forces T1,T2 nonzero before logarithms are taken. The finite upper bound also forces shaInv(L,l,E) != top before toNat is used. The source never converts infinity to zero and then uses it as a finite estimate.

The logarithmic expression is at most 2 B_C0(E,n',D') + X log l. gens_le (289ff) and num_poly give X log l <= u^9 log(2D'), u=2En'. num_three absorbs this into B_C1 with C1=max C0 9+2. Then the preceding field descent proves ShaK(C2). This is the actual quantifier discipline in SNum, not an assumption of uniform finite-level isomorphisms. The deeper class-counting and SSha realization steps remain outstanding.

## 5. The final local correction

[LemmaKOfLitN.lean:1030–1097](https://github.com/Liangebra/riemann-hypothesis/blob/60cce957f0e89bb4659801e06ccf7970f16c1b44/src/Development/AutomorphicArithmetic/AutomorphicVarieties/ModularityArgument/ThresholdArguments/DenseThresholdCases/LemmaKOfLitN.lean#L1030-L1097) estimates the local cyclotomic invariants. For a finite-index subgroup U of Z_l^× and e>0, (1+l²)^index(U) belongs to U. The valuation of its e-th power minus 1 equals 2+v_l(index U)+v_l(e), via the square-congruence lifting-the-exponent lemma (including l=2). This bounds nu(l,U,e). For the decomposition-group image at v|l, its index divides the inertia image index, which is at most n by ineOpen. Thus its valuation is at most floor(log_l n), while v_l(e)<=e. Since |S|<=n (cardAbove, 874ff), total local length <= n(2+floor(log_l n)+e).

The inertia index and lifting-the-exponent providers are separate dependencies; their downstream use respects their nonzero-index and e>0 conditions. [ShaLenEqShaInv.lean:110–150](https://github.com/Liangebra/riemann-hypothesis/blob/60cce957f0e89bb4659801e06ccf7970f16c1b44/src/Development/AutomorphicArithmetic/AutomorphicVarieties/GaloisLibrary/GaloisCohomologyLength/ShaLenEqShaInv.lean#L110-L150) identifies the model-dependent Sha length with shaInv by injective inflation to the absolute group and transport between algebraic closures/valuations, using the same cyclotomic coefficient. The equality is obtained by two inequalities for the identity of base fields, not by assuming an arbitrary group isomorphism preserves the local conditions.

[LemmaKOfLitN.lean:1098–1159](https://github.com/Liangebra/riemann-hypothesis/blob/60cce957f0e89bb4659801e06ccf7970f16c1b44/src/Development/AutomorphicArithmetic/AutomorphicVarieties/ModularityArgument/ThresholdArguments/DenseThresholdCases/LemmaKOfLitN.lean#L1098-L1159) adds the local term to s. The finite bound on extended-natural length gives finite length, then sf_card (1008ff) proves by composition factors #H²=l^k. Since l<=n+1, the added logarithmic term is <= n(e+3)log(n+1). The larger explicit polynomial term in numFin absorbs it by taking CK=max C2 0+9. This is uniform and all uses of logarithms have positive arguments. It is a consequence of the audited arithmetic length formula plus the independently substantial ShaK bound.

## 6. Tower bound has a DIFFERENT quantifier

[SecondBound.lean:54–88](https://github.com/Liangebra/riemann-hypothesis/blob/60cce957f0e89bb4659801e06ccf7970f16c1b44/src/Development/AutomorphicArithmetic/AutomorphicVarieties/LayeredIsomorphisms/SecondBound.lean#L54-L88) defines ABound for a tower with generator bound R. It supplies finitely many p-adic centres s_i (at most R) chosen before e,m,B, and if all distances ||s_i-e||>=l^(-B) then

    shaInv(L_m,l,e) <= R' (max(m,jZero(l,R))+c+B),  R'<=R.

The second bound, valid for all e>=1, is

    shaInv(L_m,l,e) <= VLen(D_{jZero},e)+R max(m-jZero,0).

[ABoundOfDescends.lean:733–795](https://github.com/Liangebra/riemann-hypothesis/blob/60cce957f0e89bb4659801e06ccf7970f16c1b44/src/Development/AutomorphicArithmetic/AutomorphicVarieties/AdditiveValuationNorms/ABoundOfDescends.lean#L733-L795) derives these from the twisted endomorphism cokernel growth bound and the determinant estimate at a single level. The centres are selected at that fixed level before all twists/layers, rather than independently for each e. [ABoundOfDescends.lean:994–1036](https://github.com/Liangebra/riemann-hypothesis/blob/60cce957f0e89bb4659801e06ccf7970f16c1b44/src/Development/AutomorphicArithmetic/AutomorphicVarieties/AdditiveValuationNorms/ABoundOfDescends.lean#L994-L1036) transports via Descends, uses a finite generator bound and unipotence, and applies Sha<=V (integral Poitou–Tate length comparison) at the fixed reference field, which is totally complex by the tower hypothesis. No invocation of Lemma K is made at the potentially much larger level m in that proof. The actual determinant/growth and descent providers are next priority.

FixedIn permits R to depend on l and the base abelian field. TwoIn requires a stronger quantifier: for each M choose R BEFORE q,d, then find a base field L0 with ABound(2,R,L0). It is wrong to call R an absolute constant independent of every field. [HVCore.lean:779–852](https://github.com/Liangebra/riemann-hypothesis/blob/60cce957f0e89bb4659801e06ccf7970f16c1b44/src/Development/AutomorphicArithmetic/AutomorphicVarieties/ModularityArgument/ThresholdArguments/DenseThresholdCases/HVCore.lean#L779-L852) actually constructs BConst with the functions R0(M) from TwoIn and Rl(l,A) from FixedIn (using field representatives), and an absolute CK. The window exponent J depends on the fixed field data via R=R0(M)+sum_l Rl(l,L(l)), and on Ct,epsilon. A common x1 bounds the individual finitely many thresholds, then X>=max(2,x1^J) suffices for the chosen windows. The union of exceptional sets has cardinality at most R. The proof selects R0 before q,d; it does not take a maximum over all q,d. The representation-invariance of ABound in partB and the actual provider Two.twoIn (which is the key uniform-in-q,d input), and the final simultaneous-window step are still residual. The existence of BConst is not the existence of a universal numeric window rank.

## Verdict / residual tasks

No mismatch found in the inspected quantifier order, numeric absorption, field degree/discriminant descent, finite-length conversion, or use of tower bounds. The explicit exponent 150 is recoverable at the mathematical witness level from the chosen 24, but the proof of the underlying torsion count and the complete tower uniformity are not yet independently audited. Prioritize (1) KT.tors_le/orbit and SSha providers, (2) Descends and determinant/growth, (3) Two.twoIn with R fixed before q,d, and (4) remaining VWin-to-nonvanishing and kernel reachability. This is not a claim of correctness of the full RH proof or fresh Lean verification.

This work was assisted by ChatGPT.
