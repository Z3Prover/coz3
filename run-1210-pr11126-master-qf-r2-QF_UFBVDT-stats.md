# .

* SAT 9
* UNSAT 4
* TIMEOUT 63
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-10-15 17:11:22 UTC
Job description: PR11126 baseline QF_UFBVDT: runner-queue recovery; one suite at a time; pinned master f05a5c651802
Job tag: pr11126-master-qf-r2-QF_UFBVDT
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: f05a5c6518028169dee82d8bb3108e71a80f3b5a
Z3 branch: f05a5c6518028169dee82d8bb3108e71a80f3b5a
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_UFBVDT.tar.zst?download=1
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
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/44788_1965f0d6d94d5d8054ba_34_QF_UFDTBV.smt2 |    0.051s | 20.86MiB| unsat | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_6_QF_UFDTBV.smt2 |    0.097s | 35.024MiB| sat | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_55_QF_UFDTBV.smt2 |    0.156s | 31.956MiB| unsat | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_40_QF_UFDTBV.smt2 |    0.268s | 61.404MiB| sat | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/39657_2866defdd1f2434b69ab_47_QF_UFDTBV.smt2 |    0.485s | 84.244MiB| sat | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_57_QF_UFDTBV.smt2 |    0.520s | 42.7MiB| unsat | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/72771_f9d228efc97cf1458e38_64_QF_UFDTBV.smt2 |    0.753s | 89.844MiB| sat | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_58_QF_UFDTBV.smt2 |    0.853s | 146.0MiB| sat | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/39657_2866defdd1f2434b69ab_48_QF_UFDTBV.smt2 |    1.325s | 147.0MiB| sat | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/41958_45c688a4814eb926c254_60_QF_UFDTBV.smt2 |    2.630s | 223.0MiB| sat | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/41958_45c688a4814eb926c254_59_QF_UFDTBV.smt2 |   11.740s | 379.0MiB| sat | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/63058_aa742630eef64f949de269382c1f9035_25_UFDTBV.smt2 |   16.423s | 2476.0MiB| unsat | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_41_QF_UFDTBV.smt2 |   19.873s | 290.0MiB| sat | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/63058_64ab9a7ef7b6c3492507_22_QF_UFDTBV.smt2 |   20.054s | 353.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/63058_64ab9a7ef7b6c3492507_24_QF_UFDTBV.smt2 |   20.065s | 509.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/44289_e5a2e5c780236919ee6a_18_QF_UFDTBV.smt2 |   20.074s | 543.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/44289_4066055e0f64d96da11a_14_QF_UFDTBV.smt2 |   20.077s | 372.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_39_QF_UFDTBV.smt2 |   20.079s | 488.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/44289_e5a2e5c780236919ee6a_17_QF_UFDTBV.smt2 |   20.079s | 532.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/63058_55d6bef5390186355f11_26_QF_UFDTBV.smt2 |   20.087s | 584.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/44289_4066055e0f64d96da11a_15_QF_UFDTBV.smt2 |   20.088s | 618.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/63058_64ab9a7ef7b6c3492507_23_QF_UFDTBV.smt2 |   20.090s | 461.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/39657_1c7158801cd59dc13f05_46_QF_UFDTBV.smt2 |   20.096s | 639.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/93493_27ab26d56d60426da02e50231269b6ff_51_QF_UFDTBV.smt2 |   20.106s | 599.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/39657_1c7158801cd59dc13f05_44_QF_UFDTBV.smt2 |   20.107s | 633.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/39657_1c7158801cd59dc13f05_45_QF_UFDTBV.smt2 |   20.108s | 636.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/17512_5c1021b0faa6b6e1791b_20_QF_UFDTBV.smt2 |   20.128s | 936.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/52759_af0c476fe3b544b9a8507f3e42472c43_13_QF_UFDTBV.smt2 |   20.133s | 1286.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_7_QF_UFDTBV.smt2 |   20.140s | 1199.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/41958_32933c5a1384696720a2_63_QF_UFDTBV.smt2 |   20.146s | 1037.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/41958_32933c5a1384696720a2_62_QF_UFDTBV.smt2 |   20.149s | 935.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_8_QF_UFDTBV.smt2 |   20.157s | 1204.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/17512_5c1021b0faa6b6e1791b_21_QF_UFDTBV.smt2 |   20.187s | 1887.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/3106_afb7bc55417e43d7a22790c3576f04fc_37_QF_UFDTBV.smt2 |   20.189s | 1807.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/17512_5c1021b0faa6b6e1791b_19_QF_UFDTBV.smt2 |   20.200s | 2019.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/41958_32933c5a1384696720a2_61_QF_UFDTBV.smt2 |   20.219s | 1899.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/44788_1965f0d6d94d5d8054ba_36_QF_UFDTBV.smt2 |   20.223s | 2151.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/52759_af0c476fe3b544b9a8507f3e42472c43_12_QF_UFDTBV.smt2 |   20.227s | 2074.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_38_QF_UFDTBV.smt2 |   20.234s | 2041.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/52759_bec3a2272267494faeecb6bfaf253e3b_11_QF_UFDTBV.smt2 |   20.249s | 2286.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/44788_1965f0d6d94d5d8054ba_35_QF_UFDTBV.smt2 |   20.252s | 2371.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/11775_ad46e5b8db4748c51973_43_QF_UFDTBV.smt2 |   20.254s | 2059.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/93493_5990a6bf5f2740164f77_50_QF_UFDTBV.smt2 |   20.303s | 3471.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/11775_ad46e5b8db4748c51973_42_QF_UFDTBV.smt2 |   20.306s | 2458.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_5_QF_UFDTBV.smt2 |   20.338s | 3681.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_76_QF_UFDTBV.smt2 |   20.360s | 3780.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_75_QF_UFDTBV.smt2 |   20.363s | 3641.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/93493_1fdb6cc8eb9c4363b5838af9eb8c7f1f_53_QF_UFDTBV.smt2 |   20.364s | 3643.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/52759_bec3a2272267494faeecb6bfaf253e3b_10_QF_UFDTBV.smt2 |   20.374s | 3539.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_2_UFDTBV.smt2 |   20.381s | 4071.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/38347_525a1ca0331f2bcbf520_54_QF_UFDTBV.smt2 |   20.386s | 4099.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_74_QF_UFDTBV.smt2 |   20.394s | 3637.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/30078_f817a923328f75af7e60_28_QF_UFDTBV.smt2 |   20.417s | 4155.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_56_QF_UFDTBV.smt2 |   20.425s | 4310.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_3_UFDTBV.smt2 |   20.425s | 3912.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_0_UFDTBV.smt2 |   20.426s | 4175.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/72658_63104dadde9c6026353f_70_QF_UFDTBV.smt2 |   20.426s | 4579.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/30078_f817a923328f75af7e60_27_QF_UFDTBV.smt2 |   20.428s | 4189.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_69_QF_UFDTBV.smt2 |   20.429s | 4389.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_72_QF_UFDTBV.smt2 |   20.444s | 4097.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/72658_63104dadde9c6026353f_71_QF_UFDTBV.smt2 |   20.446s | 4644.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_65_QF_UFDTBV.smt2 |   20.448s | 4252.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_73_QF_UFDTBV.smt2 |   20.454s | 4067.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_4_UFDTBV.smt2 |   20.475s | 4949.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_1_UFDTBV.smt2 |   20.479s | 4550.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_31_QF_UFDTBV.smt2 |   20.481s | 4665.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/44289_b077fc096b3d41cba49f8628caff7fa5_16_QF_UFDTBV.smt2 |   20.485s | 4624.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_66_QF_UFDTBV.smt2 |   20.505s | 4577.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_67_QF_UFDTBV.smt2 |   20.522s | 4671.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_68_QF_UFDTBV.smt2 |   20.527s | 4595.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_29_QF_UFDTBV.smt2 |   20.543s | 5489.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_32_QF_UFDTBV.smt2 |   20.562s | 4887.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_30_QF_UFDTBV.smt2 |   20.578s | 6181.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_33_QF_UFDTBV.smt2 |   20.584s | 6120.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/93493_4ea6163ed03941199c785278ccc42812_49_QF_UFDTBV.smt2 |   20.585s | 5530.0MiB| timeout | 0 |  |
|non-incremental/QF_UFBVDT/20230314-Jaroslav-Bendik-Certora/93493_798593962ee29ad45ac8_52_QF_UFDTBV.smt2 |   20.705s | 7207.0MiB| timeout | 0 |  |
