# .

* SAT 35
* UNSAT 9
* TIMEOUT 36
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_UFDTNIA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-16740866-files-QF_UFDTNIA.tar.zst
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 6a62a531819d3c2aa21eaaffa8411aea4d9a2f40
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_UFDTNIA.tar.zst?download=1
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
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_6_QF_UFDTNIA.smt2 |    0.048s | 21.896MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/63058_aa742630eef64f949de269382c1f9035_25_UFDTNIA.smt2 |    0.092s | 24.08MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/39657_2866defdd1f2434b69ab_47_QF_UFDTNIA.smt2 |    0.131s | 26.512MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/39657_2866defdd1f2434b69ab_48_QF_UFDTNIA.smt2 |    0.171s | 27.788MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_40_QF_UFDTNIA.smt2 |    0.172s | 23.912MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/41958_45c688a4814eb926c254_60_QF_UFDTNIA.smt2 |    0.201s | 29.796MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/52759_af0c476fe3b544b9a8507f3e42472c43_12_QF_UFDTNIA.smt2 |    0.264s | 29.22MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/41958_45c688a4814eb926c254_59_QF_UFDTNIA.smt2 |    0.267s | 31.844MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/52759_af0c476fe3b544b9a8507f3e42472c43_13_QF_UFDTNIA.smt2 |    0.323s | 30.02MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_8_QF_UFDTNIA.smt2 |    0.597s | 25.388MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/11775_ad46e5b8db4748c51973_42_QF_UFDTNIA.smt2 |    0.600s | 30.052MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_7_QF_UFDTNIA.smt2 |    0.626s | 26.036MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_58_QF_UFDTNIA.smt2 |    0.773s | 29.244MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_0_UFDTNIA.smt2 |    0.971s | 28.352MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/11775_ad46e5b8db4748c51973_43_QF_UFDTNIA.smt2 |    0.980s | 28.872MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/44289_4066055e0f64d96da11a_14_QF_UFDTNIA.smt2 |    1.055s | 44.028MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/44289_e5a2e5c780236919ee6a_17_QF_UFDTNIA.smt2 |    1.268s | 50.888MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/44289_e5a2e5c780236919ee6a_18_QF_UFDTNIA.smt2 |    1.387s | 52.08MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/17512_5c1021b0faa6b6e1791b_19_QF_UFDTNIA.smt2 |    1.638s | 25.048MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_56_QF_UFDTNIA.smt2 |    2.305s | 47.992MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/63058_55d6bef5390186355f11_26_QF_UFDTNIA.smt2 |    2.382s | 52.712MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/44289_4066055e0f64d96da11a_15_QF_UFDTNIA.smt2 |    2.576s | 78.164MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_72_QF_UFDTNIA.smt2 |    2.683s | 85.216MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/72771_f9d228efc97cf1458e38_64_QF_UFDTNIA.smt2 |    2.716s | 31.836MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_31_QF_UFDTNIA.smt2 |    2.861s | 61.756MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/63058_64ab9a7ef7b6c3492507_24_QF_UFDTNIA.smt2 |    3.255s | 75.356MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/63058_64ab9a7ef7b6c3492507_23_QF_UFDTNIA.smt2 |    3.498s | 52.796MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/63058_64ab9a7ef7b6c3492507_22_QF_UFDTNIA.smt2 |    3.847s | 47.664MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_2_UFDTNIA.smt2 |    4.273s | 95.328MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_41_QF_UFDTNIA.smt2 |    4.427s | 38.88MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_76_QF_UFDTNIA.smt2 |    6.955s | 141.0MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/39657_1c7158801cd59dc13f05_46_QF_UFDTNIA.smt2 |    6.968s | 63.016MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/39657_1c7158801cd59dc13f05_45_QF_UFDTNIA.smt2 |    7.129s | 63.456MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/93493_27ab26d56d60426da02e50231269b6ff_51_QF_UFDTNIA.smt2 |    7.154s | 85.468MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/39657_1c7158801cd59dc13f05_44_QF_UFDTNIA.smt2 |    7.529s | 67.772MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/30078_f817a923328f75af7e60_27_QF_UFDTNIA.smt2 |    8.788s | 124.0MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_66_QF_UFDTNIA.smt2 |   10.170s | 45.384MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_74_QF_UFDTNIA.smt2 |   10.346s | 157.0MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_1_UFDTNIA.smt2 |   11.244s | 46.492MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/3106_afb7bc55417e43d7a22790c3576f04fc_37_QF_UFDTNIA.smt2 |   12.016s | 76.284MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/17512_5c1021b0faa6b6e1791b_21_QF_UFDTNIA.smt2 |   12.115s | 36.704MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_39_QF_UFDTNIA.smt2 |   12.172s | 82.712MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/52759_bec3a2272267494faeecb6bfaf253e3b_11_QF_UFDTNIA.smt2 |   14.742s | 92.976MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/30078_f817a923328f75af7e60_28_QF_UFDTNIA.smt2 |   15.493s | 134.0MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/44788_1965f0d6d94d5d8054ba_36_QF_UFDTNIA.smt2 |   20.013s | 35.772MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_57_QF_UFDTNIA.smt2 |   20.013s | 28.536MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_55_QF_UFDTNIA.smt2 |   20.014s | 23.932MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/72658_63104dadde9c6026353f_70_QF_UFDTNIA.smt2 |   20.017s | 55.668MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/44788_1965f0d6d94d5d8054ba_34_QF_UFDTNIA.smt2 |   20.018s | 28.452MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_5_QF_UFDTNIA.smt2 |   20.018s | 43.632MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/17512_5c1021b0faa6b6e1791b_20_QF_UFDTNIA.smt2 |   20.018s | 37.532MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/93493_1fdb6cc8eb9c4363b5838af9eb8c7f1f_53_QF_UFDTNIA.smt2 |   20.020s | 66.788MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/41958_32933c5a1384696720a2_62_QF_UFDTNIA.smt2 |   20.020s | 29.38MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_65_QF_UFDTNIA.smt2 |   20.023s | 61.592MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20240410-certora/lia-splits/0011.smt2 |   20.027s | 162.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/41958_32933c5a1384696720a2_63_QF_UFDTNIA.smt2 |   20.035s | 34.232MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/72658_63104dadde9c6026353f_71_QF_UFDTNIA.smt2 |   20.037s | 26.744MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/41958_32933c5a1384696720a2_61_QF_UFDTNIA.smt2 |   20.039s | 35.128MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_3_UFDTNIA.smt2 |   20.040s | 44.252MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/44289_b077fc096b3d41cba49f8628caff7fa5_16_QF_UFDTNIA.smt2 |   20.043s | 89.824MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/44788_1965f0d6d94d5d8054ba_35_QF_UFDTNIA.smt2 |   20.044s | 33.924MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_69_QF_UFDTNIA.smt2 |   20.045s | 47.892MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/52759_bec3a2272267494faeecb6bfaf253e3b_10_QF_UFDTNIA.smt2 |   20.045s | 86.792MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/93493_798593962ee29ad45ac8_52_QF_UFDTNIA.smt2 |   20.045s | 75.388MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20240410-certora/nia/0013.smt2    |   20.046s | 145.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_68_QF_UFDTNIA.smt2 |   20.046s | 56.468MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_67_QF_UFDTNIA.smt2 |   20.046s | 49.408MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_32_QF_UFDTNIA.smt2 |   20.047s | 209.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_4_UFDTNIA.smt2 |   20.050s | 127.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_29_QF_UFDTNIA.smt2 |   20.050s | 172.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20240410-certora/nia-splits/0010.smt2 |   20.054s | 121.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/38347_525a1ca0331f2bcbf520_54_QF_UFDTNIA.smt2 |   20.054s | 121.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20240410-certora/lia/0014.smt2    |   20.055s | 237.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_73_QF_UFDTNIA.smt2 |   20.057s | 109.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_75_QF_UFDTNIA.smt2 |   20.057s | 129.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_30_QF_UFDTNIA.smt2 |   20.058s | 184.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_38_QF_UFDTNIA.smt2 |   20.061s | 223.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/93493_4ea6163ed03941199c785278ccc42812_49_QF_UFDTNIA.smt2 |   20.061s | 188.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_33_QF_UFDTNIA.smt2 |   20.065s | 179.0MiB| timeout | 0 |  |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/93493_5990a6bf5f2740164f77_50_QF_UFDTNIA.smt2 |   20.067s | 203.0MiB| timeout | 0 |  |  |
