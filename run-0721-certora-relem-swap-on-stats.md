# .

* SAT 146
* UNSAT 38
* TIMEOUT 124
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: bench#3143 remove_element swap-relocation on (lp.remove_element_swap=true) on certora, branch remove-element-swap-toggle
Job tag: certora-relem-swap-on
Runner: rise-runner-2
Z3 repo: levnach/z3
Z3 commit: d2c63fb40ff7ba388c0bf462afce7882821c19ca
Z3 branch: remove-element-swap-toggle
Z3 options: "-T:120 -st lp.remove_element_swap=true"
Z3 inputs: inputs/certora
Z3 commit message: lp: toggleable swap-relocation of coefficients in static_matrix::remove_element

remove_element deep-copied the relocated tail coefficient. In namespace lp the
coefficient type is rational (copyable), so this allocates on the heap for large
coefficients. Relocate large coefficients by swapping instead, avoiding the
allocation; small coefficients still use a plain copy (cheaper than swapping mpz
internals). Reported in Z3Prover/bench#3143.

Gated behind the new parameter lp.remove_element_swap (default true) so the
optimization can be toggled on/off for A/B benchmarking.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
Copilot-Session: fd5693e4-3b1d-4dac-b3be-2942ae6f31f8

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|63058_aa742630eef64f949de269382c1f9035_25_UFNIA.smt2         |    0.077s | 24.212MiB| unsat | 0 |  |  |
|65782_cd31513fdcd15701933b_6_QF_UFLIA.smt2                   |    0.094s | 22.392MiB| sat | 0 |  |  |
|63058_aa742630eef64f949de269382c1f9035_25_UFLIA.smt2         |    0.099s | 24.544MiB| unsat | 0 |  |  |
|65782_cd31513fdcd15701933b_6_QF_UFDTNIA.smt2                 |    0.115s | 21.896MiB| sat | 0 |  |  |
|65782_cd31513fdcd15701933b_6_QF_UFNIA.smt2                   |    0.116s | 21.904MiB| sat | 0 |  |  |
|63058_aa742630eef64f949de269382c1f9035_25_UFDTNIA.smt2       |    0.148s | 24.232MiB| unsat | 0 |  |  |
|65782_cd31513fdcd15701933b_6_QF_UFDTLIA.smt2                 |    0.149s | 22.224MiB| sat | 0 |  |  |
|39657_2866defdd1f2434b69ab_48_QF_UFDTNIA.smt2                |    0.164s | 27.936MiB| sat | 0 |  |  |
|44788_1965f0d6d94d5d8054ba_34_QF_UFDTLIA.smt2                |    0.170s | 24.364MiB| sat | 0 |  |  |
|3106_1c933134166dbad31f79_40_QF_UFNIA.smt2                   |    0.172s | 24.976MiB| sat | 0 |  |  |
|39657_2866defdd1f2434b69ab_47_QF_UFDTNIA.smt2                |    0.183s | 26.844MiB| sat | 0 |  |  |
|65782_cd31513fdcd15701933b_7_QF_UFLIA.smt2                   |    0.186s | 26.696MiB| sat | 0 |  |  |
|3106_1c933134166dbad31f79_40_QF_UFLIA.smt2                   |    0.187s | 25.3MiB| sat | 0 |  |  |
|44788_1965f0d6d94d5d8054ba_34_QF_UFLIA.smt2                  |    0.205s | 26.416MiB| sat | 0 |  |  |
|63058_aa742630eef64f949de269382c1f9035_25_UFDTLIA.smt2       |    0.208s | 25.548MiB| unsat | 0 |  |  |
|38347_092cc73601c78e45f4f9_58_QF_UFDTLIA.smt2                |    0.268s | 28.76MiB| sat | 0 |  |  |
|3106_1c933134166dbad31f79_40_QF_UFDTLIA.smt2                 |    0.270s | 24.668MiB| sat | 0 |  |  |
|3106_1c933134166dbad31f79_40_QF_UFDTNIA.smt2                 |    0.278s | 24.144MiB| sat | 0 |  |  |
|11775_ad46e5b8db4748c51973_43_QF_UFDTLIA.smt2                |    0.297s | 35.056MiB| sat | 0 |  |  |
|39657_2866defdd1f2434b69ab_47_QF_UFNIA.smt2                  |    0.327s | 30.784MiB| sat | 0 |  |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_13_QF_UFDTNIA.smt2    |    0.350s | 30.424MiB| sat | 0 |  |  |
|41958_45c688a4814eb926c254_60_QF_UFDTNIA.smt2                |    0.361s | 29.964MiB| sat | 0 |  |  |
|65782_cd31513fdcd15701933b_8_QF_UFDTLIA.smt2                 |    0.401s | 26.608MiB| sat | 0 |  |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_12_QF_UFDTNIA.smt2    |    0.424s | 29.412MiB| sat | 0 |  |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_13_QF_UFDTLIA.smt2    |    0.428s | 31.832MiB| sat | 0 |  |  |
|65782_cd31513fdcd15701933b_8_QF_UFLIA.smt2                   |    0.463s | 27.068MiB| sat | 0 |  |  |
|41958_45c688a4814eb926c254_59_QF_UFDTNIA.smt2                |    0.464s | 32.04MiB| sat | 0 |  |  |
|41958_32933c5a1384696720a2_61_QF_UFNIA.smt2                  |    0.555s | 25.452MiB| sat | 0 |  |  |
|44788_1965f0d6d94d5d8054ba_35_QF_UFLIA.smt2                  |    0.570s | 37.488MiB| unsat | 0 |  |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_12_QF_UFDTLIA.smt2    |    0.619s | 34.68MiB| sat | 0 |  |  |
|65782_cd31513fdcd15701933b_7_QF_UFDTLIA.smt2                 |    0.623s | 26.452MiB| sat | 0 |  |  |
|41958_45c688a4814eb926c254_60_QF_UFDTLIA.smt2                |    0.646s | 35.376MiB| sat | 0 |  |  |
|83314_a702bf8b823398c9e37a_0_UFNIA.smt2                      |    0.699s | 26.236MiB| sat | 0 |  |  |
|11775_ad46e5b8db4748c51973_42_QF_UFNIA.smt2                  |    0.704s | 31.692MiB| sat | 0 |  |  |
|38347_092cc73601c78e45f4f9_58_QF_UFLIA.smt2                  |    0.749s | 31.584MiB| sat | 0 |  |  |
|11775_ad46e5b8db4748c51973_43_QF_UFNIA.smt2                  |    0.826s | 28.7MiB| unsat | 0 |  |  |
|65782_cd31513fdcd15701933b_8_QF_UFDTNIA.smt2                 |    0.888s | 25.56MiB| sat | 0 |  |  |
|72658_63104dadde9c6026353f_70_QF_UFDTLIA.smt2                |    0.894s | 56.676MiB| sat | 0 |  |  |
|11775_ad46e5b8db4748c51973_43_QF_UFLIA.smt2                  |    0.934s | 36.0MiB| sat | 0 |  |  |
|83314_a702bf8b823398c9e37a_0_UFDTNIA.smt2                    |    1.062s | 28.316MiB| sat | 0 |  |  |
|44788_1965f0d6d94d5d8054ba_35_QF_UFDTLIA.smt2                |    1.066s | 27.864MiB| unsat | 0 |  |  |
|11775_ad46e5b8db4748c51973_42_QF_UFDTLIA.smt2                |    1.101s | 37.0MiB| sat | 0 |  |  |
|38347_092cc73601c78e45f4f9_58_QF_UFNIA.smt2                  |    1.169s | 32.356MiB| sat | 0 |  |  |
|17512_5c1021b0faa6b6e1791b_19_QF_UFDTNIA.smt2                |    1.178s | 24.788MiB| unsat | 0 |  |  |
|38347_092cc73601c78e45f4f9_58_QF_UFDTNIA.smt2                |    1.334s | 29.18MiB| sat | 0 |  |  |
|65782_cd31513fdcd15701933b_8_QF_UFNIA.smt2                   |    1.356s | 26.076MiB| sat | 0 |  |  |
|41958_45c688a4814eb926c254_59_QF_UFDTLIA.smt2                |    1.413s | 39.284MiB| sat | 0 |  |  |
|44289_4066055e0f64d96da11a_14_QF_UFDTLIA.smt2                |    1.719s | 44.06MiB| sat | 0 |  |  |
|11775_ad46e5b8db4748c51973_42_QF_UFDTNIA.smt2                |    1.792s | 30.408MiB| sat | 0 |  |  |
|44289_4066055e0f64d96da11a_14_QF_UFDTNIA.smt2                |    1.795s | 44.6MiB| sat | 0 |  |  |
|44289_e5a2e5c780236919ee6a_17_QF_UFDTLIA.smt2                |    1.870s | 51.896MiB| sat | 0 |  |  |
|11775_ad46e5b8db4748c51973_43_QF_UFDTNIA.smt2                |    1.897s | 29.1MiB| unsat | 0 |  |  |
|39657_2866defdd1f2434b69ab_48_QF_UFNIA.smt2                  |    1.926s | 41.236MiB| sat | 0 |  |  |
|3106_1c933134166dbad31f79_41_QF_UFDTLIA.smt2                 |    2.094s | 39.88MiB| sat | 0 |  |  |
|44289_e5a2e5c780236919ee6a_18_QF_UFDTLIA.smt2                |    2.147s | 50.416MiB| sat | 0 |  |  |
|39657_1c7158801cd59dc13f05_46_QF_UFDTLIA.smt2                |    2.401s | 56.02MiB| sat | 0 |  |  |
|44289_e5a2e5c780236919ee6a_17_QF_UFDTNIA.smt2                |    2.401s | 51.8MiB| sat | 0 |  |  |
|44289_e5a2e5c780236919ee6a_18_QF_UFDTNIA.smt2                |    2.430s | 52.676MiB| sat | 0 |  |  |
|39657_1c7158801cd59dc13f05_45_QF_UFDTLIA.smt2                |    2.449s | 56.652MiB| sat | 0 |  |  |
|17512_5c1021b0faa6b6e1791b_19_QF_UFNIA.smt2                  |    2.502s | 24.54MiB| unsat | 0 |  |  |
|63058_55d6bef5390186355f11_26_QF_UFDTLIA.smt2                |    2.588s | 51.128MiB| sat | 0 |  |  |
|39657_1c7158801cd59dc13f05_44_QF_UFDTLIA.smt2                |    2.739s | 53.8MiB| sat | 0 |  |  |
|65782_cd31513fdcd15701933b_7_QF_UFDTNIA.smt2                 |    2.843s | 26.856MiB| unsat | 0 |  |  |
|41958_32933c5a1384696720a2_63_QF_UFDTLIA.smt2                |    2.908s | 34.312MiB| sat | 0 |  |  |
|65782_cd31513fdcd15701933b_7_QF_UFNIA.smt2                   |    2.988s | 26.92MiB| unsat | 0 |  |  |
|63058_64ab9a7ef7b6c3492507_23_QF_UFDTLIA.smt2                |    3.054s | 68.332MiB| sat | 0 |  |  |
|11775_ad46e5b8db4748c51973_42_QF_UFLIA.smt2                  |    3.289s | 43.572MiB| sat | 0 |  |  |
|63058_64ab9a7ef7b6c3492507_22_QF_UFDTLIA.smt2                |    3.749s | 47.444MiB| sat | 0 |  |  |
|41958_45c688a4814eb926c254_60_QF_UFNIA.smt2                  |    3.852s | 50.46MiB| sat | 0 |  |  |
|41958_45c688a4814eb926c254_60_QF_UFLIA.smt2                  |    4.244s | 58.26MiB| sat | 0 |  |  |
|44289_4066055e0f64d96da11a_15_QF_UFDTNIA.smt2                |    4.310s | 79.412MiB| sat | 0 |  |  |
|44289_4066055e0f64d96da11a_15_QF_UFDTLIA.smt2                |    4.379s | 79.02MiB| sat | 0 |  |  |
|66603_accdadf23a1cf70ae043_72_QF_UFDTNIA.smt2                |    4.757s | 87.128MiB| unsat | 0 |  |  |
|66603_accdadf23a1cf70ae043_72_QF_UFDTLIA.smt2                |    4.906s | 106.0MiB| unsat | 0 |  |  |
|63058_55d6bef5390186355f11_26_QF_UFDTNIA.smt2                |    5.318s | 53.82MiB| sat | 0 |  |  |
|17512_5c1021b0faa6b6e1791b_21_QF_UFDTLIA.smt2                |    5.362s | 70.788MiB| unsat | 0 |  |  |
|41958_32933c5a1384696720a2_63_QF_UFLIA.smt2                  |    5.590s | 36.524MiB| sat | 0 |  |  |
|72771_f9d228efc97cf1458e38_64_QF_UFLIA.smt2                  |    5.662s | 43.02MiB| sat | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_69_QF_UFDTNIA.smt2    |    5.753s | 42.596MiB| sat | 0 |  |  |
|72771_f9d228efc97cf1458e38_64_QF_UFDTNIA.smt2                |    5.775s | 32.02MiB| sat | 0 |  |  |
|38347_092cc73601c78e45f4f9_56_QF_UFDTNIA.smt2                |    5.796s | 47.5MiB| sat | 0 |  |  |
|63058_64ab9a7ef7b6c3492507_24_QF_UFDTLIA.smt2                |    5.848s | 67.768MiB| sat | 0 |  |  |
|72771_f9d228efc97cf1458e38_64_QF_UFDTLIA.smt2                |    5.879s | 34.176MiB| sat | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_68_QF_UFNIA.smt2      |    5.988s | 49.892MiB| sat | 0 |  |  |
|39657_2866defdd1f2434b69ab_48_QF_UFDTLIA.smt2                |    6.052s | 33.208MiB| sat | 0 |  |  |
|41958_45c688a4814eb926c254_59_QF_UFNIA.smt2                  |    6.396s | 77.828MiB| sat | 0 |  |  |
|63058_64ab9a7ef7b6c3492507_24_QF_UFDTNIA.smt2                |    6.477s | 75.92MiB| sat | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_66_QF_UFNIA.smt2      |    6.548s | 46.132MiB| sat | 0 |  |  |
|63058_64ab9a7ef7b6c3492507_23_QF_UFDTNIA.smt2                |    6.640s | 53.536MiB| sat | 0 |  |  |
|3106_1c933134166dbad31f79_41_QF_UFDTNIA.smt2                 |    7.399s | 38.952MiB| sat | 0 |  |  |
|63058_64ab9a7ef7b6c3492507_22_QF_UFDTNIA.smt2                |    8.206s | 48.204MiB| sat | 0 |  |  |
|940_590f27b1c3c800d3243e_31_QF_UFNIA.smt2                    |    8.440s | 65.176MiB| sat | 0 |  |  |
|72658_63104dadde9c6026353f_70_QF_UFLIA.smt2                  |    9.008s | 75.36MiB| sat | 0 |  |  |
|66603_accdadf23a1cf70ae043_75_QF_UFDTLIA.smt2                |    9.129s | 129.0MiB| sat | 0 |  |  |
|44289_4066055e0f64d96da11a_14_QF_UFLIA.smt2                  |    9.223s | 101.0MiB| sat | 0 |  |  |
|17512_5c1021b0faa6b6e1791b_21_QF_UFLIA.smt2                  |    9.368s | 86.116MiB| unsat | 0 |  |  |
|17512_5c1021b0faa6b6e1791b_19_QF_UFDTLIA.smt2                |    9.410s | 74.668MiB| unsat | 0 |  |  |
|44289_e5a2e5c780236919ee6a_18_QF_UFLIA.smt2                  |    9.650s | 93.844MiB| sat | 0 |  |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_11_QF_UFDTLIA.smt2    |   10.000s | 82.22MiB| sat | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_68_QF_UFDTNIA.smt2    |   10.100s | 47.884MiB| sat | 0 |  |  |
|41958_32933c5a1384696720a2_62_QF_UFLIA.smt2                  |   11.326s | 45.2MiB| sat | 0 |  |  |
|72771_f9d228efc97cf1458e38_64_QF_UFNIA.smt2                  |   11.452s | 45.984MiB| sat | 0 |  |  |
|940_590f27b1c3c800d3243e_31_QF_UFDTNIA.smt2                  |   12.449s | 60.58MiB| sat | 0 |  |  |
|66603_accdadf23a1cf70ae043_76_QF_UFDTNIA.smt2                |   12.466s | 144.0MiB| unsat | 0 |  |  |
|3106_1c933134166dbad31f79_41_QF_UFLIA.smt2                   |   12.539s | 56.26MiB| sat | 0 |  |  |
|3106_1c933134166dbad31f79_39_QF_UFDTLIA.smt2                 |   12.637s | 80.996MiB| sat | 0 |  |  |
|93493_27ab26d56d60426da02e50231269b6ff_51_QF_UFDTLIA.smt2    |   12.664s | 75.276MiB| sat | 0 |  |  |
|39657_2866defdd1f2434b69ab_47_QF_UFDTLIA.smt2                |   12.786s | 36.704MiB| sat | 0 |  |  |
|17512_5c1021b0faa6b6e1791b_21_QF_UFNIA.smt2                  |   12.902s | 33.016MiB| unsat | 0 |  |  |
|44289_e5a2e5c780236919ee6a_17_QF_UFLIA.smt2                  |   13.149s | 101.0MiB| sat | 0 |  |  |
|65782_cd31513fdcd15701933b_5_QF_UFDTLIA.smt2                 |   14.324s | 100.0MiB| sat | 0 |  |  |
|93493_27ab26d56d60426da02e50231269b6ff_51_QF_UFDTNIA.smt2    |   14.447s | 77.656MiB| sat | 0 |  |  |
|3106_afb7bc55417e43d7a22790c3576f04fc_37_QF_UFDTLIA.smt2     |   14.562s | 85.468MiB| sat | 0 |  |  |
|41958_32933c5a1384696720a2_62_QF_UFDTLIA.smt2                |   14.842s | 57.012MiB| sat | 0 |  |  |
|39657_1c7158801cd59dc13f05_46_QF_UFDTNIA.smt2                |   14.980s | 62.724MiB| sat | 0 |  |  |
|93493_1fdb6cc8eb9c4363b5838af9eb8c7f1f_53_QF_UFDTLIA.smt2    |   15.129s | 85.604MiB| sat | 0 |  |  |
|39657_1c7158801cd59dc13f05_45_QF_UFDTNIA.smt2                |   15.284s | 63.276MiB| sat | 0 |  |  |
|66603_accdadf23a1cf70ae043_76_QF_UFDTLIA.smt2                |   15.333s | 238.0MiB| unsat | 0 |  |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_13_QF_UFLIA.smt2      |   15.652s | 122.0MiB| sat | 0 |  |  |
|39657_1c7158801cd59dc13f05_44_QF_UFDTNIA.smt2                |   16.163s | 67.872MiB| sat | 0 |  |  |
|38347_092cc73601c78e45f4f9_56_QF_UFNIA.smt2                  |   17.063s | 68.848MiB| sat | 0 |  |  |
|30078_f817a923328f75af7e60_27_QF_UFDTLIA.smt2                |   17.473s | 143.0MiB| unsat | 0 |  |  |
|83314_a702bf8b823398c9e37a_1_UFDTNIA.smt2                    |   18.046s | 44.784MiB| sat | 0 |  |  |
|66603_accdadf23a1cf70ae043_72_QF_UFLIA.smt2                  |   18.392s | 558.0MiB| unsat | 0 |  |  |
|30078_f817a923328f75af7e60_27_QF_UFDTNIA.smt2                |   19.065s | 127.0MiB| unsat | 0 |  |  |
|39657_1c7158801cd59dc13f05_45_QF_UFLIA.smt2                  |   19.174s | 77.856MiB| sat | 0 |  |  |
|3106_1c933134166dbad31f79_41_QF_UFNIA.smt2                   |   19.655s | 65.332MiB| sat | 0 |  |  |
|17512_5c1021b0faa6b6e1791b_20_QF_UFNIA.smt2                  |   19.868s | 39.88MiB| unsat | 0 |  |  |
|3106_afb7bc55417e43d7a22790c3576f04fc_37_QF_UFDTNIA.smt2     |   19.913s | 67.952MiB| sat | 0 |  |  |
|44289_b077fc096b3d41cba49f8628caff7fa5_16_QF_UFDTNIA.smt2    |   20.679s | 99.768MiB| sat | 0 |  |  |
|17512_5c1021b0faa6b6e1791b_21_QF_UFDTNIA.smt2                |   21.707s | 33.264MiB| unsat | 0 |  |  |
|44289_b077fc096b3d41cba49f8628caff7fa5_16_QF_UFDTLIA.smt2    |   22.074s | 139.0MiB| sat | 0 |  |  |
|3106_1c933134166dbad31f79_39_QF_UFDTNIA.smt2                 |   22.560s | 71.752MiB| sat | 0 |  |  |
|66603_accdadf23a1cf70ae043_74_QF_UFDTLIA.smt2                |   22.627s | 143.0MiB| sat | 0 |  |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_12_QF_UFLIA.smt2      |   22.955s | 177.0MiB| sat | 0 |  |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_10_QF_UFLIA.smt2      |   24.256s | 228.0MiB| unsat | 0 |  |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_11_QF_UFDTNIA.smt2    |   25.110s | 82.956MiB| sat | 0 |  |  |
|83314_a702bf8b823398c9e37a_2_UFDTNIA.smt2                    |   26.491s | 137.0MiB| sat | 0 |  |  |
|30078_f817a923328f75af7e60_28_QF_UFDTNIA.smt2                |   26.670s | 138.0MiB| unsat | 0 |  |  |
|83314_a702bf8b823398c9e37a_1_UFNIA.smt2                      |   26.800s | 48.788MiB| sat | 0 |  |  |
|39657_2866defdd1f2434b69ab_48_QF_UFLIA.smt2                  |   27.076s | 52.016MiB| sat | 0 |  |  |
|63058_64ab9a7ef7b6c3492507_22_QF_UFLIA.smt2                  |   27.613s | 181.0MiB| sat | 0 |  |  |
|39657_1c7158801cd59dc13f05_46_QF_UFLIA.smt2                  |   28.741s | 106.0MiB| sat | 0 |  |  |
|44788_1965f0d6d94d5d8054ba_35_QF_UFDTNIA.smt2                |   29.131s | 34.748MiB| unsat | 0 |  |  |
|44788_1965f0d6d94d5d8054ba_36_QF_UFLIA.smt2                  |   30.702s | 349.0MiB| unsat | 0 |  |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_13_QF_UFNIA.smt2      |   30.789s | 193.0MiB| sat | 0 |  |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_10_QF_UFDTLIA.smt2    |   30.897s | 206.0MiB| unsat | 0 |  |  |
|63058_64ab9a7ef7b6c3492507_24_QF_UFLIA.smt2                  |   32.724s | 219.0MiB| sat | 0 |  |  |
|3106_afb7bc55417e43d7a22790c3576f04fc_37_QF_UFNIA.smt2       |   33.390s | 128.0MiB| sat | 0 |  |  |
|30078_f817a923328f75af7e60_28_QF_UFDTLIA.smt2                |   33.453s | 149.0MiB| unsat | 0 |  |  |
|30078_f817a923328f75af7e60_27_QF_UFNIA.smt2                  |   33.630s | 422.0MiB| unsat | 0 |  |  |
|41958_45c688a4814eb926c254_59_QF_UFLIA.smt2                  |   34.509s | 107.0MiB| sat | 0 |  |  |
|66603_accdadf23a1cf70ae043_72_QF_UFNIA.smt2                  |   34.588s | 613.0MiB| unsat | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_65_QF_UFDTNIA.smt2    |   37.537s | 63.328MiB| sat | 0 |  |  |
|30078_f817a923328f75af7e60_27_QF_UFLIA.smt2                  |   41.219s | 454.0MiB| unsat | 0 |  |  |
|44289_e5a2e5c780236919ee6a_18_QF_UFNIA.smt2                  |   41.334s | 124.0MiB| sat | 0 |  |  |
|93493_1fdb6cc8eb9c4363b5838af9eb8c7f1f_53_QF_UFDTNIA.smt2    |   42.930s | 103.0MiB| sat | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_69_QF_UFNIA.smt2      |   44.223s | 52.296MiB| sat | 0 |  |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_10_QF_UFDTNIA.smt2    |   44.463s | 91.536MiB| unsat | 0 |  |  |
|39657_1c7158801cd59dc13f05_44_QF_UFLIA.smt2                  |   46.539s | 142.0MiB| sat | 0 |  |  |
|83314_a702bf8b823398c9e37a_0_UFDTLIA.smt2                    |   47.864s | 170.0MiB| sat | 0 |  |  |
|44788_1965f0d6d94d5d8054ba_36_QF_UFDTLIA.smt2                |   49.140s | 454.0MiB| unsat | 0 |  |  |
|38347_092cc73601c78e45f4f9_56_QF_UFDTLIA.smt2                |   49.231s | 184.0MiB| sat | 0 |  |  |
|940_590f27b1c3c800d3243e_32_QF_UFNIA.smt2                    |   55.244s | 198.0MiB| sat | 0 |  |  |
|30078_f817a923328f75af7e60_28_QF_UFNIA.smt2                  |   56.370s | 433.0MiB| unsat | 0 |  |  |
|93493_27ab26d56d60426da02e50231269b6ff_51_QF_UFLIA.smt2      |   60.854s | 137.0MiB| sat | 0 |  |  |
|83314_a702bf8b823398c9e37a_2_UFNIA.smt2                      |   61.923s | 235.0MiB| sat | 0 |  |  |
|93493_5990a6bf5f2740164f77_50_QF_UFDTLIA.smt2                |   63.494s | 241.0MiB| sat | 0 |  |  |
|3106_1c933134166dbad31f79_39_QF_UFNIA.smt2                   |   64.119s | 141.0MiB| sat | 0 |  |  |
|940_590f27b1c3c800d3243e_33_QF_UFNIA.smt2                    |   65.317s | 254.0MiB| sat | 0 |  |  |
|30078_f817a923328f75af7e60_28_QF_UFLIA.smt2                  |   68.696s | 508.0MiB| unsat | 0 |  |  |
|3106_1c933134166dbad31f79_39_QF_UFLIA.smt2                   |   70.564s | 196.0MiB| sat | 0 |  |  |
|41958_32933c5a1384696720a2_61_QF_UFDTLIA.smt2                |   77.041s | 101.0MiB| sat | 0 |  |  |
|44289_b077fc096b3d41cba49f8628caff7fa5_16_QF_UFLIA.smt2      |   80.270s | 202.0MiB| sat | 0 |  |  |
|83314_a702bf8b823398c9e37a_0_UFLIA.smt2                      |   80.315s | 184.0MiB| sat | 0 |  |  |
|44289_e5a2e5c780236919ee6a_17_QF_UFNIA.smt2                  |   82.747s | 152.0MiB| sat | 0 |  |  |
|940_590f27b1c3c800d3243e_29_QF_UFDTNIA.smt2                  |   84.568s | 214.0MiB| sat | 0 |  |  |
|39657_1c7158801cd59dc13f05_44_QF_UFNIA.smt2                  |   91.302s | 149.0MiB| sat | 0 |  |  |
|39657_2866defdd1f2434b69ab_47_QF_UFLIA.smt2                  |   91.303s | 80.492MiB| sat | 0 |  |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_11_QF_UFNIA.smt2      |   91.869s | 311.0MiB| sat | 0 |  |  |
|44289_4066055e0f64d96da11a_15_QF_UFNIA.smt2                  |   92.352s | 610.0MiB| sat | 0 |  |  |
|39657_1c7158801cd59dc13f05_45_QF_UFNIA.smt2                  |   97.321s | 120.0MiB| sat | 0 |  |  |
|940_590f27b1c3c800d3243e_31_QF_UFDTLIA.smt2                  |   98.850s | 243.0MiB| sat | 0 |  |  |
|39657_1c7158801cd59dc13f05_46_QF_UFNIA.smt2                  |  105.719s | 149.0MiB| sat | 0 |  |  |
|44788_1965f0d6d94d5d8054ba_36_QF_UFDTNIA.smt2                |  120.015s | 54.692MiB| timeout | 0 |  |  |
|38347_092cc73601c78e45f4f9_55_QF_UFDTNIA.smt2                |  120.015s | 25.056MiB| timeout | 0 |  |  |
|41958_32933c5a1384696720a2_63_QF_UFNIA.smt2                  |  120.015s | 45.42MiB| timeout | 0 |  |  |
|38347_092cc73601c78e45f4f9_57_QF_UFLIA.smt2                  |  120.017s | 39.204MiB| timeout | 0 |  |  |
|41958_32933c5a1384696720a2_61_QF_UFDTNIA.smt2                |  120.017s | 65.424MiB| timeout | 0 |  |  |
|52759_b3ecd2335fd16ec2eee2_9_UFDTNIA.smt2                    |  120.018s | 61.16MiB| timeout | 0 |  |  |
|72658_63104dadde9c6026353f_71_QF_UFDTNIA.smt2                |  120.020s | 36.608MiB| timeout | 0 |  |  |
|83314_a702bf8b823398c9e37a_3_UFNIA.smt2                      |  120.020s | 98.0MiB| timeout | 0 |  |  |
|38347_092cc73601c78e45f4f9_57_QF_UFDTLIA.smt2                |  120.022s | 32.5MiB| timeout | 0 |  |  |
|41958_32933c5a1384696720a2_61_QF_UFLIA.smt2                  |  120.024s | 118.0MiB| timeout | 0 |  |  |
|93493_1fdb6cc8eb9c4363b5838af9eb8c7f1f_53_QF_UFLIA.smt2      |  120.026s | 147.0MiB| timeout | 0 |  |  |
|63058_64ab9a7ef7b6c3492507_23_QF_UFNIA.smt2                  |  120.030s | 186.0MiB| timeout | 0 |  |  |
|38347_525a1ca0331f2bcbf520_54_QF_UFDTNIA.smt2                |  120.030s | 233.0MiB| timeout | 0 |  |  |
|38347_092cc73601c78e45f4f9_57_QF_UFDTNIA.smt2                |  120.030s | 30.32MiB| timeout | 0 |  |  |
|940_590f27b1c3c800d3243e_31_QF_UFLIA.smt2                    |  120.031s | 197.0MiB| timeout | 0 |  |  |
|93493_798593962ee29ad45ac8_52_QF_UFDTLIA.smt2                |  120.031s | 241.0MiB| timeout | 0 |  |  |
|72658_63104dadde9c6026353f_70_QF_UFNIA.smt2                  |  120.032s | 35.524MiB| timeout | 0 |  |  |
|63058_55d6bef5390186355f11_26_QF_UFLIA.smt2                  |  120.032s | 201.0MiB| timeout | 0 |  |  |
|44289_b077fc096b3d41cba49f8628caff7fa5_16_QF_UFNIA.smt2      |  120.033s | 174.0MiB| timeout | 0 |  |  |
|63058_64ab9a7ef7b6c3492507_24_QF_UFNIA.smt2                  |  120.033s | 234.0MiB| timeout | 0 |  |  |
|83314_a702bf8b823398c9e37a_3_UFDTNIA.smt2                    |  120.033s | 55.568MiB| timeout | 0 |  |  |
|83314_a702bf8b823398c9e37a_4_UFDTNIA.smt2                    |  120.034s | 207.0MiB| timeout | 0 |  |  |
|93493_1fdb6cc8eb9c4363b5838af9eb8c7f1f_53_QF_UFNIA.smt2      |  120.034s | 214.0MiB| timeout | 0 |  |  |
|38347_525a1ca0331f2bcbf520_54_QF_UFNIA.smt2                  |  120.034s | 259.0MiB| timeout | 0 |  |  |
|41958_32933c5a1384696720a2_62_QF_UFNIA.smt2                  |  120.035s | 34.744MiB| timeout | 0 |  |  |
|65782_cd31513fdcd15701933b_5_QF_UFNIA.smt2                   |  120.036s | 66.74MiB| timeout | 0 |  |  |
|63058_64ab9a7ef7b6c3492507_22_QF_UFNIA.smt2                  |  120.036s | 228.0MiB| timeout | 0 |  |  |
|93493_4ea6163ed03941199c785278ccc42812_49_QF_UFDTNIA.smt2    |  120.038s | 238.0MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_67_QF_UFNIA.smt2      |  120.038s | 72.308MiB| timeout | 0 |  |  |
|63058_55d6bef5390186355f11_26_QF_UFNIA.smt2                  |  120.039s | 286.0MiB| timeout | 0 |  |  |
|66603_accdadf23a1cf70ae043_73_QF_UFDTNIA.smt2                |  120.040s | 171.0MiB| timeout | 0 |  |  |
|66603_accdadf23a1cf70ae043_74_QF_UFDTNIA.smt2                |  120.041s | 163.0MiB| timeout | 0 |  |  |
|38347_092cc73601c78e45f4f9_55_QF_UFLIA.smt2                  |  120.042s | 54.768MiB| timeout | 0 |  |  |
|38347_092cc73601c78e45f4f9_56_QF_UFLIA.smt2                  |  120.043s | 230.0MiB| timeout | 0 |  |  |
|44788_1965f0d6d94d5d8054ba_34_QF_UFDTNIA.smt2                |  120.043s | 46.984MiB| timeout | 0 |  |  |
|93493_5990a6bf5f2740164f77_50_QF_UFLIA.smt2                  |  120.043s | 309.0MiB| timeout | 0 |  |  |
|17512_5c1021b0faa6b6e1791b_19_QF_UFLIA.smt2                  |  120.044s | 148.0MiB| timeout | 0 |  |  |
|44788_1965f0d6d94d5d8054ba_35_QF_UFNIA.smt2                  |  120.044s | 61.364MiB| timeout | 0 |  |  |
|38347_092cc73601c78e45f4f9_55_QF_UFNIA.smt2                  |  120.045s | 28.248MiB| timeout | 0 |  |  |
|93493_4ea6163ed03941199c785278ccc42812_49_QF_UFDTLIA.smt2    |  120.046s | 255.0MiB| timeout | 0 |  |  |
|44788_1965f0d6d94d5d8054ba_34_QF_UFNIA.smt2                  |  120.047s | 42.224MiB| timeout | 0 |  |  |
|38347_092cc73601c78e45f4f9_55_QF_UFDTLIA.smt2                |  120.047s | 27.408MiB| timeout | 0 |  |  |
|83314_a702bf8b823398c9e37a_1_UFLIA.smt2                      |  120.047s | 334.0MiB| timeout | 0 |  |  |
|93493_4ea6163ed03941199c785278ccc42812_49_QF_UFLIA.smt2      |  120.047s | 271.0MiB| timeout | 0 |  |  |
|940_590f27b1c3c800d3243e_32_QF_UFLIA.smt2                    |  120.048s | 364.0MiB| timeout | 0 |  |  |
|72658_63104dadde9c6026353f_70_QF_UFDTNIA.smt2                |  120.048s | 38.536MiB| timeout | 0 |  |  |
|72658_63104dadde9c6026353f_71_QF_UFNIA.smt2                  |  120.048s | 60.148MiB| timeout | 0 |  |  |
|93493_798593962ee29ad45ac8_52_QF_UFLIA.smt2                  |  120.049s | 329.0MiB| timeout | 0 |  |  |
|52759_b3ecd2335fd16ec2eee2_9_UFNIA.smt2                      |  120.050s | 62.908MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_65_QF_UFNIA.smt2      |  120.050s | 103.0MiB| timeout | 0 |  |  |
|940_590f27b1c3c800d3243e_30_QF_UFNIA.smt2                    |  120.052s | 309.0MiB| timeout | 0 |  |  |
|44788_1965f0d6d94d5d8054ba_36_QF_UFNIA.smt2                  |  120.052s | 68.616MiB| timeout | 0 |  |  |
|41958_32933c5a1384696720a2_63_QF_UFDTNIA.smt2                |  120.052s | 41.032MiB| timeout | 0 |  |  |
|63058_64ab9a7ef7b6c3492507_23_QF_UFLIA.smt2                  |  120.053s | 182.0MiB| timeout | 0 |  |  |
|52759_b3ecd2335fd16ec2eee2_9_UFLIA.smt2                      |  120.054s | 88.016MiB| timeout | 0 |  |  |
|65782_cd31513fdcd15701933b_5_QF_UFDTNIA.smt2                 |  120.055s | 70.848MiB| timeout | 0 |  |  |
|940_590f27b1c3c800d3243e_32_QF_UFDTNIA.smt2                  |  120.058s | 292.0MiB| timeout | 0 |  |  |
|93493_27ab26d56d60426da02e50231269b6ff_51_QF_UFNIA.smt2      |  120.060s | 171.0MiB| timeout | 0 |  |  |
|83314_a702bf8b823398c9e37a_2_UFLIA.smt2                      |  120.060s | 245.0MiB| timeout | 0 |  |  |
|17512_5c1021b0faa6b6e1791b_20_QF_UFDTNIA.smt2                |  120.061s | 54.884MiB| timeout | 0 |  |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_12_QF_UFNIA.smt2      |  120.062s | 237.0MiB| timeout | 0 |  |  |
|3106_afb7bc55417e43d7a22790c3576f04fc_37_QF_UFLIA.smt2       |  120.063s | 162.0MiB| timeout | 0 |  |  |
|940_590f27b1c3c800d3243e_29_QF_UFLIA.smt2                    |  120.064s | 409.0MiB| timeout | 0 |  |  |
|41958_32933c5a1384696720a2_62_QF_UFDTNIA.smt2                |  120.064s | 35.624MiB| timeout | 0 |  |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_11_QF_UFLIA.smt2      |  120.065s | 265.0MiB| timeout | 0 |  |  |
|93493_798593962ee29ad45ac8_52_QF_UFDTNIA.smt2                |  120.065s | 100.0MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_67_QF_UFDTNIA.smt2    |  120.066s | 97.016MiB| timeout | 0 |  |  |
|17512_5c1021b0faa6b6e1791b_20_QF_UFDTLIA.smt2                |  120.067s | 572.0MiB| timeout | 0 |  |  |
|83314_a702bf8b823398c9e37a_2_UFDTLIA.smt2                    |  120.068s | 285.0MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_66_QF_UFDTNIA.smt2    |  120.070s | 96.62MiB| timeout | 0 |  |  |
|65782_cd31513fdcd15701933b_5_QF_UFLIA.smt2                   |  120.070s | 244.0MiB| timeout | 0 |  |  |
|38347_092cc73601c78e45f4f9_57_QF_UFNIA.smt2                  |  120.070s | 40.824MiB| timeout | 0 |  |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_10_QF_UFNIA.smt2      |  120.072s | 293.0MiB| timeout | 0 |  |  |
|940_590f27b1c3c800d3243e_33_QF_UFDTNIA.smt2                  |  120.074s | 301.0MiB| timeout | 0 |  |  |
|93493_798593962ee29ad45ac8_52_QF_UFNIA.smt2                  |  120.075s | 194.0MiB| timeout | 0 |  |  |
|3106_1c933134166dbad31f79_38_QF_UFDTNIA.smt2                 |  120.076s | 295.0MiB| timeout | 0 |  |  |
|940_590f27b1c3c800d3243e_29_QF_UFNIA.smt2                    |  120.077s | 352.0MiB| timeout | 0 |  |  |
|72658_63104dadde9c6026353f_71_QF_UFDTLIA.smt2                |  120.077s | 332.0MiB| timeout | 0 |  |  |
|940_590f27b1c3c800d3243e_32_QF_UFDTLIA.smt2                  |  120.079s | 387.0MiB| timeout | 0 |  |  |
|72658_63104dadde9c6026353f_71_QF_UFLIA.smt2                  |  120.080s | 257.0MiB| timeout | 0 |  |  |
|940_590f27b1c3c800d3243e_33_QF_UFLIA.smt2                    |  120.082s | 387.0MiB| timeout | 0 |  |  |
|93493_5990a6bf5f2740164f77_50_QF_UFNIA.smt2                  |  120.082s | 354.0MiB| timeout | 0 |  |  |
|940_590f27b1c3c800d3243e_30_QF_UFDTLIA.smt2                  |  120.083s | 408.0MiB| timeout | 0 |  |  |
|940_590f27b1c3c800d3243e_29_QF_UFDTLIA.smt2                  |  120.083s | 389.0MiB| timeout | 0 |  |  |
|3106_1c933134166dbad31f79_38_QF_UFDTLIA.smt2                 |  120.083s | 315.0MiB| timeout | 0 |  |  |
|83314_a702bf8b823398c9e37a_4_UFNIA.smt2                      |  120.084s | 309.0MiB| timeout | 0 |  |  |
|52759_b3ecd2335fd16ec2eee2_9_UFDTLIA.smt2                    |  120.085s | 99.0MiB| timeout | 0 |  |  |
|93493_4ea6163ed03941199c785278ccc42812_49_QF_UFNIA.smt2      |  120.086s | 278.0MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_66_QF_UFLIA.smt2      |  120.087s | 762.0MiB| timeout | 0 |  |  |
|93493_5990a6bf5f2740164f77_50_QF_UFDTNIA.smt2                |  120.087s | 289.0MiB| timeout | 0 |  |  |
|44289_4066055e0f64d96da11a_14_QF_UFNIA.smt2                  |  120.087s | 121.0MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_68_QF_UFDTLIA.smt2    |  120.090s | 680.0MiB| timeout | 0 |  |  |
|940_590f27b1c3c800d3243e_33_QF_UFDTLIA.smt2                  |  120.090s | 435.0MiB| timeout | 0 |  |  |
|3106_1c933134166dbad31f79_38_QF_UFNIA.smt2                   |  120.093s | 755.0MiB| timeout | 0 |  |  |
|940_590f27b1c3c800d3243e_30_QF_UFLIA.smt2                    |  120.094s | 387.0MiB| timeout | 0 |  |  |
|940_590f27b1c3c800d3243e_30_QF_UFDTNIA.smt2                  |  120.095s | 213.0MiB| timeout | 0 |  |  |
|66603_accdadf23a1cf70ae043_75_QF_UFDTNIA.smt2                |  120.095s | 189.0MiB| timeout | 0 |  |  |
|83314_a702bf8b823398c9e37a_3_UFDTLIA.smt2                    |  120.098s | 560.0MiB| timeout | 0 |  |  |
|83314_a702bf8b823398c9e37a_3_UFLIA.smt2                      |  120.104s | 585.0MiB| timeout | 0 |  |  |
|17512_5c1021b0faa6b6e1791b_20_QF_UFLIA.smt2                  |  120.105s | 521.0MiB| timeout | 0 |  |  |
|83314_a702bf8b823398c9e37a_4_UFLIA.smt2                      |  120.108s | 731.0MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_68_QF_UFLIA.smt2      |  120.108s | 720.0MiB| timeout | 0 |  |  |
|44289_4066055e0f64d96da11a_15_QF_UFLIA.smt2                  |  120.110s | 579.0MiB| timeout | 0 |  |  |
|66603_accdadf23a1cf70ae043_73_QF_UFDTLIA.smt2                |  120.115s | 331.0MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_69_QF_UFDTLIA.smt2    |  120.119s | 581.0MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_69_QF_UFLIA.smt2      |  120.122s | 643.0MiB| timeout | 0 |  |  |
|83314_a702bf8b823398c9e37a_4_UFDTLIA.smt2                    |  120.122s | 782.0MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_66_QF_UFDTLIA.smt2    |  120.124s | 818.0MiB| timeout | 0 |  |  |
|83314_a702bf8b823398c9e37a_1_UFDTLIA.smt2                    |  120.125s | 401.0MiB| timeout | 0 |  |  |
|66603_accdadf23a1cf70ae043_76_QF_UFLIA.smt2                  |  120.138s | 1054.0MiB| timeout | 0 |  |  |
|66603_accdadf23a1cf70ae043_74_QF_UFNIA.smt2                  |  120.141s | 1057.0MiB| timeout | 0 |  |  |
|66603_accdadf23a1cf70ae043_75_QF_UFLIA.smt2                  |  120.153s | 1262.0MiB| timeout | 0 |  |  |
|66603_accdadf23a1cf70ae043_73_QF_UFNIA.smt2                  |  120.156s | 747.0MiB| timeout | 0 |  |  |
|3106_1c933134166dbad31f79_38_QF_UFLIA.smt2                   |  120.157s | 699.0MiB| timeout | 0 |  |  |
|66603_accdadf23a1cf70ae043_73_QF_UFLIA.smt2                  |  120.165s | 840.0MiB| timeout | 0 |  |  |
|66603_accdadf23a1cf70ae043_75_QF_UFNIA.smt2                  |  120.177s | 1096.0MiB| timeout | 0 |  |  |
|66603_accdadf23a1cf70ae043_76_QF_UFNIA.smt2                  |  120.180s | 1603.0MiB| timeout | 0 |  |  |
|66603_accdadf23a1cf70ae043_74_QF_UFLIA.smt2                  |  120.184s | 1271.0MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_67_QF_UFLIA.smt2      |  120.234s | 2103.0MiB| timeout | 0 |  |  |
|38347_525a1ca0331f2bcbf520_54_QF_UFLIA.smt2                  |  120.238s | 2050.0MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_67_QF_UFDTLIA.smt2    |  120.248s | 2085.0MiB| timeout | 0 |  |  |
|38347_525a1ca0331f2bcbf520_54_QF_UFDTLIA.smt2                |  120.255s | 2205.0MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_65_QF_UFDTLIA.smt2    |  120.282s | 2591.0MiB| timeout | 0 |  |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_65_QF_UFLIA.smt2      |  120.299s | 2434.0MiB| timeout | 0 |  |  |
