# .

* SAT 136
* UNSAT 40
* TIMEOUT 132
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: check against certora
Job tag: capture_pointer_certora_100_rs_3
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 1f67f6a2344fc8508ad25381686ce3901cf28ae8
Z3 branch: capture_pointer
Z3 options: "-T:100 smt.random_seed=3"
Z3 inputs: inputs/certora
Z3 commit message: Remove stray check-assignment debug print polluting solver output

The IF_VERBOSE(0, ...) in solver::check_assignment unconditionally wrote
"check-assignment" to stdout, corrupting solver output and breaking the
3042.smt2 regression (extra line before 'sat').

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|65782_cd31513fdcd15701933b_6_QF_UFNIA.smt2                   |    0.130s | 22.08MiB| sat | 0 |  |  |
|65782_cd31513fdcd15701933b_7_QF_UFLIA.smt2                   |    0.159s | 26.376MiB| sat | 0 |  |  |
|63058_aa742630eef64f949de269382c1f9035_25_UFLIA.smt2         |    0.165s | 24.628MiB| unsat | 0 |  |  |
|65782_cd31513fdcd15701933b_6_QF_UFDTLIA.smt2                 |    0.170s | 22.404MiB| sat | 0 |  |  |
|44788_1965f0d6d94d5d8054ba_34_QF_UFDTLIA.smt2                |    0.193s | 24.284MiB| sat | 0 |  |  |
|65782_cd31513fdcd15701933b_6_QF_UFLIA.smt2                   |    0.214s | 22.744MiB| sat | 0 |  |  |
|3106_1c933134166dbad31f79_40_QF_UFDTLIA.smt2                 |    0.215s | 24.192MiB| sat | 0 |  |  |
|3106_1c933134166dbad31f79_40_QF_UFLIA.smt2                   |    0.228s | 25.732MiB| sat | 0 |  |  |
|63058_aa742630eef64f949de269382c1f9035_25_UFDTLIA.smt2       |    0.238s | 25.44MiB| unsat | 0 |  |  |
|63058_aa742630eef64f949de269382c1f9035_25_UFDTNIA.smt2       |    0.238s | 24.876MiB| unsat | 0 |  |  |
|44788_1965f0d6d94d5d8054ba_34_QF_UFLIA.smt2                  |    0.255s | 26.292MiB| sat | 0 |  |  |
|39657_2866defdd1f2434b69ab_48_QF_UFDTNIA.smt2                |    0.258s | 28.228MiB| sat | 0 |  |  |
|63058_aa742630eef64f949de269382c1f9035_25_UFNIA.smt2         |    0.261s | 24.2MiB| unsat | 0 |  |  |
|65782_cd31513fdcd15701933b_6_QF_UFDTNIA.smt2                 |    0.308s | 21.896MiB| sat | 0 |  |  |
|11775_ad46e5b8db4748c51973_43_QF_UFDTLIA.smt2                |    0.313s | 34.14MiB| sat | 0 |  |  |
|39657_2866defdd1f2434b69ab_47_QF_UFDTNIA.smt2                |    0.319s | 26.932MiB| sat | 0 |  |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_13_QF_UFDTNIA.smt2    |    0.336s | 30.156MiB| sat | 0 |  |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_12_QF_UFDTNIA.smt2    |    0.365s | 28.584MiB| sat | 0 |  |  |
|41958_45c688a4814eb926c254_60_QF_UFDTNIA.smt2                |    0.371s | 29.264MiB| sat | 0 |  |  |
|83314_a702bf8b823398c9e37a_0_UFNIA.smt2                      |    0.389s | 26.392MiB| sat | 0 |  |  |
|38347_092cc73601c78e45f4f9_58_QF_UFDTLIA.smt2                |    0.409s | 29.828MiB| sat | 0 |  |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_13_QF_UFDTLIA.smt2    |    0.415s | 31.3MiB| sat | 0 |  |  |
|3106_1c933134166dbad31f79_40_QF_UFNIA.smt2                   |    0.456s | 24.956MiB| sat | 0 |  |  |
|3106_1c933134166dbad31f79_40_QF_UFDTNIA.smt2                 |    0.516s | 24.092MiB| sat | 0 |  |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_12_QF_UFDTLIA.smt2    |    0.540s | 33.824MiB| sat | 0 |  |  |
|65782_cd31513fdcd15701933b_8_QF_UFLIA.smt2                   |    0.543s | 27.412MiB| sat | 0 |  |  |
|44788_1965f0d6d94d5d8054ba_35_QF_UFLIA.smt2                  |    0.554s | 37.132MiB| unsat | 0 |  |  |
|41958_45c688a4814eb926c254_59_QF_UFDTNIA.smt2                |    0.555s | 31.74MiB| sat | 0 |  |  |
|65782_cd31513fdcd15701933b_7_QF_UFDTLIA.smt2                 |    0.662s | 26.152MiB| sat | 0 |  |  |
|65782_cd31513fdcd15701933b_8_QF_UFDTLIA.smt2                 |    0.749s | 26.584MiB| sat | 0 |  |  |
|38347_092cc73601c78e45f4f9_58_QF_UFLIA.smt2                  |    0.788s | 31.312MiB| sat | 0 |  |  |
|72658_63104dadde9c6026353f_70_QF_UFDTLIA.smt2                |    0.818s | 56.264MiB| sat | 0 |  |  |
|72771_f9d228efc97cf1458e38_64_QF_UFDTNIA.smt2                |    0.853s | 28.384MiB| sat | 0 |  |  |
|11775_ad46e5b8db4748c51973_42_QF_UFNIA.smt2                  |    0.926s | 31.896MiB| sat | 0 |  |  |
|44788_1965f0d6d94d5d8054ba_35_QF_UFDTLIA.smt2                |    1.034s | 27.784MiB| unsat | 0 |  |  |
|11775_ad46e5b8db4748c51973_42_QF_UFDTLIA.smt2                |    1.230s | 36.42MiB| sat | 0 |  |  |
|41958_45c688a4814eb926c254_59_QF_UFDTLIA.smt2                |    1.282s | 37.932MiB| sat | 0 |  |  |
|41958_45c688a4814eb926c254_60_QF_UFDTLIA.smt2                |    1.320s | 36.992MiB| sat | 0 |  |  |
|11775_ad46e5b8db4748c51973_42_QF_UFDTNIA.smt2                |    1.374s | 30.264MiB| sat | 0 |  |  |
|65782_cd31513fdcd15701933b_8_QF_UFNIA.smt2                   |    1.477s | 26.34MiB| sat | 0 |  |  |
|11775_ad46e5b8db4748c51973_43_QF_UFLIA.smt2                  |    1.646s | 36.236MiB| sat | 0 |  |  |
|38347_092cc73601c78e45f4f9_58_QF_UFDTNIA.smt2                |    1.785s | 28.608MiB| sat | 0 |  |  |
|41958_32933c5a1384696720a2_63_QF_UFDTLIA.smt2                |    1.946s | 32.104MiB| sat | 0 |  |  |
|11775_ad46e5b8db4748c51973_43_QF_UFNIA.smt2                  |    1.960s | 28.844MiB| unsat | 0 |  |  |
|44289_4066055e0f64d96da11a_14_QF_UFDTLIA.smt2                |    2.037s | 43.504MiB| sat | 0 |  |  |
|44289_4066055e0f64d96da11a_14_QF_UFDTNIA.smt2                |    2.157s | 43.98MiB| sat | 0 |  |  |
|83314_a702bf8b823398c9e37a_0_UFDTNIA.smt2                    |    2.422s | 28.992MiB| sat | 0 |  |  |
|39657_2866defdd1f2434b69ab_47_QF_UFNIA.smt2                  |    2.447s | 33.404MiB| sat | 0 |  |  |
|39657_1c7158801cd59dc13f05_45_QF_UFDTLIA.smt2                |    2.522s | 52.7MiB| sat | 0 |  |  |
|41958_32933c5a1384696720a2_63_QF_UFLIA.smt2                  |    2.563s | 31.164MiB| sat | 0 |  |  |
|11775_ad46e5b8db4748c51973_42_QF_UFLIA.smt2                  |    2.655s | 43.044MiB| sat | 0 |  |  |
|65782_cd31513fdcd15701933b_8_QF_UFDTNIA.smt2                 |    2.670s | 25.904MiB| sat | 0 |  |  |
|44289_e5a2e5c780236919ee6a_17_QF_UFDTLIA.smt2                |    2.688s | 50.924MiB| sat | 0 |  |  |
|3106_1c933134166dbad31f79_41_QF_UFDTLIA.smt2                 |    2.721s | 39.792MiB| sat | 0 |  |  |
|44289_e5a2e5c780236919ee6a_18_QF_UFDTLIA.smt2                |    2.832s | 51.64MiB| sat | 0 |  |  |
|65782_cd31513fdcd15701933b_7_QF_UFDTNIA.smt2                 |    2.882s | 26.136MiB| unsat | 0 |  |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_11_QF_UFDTNIA.smt2    |    2.901s | 59.692MiB| sat | 0 |  |  |
|63058_55d6bef5390186355f11_26_QF_UFDTLIA.smt2                |    3.282s | 50.448MiB| sat | 0 |  |  |
|44289_e5a2e5c780236919ee6a_17_QF_UFDTNIA.smt2                |    3.301s | 51.348MiB| sat | 0 |  |  |
|39657_1c7158801cd59dc13f05_44_QF_UFDTLIA.smt2                |    3.321s | 57.724MiB| sat | 0 |  |  |
|44289_e5a2e5c780236919ee6a_18_QF_UFDTNIA.smt2                |    3.324s | 52.008MiB| sat | 0 |  |  |
|39657_1c7158801cd59dc13f05_46_QF_UFDTLIA.smt2                |    3.393s | 58.852MiB| sat | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_68_QF_UFNIA.smt2      |    3.566s | 48.592MiB| sat | 0 |  |  |
|63058_64ab9a7ef7b6c3492507_23_QF_UFDTLIA.smt2                |    3.748s | 69.66MiB| sat | 0 |  |  |
|63058_64ab9a7ef7b6c3492507_24_QF_UFDTLIA.smt2                |    3.778s | 64.832MiB| sat | 0 |  |  |
|38347_092cc73601c78e45f4f9_56_QF_UFDTNIA.smt2                |    3.966s | 45.58MiB| sat | 0 |  |  |
|63058_64ab9a7ef7b6c3492507_22_QF_UFDTLIA.smt2                |    4.006s | 45.82MiB| sat | 0 |  |  |
|17512_5c1021b0faa6b6e1791b_21_QF_UFLIA.smt2                  |    4.104s | 66.432MiB| unsat | 0 |  |  |
|65782_cd31513fdcd15701933b_7_QF_UFNIA.smt2                   |    4.182s | 27.492MiB| unsat | 0 |  |  |
|11775_ad46e5b8db4748c51973_43_QF_UFDTNIA.smt2                |    4.587s | 29.688MiB| unsat | 0 |  |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_11_QF_UFDTLIA.smt2    |    4.721s | 75.468MiB| sat | 0 |  |  |
|63058_64ab9a7ef7b6c3492507_23_QF_UFDTNIA.smt2                |    4.750s | 52.48MiB| sat | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_69_QF_UFNIA.smt2      |    4.927s | 42.792MiB| sat | 0 |  |  |
|83314_a702bf8b823398c9e37a_1_UFNIA.smt2                      |    4.931s | 41.052MiB| sat | 0 |  |  |
|63058_55d6bef5390186355f11_26_QF_UFDTNIA.smt2                |    5.130s | 52.92MiB| sat | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_69_QF_UFDTNIA.smt2    |    5.213s | 41.66MiB| sat | 0 |  |  |
|44289_4066055e0f64d96da11a_15_QF_UFDTLIA.smt2                |    5.273s | 78.192MiB| sat | 0 |  |  |
|41958_32933c5a1384696720a2_62_QF_UFDTLIA.smt2                |    5.278s | 44.404MiB| sat | 0 |  |  |
|66603_accdadf23a1cf70ae043_72_QF_UFDTNIA.smt2                |    5.342s | 85.208MiB| unsat | 0 |  |  |
|72771_f9d228efc97cf1458e38_64_QF_UFDTLIA.smt2                |    5.521s | 32.752MiB| sat | 0 |  |  |
|17512_5c1021b0faa6b6e1791b_19_QF_UFDTNIA.smt2                |    5.567s | 25.42MiB| unsat | 0 |  |  |
|41958_45c688a4814eb926c254_60_QF_UFNIA.smt2                  |    5.601s | 52.3MiB| sat | 0 |  |  |
|66603_accdadf23a1cf70ae043_72_QF_UFDTLIA.smt2                |    5.721s | 104.0MiB| unsat | 0 |  |  |
|17512_5c1021b0faa6b6e1791b_19_QF_UFNIA.smt2                  |    6.001s | 25.076MiB| unsat | 0 |  |  |
|17512_5c1021b0faa6b6e1791b_21_QF_UFDTLIA.smt2                |    6.184s | 71.264MiB| unsat | 0 |  |  |
|63058_64ab9a7ef7b6c3492507_22_QF_UFDTNIA.smt2                |    6.265s | 47.168MiB| sat | 0 |  |  |
|72658_63104dadde9c6026353f_70_QF_UFLIA.smt2                  |    6.406s | 63.828MiB| sat | 0 |  |  |
|44289_4066055e0f64d96da11a_15_QF_UFDTNIA.smt2                |    6.445s | 81.528MiB| sat | 0 |  |  |
|39657_1c7158801cd59dc13f05_45_QF_UFLIA.smt2                  |    6.453s | 70.4MiB| sat | 0 |  |  |
|41958_45c688a4814eb926c254_60_QF_UFLIA.smt2                  |    6.460s | 61.74MiB| sat | 0 |  |  |
|44289_e5a2e5c780236919ee6a_18_QF_UFLIA.smt2                  |    6.614s | 89.884MiB| sat | 0 |  |  |
|41958_45c688a4814eb926c254_59_QF_UFNIA.smt2                  |    6.998s | 74.428MiB| sat | 0 |  |  |
|83314_a702bf8b823398c9e37a_0_UFDTLIA.smt2                    |    7.053s | 94.972MiB| sat | 0 |  |  |
|39657_2866defdd1f2434b69ab_48_QF_UFDTLIA.smt2                |    7.430s | 33.632MiB| sat | 0 |  |  |
|3106_1c933134166dbad31f79_41_QF_UFLIA.smt2                   |    7.745s | 55.54MiB| sat | 0 |  |  |
|44289_b077fc096b3d41cba49f8628caff7fa5_16_QF_UFDTNIA.smt2    |    7.751s | 62.94MiB| sat | 0 |  |  |
|38347_092cc73601c78e45f4f9_56_QF_UFNIA.smt2                  |    7.977s | 65.46MiB| sat | 0 |  |  |
|63058_64ab9a7ef7b6c3492507_24_QF_UFDTNIA.smt2                |    8.382s | 75.6MiB| sat | 0 |  |  |
|3106_1c933134166dbad31f79_39_QF_UFDTLIA.smt2                 |    8.515s | 96.708MiB| sat | 0 |  |  |
|17512_5c1021b0faa6b6e1791b_20_QF_UFNIA.smt2                  |    8.602s | 34.724MiB| unsat | 0 |  |  |
|44289_e5a2e5c780236919ee6a_17_QF_UFLIA.smt2                  |    9.234s | 99.544MiB| sat | 0 |  |  |
|3106_1c933134166dbad31f79_39_QF_UFDTNIA.smt2                 |   10.051s | 60.556MiB| sat | 0 |  |  |
|3106_afb7bc55417e43d7a22790c3576f04fc_37_QF_UFDTLIA.smt2     |   10.801s | 84.124MiB| sat | 0 |  |  |
|39657_2866defdd1f2434b69ab_47_QF_UFDTLIA.smt2                |   10.893s | 35.032MiB| sat | 0 |  |  |
|66603_accdadf23a1cf70ae043_75_QF_UFDTNIA.smt2                |   11.169s | 145.0MiB| sat | 0 |  |  |
|17512_5c1021b0faa6b6e1791b_21_QF_UFNIA.smt2                  |   11.210s | 31.72MiB| unsat | 0 |  |  |
|3106_1c933134166dbad31f79_41_QF_UFDTNIA.smt2                 |   11.394s | 40.764MiB| sat | 0 |  |  |
|41958_45c688a4814eb926c254_59_QF_UFLIA.smt2                  |   12.306s | 92.928MiB| sat | 0 |  |  |
|83314_a702bf8b823398c9e37a_1_UFDTNIA.smt2                    |   12.465s | 42.944MiB| sat | 0 |  |  |
|940_590f27b1c3c800d3243e_31_QF_UFNIA.smt2                    |   12.546s | 68.768MiB| sat | 0 |  |  |
|44289_4066055e0f64d96da11a_14_QF_UFLIA.smt2                  |   12.946s | 99.0MiB| sat | 0 |  |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_13_QF_UFLIA.smt2      |   13.009s | 114.0MiB| sat | 0 |  |  |
|66603_accdadf23a1cf70ae043_76_QF_UFDTNIA.smt2                |   14.249s | 140.0MiB| unsat | 0 |  |  |
|93493_27ab26d56d60426da02e50231269b6ff_51_QF_UFDTNIA.smt2    |   14.499s | 88.328MiB| sat | 0 |  |  |
|93493_1fdb6cc8eb9c4363b5838af9eb8c7f1f_53_QF_UFDTLIA.smt2    |   15.797s | 86.372MiB| sat | 0 |  |  |
|39657_1c7158801cd59dc13f05_44_QF_UFDTNIA.smt2                |   15.803s | 70.664MiB| sat | 0 |  |  |
|940_590f27b1c3c800d3243e_31_QF_UFDTNIA.smt2                  |   16.328s | 88.028MiB| sat | 0 |  |  |
|30078_f817a923328f75af7e60_27_QF_UFDTNIA.smt2                |   16.513s | 125.0MiB| unsat | 0 |  |  |
|3106_1c933134166dbad31f79_41_QF_UFNIA.smt2                   |   16.779s | 61.204MiB| sat | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_65_QF_UFNIA.smt2      |   16.906s | 95.72MiB| sat | 0 |  |  |
|17512_5c1021b0faa6b6e1791b_21_QF_UFDTNIA.smt2                |   17.114s | 33.312MiB| unsat | 0 |  |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_11_QF_UFNIA.smt2      |   17.663s | 220.0MiB| sat | 0 |  |  |
|66603_accdadf23a1cf70ae043_76_QF_UFDTLIA.smt2                |   17.770s | 233.0MiB| unsat | 0 |  |  |
|39657_2866defdd1f2434b69ab_48_QF_UFNIA.smt2                  |   17.907s | 47.44MiB| sat | 0 |  |  |
|30078_f817a923328f75af7e60_27_QF_UFDTLIA.smt2                |   18.488s | 142.0MiB| unsat | 0 |  |  |
|39657_1c7158801cd59dc13f05_46_QF_UFDTNIA.smt2                |   18.524s | 71.916MiB| sat | 0 |  |  |
|38347_092cc73601c78e45f4f9_58_QF_UFNIA.smt2                  |   18.577s | 33.512MiB| sat | 0 |  |  |
|72771_f9d228efc97cf1458e38_64_QF_UFNIA.smt2                  |   19.019s | 49.808MiB| sat | 0 |  |  |
|66603_accdadf23a1cf70ae043_72_QF_UFLIA.smt2                  |   19.087s | 541.0MiB| unsat | 0 |  |  |
|39657_1c7158801cd59dc13f05_44_QF_UFLIA.smt2                  |   20.133s | 83.696MiB| sat | 0 |  |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_10_QF_UFDTLIA.smt2    |   20.277s | 166.0MiB| unsat | 0 |  |  |
|72658_63104dadde9c6026353f_70_QF_UFDTNIA.smt2                |   20.349s | 26.816MiB| sat | 0 |  |  |
|72771_f9d228efc97cf1458e38_64_QF_UFLIA.smt2                  |   21.204s | 47.112MiB| sat | 0 |  |  |
|63058_64ab9a7ef7b6c3492507_22_QF_UFLIA.smt2                  |   22.038s | 174.0MiB| sat | 0 |  |  |
|83314_a702bf8b823398c9e37a_3_UFDTLIA.smt2                    |   24.616s | 326.0MiB| unsat | 0 |  |  |
|63058_64ab9a7ef7b6c3492507_23_QF_UFLIA.smt2                  |   25.135s | 179.0MiB| sat | 0 |  |  |
|66603_accdadf23a1cf70ae043_74_QF_UFDTLIA.smt2                |   25.411s | 137.0MiB| sat | 0 |  |  |
|39657_1c7158801cd59dc13f05_46_QF_UFLIA.smt2                  |   25.664s | 118.0MiB| sat | 0 |  |  |
|39657_2866defdd1f2434b69ab_48_QF_UFLIA.smt2                  |   25.800s | 45.476MiB| sat | 0 |  |  |
|39657_1c7158801cd59dc13f05_46_QF_UFNIA.smt2                  |   26.385s | 76.612MiB| sat | 0 |  |  |
|940_590f27b1c3c800d3243e_30_QF_UFDTNIA.smt2                  |   27.863s | 167.0MiB| sat | 0 |  |  |
|66603_accdadf23a1cf70ae043_75_QF_UFDTLIA.smt2                |   28.864s | 137.0MiB| sat | 0 |  |  |
|940_590f27b1c3c800d3243e_29_QF_UFNIA.smt2                    |   29.097s | 195.0MiB| sat | 0 |  |  |
|41958_32933c5a1384696720a2_62_QF_UFLIA.smt2                  |   30.005s | 62.892MiB| sat | 0 |  |  |
|44289_b077fc096b3d41cba49f8628caff7fa5_16_QF_UFDTLIA.smt2    |   32.962s | 141.0MiB| sat | 0 |  |  |
|30078_f817a923328f75af7e60_28_QF_UFDTNIA.smt2                |   33.696s | 134.0MiB| unsat | 0 |  |  |
|93493_27ab26d56d60426da02e50231269b6ff_51_QF_UFNIA.smt2      |   33.978s | 147.0MiB| sat | 0 |  |  |
|41958_32933c5a1384696720a2_62_QF_UFNIA.smt2                  |   34.115s | 28.248MiB| unsat | 0 |  |  |
|39657_1c7158801cd59dc13f05_45_QF_UFDTNIA.smt2                |   35.554s | 68.624MiB| sat | 0 |  |  |
|66603_accdadf23a1cf70ae043_72_QF_UFNIA.smt2                  |   35.959s | 596.0MiB| unsat | 0 |  |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_12_QF_UFLIA.smt2      |   38.543s | 177.0MiB| sat | 0 |  |  |
|63058_64ab9a7ef7b6c3492507_22_QF_UFNIA.smt2                  |   39.748s | 178.0MiB| sat | 0 |  |  |
|3106_afb7bc55417e43d7a22790c3576f04fc_37_QF_UFDTNIA.smt2     |   42.297s | 88.08MiB| sat | 0 |  |  |
|30078_f817a923328f75af7e60_27_QF_UFNIA.smt2                  |   43.742s | 413.0MiB| unsat | 0 |  |  |
|44788_1965f0d6d94d5d8054ba_36_QF_UFLIA.smt2                  |   43.850s | 424.0MiB| unsat | 0 |  |  |
|38347_092cc73601c78e45f4f9_56_QF_UFDTLIA.smt2                |   44.343s | 158.0MiB| sat | 0 |  |  |
|93493_27ab26d56d60426da02e50231269b6ff_51_QF_UFDTLIA.smt2    |   44.561s | 147.0MiB| sat | 0 |  |  |
|30078_f817a923328f75af7e60_28_QF_UFDTLIA.smt2                |   47.598s | 158.0MiB| unsat | 0 |  |  |
|30078_f817a923328f75af7e60_27_QF_UFLIA.smt2                  |   54.300s | 503.0MiB| unsat | 0 |  |  |
|93493_5990a6bf5f2740164f77_50_QF_UFDTLIA.smt2                |   56.320s | 208.0MiB| sat | 0 |  |  |
|940_590f27b1c3c800d3243e_33_QF_UFNIA.smt2                    |   57.062s | 198.0MiB| sat | 0 |  |  |
|44788_1965f0d6d94d5d8054ba_36_QF_UFDTLIA.smt2                |   58.977s | 542.0MiB| unsat | 0 |  |  |
|63058_64ab9a7ef7b6c3492507_24_QF_UFLIA.smt2                  |   60.210s | 254.0MiB| sat | 0 |  |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_10_QF_UFDTNIA.smt2    |   62.765s | 97.332MiB| unsat | 0 |  |  |
|940_590f27b1c3c800d3243e_32_QF_UFNIA.smt2                    |   63.569s | 180.0MiB| sat | 0 |  |  |
|30078_f817a923328f75af7e60_28_QF_UFNIA.smt2                  |   63.939s | 419.0MiB| unsat | 0 |  |  |
|44289_b077fc096b3d41cba49f8628caff7fa5_16_QF_UFLIA.smt2      |   66.574s | 186.0MiB| sat | 0 |  |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_11_QF_UFLIA.smt2      |   67.569s | 270.0MiB| sat | 0 |  |  |
|44289_e5a2e5c780236919ee6a_17_QF_UFNIA.smt2                  |   68.753s | 106.0MiB| sat | 0 |  |  |
|83314_a702bf8b823398c9e37a_2_UFDTNIA.smt2                    |   71.552s | 160.0MiB| sat | 0 |  |  |
|30078_f817a923328f75af7e60_28_QF_UFLIA.smt2                  |   73.003s | 464.0MiB| unsat | 0 |  |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_10_QF_UFLIA.smt2      |   73.118s | 323.0MiB| unsat | 0 |  |  |
|17512_5c1021b0faa6b6e1791b_19_QF_UFDTLIA.smt2                |   74.903s | 109.0MiB| unsat | 0 |  |  |
|44289_e5a2e5c780236919ee6a_18_QF_UFNIA.smt2                  |   75.286s | 123.0MiB| sat | 0 |  |  |
|39657_1c7158801cd59dc13f05_45_QF_UFNIA.smt2                  |   81.458s | 112.0MiB| sat | 0 |  |  |
|17512_5c1021b0faa6b6e1791b_20_QF_UFDTNIA.smt2                |   83.569s | 40.276MiB| unsat | 0 |  |  |
|72658_63104dadde9c6026353f_70_QF_UFNIA.smt2                  |  100.015s | 41.188MiB| timeout | 0 |  |  |
|52759_b3ecd2335fd16ec2eee2_9_UFDTNIA.smt2                    |  100.020s | 43.972MiB| timeout | 0 |  |  |
|83314_a702bf8b823398c9e37a_3_UFDTNIA.smt2                    |  100.020s | 81.748MiB| timeout | 0 |  |  |
|83314_a702bf8b823398c9e37a_3_UFNIA.smt2                      |  100.020s | 70.116MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_66_QF_UFNIA.smt2      |  100.026s | 109.0MiB| timeout | 0 |  |  |
|66603_accdadf23a1cf70ae043_73_QF_UFDTNIA.smt2                |  100.028s | 171.0MiB| timeout | 0 |  |  |
|63058_64ab9a7ef7b6c3492507_24_QF_UFNIA.smt2                  |  100.031s | 210.0MiB| timeout | 0 |  |  |
|38347_525a1ca0331f2bcbf520_54_QF_UFNIA.smt2                  |  100.031s | 199.0MiB| timeout | 0 |  |  |
|44289_b077fc096b3d41cba49f8628caff7fa5_16_QF_UFNIA.smt2      |  100.034s | 193.0MiB| timeout | 0 |  |  |
|41958_32933c5a1384696720a2_61_QF_UFNIA.smt2                  |  100.035s | 39.2MiB| timeout | 0 |  |  |
|3106_1c933134166dbad31f79_39_QF_UFLIA.smt2                   |  100.036s | 121.0MiB| timeout | 0 |  |  |
|63058_55d6bef5390186355f11_26_QF_UFNIA.smt2                  |  100.037s | 225.0MiB| timeout | 0 |  |  |
|72658_63104dadde9c6026353f_71_QF_UFDTNIA.smt2                |  100.037s | 32.032MiB| timeout | 0 |  |  |
|44788_1965f0d6d94d5d8054ba_35_QF_UFDTNIA.smt2                |  100.038s | 36.544MiB| timeout | 0 |  |  |
|44788_1965f0d6d94d5d8054ba_34_QF_UFNIA.smt2                  |  100.039s | 37.832MiB| timeout | 0 |  |  |
|3106_1c933134166dbad31f79_39_QF_UFNIA.smt2                   |  100.041s | 165.0MiB| timeout | 0 |  |  |
|93493_4ea6163ed03941199c785278ccc42812_49_QF_UFDTNIA.smt2    |  100.041s | 231.0MiB| timeout | 0 |  |  |
|93493_4ea6163ed03941199c785278ccc42812_49_QF_UFNIA.smt2      |  100.044s | 263.0MiB| timeout | 0 |  |  |
|93493_1fdb6cc8eb9c4363b5838af9eb8c7f1f_53_QF_UFLIA.smt2      |  100.045s | 140.0MiB| timeout | 0 |  |  |
|940_590f27b1c3c800d3243e_30_QF_UFNIA.smt2                    |  100.045s | 256.0MiB| timeout | 0 |  |  |
|93493_1fdb6cc8eb9c4363b5838af9eb8c7f1f_53_QF_UFNIA.smt2      |  100.045s | 225.0MiB| timeout | 0 |  |  |
|38347_092cc73601c78e45f4f9_55_QF_UFNIA.smt2                  |  100.046s | 26.568MiB| timeout | 0 |  |  |
|38347_092cc73601c78e45f4f9_56_QF_UFLIA.smt2                  |  100.046s | 199.0MiB| timeout | 0 |  |  |
|940_590f27b1c3c800d3243e_31_QF_UFDTLIA.smt2                  |  100.047s | 257.0MiB| timeout | 0 |  |  |
|63058_55d6bef5390186355f11_26_QF_UFLIA.smt2                  |  100.047s | 205.0MiB| timeout | 0 |  |  |
|41958_32933c5a1384696720a2_63_QF_UFDTNIA.smt2                |  100.048s | 33.576MiB| timeout | 0 |  |  |
|44788_1965f0d6d94d5d8054ba_36_QF_UFNIA.smt2                  |  100.049s | 67.324MiB| timeout | 0 |  |  |
|940_590f27b1c3c800d3243e_33_QF_UFDTLIA.smt2                  |  100.049s | 373.0MiB| timeout | 0 |  |  |
|93493_798593962ee29ad45ac8_52_QF_UFDTLIA.smt2                |  100.050s | 214.0MiB| timeout | 0 |  |  |
|38347_525a1ca0331f2bcbf520_54_QF_UFDTNIA.smt2                |  100.050s | 162.0MiB| timeout | 0 |  |  |
|3106_afb7bc55417e43d7a22790c3576f04fc_37_QF_UFNIA.smt2       |  100.051s | 173.0MiB| timeout | 0 |  |  |
|940_590f27b1c3c800d3243e_32_QF_UFLIA.smt2                    |  100.052s | 362.0MiB| timeout | 0 |  |  |
|83314_a702bf8b823398c9e37a_2_UFNIA.smt2                      |  100.052s | 153.0MiB| timeout | 0 |  |  |
|83314_a702bf8b823398c9e37a_1_UFLIA.smt2                      |  100.052s | 326.0MiB| timeout | 0 |  |  |
|52759_b3ecd2335fd16ec2eee2_9_UFNIA.smt2                      |  100.053s | 64.984MiB| timeout | 0 |  |  |
|940_590f27b1c3c800d3243e_30_QF_UFDTLIA.smt2                  |  100.053s | 383.0MiB| timeout | 0 |  |  |
|83314_a702bf8b823398c9e37a_4_UFDTNIA.smt2                    |  100.054s | 311.0MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_68_QF_UFDTNIA.smt2    |  100.054s | 69.508MiB| timeout | 0 |  |  |
|93493_4ea6163ed03941199c785278ccc42812_49_QF_UFLIA.smt2      |  100.054s | 315.0MiB| timeout | 0 |  |  |
|38347_092cc73601c78e45f4f9_55_QF_UFDTNIA.smt2                |  100.055s | 24.804MiB| timeout | 0 |  |  |
|3106_afb7bc55417e43d7a22790c3576f04fc_37_QF_UFLIA.smt2       |  100.056s | 163.0MiB| timeout | 0 |  |  |
|940_590f27b1c3c800d3243e_29_QF_UFDTNIA.smt2                  |  100.056s | 377.0MiB| timeout | 0 |  |  |
|940_590f27b1c3c800d3243e_29_QF_UFLIA.smt2                    |  100.057s | 344.0MiB| timeout | 0 |  |  |
|66603_accdadf23a1cf70ae043_76_QF_UFLIA.smt2                  |  100.057s | 479.0MiB| timeout | 0 |  |  |
|93493_4ea6163ed03941199c785278ccc42812_49_QF_UFDTLIA.smt2    |  100.058s | 239.0MiB| timeout | 0 |  |  |
|17512_5c1021b0faa6b6e1791b_19_QF_UFLIA.smt2                  |  100.061s | 148.0MiB| timeout | 0 |  |  |
|72658_63104dadde9c6026353f_71_QF_UFNIA.smt2                  |  100.061s | 35.86MiB| timeout | 0 |  |  |
|52759_b3ecd2335fd16ec2eee2_9_UFDTLIA.smt2                    |  100.063s | 71.528MiB| timeout | 0 |  |  |
|93493_5990a6bf5f2740164f77_50_QF_UFLIA.smt2                  |  100.063s | 289.0MiB| timeout | 0 |  |  |
|65782_cd31513fdcd15701933b_5_QF_UFNIA.smt2                   |  100.067s | 69.072MiB| timeout | 0 |  |  |
|44289_4066055e0f64d96da11a_14_QF_UFNIA.smt2                  |  100.067s | 121.0MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_65_QF_UFDTNIA.smt2    |  100.068s | 64.98MiB| timeout | 0 |  |  |
|93493_27ab26d56d60426da02e50231269b6ff_51_QF_UFLIA.smt2      |  100.069s | 176.0MiB| timeout | 0 |  |  |
|41958_32933c5a1384696720a2_61_QF_UFDTLIA.smt2                |  100.071s | 94.48MiB| timeout | 0 |  |  |
|44788_1965f0d6d94d5d8054ba_34_QF_UFDTNIA.smt2                |  100.072s | 60.836MiB| timeout | 0 |  |  |
|38347_092cc73601c78e45f4f9_57_QF_UFDTNIA.smt2                |  100.073s | 27.96MiB| timeout | 0 |  |  |
|41958_32933c5a1384696720a2_61_QF_UFLIA.smt2                  |  100.073s | 99.0MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_67_QF_UFNIA.smt2      |  100.073s | 68.296MiB| timeout | 0 |  |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_13_QF_UFNIA.smt2      |  100.074s | 179.0MiB| timeout | 0 |  |  |
|65782_cd31513fdcd15701933b_5_QF_UFLIA.smt2                   |  100.075s | 220.0MiB| timeout | 0 |  |  |
|38347_092cc73601c78e45f4f9_57_QF_UFLIA.smt2                  |  100.076s | 37.484MiB| timeout | 0 |  |  |
|93493_1fdb6cc8eb9c4363b5838af9eb8c7f1f_53_QF_UFDTNIA.smt2    |  100.076s | 94.612MiB| timeout | 0 |  |  |
|83314_a702bf8b823398c9e37a_2_UFDTLIA.smt2                    |  100.078s | 254.0MiB| timeout | 0 |  |  |
|44788_1965f0d6d94d5d8054ba_35_QF_UFNIA.smt2                  |  100.078s | 55.664MiB| timeout | 0 |  |  |
|63058_64ab9a7ef7b6c3492507_23_QF_UFNIA.smt2                  |  100.079s | 180.0MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_68_QF_UFDTLIA.smt2    |  100.079s | 601.0MiB| timeout | 0 |  |  |
|3106_1c933134166dbad31f79_38_QF_UFLIA.smt2                   |  100.080s | 618.0MiB| timeout | 0 |  |  |
|41958_32933c5a1384696720a2_62_QF_UFDTNIA.smt2                |  100.081s | 37.188MiB| timeout | 0 |  |  |
|83314_a702bf8b823398c9e37a_4_UFNIA.smt2                      |  100.081s | 250.0MiB| timeout | 0 |  |  |
|940_590f27b1c3c800d3243e_31_QF_UFLIA.smt2                    |  100.082s | 206.0MiB| timeout | 0 |  |  |
|83314_a702bf8b823398c9e37a_0_UFLIA.smt2                      |  100.082s | 192.0MiB| timeout | 0 |  |  |
|93493_5990a6bf5f2740164f77_50_QF_UFNIA.smt2                  |  100.085s | 360.0MiB| timeout | 0 |  |  |
|93493_5990a6bf5f2740164f77_50_QF_UFDTNIA.smt2                |  100.086s | 296.0MiB| timeout | 0 |  |  |
|83314_a702bf8b823398c9e37a_4_UFDTLIA.smt2                    |  100.086s | 697.0MiB| timeout | 0 |  |  |
|940_590f27b1c3c800d3243e_29_QF_UFDTLIA.smt2                  |  100.087s | 388.0MiB| timeout | 0 |  |  |
|72658_63104dadde9c6026353f_71_QF_UFLIA.smt2                  |  100.088s | 223.0MiB| timeout | 0 |  |  |
|17512_5c1021b0faa6b6e1791b_20_QF_UFLIA.smt2                  |  100.089s | 439.0MiB| timeout | 0 |  |  |
|52759_b3ecd2335fd16ec2eee2_9_UFLIA.smt2                      |  100.090s | 67.028MiB| timeout | 0 |  |  |
|940_590f27b1c3c800d3243e_33_QF_UFDTNIA.smt2                  |  100.090s | 266.0MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_66_QF_UFLIA.smt2      |  100.091s | 705.0MiB| timeout | 0 |  |  |
|66603_accdadf23a1cf70ae043_74_QF_UFDTNIA.smt2                |  100.091s | 147.0MiB| timeout | 0 |  |  |
|41958_32933c5a1384696720a2_63_QF_UFNIA.smt2                  |  100.093s | 34.748MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_66_QF_UFDTNIA.smt2    |  100.094s | 75.008MiB| timeout | 0 |  |  |
|39657_1c7158801cd59dc13f05_44_QF_UFNIA.smt2                  |  100.097s | 118.0MiB| timeout | 0 |  |  |
|940_590f27b1c3c800d3243e_32_QF_UFDTLIA.smt2                  |  100.097s | 650.0MiB| timeout | 0 |  |  |
|72658_63104dadde9c6026353f_71_QF_UFDTLIA.smt2                |  100.098s | 265.0MiB| timeout | 0 |  |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_10_QF_UFNIA.smt2      |  100.099s | 263.0MiB| timeout | 0 |  |  |
|3106_1c933134166dbad31f79_38_QF_UFDTLIA.smt2                 |  100.099s | 296.0MiB| timeout | 0 |  |  |
|83314_a702bf8b823398c9e37a_3_UFLIA.smt2                      |  100.100s | 489.0MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_67_QF_UFDTNIA.smt2    |  100.100s | 62.996MiB| timeout | 0 |  |  |
|940_590f27b1c3c800d3243e_32_QF_UFDTNIA.smt2                  |  100.101s | 958.0MiB| timeout | 0 |  |  |
|83314_a702bf8b823398c9e37a_2_UFLIA.smt2                      |  100.101s | 220.0MiB| timeout | 0 |  |  |
|38347_092cc73601c78e45f4f9_55_QF_UFLIA.smt2                  |  100.102s | 37.268MiB| timeout | 0 |  |  |
|39657_2866defdd1f2434b69ab_47_QF_UFLIA.smt2                  |  100.102s | 107.0MiB| timeout | 0 |  |  |
|38347_092cc73601c78e45f4f9_57_QF_UFDTLIA.smt2                |  100.102s | 31.1MiB| timeout | 0 |  |  |
|83314_a702bf8b823398c9e37a_1_UFDTLIA.smt2                    |  100.102s | 364.0MiB| timeout | 0 |  |  |
|3106_1c933134166dbad31f79_38_QF_UFNIA.smt2                   |  100.104s | 746.0MiB| timeout | 0 |  |  |
|66603_accdadf23a1cf70ae043_73_QF_UFDTLIA.smt2                |  100.104s | 292.0MiB| timeout | 0 |  |  |
|38347_092cc73601c78e45f4f9_57_QF_UFNIA.smt2                  |  100.104s | 35.972MiB| timeout | 0 |  |  |
|93493_798593962ee29ad45ac8_52_QF_UFNIA.smt2                  |  100.107s | 316.0MiB| timeout | 0 |  |  |
|93493_798593962ee29ad45ac8_52_QF_UFDTNIA.smt2                |  100.110s | 203.0MiB| timeout | 0 |  |  |
|93493_798593962ee29ad45ac8_52_QF_UFLIA.smt2                  |  100.110s | 230.0MiB| timeout | 0 |  |  |
|940_590f27b1c3c800d3243e_30_QF_UFLIA.smt2                    |  100.111s | 380.0MiB| timeout | 0 |  |  |
|83314_a702bf8b823398c9e37a_4_UFLIA.smt2                      |  100.114s | 651.0MiB| timeout | 0 |  |  |
|3106_1c933134166dbad31f79_38_QF_UFDTNIA.smt2                 |  100.119s | 289.0MiB| timeout | 0 |  |  |
|38347_092cc73601c78e45f4f9_55_QF_UFDTLIA.smt2                |  100.122s | 27.236MiB| timeout | 0 |  |  |
|65782_cd31513fdcd15701933b_5_QF_UFDTNIA.smt2                 |  100.124s | 57.28MiB| timeout | 0 |  |  |
|65782_cd31513fdcd15701933b_5_QF_UFDTLIA.smt2                 |  100.127s | 251.0MiB| timeout | 0 |  |  |
|41958_32933c5a1384696720a2_61_QF_UFDTNIA.smt2                |  100.127s | 41.172MiB| timeout | 0 |  |  |
|44788_1965f0d6d94d5d8054ba_36_QF_UFDTNIA.smt2                |  100.128s | 43.336MiB| timeout | 0 |  |  |
|44289_4066055e0f64d96da11a_15_QF_UFLIA.smt2                  |  100.128s | 532.0MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_68_QF_UFLIA.smt2      |  100.135s | 749.0MiB| timeout | 0 |  |  |
|17512_5c1021b0faa6b6e1791b_20_QF_UFDTLIA.smt2                |  100.136s | 478.0MiB| timeout | 0 |  |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_12_QF_UFNIA.smt2      |  100.143s | 304.0MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_69_QF_UFDTLIA.smt2    |  100.144s | 584.0MiB| timeout | 0 |  |  |
|66603_accdadf23a1cf70ae043_73_QF_UFNIA.smt2                  |  100.146s | 669.0MiB| timeout | 0 |  |  |
|940_590f27b1c3c800d3243e_33_QF_UFLIA.smt2                    |  100.147s | 369.0MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_66_QF_UFDTLIA.smt2    |  100.157s | 683.0MiB| timeout | 0 |  |  |
|66603_accdadf23a1cf70ae043_74_QF_UFNIA.smt2                  |  100.159s | 916.0MiB| timeout | 0 |  |  |
|66603_accdadf23a1cf70ae043_73_QF_UFLIA.smt2                  |  100.162s | 814.0MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_69_QF_UFLIA.smt2      |  100.164s | 536.0MiB| timeout | 0 |  |  |
|66603_accdadf23a1cf70ae043_76_QF_UFNIA.smt2                  |  100.172s | 1551.0MiB| timeout | 0 |  |  |
|44289_4066055e0f64d96da11a_15_QF_UFNIA.smt2                  |  100.176s | 629.0MiB| timeout | 0 |  |  |
|66603_accdadf23a1cf70ae043_75_QF_UFNIA.smt2                  |  100.184s | 1042.0MiB| timeout | 0 |  |  |
|66603_accdadf23a1cf70ae043_75_QF_UFLIA.smt2                  |  100.199s | 1190.0MiB| timeout | 0 |  |  |
|66603_accdadf23a1cf70ae043_74_QF_UFLIA.smt2                  |  100.199s | 959.0MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_65_QF_UFDTLIA.smt2    |  100.225s | 2303.0MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_65_QF_UFLIA.smt2      |  100.238s | 2224.0MiB| timeout | 0 |  |  |
|38347_525a1ca0331f2bcbf520_54_QF_UFLIA.smt2                  |  100.273s | 2026.0MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_67_QF_UFDTLIA.smt2    |  100.288s | 2021.0MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_67_QF_UFLIA.smt2      |  100.292s | 1986.0MiB| timeout | 0 |  |  |
|38347_525a1ca0331f2bcbf520_54_QF_UFDTLIA.smt2                |  100.295s | 1983.0MiB| timeout | 0 |  |  |
