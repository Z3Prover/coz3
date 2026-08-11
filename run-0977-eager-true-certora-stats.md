# .

* SAT 104
* UNSAT 23
* TIMEOUT 181
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: arith.nl.optimize_bounds_eager=true on certora, branch nla-avoid-grobner-horner (8195d4f77)
Job tag: eager-true-certora
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 8195d4f771ca40f94dd4635c0ea6c9f296144a05
Z3 branch: nla-avoid-grobner-horner
Z3 options: "-T:20 model_validate=true smt.arith.solver=6 smt.arith.nl.optimize_bounds_eager=true"
Z3 inputs: inputs/certora
Z3 commit message: Default the eager bounds mode to on

tr.sh (fstar corpus, seed 33355) had solver=6 averaging 0.28s against
0.07s for solver=2, with the time going to Horner cross-nesting, Grobner
saturation, bounded nlsat and monomial patching on the UInt128/Matrix
families. With the eager mode on the corpus average drops to 0.12s with
identical results, and the 10-check budget contains the QF_NIA exposure:
on a 400-file random QF_NIA sample the flip solves 304 vs 301 with a
0.95 penalized time ratio, and a 300-file QF_NRA sample is unchanged
(265 solved both ways, no flips).

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | INFO |
|------------|----------:|---------:|-------------:| ----------:|------|
|65782_cd31513fdcd15701933b_6_QF_UFDTNIA.smt2                 |    0.066s | 21.992MiB| sat | 0 |  |
|65782_cd31513fdcd15701933b_6_QF_UFLIA.smt2                   |    0.092s | 22.464MiB| sat | 0 |  |
|65782_cd31513fdcd15701933b_6_QF_UFDTLIA.smt2                 |    0.099s | 22.488MiB| sat | 0 |  |
|3106_1c933134166dbad31f79_40_QF_UFDTNIA.smt2                 |    0.112s | 24.292MiB| sat | 0 |  |
|63058_aa742630eef64f949de269382c1f9035_25_UFLIA.smt2         |    0.136s | 24.768MiB| unsat | 0 |  |
|39657_2866defdd1f2434b69ab_48_QF_UFDTNIA.smt2                |    0.164s | 27.624MiB| sat | 0 |  |
|63058_aa742630eef64f949de269382c1f9035_25_UFDTNIA.smt2       |    0.169s | 24.28MiB| unsat | 0 |  |
|63058_aa742630eef64f949de269382c1f9035_25_UFDTLIA.smt2       |    0.185s | 25.64MiB| unsat | 0 |  |
|63058_aa742630eef64f949de269382c1f9035_25_UFNIA.smt2         |    0.199s | 24.5MiB| unsat | 0 |  |
|44788_1965f0d6d94d5d8054ba_34_QF_UFDTLIA.smt2                |    0.208s | 24.22MiB| sat | 0 |  |
|3106_1c933134166dbad31f79_40_QF_UFDTLIA.smt2                 |    0.213s | 24.548MiB| sat | 0 |  |
|65782_cd31513fdcd15701933b_7_QF_UFLIA.smt2                   |    0.221s | 26.9MiB| sat | 0 |  |
|3106_1c933134166dbad31f79_40_QF_UFLIA.smt2                   |    0.244s | 25.564MiB| sat | 0 |  |
|38347_092cc73601c78e45f4f9_58_QF_UFDTLIA.smt2                |    0.253s | 29.616MiB| sat | 0 |  |
|65782_cd31513fdcd15701933b_6_QF_UFNIA.smt2                   |    0.271s | 22.052MiB| sat | 0 |  |
|44788_1965f0d6d94d5d8054ba_34_QF_UFLIA.smt2                  |    0.280s | 26.648MiB| sat | 0 |  |
|39657_2866defdd1f2434b69ab_47_QF_UFDTNIA.smt2                |    0.284s | 27.224MiB| sat | 0 |  |
|38347_092cc73601c78e45f4f9_58_QF_UFDTNIA.smt2                |    0.340s | 28.952MiB| sat | 0 |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_13_QF_UFDTLIA.smt2    |    0.385s | 31.66MiB| sat | 0 |  |
|11775_ad46e5b8db4748c51973_43_QF_UFDTLIA.smt2                |    0.406s | 34.476MiB| sat | 0 |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_12_QF_UFDTNIA.smt2    |    0.408s | 29.324MiB| sat | 0 |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_13_QF_UFDTNIA.smt2    |    0.445s | 31.896MiB| sat | 0 |  |
|41958_45c688a4814eb926c254_59_QF_UFDTNIA.smt2                |    0.502s | 32.46MiB| sat | 0 |  |
|44788_1965f0d6d94d5d8054ba_35_QF_UFLIA.smt2                  |    0.537s | 37.272MiB| unsat | 0 |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_12_QF_UFDTLIA.smt2    |    0.555s | 34.496MiB| sat | 0 |  |
|65782_cd31513fdcd15701933b_7_QF_UFDTLIA.smt2                 |    0.587s | 26.444MiB| sat | 0 |  |
|41958_45c688a4814eb926c254_60_QF_UFDTNIA.smt2                |    0.593s | 30.436MiB| sat | 0 |  |
|65782_cd31513fdcd15701933b_8_QF_UFLIA.smt2                   |    0.627s | 27.908MiB| sat | 0 |  |
|65782_cd31513fdcd15701933b_8_QF_UFDTLIA.smt2                 |    0.652s | 26.844MiB| sat | 0 |  |
|38347_092cc73601c78e45f4f9_58_QF_UFNIA.smt2                  |    0.799s | 31.692MiB| sat | 0 |  |
|41958_45c688a4814eb926c254_60_QF_UFDTLIA.smt2                |    0.834s | 34.868MiB| sat | 0 |  |
|44788_1965f0d6d94d5d8054ba_35_QF_UFDTLIA.smt2                |    0.903s | 27.884MiB| unsat | 0 |  |
|65782_cd31513fdcd15701933b_8_QF_UFNIA.smt2                   |    0.927s | 26.04MiB| sat | 0 |  |
|11775_ad46e5b8db4748c51973_43_QF_UFNIA.smt2                  |    0.949s | 28.548MiB| unsat | 0 |  |
|39657_2866defdd1f2434b69ab_48_QF_UFNIA.smt2                  |    0.965s | 39.06MiB| sat | 0 |  |
|83314_a702bf8b823398c9e37a_0_UFNIA.smt2                      |    1.048s | 26.956MiB| sat | 0 |  |
|11775_ad46e5b8db4748c51973_42_QF_UFDTLIA.smt2                |    1.100s | 37.084MiB| sat | 0 |  |
|11775_ad46e5b8db4748c51973_43_QF_UFLIA.smt2                  |    1.107s | 35.628MiB| sat | 0 |  |
|83314_a702bf8b823398c9e37a_0_UFDTNIA.smt2                    |    1.154s | 28.38MiB| sat | 0 |  |
|38347_092cc73601c78e45f4f9_58_QF_UFLIA.smt2                  |    1.169s | 32.144MiB| sat | 0 |  |
|41958_45c688a4814eb926c254_59_QF_UFDTLIA.smt2                |    1.259s | 40.248MiB| sat | 0 |  |
|72658_63104dadde9c6026353f_70_QF_UFDTLIA.smt2                |    1.363s | 56.92MiB| sat | 0 |  |
|11775_ad46e5b8db4748c51973_42_QF_UFLIA.smt2                  |    1.364s | 40.616MiB| sat | 0 |  |
|11775_ad46e5b8db4748c51973_42_QF_UFDTNIA.smt2                |    1.379s | 30.292MiB| sat | 0 |  |
|3106_1c933134166dbad31f79_40_QF_UFNIA.smt2                   |    1.394s | 25.256MiB| sat | 0 |  |
|3106_1c933134166dbad31f79_41_QF_UFDTLIA.smt2                 |    1.419s | 40.06MiB| sat | 0 |  |
|11775_ad46e5b8db4748c51973_42_QF_UFNIA.smt2                  |    1.489s | 33.752MiB| sat | 0 |  |
|11775_ad46e5b8db4748c51973_43_QF_UFDTNIA.smt2                |    1.593s | 29.896MiB| unsat | 0 |  |
|41958_45c688a4814eb926c254_60_QF_UFNIA.smt2                  |    1.632s | 50.592MiB| sat | 0 |  |
|44289_e5a2e5c780236919ee6a_17_QF_UFDTLIA.smt2                |    1.728s | 49.848MiB| sat | 0 |  |
|44289_4066055e0f64d96da11a_14_QF_UFDTLIA.smt2                |    1.797s | 44.292MiB| sat | 0 |  |
|44289_4066055e0f64d96da11a_14_QF_UFDTNIA.smt2                |    1.865s | 44.844MiB| sat | 0 |  |
|39657_1c7158801cd59dc13f05_45_QF_UFDTLIA.smt2                |    2.195s | 56.144MiB| sat | 0 |  |
|44289_e5a2e5c780236919ee6a_17_QF_UFDTNIA.smt2                |    2.257s | 51.232MiB| sat | 0 |  |
|63058_55d6bef5390186355f11_26_QF_UFDTLIA.smt2                |    2.302s | 50.768MiB| sat | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_69_QF_UFNIA.smt2      |    2.316s | 41.26MiB| sat | 0 |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_11_QF_UFDTNIA.smt2    |    2.326s | 68.224MiB| sat | 0 |  |
|65782_cd31513fdcd15701933b_7_QF_UFNIA.smt2                   |    2.378s | 27.044MiB| unsat | 0 |  |
|39657_1c7158801cd59dc13f05_44_QF_UFDTLIA.smt2                |    2.389s | 53.444MiB| sat | 0 |  |
|39657_1c7158801cd59dc13f05_46_QF_UFDTLIA.smt2                |    2.455s | 52.664MiB| sat | 0 |  |
|44289_e5a2e5c780236919ee6a_18_QF_UFDTLIA.smt2                |    2.532s | 52.132MiB| sat | 0 |  |
|65782_cd31513fdcd15701933b_8_QF_UFDTNIA.smt2                 |    2.600s | 26.644MiB| sat | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_66_QF_UFNIA.smt2      |    2.734s | 42.264MiB| sat | 0 |  |
|41958_32933c5a1384696720a2_63_QF_UFDTLIA.smt2                |    2.857s | 34.248MiB| sat | 0 |  |
|17512_5c1021b0faa6b6e1791b_21_QF_UFLIA.smt2                  |    3.034s | 67.608MiB| unsat | 0 |  |
|63058_64ab9a7ef7b6c3492507_23_QF_UFDTLIA.smt2                |    3.093s | 51.28MiB| sat | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_68_QF_UFDTNIA.smt2    |    3.410s | 46.856MiB| sat | 0 |  |
|65782_cd31513fdcd15701933b_7_QF_UFDTNIA.smt2                 |    3.514s | 27.46MiB| unsat | 0 |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_11_QF_UFDTLIA.smt2    |    3.624s | 71.216MiB| sat | 0 |  |
|44289_e5a2e5c780236919ee6a_18_QF_UFDTNIA.smt2                |    3.628s | 52.788MiB| sat | 0 |  |
|63058_64ab9a7ef7b6c3492507_22_QF_UFDTLIA.smt2                |    3.711s | 46.94MiB| sat | 0 |  |
|41958_32933c5a1384696720a2_63_QF_UFLIA.smt2                  |    3.957s | 35.324MiB| sat | 0 |  |
|17512_5c1021b0faa6b6e1791b_19_QF_UFNIA.smt2                  |    3.998s | 24.752MiB| unsat | 0 |  |
|63058_64ab9a7ef7b6c3492507_23_QF_UFDTNIA.smt2                |    4.181s | 53.424MiB| sat | 0 |  |
|41958_45c688a4814eb926c254_59_QF_UFNIA.smt2                  |    4.426s | 75.352MiB| sat | 0 |  |
|3106_1c933134166dbad31f79_41_QF_UFLIA.smt2                   |    4.494s | 51.588MiB| sat | 0 |  |
|63058_64ab9a7ef7b6c3492507_24_QF_UFDTLIA.smt2                |    4.546s | 67.216MiB| sat | 0 |  |
|41958_32933c5a1384696720a2_61_QF_UFDTNIA.smt2                |    4.661s | 32.504MiB| sat | 0 |  |
|44289_4066055e0f64d96da11a_15_QF_UFDTLIA.smt2                |    4.737s | 79.176MiB| sat | 0 |  |
|83314_a702bf8b823398c9e37a_2_UFDTNIA.smt2                    |    4.773s | 72.716MiB| sat | 0 |  |
|66603_accdadf23a1cf70ae043_72_QF_UFDTNIA.smt2                |    4.868s | 86.792MiB| unsat | 0 |  |
|17512_5c1021b0faa6b6e1791b_21_QF_UFDTLIA.smt2                |    4.929s | 68.744MiB| unsat | 0 |  |
|44289_4066055e0f64d96da11a_15_QF_UFDTNIA.smt2                |    5.065s | 79.7MiB| sat | 0 |  |
|66603_accdadf23a1cf70ae043_72_QF_UFDTLIA.smt2                |    5.188s | 106.0MiB| unsat | 0 |  |
|72771_f9d228efc97cf1458e38_64_QF_UFDTLIA.smt2                |    5.347s | 33.452MiB| sat | 0 |  |
|41958_45c688a4814eb926c254_60_QF_UFLIA.smt2                  |    5.563s | 62.516MiB| sat | 0 |  |
|41958_32933c5a1384696720a2_62_QF_UFDTLIA.smt2                |    6.136s | 47.172MiB| sat | 0 |  |
|39657_2866defdd1f2434b69ab_47_QF_UFNIA.smt2                  |    6.267s | 35.372MiB| sat | 0 |  |
|72771_f9d228efc97cf1458e38_64_QF_UFDTNIA.smt2                |    6.336s | 31.748MiB| sat | 0 |  |
|39657_1c7158801cd59dc13f05_44_QF_UFLIA.smt2                  |    6.425s | 66.896MiB| sat | 0 |  |
|38347_092cc73601c78e45f4f9_56_QF_UFDTNIA.smt2                |    6.509s | 49.308MiB| sat | 0 |  |
|66603_accdadf23a1cf70ae043_73_QF_UFDTNIA.smt2                |    6.583s | 97.256MiB| sat | 0 |  |
|39657_2866defdd1f2434b69ab_48_QF_UFDTLIA.smt2                |    6.666s | 33.06MiB| sat | 0 |  |
|17512_5c1021b0faa6b6e1791b_19_QF_UFDTNIA.smt2                |    7.133s | 25.324MiB| unsat | 0 |  |
|63058_64ab9a7ef7b6c3492507_22_QF_UFDTNIA.smt2                |    7.623s | 47.824MiB| sat | 0 |  |
|72658_63104dadde9c6026353f_70_QF_UFLIA.smt2                  |    8.326s | 71.448MiB| sat | 0 |  |
|38347_092cc73601c78e45f4f9_56_QF_UFNIA.smt2                  |    8.521s | 65.424MiB| sat | 0 |  |
|3106_1c933134166dbad31f79_41_QF_UFNIA.smt2                   |    8.590s | 53.988MiB| sat | 0 |  |
|39657_1c7158801cd59dc13f05_46_QF_UFDTNIA.smt2                |    9.066s | 61.512MiB| sat | 0 |  |
|17512_5c1021b0faa6b6e1791b_21_QF_UFNIA.smt2                  |    9.238s | 31.788MiB| unsat | 0 |  |
|940_590f27b1c3c800d3243e_31_QF_UFDTNIA.smt2                  |    9.290s | 65.052MiB| sat | 0 |  |
|17512_5c1021b0faa6b6e1791b_21_QF_UFDTNIA.smt2                |    9.728s | 33.28MiB| unsat | 0 |  |
|940_590f27b1c3c800d3243e_31_QF_UFNIA.smt2                    |   10.033s | 67.332MiB| sat | 0 |  |
|44289_e5a2e5c780236919ee6a_17_QF_UFLIA.smt2                  |   10.126s | 103.0MiB| sat | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_65_QF_UFDTNIA.smt2    |   10.389s | 63.672MiB| sat | 0 |  |
|44289_e5a2e5c780236919ee6a_18_QF_UFLIA.smt2                  |   10.447s | 94.336MiB| sat | 0 |  |
|63058_64ab9a7ef7b6c3492507_24_QF_UFDTNIA.smt2                |   10.831s | 76.812MiB| sat | 0 |  |
|39657_1c7158801cd59dc13f05_46_QF_UFLIA.smt2                  |   12.175s | 75.072MiB| sat | 0 |  |
|3106_afb7bc55417e43d7a22790c3576f04fc_37_QF_UFDTLIA.smt2     |   12.329s | 81.448MiB| sat | 0 |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_13_QF_UFLIA.smt2      |   12.397s | 120.0MiB| sat | 0 |  |
|66603_accdadf23a1cf70ae043_76_QF_UFDTNIA.smt2                |   12.707s | 144.0MiB| unsat | 0 |  |
|93493_27ab26d56d60426da02e50231269b6ff_51_QF_UFDTLIA.smt2    |   12.781s | 75.096MiB| sat | 0 |  |
|93493_27ab26d56d60426da02e50231269b6ff_51_QF_UFDTNIA.smt2    |   13.253s | 82.032MiB| sat | 0 |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_11_QF_UFNIA.smt2      |   13.629s | 224.0MiB| sat | 0 |  |
|63058_55d6bef5390186355f11_26_QF_UFDTNIA.smt2                |   13.803s | 64.132MiB| sat | 0 |  |
|72771_f9d228efc97cf1458e38_64_QF_UFLIA.smt2                  |   13.903s | 48.148MiB| sat | 0 |  |
|66603_accdadf23a1cf70ae043_75_QF_UFDTLIA.smt2                |   14.094s | 135.0MiB| sat | 0 |  |
|66603_accdadf23a1cf70ae043_72_QF_UFLIA.smt2                  |   14.103s | 558.0MiB| unsat | 0 |  |
|63058_64ab9a7ef7b6c3492507_22_QF_UFLIA.smt2                  |   15.420s | 149.0MiB| sat | 0 |  |
|66603_accdadf23a1cf70ae043_76_QF_UFDTLIA.smt2                |   15.530s | 237.0MiB| unsat | 0 |  |
|3106_afb7bc55417e43d7a22790c3576f04fc_37_QF_UFDTNIA.smt2     |   16.075s | 67.22MiB| sat | 0 |  |
|39657_2866defdd1f2434b69ab_48_QF_UFLIA.smt2                  |   16.822s | 44.484MiB| sat | 0 |  |
|3106_1c933134166dbad31f79_41_QF_UFDTNIA.smt2                 |   17.087s | 42.172MiB| sat | 0 |  |
|30078_f817a923328f75af7e60_27_QF_UFDTNIA.smt2                |   17.607s | 127.0MiB| unsat | 0 |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_10_QF_UFDTLIA.smt2    |   18.017s | 182.0MiB| unsat | 0 |  |
|3106_1c933134166dbad31f79_39_QF_UFDTNIA.smt2                 |   19.186s | 73.236MiB| sat | 0 |  |
|39657_1c7158801cd59dc13f05_45_QF_UFDTNIA.smt2                |   19.518s | 64.468MiB| sat | 0 |  |
|44788_1965f0d6d94d5d8054ba_34_QF_UFDTNIA.smt2                |   20.014s | 30.308MiB| timeout | 0 |  |
|72658_63104dadde9c6026353f_71_QF_UFDTNIA.smt2                |   20.014s | 26.492MiB| timeout | 0 |  |
|39657_1c7158801cd59dc13f05_44_QF_UFNIA.smt2                  |   20.018s | 87.484MiB| timeout | 0 |  |
|39657_1c7158801cd59dc13f05_44_QF_UFDTNIA.smt2                |   20.020s | 81.956MiB| timeout | 0 |  |
|30078_f817a923328f75af7e60_28_QF_UFDTNIA.smt2                |   20.021s | 132.0MiB| timeout | 0 |  |
|93493_1fdb6cc8eb9c4363b5838af9eb8c7f1f_53_QF_UFNIA.smt2      |   20.021s | 104.0MiB| timeout | 0 |  |
|30078_f817a923328f75af7e60_27_QF_UFDTLIA.smt2                |   20.022s | 147.0MiB| timeout | 0 |  |
|39657_1c7158801cd59dc13f05_45_QF_UFNIA.smt2                  |   20.022s | 78.888MiB| timeout | 0 |  |
|44788_1965f0d6d94d5d8054ba_34_QF_UFNIA.smt2                  |   20.023s | 30.764MiB| timeout | 0 |  |
|3106_afb7bc55417e43d7a22790c3576f04fc_37_QF_UFNIA.smt2       |   20.023s | 117.0MiB| timeout | 0 |  |
|66603_accdadf23a1cf70ae043_74_QF_UFDTLIA.smt2                |   20.024s | 130.0MiB| timeout | 0 |  |
|93493_5990a6bf5f2740164f77_50_QF_UFNIA.smt2                  |   20.025s | 72.348MiB| timeout | 0 |  |
|65782_cd31513fdcd15701933b_5_QF_UFDTNIA.smt2                 |   20.025s | 40.652MiB| timeout | 0 |  |
|3106_1c933134166dbad31f79_39_QF_UFNIA.smt2                   |   20.025s | 111.0MiB| timeout | 0 |  |
|63058_64ab9a7ef7b6c3492507_23_QF_UFLIA.smt2                  |   20.026s | 155.0MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_3_UFNIA.smt2                      |   20.027s | 40.004MiB| timeout | 0 |  |
|44289_b077fc096b3d41cba49f8628caff7fa5_16_QF_UFNIA.smt2      |   20.027s | 105.0MiB| timeout | 0 |  |
|44289_4066055e0f64d96da11a_14_QF_UFLIA.smt2                  |   20.027s | 108.0MiB| timeout | 0 |  |
|93493_798593962ee29ad45ac8_52_QF_UFLIA.smt2                  |   20.027s | 178.0MiB| timeout | 0 |  |
|3106_1c933134166dbad31f79_38_QF_UFNIA.smt2                   |   20.029s | 131.0MiB| timeout | 0 |  |
|41958_32933c5a1384696720a2_62_QF_UFLIA.smt2                  |   20.030s | 53.94MiB| timeout | 0 |  |
|38347_525a1ca0331f2bcbf520_54_QF_UFNIA.smt2                  |   20.030s | 146.0MiB| timeout | 0 |  |
|38347_092cc73601c78e45f4f9_57_QF_UFLIA.smt2                  |   20.030s | 32.952MiB| timeout | 0 |  |
|38347_092cc73601c78e45f4f9_56_QF_UFLIA.smt2                  |   20.031s | 134.0MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_32_QF_UFDTNIA.smt2                  |   20.032s | 191.0MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_29_QF_UFNIA.smt2                    |   20.033s | 157.0MiB| timeout | 0 |  |
|17512_5c1021b0faa6b6e1791b_20_QF_UFDTNIA.smt2                |   20.033s | 38.396MiB| timeout | 0 |  |
|63058_64ab9a7ef7b6c3492507_24_QF_UFLIA.smt2                  |   20.034s | 182.0MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_0_UFDTLIA.smt2                    |   20.035s | 101.0MiB| timeout | 0 |  |
|93493_27ab26d56d60426da02e50231269b6ff_51_QF_UFNIA.smt2      |   20.037s | 92.564MiB| timeout | 0 |  |
|39657_1c7158801cd59dc13f05_46_QF_UFNIA.smt2                  |   20.037s | 74.804MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_29_QF_UFLIA.smt2                    |   20.039s | 268.0MiB| timeout | 0 |  |
|93493_4ea6163ed03941199c785278ccc42812_49_QF_UFNIA.smt2      |   20.040s | 166.0MiB| timeout | 0 |  |
|41958_32933c5a1384696720a2_63_QF_UFDTNIA.smt2                |   20.040s | 30.504MiB| timeout | 0 |  |
|93493_4ea6163ed03941199c785278ccc42812_49_QF_UFDTLIA.smt2    |   20.040s | 167.0MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_32_QF_UFLIA.smt2                    |   20.041s | 286.0MiB| timeout | 0 |  |
|38347_525a1ca0331f2bcbf520_54_QF_UFDTNIA.smt2                |   20.042s | 118.0MiB| timeout | 0 |  |
|93493_798593962ee29ad45ac8_52_QF_UFDTLIA.smt2                |   20.042s | 135.0MiB| timeout | 0 |  |
|3106_1c933134166dbad31f79_38_QF_UFDTNIA.smt2                 |   20.042s | 204.0MiB| timeout | 0 |  |
|44289_b077fc096b3d41cba49f8628caff7fa5_16_QF_UFDTLIA.smt2    |   20.043s | 143.0MiB| timeout | 0 |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_12_QF_UFLIA.smt2      |   20.043s | 180.0MiB| timeout | 0 |  |
|41958_32933c5a1384696720a2_61_QF_UFLIA.smt2                  |   20.044s | 63.024MiB| timeout | 0 |  |
|52759_b3ecd2335fd16ec2eee2_9_UFLIA.smt2                      |   20.044s | 51.348MiB| timeout | 0 |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_10_QF_UFLIA.smt2      |   20.044s | 207.0MiB| timeout | 0 |  |
|72658_63104dadde9c6026353f_70_QF_UFNIA.smt2                  |   20.045s | 29.428MiB| timeout | 0 |  |
|93493_1fdb6cc8eb9c4363b5838af9eb8c7f1f_53_QF_UFLIA.smt2      |   20.045s | 100.0MiB| timeout | 0 |  |
|93493_5990a6bf5f2740164f77_50_QF_UFLIA.smt2                  |   20.045s | 129.0MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_30_QF_UFDTLIA.smt2                  |   20.046s | 257.0MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_32_QF_UFNIA.smt2                    |   20.047s | 160.0MiB| timeout | 0 |  |
|52759_b3ecd2335fd16ec2eee2_9_UFNIA.smt2                      |   20.047s | 43.912MiB| timeout | 0 |  |
|63058_64ab9a7ef7b6c3492507_22_QF_UFNIA.smt2                  |   20.047s | 180.0MiB| timeout | 0 |  |
|41958_32933c5a1384696720a2_62_QF_UFDTNIA.smt2                |   20.047s | 29.22MiB| timeout | 0 |  |
|38347_092cc73601c78e45f4f9_57_QF_UFNIA.smt2                  |   20.047s | 32.024MiB| timeout | 0 |  |
|44289_4066055e0f64d96da11a_15_QF_UFNIA.smt2                  |   20.048s | 318.0MiB| timeout | 0 |  |
|38347_092cc73601c78e45f4f9_55_QF_UFLIA.smt2                  |   20.049s | 26.452MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_29_QF_UFDTNIA.smt2                  |   20.049s | 157.0MiB| timeout | 0 |  |
|3106_1c933134166dbad31f79_38_QF_UFDTLIA.smt2                 |   20.050s | 206.0MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_29_QF_UFDTLIA.smt2                  |   20.051s | 266.0MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_32_QF_UFDTLIA.smt2                  |   20.051s | 281.0MiB| timeout | 0 |  |
|72658_63104dadde9c6026353f_71_QF_UFNIA.smt2                  |   20.051s | 27.332MiB| timeout | 0 |  |
|38347_092cc73601c78e45f4f9_55_QF_UFDTLIA.smt2                |   20.052s | 24.848MiB| timeout | 0 |  |
|39657_2866defdd1f2434b69ab_47_QF_UFLIA.smt2                  |   20.052s | 43.576MiB| timeout | 0 |  |
|44788_1965f0d6d94d5d8054ba_36_QF_UFDTNIA.smt2                |   20.053s | 33.456MiB| timeout | 0 |  |
|66603_accdadf23a1cf70ae043_76_QF_UFLIA.smt2                  |   20.053s | 479.0MiB| timeout | 0 |  |
|41958_32933c5a1384696720a2_61_QF_UFNIA.smt2                  |   20.053s | 36.788MiB| timeout | 0 |  |
|52759_b3ecd2335fd16ec2eee2_9_UFDTLIA.smt2                    |   20.054s | 48.428MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_1_UFNIA.smt2                      |   20.054s | 45.592MiB| timeout | 0 |  |
|38347_092cc73601c78e45f4f9_55_QF_UFDTNIA.smt2                |   20.054s | 23.664MiB| timeout | 0 |  |
|17512_5c1021b0faa6b6e1791b_20_QF_UFNIA.smt2                  |   20.055s | 36.912MiB| timeout | 0 |  |
|72658_63104dadde9c6026353f_70_QF_UFDTNIA.smt2                |   20.055s | 30.364MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_69_QF_UFDTNIA.smt2    |   20.056s | 49.584MiB| timeout | 0 |  |
|38347_092cc73601c78e45f4f9_56_QF_UFDTLIA.smt2                |   20.056s | 136.0MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_2_UFNIA.smt2                      |   20.057s | 139.0MiB| timeout | 0 |  |
|30078_f817a923328f75af7e60_27_QF_UFLIA.smt2                  |   20.057s | 419.0MiB| timeout | 0 |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_11_QF_UFLIA.smt2      |   20.058s | 221.0MiB| timeout | 0 |  |
|44788_1965f0d6d94d5d8054ba_35_QF_UFNIA.smt2                  |   20.059s | 44.944MiB| timeout | 0 |  |
|3106_1c933134166dbad31f79_39_QF_UFDTLIA.smt2                 |   20.060s | 69.644MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_3_UFDTNIA.smt2                    |   20.060s | 43.66MiB| timeout | 0 |  |
|65782_cd31513fdcd15701933b_5_QF_UFNIA.smt2                   |   20.060s | 41.364MiB| timeout | 0 |  |
|41958_45c688a4814eb926c254_59_QF_UFLIA.smt2                  |   20.060s | 85.412MiB| timeout | 0 |  |
|38347_092cc73601c78e45f4f9_57_QF_UFDTNIA.smt2                |   20.060s | 26.92MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_66_QF_UFDTNIA.smt2    |   20.060s | 51.736MiB| timeout | 0 |  |
|72658_63104dadde9c6026353f_71_QF_UFLIA.smt2                  |   20.061s | 118.0MiB| timeout | 0 |  |
|93493_27ab26d56d60426da02e50231269b6ff_51_QF_UFLIA.smt2      |   20.061s | 87.076MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_66_QF_UFLIA.smt2      |   20.061s | 516.0MiB| timeout | 0 |  |
|41958_32933c5a1384696720a2_61_QF_UFDTLIA.smt2                |   20.062s | 64.912MiB| timeout | 0 |  |
|3106_1c933134166dbad31f79_39_QF_UFLIA.smt2                   |   20.062s | 111.0MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_69_QF_UFDTLIA.smt2    |   20.062s | 445.0MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_33_QF_UFLIA.smt2                    |   20.063s | 264.0MiB| timeout | 0 |  |
|44788_1965f0d6d94d5d8054ba_36_QF_UFNIA.smt2                  |   20.063s | 36.916MiB| timeout | 0 |  |
|66603_accdadf23a1cf70ae043_74_QF_UFDTNIA.smt2                |   20.063s | 124.0MiB| timeout | 0 |  |
|39657_1c7158801cd59dc13f05_45_QF_UFLIA.smt2                  |   20.063s | 80.056MiB| timeout | 0 |  |
|44289_e5a2e5c780236919ee6a_18_QF_UFNIA.smt2                  |   20.065s | 98.588MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_4_UFDTNIA.smt2                    |   20.065s | 111.0MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_31_QF_UFLIA.smt2                    |   20.065s | 139.0MiB| timeout | 0 |  |
|44289_4066055e0f64d96da11a_14_QF_UFNIA.smt2                  |   20.065s | 110.0MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_0_UFLIA.smt2                      |   20.066s | 99.804MiB| timeout | 0 |  |
|52759_b3ecd2335fd16ec2eee2_9_UFDTNIA.smt2                    |   20.066s | 44.272MiB| timeout | 0 |  |
|65782_cd31513fdcd15701933b_5_QF_UFDTLIA.smt2                 |   20.067s | 114.0MiB| timeout | 0 |  |
|93493_1fdb6cc8eb9c4363b5838af9eb8c7f1f_53_QF_UFDTNIA.smt2    |   20.067s | 69.748MiB| timeout | 0 |  |
|38347_092cc73601c78e45f4f9_55_QF_UFNIA.smt2                  |   20.068s | 24.068MiB| timeout | 0 |  |
|93493_5990a6bf5f2740164f77_50_QF_UFDTLIA.smt2                |   20.069s | 173.0MiB| timeout | 0 |  |
|63058_55d6bef5390186355f11_26_QF_UFNIA.smt2                  |   20.070s | 176.0MiB| timeout | 0 |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_12_QF_UFNIA.smt2      |   20.072s | 187.0MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_1_UFDTNIA.smt2                    |   20.072s | 45.412MiB| timeout | 0 |  |
|72658_63104dadde9c6026353f_71_QF_UFDTLIA.smt2                |   20.072s | 157.0MiB| timeout | 0 |  |
|41958_32933c5a1384696720a2_63_QF_UFNIA.smt2                  |   20.074s | 29.2MiB| timeout | 0 |  |
|44289_b077fc096b3d41cba49f8628caff7fa5_16_QF_UFLIA.smt2      |   20.074s | 149.0MiB| timeout | 0 |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_10_QF_UFNIA.smt2      |   20.074s | 213.0MiB| timeout | 0 |  |
|44788_1965f0d6d94d5d8054ba_36_QF_UFDTLIA.smt2                |   20.075s | 250.0MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_67_QF_UFNIA.smt2      |   20.075s | 58.092MiB| timeout | 0 |  |
|41958_32933c5a1384696720a2_62_QF_UFNIA.smt2                  |   20.075s | 27.632MiB| timeout | 0 |  |
|44788_1965f0d6d94d5d8054ba_35_QF_UFDTNIA.smt2                |   20.075s | 38.336MiB| timeout | 0 |  |
|39657_2866defdd1f2434b69ab_47_QF_UFDTLIA.smt2                |   20.075s | 38.72MiB| timeout | 0 |  |
|93493_4ea6163ed03941199c785278ccc42812_49_QF_UFLIA.smt2      |   20.076s | 148.0MiB| timeout | 0 |  |
|72771_f9d228efc97cf1458e38_64_QF_UFNIA.smt2                  |   20.076s | 48.116MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_2_UFLIA.smt2                      |   20.076s | 168.0MiB| timeout | 0 |  |
|44788_1965f0d6d94d5d8054ba_36_QF_UFLIA.smt2                  |   20.076s | 260.0MiB| timeout | 0 |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_13_QF_UFNIA.smt2      |   20.077s | 155.0MiB| timeout | 0 |  |
|17512_5c1021b0faa6b6e1791b_19_QF_UFDTLIA.smt2                |   20.079s | 85.856MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_33_QF_UFNIA.smt2                    |   20.079s | 154.0MiB| timeout | 0 |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_10_QF_UFDTNIA.smt2    |   20.080s | 83.696MiB| timeout | 0 |  |
|63058_64ab9a7ef7b6c3492507_23_QF_UFNIA.smt2                  |   20.081s | 193.0MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_33_QF_UFDTLIA.smt2                  |   20.082s | 256.0MiB| timeout | 0 |  |
|17512_5c1021b0faa6b6e1791b_20_QF_UFDTLIA.smt2                |   20.083s | 253.0MiB| timeout | 0 |  |
|65782_cd31513fdcd15701933b_5_QF_UFLIA.smt2                   |   20.083s | 110.0MiB| timeout | 0 |  |
|66603_accdadf23a1cf70ae043_73_QF_UFDTLIA.smt2                |   20.084s | 239.0MiB| timeout | 0 |  |
|63058_64ab9a7ef7b6c3492507_24_QF_UFNIA.smt2                  |   20.084s | 176.0MiB| timeout | 0 |  |
|63058_55d6bef5390186355f11_26_QF_UFLIA.smt2                  |   20.085s | 180.0MiB| timeout | 0 |  |
|93493_798593962ee29ad45ac8_52_QF_UFDTNIA.smt2                |   20.088s | 76.792MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_68_QF_UFNIA.smt2      |   20.088s | 53.436MiB| timeout | 0 |  |
|93493_5990a6bf5f2740164f77_50_QF_UFDTNIA.smt2                |   20.089s | 181.0MiB| timeout | 0 |  |
|44289_e5a2e5c780236919ee6a_17_QF_UFNIA.smt2                  |   20.089s | 98.812MiB| timeout | 0 |  |
|66603_accdadf23a1cf70ae043_75_QF_UFDTNIA.smt2                |   20.090s | 136.0MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_30_QF_UFLIA.smt2                    |   20.090s | 280.0MiB| timeout | 0 |  |
|3106_afb7bc55417e43d7a22790c3576f04fc_37_QF_UFLIA.smt2       |   20.093s | 102.0MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_68_QF_UFLIA.smt2      |   20.093s | 503.0MiB| timeout | 0 |  |
|30078_f817a923328f75af7e60_28_QF_UFDTLIA.smt2                |   20.094s | 151.0MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_69_QF_UFLIA.smt2      |   20.094s | 482.0MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_67_QF_UFDTNIA.smt2    |   20.094s | 64.872MiB| timeout | 0 |  |
|38347_092cc73601c78e45f4f9_57_QF_UFDTLIA.smt2                |   20.096s | 29.72MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_65_QF_UFNIA.smt2      |   20.097s | 75.472MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_4_UFDTLIA.smt2                    |   20.097s | 560.0MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_30_QF_UFDTNIA.smt2                  |   20.097s | 153.0MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_1_UFLIA.smt2                      |   20.098s | 228.0MiB| timeout | 0 |  |
|3106_1c933134166dbad31f79_38_QF_UFLIA.smt2                   |   20.098s | 131.0MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_65_QF_UFDTLIA.smt2    |   20.099s | 969.0MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_30_QF_UFNIA.smt2                    |   20.099s | 163.0MiB| timeout | 0 |  |
|30078_f817a923328f75af7e60_28_QF_UFLIA.smt2                  |   20.100s | 385.0MiB| timeout | 0 |  |
|38347_525a1ca0331f2bcbf520_54_QF_UFLIA.smt2                  |   20.102s | 499.0MiB| timeout | 0 |  |
|93493_4ea6163ed03941199c785278ccc42812_49_QF_UFDTNIA.smt2    |   20.103s | 173.0MiB| timeout | 0 |  |
|93493_1fdb6cc8eb9c4363b5838af9eb8c7f1f_53_QF_UFDTLIA.smt2    |   20.103s | 90.516MiB| timeout | 0 |  |
|17512_5c1021b0faa6b6e1791b_19_QF_UFLIA.smt2                  |   20.105s | 80.028MiB| timeout | 0 |  |
|44289_4066055e0f64d96da11a_15_QF_UFLIA.smt2                  |   20.107s | 366.0MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_68_QF_UFDTLIA.smt2    |   20.109s | 515.0MiB| timeout | 0 |  |
|44289_b077fc096b3d41cba49f8628caff7fa5_16_QF_UFDTNIA.smt2    |   20.110s | 75.052MiB| timeout | 0 |  |
|66603_accdadf23a1cf70ae043_75_QF_UFLIA.smt2                  |   20.110s | 597.0MiB| timeout | 0 |  |
|66603_accdadf23a1cf70ae043_73_QF_UFNIA.smt2                  |   20.113s | 433.0MiB| timeout | 0 |  |
|66603_accdadf23a1cf70ae043_76_QF_UFNIA.smt2                  |   20.114s | 486.0MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_4_UFNIA.smt2                      |   20.118s | 148.0MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_31_QF_UFDTLIA.smt2                  |   20.120s | 137.0MiB| timeout | 0 |  |
|93493_798593962ee29ad45ac8_52_QF_UFNIA.smt2                  |   20.121s | 173.0MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_67_QF_UFLIA.smt2      |   20.123s | 487.0MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_66_QF_UFDTLIA.smt2    |   20.124s | 559.0MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_33_QF_UFDTNIA.smt2                  |   20.128s | 183.0MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_4_UFLIA.smt2                      |   20.128s | 552.0MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_3_UFDTLIA.smt2                    |   20.133s | 211.0MiB| timeout | 0 |  |
|66603_accdadf23a1cf70ae043_72_QF_UFNIA.smt2                  |   20.135s | 595.0MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_2_UFDTLIA.smt2                    |   20.137s | 148.0MiB| timeout | 0 |  |
|66603_accdadf23a1cf70ae043_74_QF_UFNIA.smt2                  |   20.139s | 666.0MiB| timeout | 0 |  |
|66603_accdadf23a1cf70ae043_75_QF_UFNIA.smt2                  |   20.140s | 657.0MiB| timeout | 0 |  |
|66603_accdadf23a1cf70ae043_73_QF_UFLIA.smt2                  |   20.145s | 444.0MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_1_UFDTLIA.smt2                    |   20.145s | 255.0MiB| timeout | 0 |  |
|30078_f817a923328f75af7e60_28_QF_UFNIA.smt2                  |   20.150s | 384.0MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_65_QF_UFLIA.smt2      |   20.151s | 1007.0MiB| timeout | 0 |  |
|17512_5c1021b0faa6b6e1791b_20_QF_UFLIA.smt2                  |   20.151s | 326.0MiB| timeout | 0 |  |
|30078_f817a923328f75af7e60_27_QF_UFNIA.smt2                  |   20.155s | 397.0MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_67_QF_UFDTLIA.smt2    |   20.157s | 471.0MiB| timeout | 0 |  |
|66603_accdadf23a1cf70ae043_74_QF_UFLIA.smt2                  |   20.164s | 612.0MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_3_UFLIA.smt2                      |   20.164s | 270.0MiB| timeout | 0 |  |
|38347_525a1ca0331f2bcbf520_54_QF_UFDTLIA.smt2                |   20.170s | 483.0MiB| timeout | 0 |  |
