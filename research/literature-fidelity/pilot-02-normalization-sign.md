# GC-009 — Arithmetic Frobenius and the sign of the normalized local invariant

Source snapshot: d99457ad0444896f735cb10998158c7fb53d4b59. This is a continuation of pilot-02-followup, with an independent cochain calculation. Status: conventional derivation and static source inspection, not a newly elaborated Lean bridge.

## 1. Source conventions

* [IsFrob, Statement.lean, lines 72–80](https://github.com/Liangebra/riemann-hypothesis/blob/d99457ad0444896f735cb10998158c7fb53d4b59/src/Formalism/ConjugateSelfDual/Statement.lean#L72-L80): the condition is that sigma(x) - x^q belongs to the maximal ideal for every integral x. Thus the convention is arithmetic Frobenius, not its inverse. normV is the residue-field cardinality.
* [IsNormalised, lines 72–81](https://github.com/Liangebra/riemann-hypothesis/blob/d99457ad0444896f735cb10998158c7fb53d4b59/src/Development/AbelianVarietyTheory/PublishedTheorems/GaloisCohomology%E2%82%82/PoitouTateNineTermTransgressionZeroApply.lean#L72-L81): for n=l^r, y^n=pi, g(y)/y=zeta^kappa(g), and chi(Frob)=1, the prescribed cocycle is kappa(g) chi(h), and the invariant is required to return 1 in Z/n. This is Kummer-first, not character-first.

## 2. Independent calculation of the sign

Work in a nonarchimedean local field K of characteristic zero, with absolute Galois group G, a uniformizer pi, and a primitive n-th root zeta in a separable closure. The coefficient identification Z/n -> mu_n sends a to zeta^a and carries the cyclotomic action. No hypothesis mu_n subset K is needed.

Let chi:G -> Z/n be the unramified character sending arithmetic Frobenius to 1, and b(g) its integer representative in [0,n-1]. Let c(g,h)=(b(g)+b(h)-b(gh))/n. The cocycle c is the connecting image of chi/n under 0 -> Z -> Q -> Q/Z -> 0. The cocycle pi^c has classical Brauer invariant +1/n: this follows by applying the valuation and then evaluating the recovered character at arithmetic Frobenius. See J. S. Milne, [Class Field Theory, Ch. III, §1, “The invariant map”, printed p. 97, and §3, Lemma 3.7](https://www.math.mcgill.ca/darmon/courses/18-19/cft/refs/milne.pdf).

Define a multiplicative 1-cochain t(g)=y^b(g). Its coboundary satisfies

    (dt)(g,h) = g(t(h)) t(gh)^(-1) t(g)
               = zeta^(kappa(g)*b(h)) pi^c(g,h).

Hence the image of the specified Kummer-first cocycle in H^2(G,Ksep^×) is the inverse of the class of pi^c. By the Kummer sequence and Hilbert 90, H^2(G,mu_n) identifies with Br(K)[n]. Consequently

    inv_classical([kappa cup chi]) = -1/n mod Z.

Writing i_n: Z/n -> (1/n)Z/Z, a -> a/n, the required invariant is

    j = - i_n^(-1) o inv_classical.

The proof uses the explicit coboundary rather than a potentially ambiguous convention for the cup-product pairing. For n=2 the sign is invisible, but for n=3 it separates 1 and -1. The calculation also applies to n divisible by the residue characteristic. If chi is initially given only as an abstract homomorphism killing inertia, the standard identification G/I = Zhat implies continuity: n Zhat is open and is in its kernel. This point needs formal transport to the project group.

## 3. Why this does not obstruct nine-term exactness

[The definitions of tr0, tr1, tr2 and NineTermExact](https://github.com/Liangebra/riemann-hypothesis/blob/d99457ad0444896f735cb10998158c7fb53d4b59/src/Development/AbelianVarietyTheory/PublishedTheorems/GaloisCohomology%E2%82%82/PoitouTateNineTermTransgressionZeroApply.lean#L150-L206) show that each tr_i is a sum of evaluations using the same family j_v. Replacing every j_v by -j_v multiplies every tr_i by -1. The ranges and kernels of these maps do not change; surjectivity of tr2 is preserved. The degree-one pairing also has the reversed order (tr1 is dual-first), which only changes its overall sign relative to the other cup order. The original connecting maps delta1 and delta2 still witness exactness. ShaDual is independent of j, and higher vanishing is also independent of j. Orthogonality and perfectness are unaffected by postcomposition with negation. Independent arbitrary sign changes at individual places would not justify this reasoning.

Thus the normalization has a conventional mathematical construction compatible with the classical sequence, conditional on the semantic identifications of groups and cohomology. Do not substitute the positive classical invariant without a sign conversion. This is not a proof-failure claim.

## 4. Residual bridge obligations

* Establish a topological isomorphism from the project's Dec at A to G_(F_v), compatible with inertia, the chosen place, residue Frobenius, and the cyclotomic coefficient action. The inspected IsFrob and Inertia definitions give the expected algebraic residue conditions, not the complete topological bridge.
* Transport the project's H2 quotient, Z2 cocycles and continuous coboundary convention to standard continuous group cohomology. Check the action on mu_n through the fixed primitive root. The sign result here is for the standard inhomogeneous differential and the explicit project cocycle expression.
* Construct the local invariant via the classical Brauer map and prove the required uniform family and normalization. The project uses a single family for all S; the classical construction supplies one map per place, independently of S.
* Verify a reusable Lean lemma that simultaneous negation of all local invariants preserves NineTermExact, using the three transgression definitions. No compilation claimed.
* Independently inspect the actual proof provider and current kernel dependencies. Retain candidate narrowMatch until the transport and formal validation are completed.

This work was assisted by ChatGPT.
