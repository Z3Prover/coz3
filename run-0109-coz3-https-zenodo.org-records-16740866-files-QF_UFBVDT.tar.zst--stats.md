# .

* SAT 7
* UNSAT 3
* TIMEOUT 66
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_UFBVDT.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-16740866-files-QF_UFBVDT.tar.zst-
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 6a62a531819d3c2aa21eaaffa8411aea4d9a2f40
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_UFBVDT.tar.zst?download=1
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
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/44788_1965f0d6d94d5d8054ba_34_QF_UFDTBV.smt2 |    0.055s | 20.388MiB| unsat | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_6_QF_UFDTBV.smt2 |    0.090s | 34.532MiB| sat | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_55_QF_UFDTBV.smt2 |    0.194s | 31.728MiB| unsat | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_40_QF_UFDTBV.smt2 |    0.300s | 60.964MiB| sat | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/39657_2866defdd1f2434b69ab_47_QF_UFDTBV.smt2 |    0.568s | 85.632MiB| sat | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_57_QF_UFDTBV.smt2 |    0.758s | 42.416MiB| unsat | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_58_QF_UFDTBV.smt2 |    0.946s | 136.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/72771_f9d228efc97cf1458e38_64_QF_UFDTBV.smt2 |    1.005s | 89.636MiB| sat | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/39657_2866defdd1f2434b69ab_48_QF_UFDTBV.smt2 |    1.178s | 139.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/41958_45c688a4814eb926c254_60_QF_UFDTBV.smt2 |    1.773s | 221.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/63058_64ab9a7ef7b6c3492507_22_QF_UFDTBV.smt2 |   20.152s | 396.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/44289_4066055e0f64d96da11a_14_QF_UFDTBV.smt2 |   20.211s | 479.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/63058_64ab9a7ef7b6c3492507_23_QF_UFDTBV.smt2 |   20.224s | 450.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/41958_45c688a4814eb926c254_59_QF_UFDTBV.smt2 |   20.247s | 335.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_41_QF_UFDTBV.smt2 |   20.249s | 304.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/44289_4066055e0f64d96da11a_15_QF_UFDTBV.smt2 |   20.291s | 650.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_39_QF_UFDTBV.smt2 |   20.292s | 608.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/39657_1c7158801cd59dc13f05_46_QF_UFDTBV.smt2 |   20.331s | 763.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/44289_e5a2e5c780236919ee6a_17_QF_UFDTBV.smt2 |   20.345s | 537.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/39657_1c7158801cd59dc13f05_44_QF_UFDTBV.smt2 |   20.353s | 772.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/44289_e5a2e5c780236919ee6a_18_QF_UFDTBV.smt2 |   20.363s | 535.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/63058_64ab9a7ef7b6c3492507_24_QF_UFDTBV.smt2 |   20.372s | 515.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/63058_55d6bef5390186355f11_26_QF_UFDTBV.smt2 |   20.410s | 962.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/39657_1c7158801cd59dc13f05_45_QF_UFDTBV.smt2 |   20.483s | 762.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/17512_5c1021b0faa6b6e1791b_20_QF_UFDTBV.smt2 |   20.635s | 1124.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/93493_27ab26d56d60426da02e50231269b6ff_51_QF_UFDTBV.smt2 |   20.755s | 1923.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/41958_32933c5a1384696720a2_62_QF_UFDTBV.smt2 |   20.790s | 1996.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/41958_32933c5a1384696720a2_63_QF_UFDTBV.smt2 |   20.870s | 3664.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/52759_af0c476fe3b544b9a8507f3e42472c43_13_QF_UFDTBV.smt2 |   20.921s | 3728.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_8_QF_UFDTBV.smt2 |   20.958s | 3609.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/3106_afb7bc55417e43d7a22790c3576f04fc_37_QF_UFDTBV.smt2 |   21.026s | 3830.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/17512_5c1021b0faa6b6e1791b_21_QF_UFDTBV.smt2 |   21.030s | 3824.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/44788_1965f0d6d94d5d8054ba_36_QF_UFDTBV.smt2 |   21.094s | 4193.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_7_QF_UFDTBV.smt2 |   21.159s | 3676.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/72658_63104dadde9c6026353f_70_QF_UFDTBV.smt2 |   21.218s | 4474.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_38_QF_UFDTBV.smt2 |   21.227s | 3870.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/17512_5c1021b0faa6b6e1791b_19_QF_UFDTBV.smt2 |   21.284s | 3883.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/41958_32933c5a1384696720a2_61_QF_UFDTBV.smt2 |   21.382s | 7301.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/30078_f817a923328f75af7e60_27_QF_UFDTBV.smt2 |   21.425s | 7517.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/11775_ad46e5b8db4748c51973_43_QF_UFDTBV.smt2 |   21.429s | 7355.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/63058_aa742630eef64f949de269382c1f9035_25_UFDTBV.smt2 |   21.434s | 7723.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_75_QF_UFDTBV.smt2 |   21.445s | 7677.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_68_QF_UFDTBV.smt2 |   21.477s | 7473.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_32_QF_UFDTBV.smt2 |   21.478s | 7868.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_72_QF_UFDTBV.smt2 |   21.480s | 7534.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_2_UFDTBV.smt2 |   21.484s | 7728.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/44788_1965f0d6d94d5d8054ba_35_QF_UFDTBV.smt2 |   21.486s | 7367.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_33_QF_UFDTBV.smt2 |   21.491s | 7554.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_56_QF_UFDTBV.smt2 |   21.491s | 7252.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/52759_bec3a2272267494faeecb6bfaf253e3b_11_QF_UFDTBV.smt2 |   21.492s | 7561.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_30_QF_UFDTBV.smt2 |   21.506s | 7804.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/93493_4ea6163ed03941199c785278ccc42812_49_QF_UFDTBV.smt2 |   21.525s | 7310.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_31_QF_UFDTBV.smt2 |   21.530s | 7524.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_3_UFDTBV.smt2 |   21.533s | 7412.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/72658_63104dadde9c6026353f_71_QF_UFDTBV.smt2 |   21.577s | 5736.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_73_QF_UFDTBV.smt2 |   21.654s | 7938.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/38347_525a1ca0331f2bcbf520_54_QF_UFDTBV.smt2 |   21.697s | 7270.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/30078_f817a923328f75af7e60_28_QF_UFDTBV.smt2 |   21.720s | 7526.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_67_QF_UFDTBV.smt2 |   21.740s | 7447.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_74_QF_UFDTBV.smt2 |   21.746s | 7812.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/52759_af0c476fe3b544b9a8507f3e42472c43_12_QF_UFDTBV.smt2 |   21.760s | 7511.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/44289_b077fc096b3d41cba49f8628caff7fa5_16_QF_UFDTBV.smt2 |   21.763s | 7398.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/93493_5990a6bf5f2740164f77_50_QF_UFDTBV.smt2 |   21.764s | 7343.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_1_UFDTBV.smt2 |   21.776s | 7707.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_65_QF_UFDTBV.smt2 |   21.782s | 7519.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_29_QF_UFDTBV.smt2 |   21.787s | 7701.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/52759_bec3a2272267494faeecb6bfaf253e3b_10_QF_UFDTBV.smt2 |   21.789s | 7471.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_0_UFDTBV.smt2 |   21.798s | 7342.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/11775_ad46e5b8db4748c51973_42_QF_UFDTBV.smt2 |   21.808s | 7278.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_66_QF_UFDTBV.smt2 |   21.825s | 7448.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_69_QF_UFDTBV.smt2 |   21.845s | 7778.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/93493_798593962ee29ad45ac8_52_QF_UFDTBV.smt2 |   21.861s | 8458.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_76_QF_UFDTBV.smt2 |   21.909s | 7086.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_5_QF_UFDTBV.smt2 |   21.966s | 7675.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_4_UFDTBV.smt2 |   21.968s | 7805.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/93493_1fdb6cc8eb9c4363b5838af9eb8c7f1f_53_QF_UFDTBV.smt2 |   21.984s | 7861.0MiB| timeout | 0 |  |  |
