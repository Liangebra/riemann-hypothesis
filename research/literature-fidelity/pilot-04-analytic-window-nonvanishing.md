# Pilot 04 — from arithmetic windows to analytic zero exclusion

Snapshot eef8f503a2dc397943799af4bac4cf8b2fbef59d. This note audits the final analytic window criterion and the located arithmetic-to-analytic interfaces, separately from the still unresolved central automorphic/geometric assembly. Static declaration-body inspection only; no fresh Lean build or kernel dependency traversal.

## 1. The exact analytic target

[LandauZeroFreeCriterion.lean:48–74](https://github.com/Liangebra/riemann-hypothesis/blob/eef8f503a2dc397943799af4bac4cf8b2fbef59d/src/Development/AutomorphicArithmetic/DedekindLFunctions/LandauZeroFreeCriterion.lean#L48-L74) defines the real function g(u)=Gm(F,exp u), a zero set Z consisting of zeros of the finite family of L-functions and their conjugate locations in Re(s)>0, and PoleData. Its hypotheses include measurability, |g(u)|<=C0 exp(2u) for u>=0, the Laplace transform Phi for Re(s)>2, holomorphy off Z in Re(s)>0, local discreteness throughout that half-plane, and an actual simple pole of nonzero residue at every rho in Z. For a REAL rho the residue must be strictly positive real.

For fixed 0<theta<1 and delta>0 the desired window condition is: for every eta>0 and U0, there exists U>=U0 and a nonnegative majorant R on [U,(1+delta)U], with g<=R there, integrable weighted majorant and integral R(u)exp(-theta u) <= exp(eta U). This is a ONE-SIDED condition, not an absolute-value bound. [ZeroFreeAllOfTopG.lean:111–175](https://github.com/Liangebra/riemann-hypothesis/blob/eef8f503a2dc397943799af4bac4cf8b2fbef59d/src/Development/AutomorphicArithmetic/CharacterBoxes/ZeroFreeAllOfTopG.lean#L111-L175) proves it from Gm(F,x)<=C x^theta on arbitrarily late intervals [Y,Y^(1+delta)]: take R(x)=|C|x^theta in the x-variable, integrate R(x)x^(-theta-1) to get |C|delta log Y <=Y^eta eventually, and use x=exp u with the Jacobian. Delta is fixed positive; it does not shrink with Y.

## 2. No pole cancellation from the finite weighted family

[ZeroFreeAllOfTopG.lean:923–1025](https://github.com/Liangebra/riemann-hypothesis/blob/eef8f503a2dc397943799af4bac4cf8b2fbef59d/src/Development/AutomorphicArithmetic/CharacterBoxes/ZeroFreeAllOfTopG.lean#L923-L1025) constructs individual Mellin transforms using log derivatives, treating the primitive trivial character of modulus 1 separately by the entire function H(s)=(s-1)zeta(s), extended by H(1)=1. The value H'(1)=Euler's constant is computed using the residue and constant-term limits at 1. This removes the pole at 1 rather than treating zeta as an entire function. Nonprincipal characters use their entire L-functions and nonvanishing at 1. The analytic identities on Re(s)>2 follow from the ramp integrals. These detailed integral and local holomorphic removal providers (mellin_Gchi_modOne, mellin_Gchi_of_ne_one, poleData_abstract) deserve their own line-by-line audit.

At a zero rho of multiplicity m, the constructed transform has simple pole with residue

    -m/(rho(rho-1)).

The exclusions rho=0,1 are essential. [ZeroFreeAllOfTopG.lean:1034–1193](https://github.com/Liangebra/riemann-hypothesis/blob/eef8f503a2dc397943799af4bac4cf8b2fbef59d/src/Development/AutomorphicArithmetic/CharacterBoxes/ZeroFreeAllOfTopG.lean#L1034-L1193) explicitly averages the transforms of chi and chi^-1, with positive weights a_i/2, because Re Gchi=(Gchi+Gchi^-1)/2. It proves the zero set equality using LFunction conjugation, and obtains the total residue

    c_rho = -S_rho/(rho(rho-1)),
    S_rho = sum_i (a_i/2)(m_i(rho)+m'_i(rho)) > 0.

There is no cancellation between simultaneous zeros because every contribution has the same complex factor and a nonnegative real coefficient. If rho is real, 0<rho<1 by the known zero-free region Re>=1, hence c_rho>0. The proof needs this sign in one branch of the final argument. The finite family need not contain conjugate characters already; averaging introduces their transforms. The finite-union isolation argument is explicit. Principal characters with larger modulus are excluded from this family, so the separate Euler-factor reduction at the endpoint still matters.

## 3. The analytic window argument, in conventional mathematics

The key point is not classical Landau's usual global convergence theorem. The source itself notes immediately before landau_window that its window-only criterion is not the statement of Landau's theorem. This note therefore reconstructs its actual argument rather than declaring an exact literature match.

### 3.1 High-order Laplace moments

For fixed sigma>2 define A_k(z)=(-1)^k Phi^(k)(z)/k!, z=sigma+i gamma. By differentiation under the Laplace integral (landau_coeff, [landauWindow.lean:198–246](https://github.com/Liangebra/riemann-hypothesis/blob/eef8f503a2dc397943799af4bac4cf8b2fbef59d/src/Development/AutomorphicArithmetic/DedekindLFunctions/LandauTheorems/landauWindow.lean#L198-L246)),

    A_k(z)= integral_0^infty g(u) u^k exp(-zu)/k! du.

Use the nonnegative kernel w_k(u)=u^k exp(-sigma u)/k!. The elementary inequality

    |A_k(sigma+i gamma)|+Re A_k(sigma)
      <= integral (|g|+g) w_k

is important: it permits one-sided window control even when g is very negative. On the window |g|+g<=2R; outside |g|+g<=2C0 exp(2u). Also

    u^k exp(-(sigma-theta)u)/k! <= (sigma-theta)^(-k)

by the exponential series. Therefore

    |A_k(sigma+i gamma)|+Re A_k(sigma)
      <= 2 (sigma-theta)^(-k) integral_window R exp(-theta u)
         +2C0 integral_outside u^k exp(-(sigma-2)u)/k!.

This is [landauWindow.lean:876–977](https://github.com/Liangebra/riemann-hypothesis/blob/eef8f503a2dc397943799af4bac4cf8b2fbef59d/src/Development/AutomorphicArithmetic/DedekindLFunctions/LandauTheorems/landauWindow.lean#L876-L977). No lower bound on g is required on the window.

### 3.2 Explicit tail concentration (uniform in gamma)

[landauWindow.lean:758–810](https://github.com/Liangebra/riemann-hypothesis/blob/eef8f503a2dc397943799af4bac4cf8b2fbef59d/src/Development/AutomorphicArithmetic/DedekindLFunctions/LandauTheorems/landauWindow.lean#L758-L810) supplies fixed constants depending only on delta: a=2+6/delta, M=a(1+delta), sigma=M(2+a)>2, and k=ceil((1+delta/2)sigma U), so k>=U. To see the Chernoff bounds directly, put b=sigma-2. For the right tail u>V=(1+delta)U, exp(-bu)<=exp(-aV)exp(-(b-a)u); for the left tail u<U use exp(-bu)<=exp(aU)exp(-(b+a)u). Integrating each over all positive u gives

    tail <= exp(-aV)/(b-a)^(k+1) + exp(aU)/(b+a)^(k+1).

The actual proof uses log x<=x-1 for the ratios sigma/(b±a). Its constants give sigma/(b-a)-1=1/(M-1), (1+delta/2)sigma=M(M-1), hence k/(M-1)<=MU+1=aV+1. Also (1+delta/2)(a-2)=a+1 and a(b+a)<=(a+1)sigma, hence k(a-2)/(b+a)>=aU. Thus tail<=C_tail/sigma^k with C_tail=exp(1)/(b-a)+1/(b+a), independent of U,k,gamma. These are the exact bounds used, not an unjustified approximation of the peak of a gamma density.

Together with the window hypothesis and sigma>sigma-theta>0 this gives [landauWindow.lean:1000–1033](https://github.com/Liangebra/riemann-hypothesis/blob/eef8f503a2dc397943799af4bac4cf8b2fbef59d/src/Development/AutomorphicArithmetic/DedekindLFunctions/LandauTheorems/landauWindow.lean#L1000-L1033): there exist sigma>2,C>0 such that for EVERY eta>0 and U0, one can choose U>=U0 and k>=U with

    |A_k(sigma+i gamma)|+Re A_k(sigma)
      <= C exp(eta U)/(sigma-theta)^k

for ALL real gamma. The choice of k precedes gamma; no phase adjustment of the window depending on a pole is needed.

### 3.3 An isolated pole forces exponentially larger moments

For a pole rho and center z, if the disk of radius r contains only that pole and d=|z-rho|<r, remove c/(s-rho) using the local analytic germ. The analytic remainder has coefficients bounded by M/e^k for any d<e<r by Cauchy's estimate. Therefore

    |A_k(z)-c/(z-rho)^(k+1)|<=M/e^k.

The actual provider is [landauWindow.lean:512–590](https://github.com/Liangebra/riemann-hypothesis/blob/eef8f503a2dc397943799af4bac4cf8b2fbef59d/src/Development/AutomorphicArithmetic/DedekindLFunctions/LandauTheorems/landauWindow.lean#L512-L590). The pole-removal proof and Cauchy bounds are separate from the arithmetic argument. The final contradiction is elementary: if 0<d<e<=E=sigma-theta and

    c0/d^k <= M/e^k + C exp(eta U)/E^k

for arbitrarily large U, k>=U, choose 0<eta<log(e/d). Multiply by d^k. The RHS is bounded in absolute value by (|M|+|C|)exp(-(log(e/d)-eta)U), which tends to zero, contradicting c0>0. This is the quantifier order in [landauWindow.lean:980–998](https://github.com/Liangebra/riemann-hypothesis/blob/eef8f503a2dc397943799af4bac4cf8b2fbef59d/src/Development/AutomorphicArithmetic/DedekindLFunctions/LandauTheorems/landauWindow.lean#L980-L998).

### 3.4 Why one cannot simply select an arbitrary nonreal zero

The real-axis term Re A_k(sigma) could be large and negative due to other poles. [landauWindow.lean:250–322](https://github.com/Liangebra/riemann-hypothesis/blob/eef8f503a2dc397943799af4bac4cf8b2fbef59d/src/Development/AutomorphicArithmetic/DedekindLFunctions/LandauTheorems/landauWindow.lean#L250-L322) proves a dichotomy using the finite set of poles in the closed disk centered at sigma of radius E. If there is a zero rho0 with Re rho0>theta, either:

(i) there is a pole-free real-centered disk of radius D<=E and a zero with sigma-Re rho1<D; or
(ii) the nearest pole to sigma is a unique real beta>theta, with a larger disk of radius r<=E containing no other pole.

For the second claim, if no zero has horizontal distance less than the nearest distance d from sigma, every pole at distance d must have real part sigma-d and imaginary part zero; finite discreteness then gives a gap to all others. If the real-centered disk initially has no poles, choose D=E. No rightmost zero is assumed to exist.

In case (i) the nearest-pole selection in [landauWindow.lean:410–501](https://github.com/Liangebra/riemann-hypothesis/blob/eef8f503a2dc397943799af4bac4cf8b2fbef59d/src/Development/AutomorphicArithmetic/DedekindLFunctions/LandauTheorems/landauWindow.lean#L410-L501) gives a possibly shifted center z=sigma+i gamma, a unique pole rho with d<r<D, and Cauchy bounds for both its analytic remainder and the entire real-centered transform on a radius e=(d+r)/2. The pole term gives |c|/d^(k+1), whereas Re A_k(sigma)>=-M'/e^k. This yields the contradiction above. The exact nearest-pole tie breaking argument is a residual detail to inspect independently.

In case (ii), residue positivity at beta gives Re A_k(sigma)>=c_beta/(sigma-beta)^(k+1)-M/e^k. The norm term is nonnegative; the same contradiction follows. Thus there are no poles in Re>theta. [landauWindow.lean:1051–1140](https://github.com/Liangebra/riemann-hypothesis/blob/eef8f503a2dc397943799af4bac4cf8b2fbef59d/src/Development/AutomorphicArithmetic/DedekindLFunctions/LandauTheorems/landauWindow.lean#L1051-L1140) implements both branches. This is a plausible and explicit mathematical window proof, not the false inference that a large value of a complex transform must make its real part large. No discrepancy found in the inspected tail and final comparison arguments.

## 4. Actual arithmetic windows versus analytic windows

[JointWinOfCounts.lean:675–775](https://github.com/Liangebra/riemann-hypothesis/blob/eef8f503a2dc397943799af4bac4cf8b2fbef59d/src/Development/AutomorphicArithmetic/AutomorphicVarieties/HeightAndVanishing/JointWinOfCounts.lean#L675-L775) proves vwinAt_of_joint. Fix psi, eps and admissible exponent function e(x). It chooses t(x)=floor(x^Ct) and uses JointWin and ThmBc with constants selected before X,q,d. It enlarges the common threshold L and exponent J'=max(J,1) to make x>=X^(1/J')>=L at every window point. The direction of J enlargement is correct: X^(1/J')<=X^(1/J) for X>=1. It checks q,d prime and coprime to the fixed conductor, makes their logarithmic product small enough, and transports field assumptions to the actual CM field E. It combines the three bounds via vvle_of_parts, the minus bound, conductor/height estimates and error_terms_le_sqrt_exp_rpow to obtain the required VVle. Specializing Ct=401 gives VWin. The individual conductor, error, minus and counting bounds remain to review; merely proving the simultaneous windows in Pilot 03 was insufficient.

[DivinNOfHypV.lean:51–98](https://github.com/Liangebra/riemann-hypothesis/blob/eef8f503a2dc397943799af4bac4cf8b2fbef59d/src/Development/AutomorphicArithmetic/CharacterBoxes/DivinNOfHypV.lean#L51-L98) shows that VVle actually gives finite Sha lengths for the relevant twists, summability of the nonnegative weighted prime terms vvTerm, and the total bound. It proves finite partial sums bounded by the same B before deriving summability, rather than treating a formally defined infinite sum as automatically convergent. [DivinNOfHypV.lean:162–180](https://github.com/Liangebra/riemann-hypothesis/blob/eef8f503a2dc397943799af4bac4cf8b2fbef59d/src/Development/AutomorphicArithmetic/CharacterBoxes/DivinNOfHypV.lean#L162-L180) uses this with LinkN and SmallLevelRankEq, and a common sufficiently large x threshold where log(x)^2<=x^eps, to prove DIVinN. These are concrete consumers of the arithmetic length estimates. The host/geometry providers of LinkN and SmallLevelRankEq and the actual use of DIVinN in ChainTied are not yet established in this note.

[LevelGmLeOnFamilyScales.lean:1337–1404](https://github.com/Liangebra/riemann-hypothesis/blob/eef8f503a2dc397943799af4bac4cf8b2fbef59d/src/Development/AutomorphicArithmetic/PrimitiveCosets/LevelGmLeOnFamilyScales.lean#L1337-L1404) supplies the crucial comparison once a tied host exists:

    Lambda.Gm(x) <= x^(1/2+(eD+eL+7)eps).

It actually invokes ch_core, ch_top, the explicit formula comparison gmA_eq and sigA_le, with the error exponent CErr bounded by eD+eL+3. It absorbs the remaining fixed factor 1+4 Lambda.o into x^eps after an eventual threshold. The bound holds for sufficiently large x with EAtH and CoreAtH, not every arbitrarily chosen x. The proofs of ch_core/ch_top, explicit formula matching, and the production of the geometric data are the important remaining central bridges.

[LevelGmLeOnFamilyScales.lean:1421–1473](https://github.com/Liangebra/riemann-hypothesis/blob/eef8f503a2dc397943799af4bac4cf8b2fbef59d/src/Development/AutomorphicArithmetic/PrimitiveCosets/LevelGmLeOnFamilyScales.lean#L1421-L1473) extracts CoreAtH from ChainTied and obtains the comparison on a family window, using an actual witness index i. [ZeroFreeAllOfTopG.lean:1255–1323](https://github.com/Liangebra/riemann-hypothesis/blob/eef8f503a2dc397943799af4bac4cf8b2fbef59d/src/Development/AutomorphicArithmetic/CharacterBoxes/ZeroFreeAllOfTopG.lean#L1255-L1323) defines a finite CharFamily from Lambda.KEPT with weight 1 at the primitive trivial character and Lambda.wt at the others; proves all weights positive and admissibility from primitivity, and verifies Gm equality by splitting off the trivial character. This uses the actual prime-ramp equality, not an arbitrary identification of weighted prime sums. It applies the analytic criterion with delta=la-1>0, C=1. [ZeroFreeAllOfTopG.lean:1329–1358](https://github.com/Liangebra/riemann-hypothesis/blob/eef8f503a2dc397943799af4bac4cf8b2fbef59d/src/Development/AutomorphicArithmetic/CharacterBoxes/ZeroFreeAllOfTopG.lean#L1329-L1358) combines family connectedness and host existence in zeroFreeAll_of_topG, invoking zeroFreeOn_of_tiedWin_cyc. The definition of FamilyClassConnected ([DivinNOfHypV.lean:54–56](https://github.com/Liangebra/riemann-hypothesis/blob/eef8f503a2dc397943799af4bac4cf8b2fbef59d/src/Development/AutomorphicArithmetic/CharacterBoxes/DivinNOfHypV.lean#L54-L56)) chooses eD,eL,eps1 BEFORE eps, which is necessary: choosing eps later arbitrarily small makes 1/2+(eD+eL+7)eps less than any fixed sigma>1/2 (with the appropriate theta<1). The actual body of zeroFreeOn_of_tiedWin_cyc remains to locate and inspect.

## 5. The unresolved central connection must not be hidden

At the TOP, [EveryCharacterOfLiteratureOfVWin.lean:1695–1701](https://github.com/Liangebra/riemann-hypothesis/blob/eef8f503a2dc397943799af4bac4cf8b2fbef59d/src/Development/AutomorphicArithmetic/ThetaCorrespondence/ThetaPairLifts/ThirdPlaceSeries/EveryCharacterOfLiteratureOfVWin.lean#L1695-L1701) calls every_character_of_theta_integrals_local_matching, passing hV and several large published-result bundles. This call is verified. At the BOTTOM of the final analytic chain, the conditional zeroFreeAll_of_topG and formB bodies above are verified. However, the body of every_character_of_theta_integrals_local_matching was not located in this snapshot in the present audit; default-branch GitHub code search returned no results. Therefore the link hV -> actual host existence and TopG -> zeroFreeAll -> EveryCharacter remains an explicitly UNRESOLVED source chain, not a proof of complete Main reachability. Likewise, the earlier Greenberg–Wiles field consumption is not closed. Do not infer a necessary dependency merely from an argument being supplied.

The endpoint itself is straightforward: [Main.lean:43–60](https://github.com/Liangebra/riemann-hypothesis/blob/eef8f503a2dc397943799af4bac4cf8b2fbef59d/src/Main.lean#L43-L60) calls everyCharacter and Claim.zeta. [ClaimStrip.lean:53–108](https://github.com/Liangebra/riemann-hypothesis/blob/eef8f503a2dc397943799af4bac4cf8b2fbef59d/src/Development/AutomorphicArithmetic/CharacterBoxes/ClaimStrip.lean#L53-L108) proves that a zero of an imprimitive character in the strip transfers to its primitive character because all extra Euler factors are nonzero for Re>0. For a primitive character the functional equation sends a zero at s to a zero of the inverse character at 1-s, since the gamma factors are nonzero for positive real part. This excludes Re<1/2 given the right-half-plane assertion; Re>1/2 is excluded directly. The zeta specialization separately handles Re>=1 using known nonvanishing, and Re<=0 using the trivial-zero characterization. It does not need any new arithmetic window estimates after EveryCharacter is established.

## 6. Verdict and next tasks

The analytic one-sided window criterion is now substantially source-compared, including residue noncancellation, tail localization, the real-axis sign issue, and exponential contradiction. The actual arithmetic window output, host comparison and endpoint are located. The central geometric/automorphic assembly remains open and is now the highest priority rather than indefinitely expanding standard number theory. Next: find the declaration body of every_character_of_theta_integrals_local_matching (including possible clumped modules), then follow TopG/HostExW and FamilyClassConnected to ChainTied, and independently audit ch_core/ch_top and the prime-sum/explicit formula bridge. Also inspect nearest-pole selection and individual Mellin formulas and the final eps selection. No full RH verification, no new Lean compilation, no production code changes.

This work was assisted by ChatGPT.
