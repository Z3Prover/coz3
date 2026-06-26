# .

* SAT 31
* UNSAT 2
* TIMEOUT 26
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_FPLRA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-16740866-files-QF_FPLRA.tar.zst-d
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 6a62a531819d3c2aa21eaaffa8411aea4d9a2f40
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_FPLRA.tar.zst?download=1
Z3 commit message: qsat: decide quantifier-free goals so qe2 returns sat instead of unknown (fixes iss-7027/small-30) (#9970)

## Summary

Fixes the `iss-7027/small-30` snapshot regression (`Z3Prover/bench`
discussion #2705) **at its root**, instead of working around it by
retuning the LP heuristics.

- **Benchmark:** `inputs/issues/iss-7027/small-30.smt2` —
`(check-sat-using qe2)` over a single `(distinct ...)` of 33 mixed
Int/Real terms.
- The recorded oracle was `unknown`; current `master` produces
`timeout`.

## Root cause

`unknown`/`timeout` are both wrong here: the formula is a `distinct`
over 33 terms (free Int/Real constants plus the literals `0`/`1`), which
is **trivially `sat`** — there are infinitely many distinct reals.

The real bug is in the `qsat` tactic that backs `qe2`. Running
quantifier elimination on a **quantifier-free** formula has nothing to
eliminate, so `qsat` left an undecided residual goal and
`check-sat-using` reported `unknown`. This reproduces on any ground
formula with free variables, e.g.:

```
(declare-fun a () Int)(assert (> a 0))(check-sat-using qe2)   ; -> unknown (should be sat)
```

For `small-30` the QE alternation additionally drove `theory_lra`
integer branch-and-bound down a non-terminating path, surfacing as a
`timeout` under the capture budget (the symptom the `random_hammers`
schedule change happened to expose).

## Fix

Under `check-sat` semantics, top-level free variables are implicitly
existentially quantified. So when the `qsat` input has no quantifiers,
decide satisfiability directly (route through the existing `qsat_sat`
path) instead of producing a residual goal. `qe2`/`qe` now return
`sat`/`unsat` for ground formulas.

QE of genuinely-quantified formulas is unchanged: `apply qe2` on a
quantified goal produces the same projected formula as before (verified
identical to `master`). Only the degenerate quantifier-free case is
affected.

This supersedes the previous approach in this PR (reverting the
`lp.random_hammers` default). That default is left **unchanged**
(`true`), preserving #9958's aggregate QF_LIA benefit. `small-30` now
returns `sat` in ~0.01s regardless of the heuristic schedule, because
the QE machinery no longer runs on this ground instance.

Two changes:
- `src/qe/qsat.cpp`: short-circuit quantifier-free input to the
satisfiability decision path.
- `Z3Prover/bench` `inputs/issues/iss-7027/small-30.expected.out`:
oracle updated `unknown` -> `sat` (to be committed alongside this fix).

## Validation

```
$ z3 small-30.smt2
sat            # ~0.01s

$ echo '(declare-fun a () Int)(assert (> a 0))(check-sat-using qe2)' | z3 -in
sat
$ echo '(declare-fun a () Int)(assert (and (> a 0)(< a 0)))(check-sat-using qe2)' | z3 -in
unsat
```

Full unit-test suite (`test-z3 /a`) passes (92/92). Quantified `qe2`
round-trips (`apply qe2`) match `master` byte-for-byte.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|non-incremental/QF_FPLRA/schanda/spark/zeros_consistent_1.smt2 |    0.027s | 20.04MiB| unsat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/Double_div_bad_false-unreach-call.i_1.smt2 |    0.051s | 20.376MiB| unsat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c_3.smt2 |    1.327s | 333.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/Double_div_bad_false-unreach-call.i_0.smt2 |    2.001s | 164.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c.p+cfa-reducer.c_4.smt2 |    2.681s | 265.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0520_true-unreach-call.c_0.smt2 |    3.216s | 200.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0250b_true-unreach-call.c_4.smt2 |    3.996s | 298.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0310_true-unreach-call.c_3.smt2 |    4.141s | 176.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0320_true-unreach-call.c_7.smt2 |    4.715s | 374.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0330a_true-unreach-call.c_6.smt2 |    5.247s | 496.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c.p+cfa-reducer.c_2.smt2 |    5.256s | 396.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Newton_pseudoconstant_true-unreach-call.c_1.smt2 |    5.749s | 423.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0320_true-unreach-call.c_5.smt2 |    6.052s | 496.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0873a_true-unreach-call.c_3.smt2 |    6.461s | 366.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0470_true-unreach-call.c_12.smt2 |    7.038s | 396.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c.p+cfa-reducer.c_1.smt2 |    7.389s | 396.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20230321-UltimateAutomizerSvcomp2023/double_req_bl_0870b.c_1.smt2 |    7.629s | 212.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0260_true-unreach-call.c_1.smt2 |    8.360s | 697.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0270a_true-unreach-call.c_2.smt2 |    8.462s | 696.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0490a_true-unreach-call.c_9.smt2 |    8.519s | 396.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0490a_true-unreach-call.c_0.smt2 |    8.766s | 397.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/float_req_bl_0490a_true-unreach-call.c_10.smt2 |    8.827s | 448.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0460_true-unreach-call.c_2.smt2 |    8.881s | 490.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0330b_true-unreach-call.c_10.smt2 |    8.954s | 499.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/sqrt_Householder_constant_true-unreach-call.c.p+cfa-reducer.c_3.smt2 |    9.860s | 775.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0870b_true-unreach-call.c_3.smt2 |   10.273s | 234.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20170501-Heizmann-UltimateAutomizer/filter2_reinit_true-unreach-call.c_7.smt2 |   11.170s | 173.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20170501-Heizmann-UltimateAutomizer/image_filter_true-unreach-call.c_2.smt2 |   11.448s | 197.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0320_true-unreach-call.c_6.smt2 |   13.716s | 189.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/Rump_double_true-unreach-call_true-termination.c_0.smt2 |   15.676s | 2665.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/cos_polynomial_true-unreach-call_true-termination.c_0.smt2 |   16.620s | 531.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0240a_true-unreach-call.c_6.smt2 |   17.323s | 393.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0270a_true-unreach-call.c_19.smt2 |   18.015s | 1363.0MiB| sat | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0833_true-unreach-call.c_11.smt2 |   20.050s | 410.0MiB| timeout | 0 |  |  |
|non-incremental/QF_FPLRA/20230321-UltimateAutomizerSvcomp2023/cos_polynomial.c_0.smt2 |   20.060s | 466.0MiB| timeout | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0874_true-unreach-call.c_3.smt2 |   20.074s | 225.0MiB| timeout | 0 |  |  |
|non-incremental/QF_FPLRA/20170501-Heizmann-UltimateAutomizer/cos_polynomial_true-unreach-call.c_9.smt2 |   20.105s | 517.0MiB| timeout | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0550b_true-unreach-call.c_9.smt2 |   20.120s | 425.0MiB| timeout | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0490a_true-unreach-call.c_2.smt2 |   20.128s | 493.0MiB| timeout | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0270a_true-unreach-call.c_21.smt2 |   20.137s | 1145.0MiB| timeout | 0 |  |  |
|non-incremental/QF_FPLRA/20230321-UltimateAutomizerSvcomp2023/double_req_bl_0250a.c_1.smt2 |   20.167s | 1092.0MiB| timeout | 0 |  |  |
|non-incremental/QF_FPLRA/2019-Gudemann/lnLaw.smt2            |   20.182s | 1901.0MiB| timeout | 0 |  |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propLnExp2.smt2       |   20.199s | 1889.0MiB| timeout | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0270a_true-unreach-call.c_4.smt2 |   20.202s | 1098.0MiB| timeout | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0260_true-unreach-call.c_3.smt2 |   20.205s | 1092.0MiB| timeout | 0 |  |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propLnExp0.smt2       |   20.211s | 1904.0MiB| timeout | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0490a_true-unreach-call.c_3.smt2 |   20.215s | 1632.0MiB| timeout | 0 |  |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propLnExp1.smt2       |   20.217s | 1903.0MiB| timeout | 0 |  |  |
|non-incremental/QF_FPLRA/20190429-UltimateAutomizerSvcomp2019/double_req_bl_0460_true-unreach-call.c_3.smt2 |   20.217s | 1633.0MiB| timeout | 0 |  |  |
|non-incremental/QF_FPLRA/2019-Gudemann/expLaw.smt2           |   20.231s | 1914.0MiB| timeout | 0 |  |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propLnExp3.smt2       |   20.233s | 1898.0MiB| timeout | 0 |  |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propExpLn2.smt2       |   20.250s | 1898.0MiB| timeout | 0 |  |  |
|non-incremental/QF_FPLRA/2019-Gudemann/expInUnit.smt2        |   20.255s | 1901.0MiB| timeout | 0 |  |  |
|non-incremental/QF_FPLRA/20230321-UltimateAutomizerSvcomp2023/double_req_bl_0490a.c_4.smt2 |   20.256s | 1647.0MiB| timeout | 0 |  |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propExpLn3.smt2       |   20.302s | 2652.0MiB| timeout | 0 |  |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propExpLn0.smt2       |   20.346s | 3682.0MiB| timeout | 0 |  |  |
|non-incremental/QF_FPLRA/2019-Gudemann/propExpLn1.smt2       |   20.347s | 3696.0MiB| timeout | 0 |  |  |
|non-incremental/QF_FPLRA/2019-Gudemann/maxReward.smt2        |   20.869s | 8691.0MiB| timeout | 0 |  |  |
|non-incremental/QF_FPLRA/2019-Gudemann/expInUnit2.smt2       |   21.042s | 10.69GiB| timeout | 0 |  |  |
