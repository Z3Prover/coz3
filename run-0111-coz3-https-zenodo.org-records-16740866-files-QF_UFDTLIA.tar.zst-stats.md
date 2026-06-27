# .

* SAT 39
* UNSAT 8
* TIMEOUT 29
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_UFDTLIA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-16740866-files-QF_UFDTLIA.tar.zst
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 6a62a531819d3c2aa21eaaffa8411aea4d9a2f40
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_UFDTLIA.tar.zst?download=1
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
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_6_QF_UFDTLIA.smt2 |    0.092s | 22.42MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/63058_aa742630eef64f949de269382c1f9035_25_UFDTLIA.smt2 |    0.100s | 25.372MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/44788_1965f0d6d94d5d8054ba_34_QF_UFDTLIA.smt2 |    0.115s | 24.308MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_40_QF_UFDTLIA.smt2 |    0.188s | 24.588MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_58_QF_UFDTLIA.smt2 |    0.277s | 28.752MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_7_QF_UFDTLIA.smt2 |    0.294s | 26.36MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/11775_ad46e5b8db4748c51973_43_QF_UFDTLIA.smt2 |    0.330s | 35.144MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_8_QF_UFDTLIA.smt2 |    0.355s | 26.904MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/52759_af0c476fe3b544b9a8507f3e42472c43_12_QF_UFDTLIA.smt2 |    0.357s | 34.424MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/41958_45c688a4814eb926c254_60_QF_UFDTLIA.smt2 |    0.366s | 34.78MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/52759_af0c476fe3b544b9a8507f3e42472c43_13_QF_UFDTLIA.smt2 |    0.409s | 31.404MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/44788_1965f0d6d94d5d8054ba_35_QF_UFDTLIA.smt2 |    0.490s | 27.724MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/11775_ad46e5b8db4748c51973_42_QF_UFDTLIA.smt2 |    0.543s | 36.736MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/41958_45c688a4814eb926c254_59_QF_UFDTLIA.smt2 |    0.731s | 39.064MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/72658_63104dadde9c6026353f_70_QF_UFDTLIA.smt2 |    0.812s | 56.46MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_41_QF_UFDTLIA.smt2 |    0.987s | 39.88MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/44289_4066055e0f64d96da11a_14_QF_UFDTLIA.smt2 |    1.086s | 43.58MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/44289_e5a2e5c780236919ee6a_18_QF_UFDTLIA.smt2 |    1.121s | 49.616MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/39657_1c7158801cd59dc13f05_46_QF_UFDTLIA.smt2 |    1.173s | 56.036MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/44289_e5a2e5c780236919ee6a_17_QF_UFDTLIA.smt2 |    1.179s | 51.14MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/39657_1c7158801cd59dc13f05_45_QF_UFDTLIA.smt2 |    1.241s | 56.524MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/39657_1c7158801cd59dc13f05_44_QF_UFDTLIA.smt2 |    1.363s | 53.584MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/63058_55d6bef5390186355f11_26_QF_UFDTLIA.smt2 |    1.458s | 50.28MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/41958_32933c5a1384696720a2_63_QF_UFDTLIA.smt2 |    1.577s | 34.148MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/63058_64ab9a7ef7b6c3492507_23_QF_UFDTLIA.smt2 |    1.633s | 67.508MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/63058_64ab9a7ef7b6c3492507_22_QF_UFDTLIA.smt2 |    1.817s | 46.552MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/44289_4066055e0f64d96da11a_15_QF_UFDTLIA.smt2 |    2.426s | 77.656MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/72771_f9d228efc97cf1458e38_64_QF_UFDTLIA.smt2 |    2.783s | 33.844MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_72_QF_UFDTLIA.smt2 |    2.838s | 104.0MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/17512_5c1021b0faa6b6e1791b_21_QF_UFDTLIA.smt2 |    2.928s | 70.368MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/63058_64ab9a7ef7b6c3492507_24_QF_UFDTLIA.smt2 |    3.046s | 67.004MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/39657_2866defdd1f2434b69ab_48_QF_UFDTLIA.smt2 |    3.381s | 32.84MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/52759_bec3a2272267494faeecb6bfaf253e3b_11_QF_UFDTLIA.smt2 |    5.156s | 81.952MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/17512_5c1021b0faa6b6e1791b_19_QF_UFDTLIA.smt2 |    5.180s | 74.588MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_75_QF_UFDTLIA.smt2 |    5.945s | 126.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/93493_27ab26d56d60426da02e50231269b6ff_51_QF_UFDTLIA.smt2 |    6.406s | 81.724MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/39657_2866defdd1f2434b69ab_47_QF_UFDTLIA.smt2 |    7.287s | 36.54MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/41958_32933c5a1384696720a2_62_QF_UFDTLIA.smt2 |    7.715s | 57.504MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_5_QF_UFDTLIA.smt2 |    7.742s | 100.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/93493_1fdb6cc8eb9c4363b5838af9eb8c7f1f_53_QF_UFDTLIA.smt2 |    8.123s | 84.316MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/3106_afb7bc55417e43d7a22790c3576f04fc_37_QF_UFDTLIA.smt2 |    8.492s | 86.744MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/30078_f817a923328f75af7e60_27_QF_UFDTLIA.smt2 |    8.586s | 140.0MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_76_QF_UFDTLIA.smt2 |    8.638s | 233.0MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_39_QF_UFDTLIA.smt2 |    8.884s | 83.424MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/44289_b077fc096b3d41cba49f8628caff7fa5_16_QF_UFDTLIA.smt2 |   14.218s | 137.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_74_QF_UFDTLIA.smt2 |   14.859s | 141.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/30078_f817a923328f75af7e60_28_QF_UFDTLIA.smt2 |   19.179s | 147.0MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_56_QF_UFDTLIA.smt2 |   20.022s | 159.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_31_QF_UFDTLIA.smt2 |   20.025s | 163.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/41958_32933c5a1384696720a2_61_QF_UFDTLIA.smt2 |   20.025s | 90.824MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_55_QF_UFDTLIA.smt2 |   20.030s | 25.356MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_38_QF_UFDTLIA.smt2 |   20.031s | 259.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_57_QF_UFDTLIA.smt2 |   20.032s | 30.028MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_73_QF_UFDTLIA.smt2 |   20.040s | 239.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/72658_63104dadde9c6026353f_71_QF_UFDTLIA.smt2 |   20.048s | 220.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/44788_1965f0d6d94d5d8054ba_36_QF_UFDTLIA.smt2 |   20.053s | 383.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_0_UFDTLIA.smt2 |   20.058s | 144.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/52759_bec3a2272267494faeecb6bfaf253e3b_10_QF_UFDTLIA.smt2 |   20.058s | 204.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_2_UFDTLIA.smt2 |   20.062s | 205.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_30_QF_UFDTLIA.smt2 |   20.066s | 292.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_68_QF_UFDTLIA.smt2 |   20.068s | 553.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_1_UFDTLIA.smt2 |   20.073s | 330.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_3_UFDTLIA.smt2 |   20.074s | 217.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/93493_798593962ee29ad45ac8_52_QF_UFDTLIA.smt2 |   20.074s | 147.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/17512_5c1021b0faa6b6e1791b_20_QF_UFDTLIA.smt2 |   20.076s | 402.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/93493_4ea6163ed03941199c785278ccc42812_49_QF_UFDTLIA.smt2 |   20.083s | 200.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/93493_5990a6bf5f2740164f77_50_QF_UFDTLIA.smt2 |   20.086s | 188.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_33_QF_UFDTLIA.smt2 |   20.100s | 287.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_29_QF_UFDTLIA.smt2 |   20.102s | 298.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_32_QF_UFDTLIA.smt2 |   20.104s | 291.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_65_QF_UFDTLIA.smt2 |   20.115s | 918.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_69_QF_UFDTLIA.smt2 |   20.119s | 504.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_4_UFDTLIA.smt2 |   20.136s | 653.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_66_QF_UFDTLIA.smt2 |   20.140s | 644.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/38347_525a1ca0331f2bcbf520_54_QF_UFDTLIA.smt2 |   20.152s | 1280.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_67_QF_UFDTLIA.smt2 |   20.162s | 1179.0MiB| timeout | 0 |  |  |
