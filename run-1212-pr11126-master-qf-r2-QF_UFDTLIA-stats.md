# .

* SAT 38
* UNSAT 9
* TIMEOUT 29
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-10-15 17:55:48 UTC
Job description: PR11126 baseline QF_UFDTLIA: runner-queue recovery; one suite at a time; pinned master f05a5c651802
Job tag: pr11126-master-qf-r2-QF_UFDTLIA
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: f05a5c6518028169dee82d8bb3108e71a80f3b5a
Z3 branch: f05a5c6518028169dee82d8bb3108e71a80f3b5a
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_UFDTLIA.tar.zst?download=1
Z3 commit message: Add Tseitin/De Morgan QF_UF rewrite matchers (demorgan_and, implies_as_or, eq_not)

Adds op_and/op_or/op_implies interpreted Boolean operators and three new
soundness-proved rewrite matchers to Z3PatternMatchedProofChecker.fst,
closing the dominant QF_UF unmatched-leaf shapes found in the Round 2
revisit (De Morgan and-expansion and eq/not rearrangement from Z3's
Tseitin CNF normalization). Mirrored in extract_z3_proof.py.

Re-ran the 100-file QF_UF benchmark at 30s timeout: matched leaves rose
from 82/167 to 143/167 (49% -> 86%), unmatched leaves dropped from 85 to
24, with all 98 extracted proofs still passing end-to-end (no
regressions). Documented in QF_AX_QF_UF_BENCHMARK_REPORT.md's new
'Round 3' section.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | INFO |
|------------|----------:|---------:|-------------:| ----------:|------|
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_6_QF_UFDTLIA.smt2 |    0.067s | 22.608MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/44788_1965f0d6d94d5d8054ba_34_QF_UFDTLIA.smt2 |    0.099s | 24.552MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_40_QF_UFDTLIA.smt2 |    0.103s | 24.632MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/63058_aa742630eef64f949de269382c1f9035_25_UFDTLIA.smt2 |    0.116s | 26.128MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/52759_af0c476fe3b544b9a8507f3e42472c43_12_QF_UFDTLIA.smt2 |    0.272s | 34.952MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_58_QF_UFDTLIA.smt2 |    0.274s | 29.952MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_7_QF_UFDTLIA.smt2 |    0.274s | 26.588MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/11775_ad46e5b8db4748c51973_43_QF_UFDTLIA.smt2 |    0.290s | 34.824MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_8_QF_UFDTLIA.smt2 |    0.308s | 26.96MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/41958_45c688a4814eb926c254_60_QF_UFDTLIA.smt2 |    0.341s | 35.564MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/52759_af0c476fe3b544b9a8507f3e42472c43_13_QF_UFDTLIA.smt2 |    0.353s | 31.9MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/44788_1965f0d6d94d5d8054ba_35_QF_UFDTLIA.smt2 |    0.495s | 28.312MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/11775_ad46e5b8db4748c51973_42_QF_UFDTLIA.smt2 |    0.509s | 37.456MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/41958_45c688a4814eb926c254_59_QF_UFDTLIA.smt2 |    0.719s | 39.96MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/72658_63104dadde9c6026353f_70_QF_UFDTLIA.smt2 |    0.734s | 56.916MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/44289_4066055e0f64d96da11a_14_QF_UFDTLIA.smt2 |    0.986s | 43.876MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/44289_e5a2e5c780236919ee6a_17_QF_UFDTLIA.smt2 |    0.986s | 49.544MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_41_QF_UFDTLIA.smt2 |    1.030s | 40.312MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/39657_1c7158801cd59dc13f05_45_QF_UFDTLIA.smt2 |    1.091s | 56.272MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/44289_e5a2e5c780236919ee6a_18_QF_UFDTLIA.smt2 |    1.138s | 52.908MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/63058_55d6bef5390186355f11_26_QF_UFDTLIA.smt2 |    1.168s | 50.456MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/39657_1c7158801cd59dc13f05_46_QF_UFDTLIA.smt2 |    1.229s | 55.756MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/39657_2866defdd1f2434b69ab_48_QF_UFDTLIA.smt2 |    1.325s | 31.6MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/41958_32933c5a1384696720a2_63_QF_UFDTLIA.smt2 |    1.452s | 34.5MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/63058_64ab9a7ef7b6c3492507_23_QF_UFDTLIA.smt2 |    1.461s | 51.48MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/63058_64ab9a7ef7b6c3492507_22_QF_UFDTLIA.smt2 |    1.577s | 46.688MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/63058_64ab9a7ef7b6c3492507_24_QF_UFDTLIA.smt2 |    1.647s | 66.996MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/52759_bec3a2272267494faeecb6bfaf253e3b_11_QF_UFDTLIA.smt2 |    1.774s | 73.208MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/72771_f9d228efc97cf1458e38_64_QF_UFDTLIA.smt2 |    1.904s | 31.728MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/39657_1c7158801cd59dc13f05_44_QF_UFDTLIA.smt2 |    2.108s | 56.476MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/44289_4066055e0f64d96da11a_15_QF_UFDTLIA.smt2 |    2.195s | 78.104MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/17512_5c1021b0faa6b6e1791b_21_QF_UFDTLIA.smt2 |    2.439s | 68.776MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_72_QF_UFDTLIA.smt2 |    2.677s | 106.0MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_0_UFDTLIA.smt2 |    3.996s | 93.264MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/41958_32933c5a1384696720a2_62_QF_UFDTLIA.smt2 |    5.656s | 55.9MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/93493_1fdb6cc8eb9c4363b5838af9eb8c7f1f_53_QF_UFDTLIA.smt2 |    6.782s | 85.86MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/93493_27ab26d56d60426da02e50231269b6ff_51_QF_UFDTLIA.smt2 |    6.979s | 72.624MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_75_QF_UFDTLIA.smt2 |    7.120s | 132.0MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/3106_afb7bc55417e43d7a22790c3576f04fc_37_QF_UFDTLIA.smt2 |    7.326s | 88.456MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_76_QF_UFDTLIA.smt2 |    8.005s | 236.0MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/30078_f817a923328f75af7e60_27_QF_UFDTLIA.smt2 |    9.136s | 146.0MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/52759_bec3a2272267494faeecb6bfaf253e3b_10_QF_UFDTLIA.smt2 |   10.688s | 155.0MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/17512_5c1021b0faa6b6e1791b_19_QF_UFDTLIA.smt2 |   12.582s | 94.944MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/41958_32933c5a1384696720a2_61_QF_UFDTLIA.smt2 |   12.742s | 65.94MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_56_QF_UFDTLIA.smt2 |   13.057s | 150.0MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/39657_2866defdd1f2434b69ab_47_QF_UFDTLIA.smt2 |   15.360s | 40.704MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/30078_f817a923328f75af7e60_28_QF_UFDTLIA.smt2 |   18.006s | 154.0MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_55_QF_UFDTLIA.smt2 |   20.027s | 25.068MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_57_QF_UFDTLIA.smt2 |   20.030s | 30.572MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/93493_5990a6bf5f2740164f77_50_QF_UFDTLIA.smt2 |   20.031s | 187.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_39_QF_UFDTLIA.smt2 |   20.032s | 76.328MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_5_QF_UFDTLIA.smt2 |   20.033s | 150.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/93493_798593962ee29ad45ac8_52_QF_UFDTLIA.smt2 |   20.034s | 146.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_31_QF_UFDTLIA.smt2 |   20.035s | 165.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/44289_b077fc096b3d41cba49f8628caff7fa5_16_QF_UFDTLIA.smt2 |   20.036s | 151.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/72658_63104dadde9c6026353f_71_QF_UFDTLIA.smt2 |   20.040s | 192.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_74_QF_UFDTLIA.smt2 |   20.041s | 137.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_2_UFDTLIA.smt2 |   20.045s | 170.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_38_QF_UFDTLIA.smt2 |   20.048s | 213.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_73_QF_UFDTLIA.smt2 |   20.048s | 246.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_29_QF_UFDTLIA.smt2 |   20.050s | 299.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_33_QF_UFDTLIA.smt2 |   20.056s | 269.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/93493_4ea6163ed03941199c785278ccc42812_49_QF_UFDTLIA.smt2 |   20.057s | 182.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_30_QF_UFDTLIA.smt2 |   20.058s | 278.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_32_QF_UFDTLIA.smt2 |   20.058s | 282.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_3_UFDTLIA.smt2 |   20.059s | 246.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/44788_1965f0d6d94d5d8054ba_36_QF_UFDTLIA.smt2 |   20.063s | 382.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_1_UFDTLIA.smt2 |   20.064s | 333.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/17512_5c1021b0faa6b6e1791b_20_QF_UFDTLIA.smt2 |   20.065s | 344.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_69_QF_UFDTLIA.smt2 |   20.076s | 524.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_68_QF_UFDTLIA.smt2 |   20.090s | 552.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_66_QF_UFDTLIA.smt2 |   20.091s | 678.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_4_UFDTLIA.smt2 |   20.091s | 664.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_65_QF_UFDTLIA.smt2 |   20.108s | 969.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_67_QF_UFDTLIA.smt2 |   20.140s | 1347.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/38347_525a1ca0331f2bcbf520_54_QF_UFDTLIA.smt2 |   20.168s | 1640.0MiB| timeout | 0 |  |
