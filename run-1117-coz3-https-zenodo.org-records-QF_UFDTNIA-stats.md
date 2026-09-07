# .

* SAT 39
* UNSAT 11
* TIMEOUT 30
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-09-07 02:13:36 UTC
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_UFDTNIA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-QF_UFDTNIA
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 53621bed781b1f80d49d57b65e13b6ef814e563a
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_UFDTNIA.tar.zst?download=1
Z3 commit message: Add witness_instantiation simplifier and fix TPTP preprocessing pipeline fixpoint (#10679)

## Summary

Adds a new opt-in TPTP preprocessing pass, ptp.witness_instantiation,
that synthesizes a fresh witness constant for any uninterpreted sort
with no ground term anywhere in the problem, and uses it to instantiate
orall-quantified formulas over that sort. This is sound (all sorts are
non-empty in classical logic) and lets search-time E-matching proceed on
axioms that would otherwise never fire for lack of a ground term to
match against (e.g. ANA068^1.p's FINITE_REAL_INTERVAL_1 axiom,
quantified over eal, with no ground eal term anywhere in the problem).

Also fixes a real bug in the TPTP preprocessing pipeline: it was only
flushed once before check_sat, so simplifiers that add new formulas
(lambda_reify_simplifier, leibniz_simplifier,
witness_instantiation_simplifier) never got a second pass to fully
simplify/rewrite those newly-added formulas. Adds an extra push()/pop(1)
flush right before check_sat, gated on any of these three passes being
enabled, guarded by a ry/catch so a latent lambda_reify_simplifier
sort-mismatch bug (which check_sat's own exception handling already
downgrades gracefully to GaveUp) can't escape as a hard crash.

This fixes ANA068^1.p and 3 of its 4 sibling problems with
ptp.reify_lambda_literals=true (previously all timed out).

## Testing

- Full 5,279-file TPTP higher-order regression confirms both
eify_lambda_literals and witness_instantiation have an inherent
net-negative trade-off at full corpus scale (gains on some problems,
timeouts on many others due to extra instantiation noise), consistent
with eify_lambda_literals's pre-existing regression profile
(historically measured at 53 gained / 591 lost even without this fix).
Both remain opt-in, default off; default behavior is unaffected.
- 99/99 unit tests pass (
inja test-z3 + ./test-z3.exe /a).
- Merged latest origin/master into this branch; rebuilt cleanly and
re-verified ANA068^1.p still solves.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

---------

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | INFO |
|------------|----------:|---------:|-------------:| ----------:|------|
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/63058_aa742630eef64f949de269382c1f9035_25_UFDTNIA.smt2 |    0.087s | 24.1MiB| unsat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_6_QF_UFDTNIA.smt2 |    0.094s | 21.892MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_40_QF_UFDTNIA.smt2 |    0.100s | 23.92MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/39657_2866defdd1f2434b69ab_48_QF_UFDTNIA.smt2 |    0.129s | 27.712MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/39657_2866defdd1f2434b69ab_47_QF_UFDTNIA.smt2 |    0.174s | 27.084MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/52759_af0c476fe3b544b9a8507f3e42472c43_12_QF_UFDTNIA.smt2 |    0.240s | 29.168MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_0_UFDTNIA.smt2 |    0.258s | 28.272MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_58_QF_UFDTNIA.smt2 |    0.278s | 28.812MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/41958_45c688a4814eb926c254_60_QF_UFDTNIA.smt2 |    0.293s | 30.456MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/41958_45c688a4814eb926c254_59_QF_UFDTNIA.smt2 |    0.297s | 32.364MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/52759_af0c476fe3b544b9a8507f3e42472c43_13_QF_UFDTNIA.smt2 |    0.359s | 31.272MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/11775_ad46e5b8db4748c51973_42_QF_UFDTNIA.smt2 |    0.722s | 30.448MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_8_QF_UFDTNIA.smt2 |    0.903s | 26.26MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/11775_ad46e5b8db4748c51973_43_QF_UFDTNIA.smt2 |    1.010s | 30.812MiB| unsat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/44289_4066055e0f64d96da11a_14_QF_UFDTNIA.smt2 |    1.013s | 44.54MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/44289_e5a2e5c780236919ee6a_17_QF_UFDTNIA.smt2 |    1.192s | 50.92MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/52759_bec3a2272267494faeecb6bfaf253e3b_11_QF_UFDTNIA.smt2 |    1.258s | 68.12MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_68_QF_UFDTNIA.smt2 |    1.325s | 46.552MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/44289_e5a2e5c780236919ee6a_18_QF_UFDTNIA.smt2 |    1.708s | 52.428MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/63058_64ab9a7ef7b6c3492507_23_QF_UFDTNIA.smt2 |    1.987s | 53.152MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_69_QF_UFDTNIA.smt2 |    2.256s | 43.536MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_2_UFDTNIA.smt2 |    2.293s | 72.34MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/72771_f9d228efc97cf1458e38_64_QF_UFDTNIA.smt2 |    2.697s | 31.42MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_72_QF_UFDTNIA.smt2 |    2.737s | 86.616MiB| unsat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/44289_4066055e0f64d96da11a_15_QF_UFDTNIA.smt2 |    2.750s | 79.188MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/17512_5c1021b0faa6b6e1791b_19_QF_UFDTNIA.smt2 |    2.947s | 25.28MiB| unsat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_56_QF_UFDTNIA.smt2 |    3.235s | 48.38MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/63058_64ab9a7ef7b6c3492507_22_QF_UFDTNIA.smt2 |    3.413s | 47.432MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/39657_1c7158801cd59dc13f05_46_QF_UFDTNIA.smt2 |    3.857s | 61.204MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_73_QF_UFDTNIA.smt2 |    3.883s | 96.832MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_65_QF_UFDTNIA.smt2 |    4.236s | 62.588MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_31_QF_UFDTNIA.smt2 |    4.252s | 64.7MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/63058_64ab9a7ef7b6c3492507_24_QF_UFDTNIA.smt2 |    5.120s | 76.796MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/63058_55d6bef5390186355f11_26_QF_UFDTNIA.smt2 |    5.209s | 63.9MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/17512_5c1021b0faa6b6e1791b_21_QF_UFDTNIA.smt2 |    5.690s | 33.42MiB| unsat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/93493_27ab26d56d60426da02e50231269b6ff_51_QF_UFDTNIA.smt2 |    6.677s | 81.956MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_41_QF_UFDTNIA.smt2 |    6.777s | 41.908MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_76_QF_UFDTNIA.smt2 |    7.013s | 143.0MiB| unsat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/41958_32933c5a1384696720a2_61_QF_UFDTNIA.smt2 |    7.353s | 34.996MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/3106_afb7bc55417e43d7a22790c3576f04fc_37_QF_UFDTNIA.smt2 |    7.667s | 67.092MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/30078_f817a923328f75af7e60_27_QF_UFDTNIA.smt2 |    8.395s | 126.0MiB| unsat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/39657_1c7158801cd59dc13f05_45_QF_UFDTNIA.smt2 |    8.596s | 64.272MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_7_QF_UFDTNIA.smt2 |    9.277s | 28.88MiB| unsat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/44289_b077fc096b3d41cba49f8628caff7fa5_16_QF_UFDTNIA.smt2 |   11.035s | 80.46MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_39_QF_UFDTNIA.smt2 |   11.286s | 73.164MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_29_QF_UFDTNIA.smt2 |   11.543s | 161.0MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/17512_5c1021b0faa6b6e1791b_20_QF_UFDTNIA.smt2 |   11.559s | 38.404MiB| unsat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/30078_f817a923328f75af7e60_28_QF_UFDTNIA.smt2 |   12.911s | 133.0MiB| unsat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/39657_1c7158801cd59dc13f05_44_QF_UFDTNIA.smt2 |   12.937s | 82.756MiB| sat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/52759_bec3a2272267494faeecb6bfaf253e3b_10_QF_UFDTNIA.smt2 |   15.877s | 85.016MiB| unsat | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_1_UFDTNIA.smt2 |   20.013s | 47.184MiB| timeout | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/41958_32933c5a1384696720a2_62_QF_UFDTNIA.smt2 |   20.015s | 29.34MiB| timeout | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_55_QF_UFDTNIA.smt2 |   20.016s | 24.104MiB| timeout | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_5_QF_UFDTNIA.smt2 |   20.017s | 42.06MiB| timeout | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_3_UFDTNIA.smt2 |   20.019s | 45.564MiB| timeout | 0 |  |
|non-incremental/QF_UFDTNIA/20240410-certora/nia-splits/0010.smt2 |   20.022s | 121.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTNIA/20240410-certora/lia-splits/0011.smt2 |   20.026s | 157.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/72658_63104dadde9c6026353f_70_QF_UFDTNIA.smt2 |   20.029s | 30.288MiB| timeout | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_66_QF_UFDTNIA.smt2 |   20.034s | 54.14MiB| timeout | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/93493_798593962ee29ad45ac8_52_QF_UFDTNIA.smt2 |   20.035s | 77.236MiB| timeout | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/93493_1fdb6cc8eb9c4363b5838af9eb8c7f1f_53_QF_UFDTNIA.smt2 |   20.035s | 72.868MiB| timeout | 0 |  |
|non-incremental/QF_UFDTNIA/20240410-certora/nia/0013.smt2    |   20.045s | 144.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_57_QF_UFDTNIA.smt2 |   20.053s | 27.284MiB| timeout | 0 |  |
|non-incremental/QF_UFDTNIA/20240410-certora/lia/0014.smt2    |   20.054s | 230.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/72658_63104dadde9c6026353f_71_QF_UFDTNIA.smt2 |   20.060s | 30.244MiB| timeout | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/44788_1965f0d6d94d5d8054ba_35_QF_UFDTNIA.smt2 |   20.062s | 38.6MiB| timeout | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_75_QF_UFDTNIA.smt2 |   20.062s | 364.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_33_QF_UFDTNIA.smt2 |   20.065s | 197.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/44788_1965f0d6d94d5d8054ba_34_QF_UFDTNIA.smt2 |   20.068s | 48.68MiB| timeout | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/41958_32933c5a1384696720a2_63_QF_UFDTNIA.smt2 |   20.069s | 31.48MiB| timeout | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_67_QF_UFDTNIA.smt2 |   20.070s | 60.02MiB| timeout | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/44788_1965f0d6d94d5d8054ba_36_QF_UFDTNIA.smt2 |   20.070s | 42.288MiB| timeout | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/38347_525a1ca0331f2bcbf520_54_QF_UFDTNIA.smt2 |   20.072s | 133.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_4_UFDTNIA.smt2 |   20.076s | 125.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/93493_4ea6163ed03941199c785278ccc42812_49_QF_UFDTNIA.smt2 |   20.076s | 184.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_74_QF_UFDTNIA.smt2 |   20.076s | 133.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_32_QF_UFDTNIA.smt2 |   20.082s | 205.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_38_QF_UFDTNIA.smt2 |   20.083s | 219.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_30_QF_UFDTNIA.smt2 |   20.084s | 164.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTNIA/20230314-Jaroslav-Bendik-Certora/93493_5990a6bf5f2740164f77_50_QF_UFDTNIA.smt2 |   20.085s | 191.0MiB| timeout | 0 |  |
