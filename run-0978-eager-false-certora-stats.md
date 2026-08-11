# .

* SAT 105
* UNSAT 23
* TIMEOUT 180
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: arith.nl.optimize_bounds_eager=false on certora, branch nla-avoid-grobner-horner (8195d4f77)
Job tag: eager-false-certora
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 8195d4f771ca40f94dd4635c0ea6c9f296144a05
Z3 branch: nla-avoid-grobner-horner
Z3 options: "-T:20 model_validate=true smt.arith.solver=6 smt.arith.nl.optimize_bounds_eager=false"
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
|65782_cd31513fdcd15701933b_6_QF_UFLIA.smt2                   |    0.058s | 22.368MiB| sat | 0 |  |
|3106_1c933134166dbad31f79_40_QF_UFDTNIA.smt2                 |    0.079s | 24.108MiB| sat | 0 |  |
|65782_cd31513fdcd15701933b_6_QF_UFDTLIA.smt2                 |    0.083s | 22.368MiB| sat | 0 |  |
|65782_cd31513fdcd15701933b_6_QF_UFDTNIA.smt2                 |    0.118s | 21.904MiB| sat | 0 |  |
|63058_aa742630eef64f949de269382c1f9035_25_UFNIA.smt2         |    0.121s | 24.384MiB| unsat | 0 |  |
|63058_aa742630eef64f949de269382c1f9035_25_UFDTNIA.smt2       |    0.141s | 24.32MiB| unsat | 0 |  |
|39657_2866defdd1f2434b69ab_48_QF_UFDTNIA.smt2                |    0.146s | 27.708MiB| sat | 0 |  |
|63058_aa742630eef64f949de269382c1f9035_25_UFLIA.smt2         |    0.155s | 24.652MiB| unsat | 0 |  |
|65782_cd31513fdcd15701933b_6_QF_UFNIA.smt2                   |    0.178s | 22.068MiB| sat | 0 |  |
|3106_1c933134166dbad31f79_40_QF_UFLIA.smt2                   |    0.192s | 25.656MiB| sat | 0 |  |
|65782_cd31513fdcd15701933b_7_QF_UFLIA.smt2                   |    0.218s | 26.788MiB| sat | 0 |  |
|63058_aa742630eef64f949de269382c1f9035_25_UFDTLIA.smt2       |    0.220s | 25.652MiB| unsat | 0 |  |
|3106_1c933134166dbad31f79_40_QF_UFDTLIA.smt2                 |    0.227s | 24.452MiB| sat | 0 |  |
|44788_1965f0d6d94d5d8054ba_34_QF_UFDTLIA.smt2                |    0.242s | 24.412MiB| sat | 0 |  |
|39657_2866defdd1f2434b69ab_47_QF_UFDTNIA.smt2                |    0.245s | 27.284MiB| sat | 0 |  |
|44788_1965f0d6d94d5d8054ba_34_QF_UFLIA.smt2                  |    0.246s | 26.636MiB| sat | 0 |  |
|38347_092cc73601c78e45f4f9_58_QF_UFDTLIA.smt2                |    0.333s | 29.596MiB| sat | 0 |  |
|11775_ad46e5b8db4748c51973_43_QF_UFDTLIA.smt2                |    0.343s | 34.492MiB| sat | 0 |  |
|38347_092cc73601c78e45f4f9_58_QF_UFDTNIA.smt2                |    0.350s | 28.932MiB| sat | 0 |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_13_QF_UFDTLIA.smt2    |    0.377s | 31.676MiB| sat | 0 |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_13_QF_UFDTNIA.smt2    |    0.392s | 31.384MiB| sat | 0 |  |
|41958_45c688a4814eb926c254_59_QF_UFDTNIA.smt2                |    0.397s | 32.54MiB| sat | 0 |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_12_QF_UFDTNIA.smt2    |    0.409s | 29.268MiB| sat | 0 |  |
|44788_1965f0d6d94d5d8054ba_35_QF_UFLIA.smt2                  |    0.523s | 37.14MiB| unsat | 0 |  |
|41958_45c688a4814eb926c254_60_QF_UFDTNIA.smt2                |    0.531s | 30.488MiB| sat | 0 |  |
|65782_cd31513fdcd15701933b_7_QF_UFDTLIA.smt2                 |    0.581s | 26.288MiB| sat | 0 |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_12_QF_UFDTLIA.smt2    |    0.602s | 34.704MiB| sat | 0 |  |
|65782_cd31513fdcd15701933b_8_QF_UFDTLIA.smt2                 |    0.631s | 26.74MiB| sat | 0 |  |
|65782_cd31513fdcd15701933b_8_QF_UFLIA.smt2                   |    0.675s | 27.772MiB| sat | 0 |  |
|38347_092cc73601c78e45f4f9_58_QF_UFNIA.smt2                  |    0.685s | 31.664MiB| sat | 0 |  |
|41958_45c688a4814eb926c254_60_QF_UFDTLIA.smt2                |    0.823s | 34.848MiB| sat | 0 |  |
|83314_a702bf8b823398c9e37a_0_UFNIA.smt2                      |    0.844s | 27.14MiB| sat | 0 |  |
|38347_092cc73601c78e45f4f9_58_QF_UFLIA.smt2                  |    0.913s | 32.332MiB| sat | 0 |  |
|39657_2866defdd1f2434b69ab_48_QF_UFNIA.smt2                  |    0.958s | 39.016MiB| sat | 0 |  |
|44788_1965f0d6d94d5d8054ba_35_QF_UFDTLIA.smt2                |    0.979s | 27.868MiB| unsat | 0 |  |
|11775_ad46e5b8db4748c51973_43_QF_UFLIA.smt2                  |    1.056s | 35.652MiB| sat | 0 |  |
|11775_ad46e5b8db4748c51973_42_QF_UFDTLIA.smt2                |    1.145s | 37.244MiB| sat | 0 |  |
|11775_ad46e5b8db4748c51973_42_QF_UFNIA.smt2                  |    1.169s | 32.4MiB| sat | 0 |  |
|72658_63104dadde9c6026353f_70_QF_UFDTLIA.smt2                |    1.254s | 57.048MiB| sat | 0 |  |
|11775_ad46e5b8db4748c51973_42_QF_UFDTNIA.smt2                |    1.276s | 30.328MiB| sat | 0 |  |
|11775_ad46e5b8db4748c51973_42_QF_UFLIA.smt2                  |    1.368s | 40.548MiB| sat | 0 |  |
|3106_1c933134166dbad31f79_40_QF_UFNIA.smt2                   |    1.409s | 25.276MiB| sat | 0 |  |
|3106_1c933134166dbad31f79_41_QF_UFDTLIA.smt2                 |    1.496s | 40.272MiB| sat | 0 |  |
|41958_45c688a4814eb926c254_60_QF_UFNIA.smt2                  |    1.573s | 50.556MiB| sat | 0 |  |
|41958_45c688a4814eb926c254_59_QF_UFDTLIA.smt2                |    1.627s | 40.048MiB| sat | 0 |  |
|11775_ad46e5b8db4748c51973_43_QF_UFNIA.smt2                  |    1.636s | 29.392MiB| unsat | 0 |  |
|44289_e5a2e5c780236919ee6a_17_QF_UFDTLIA.smt2                |    1.726s | 49.84MiB| sat | 0 |  |
|65782_cd31513fdcd15701933b_8_QF_UFNIA.smt2                   |    1.941s | 26.748MiB| sat | 0 |  |
|44289_4066055e0f64d96da11a_14_QF_UFDTLIA.smt2                |    1.983s | 44.056MiB| sat | 0 |  |
|44289_4066055e0f64d96da11a_14_QF_UFDTNIA.smt2                |    2.040s | 44.836MiB| sat | 0 |  |
|44289_e5a2e5c780236919ee6a_17_QF_UFDTNIA.smt2                |    2.292s | 51.26MiB| sat | 0 |  |
|39657_1c7158801cd59dc13f05_46_QF_UFDTLIA.smt2                |    2.338s | 52.764MiB| sat | 0 |  |
|65782_cd31513fdcd15701933b_8_QF_UFDTNIA.smt2                 |    2.343s | 26.32MiB| sat | 0 |  |
|39657_1c7158801cd59dc13f05_44_QF_UFDTLIA.smt2                |    2.363s | 53.292MiB| sat | 0 |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_11_QF_UFDTNIA.smt2    |    2.375s | 68.152MiB| sat | 0 |  |
|63058_55d6bef5390186355f11_26_QF_UFDTLIA.smt2                |    2.396s | 50.648MiB| sat | 0 |  |
|39657_1c7158801cd59dc13f05_45_QF_UFDTLIA.smt2                |    2.467s | 56.128MiB| sat | 0 |  |
|11775_ad46e5b8db4748c51973_43_QF_UFDTNIA.smt2                |    2.654s | 29.64MiB| unsat | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_69_QF_UFNIA.smt2      |    2.732s | 41.632MiB| sat | 0 |  |
|44289_e5a2e5c780236919ee6a_18_QF_UFDTLIA.smt2                |    2.785s | 52.16MiB| sat | 0 |  |
|63058_64ab9a7ef7b6c3492507_23_QF_UFDTLIA.smt2                |    2.801s | 51.42MiB| sat | 0 |  |
|17512_5c1021b0faa6b6e1791b_21_QF_UFLIA.smt2                  |    2.989s | 67.456MiB| unsat | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_68_QF_UFDTNIA.smt2    |    2.991s | 46.82MiB| sat | 0 |  |
|41958_32933c5a1384696720a2_63_QF_UFDTLIA.smt2                |    3.175s | 34.324MiB| sat | 0 |  |
|38347_092cc73601c78e45f4f9_56_QF_UFDTNIA.smt2                |    3.351s | 45.676MiB| sat | 0 |  |
|44289_e5a2e5c780236919ee6a_18_QF_UFDTNIA.smt2                |    3.526s | 52.836MiB| sat | 0 |  |
|63058_64ab9a7ef7b6c3492507_22_QF_UFDTLIA.smt2                |    3.659s | 47.096MiB| sat | 0 |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_11_QF_UFDTLIA.smt2    |    3.860s | 71.304MiB| sat | 0 |  |
|41958_32933c5a1384696720a2_63_QF_UFLIA.smt2                  |    3.924s | 35.296MiB| sat | 0 |  |
|17512_5c1021b0faa6b6e1791b_19_QF_UFNIA.smt2                  |    3.987s | 24.756MiB| unsat | 0 |  |
|83314_a702bf8b823398c9e37a_0_UFDTNIA.smt2                    |    3.994s | 29.52MiB| sat | 0 |  |
|44289_b077fc096b3d41cba49f8628caff7fa5_16_QF_UFDTNIA.smt2    |    4.157s | 57.34MiB| sat | 0 |  |
|83314_a702bf8b823398c9e37a_2_UFDTNIA.smt2                    |    4.415s | 72.628MiB| sat | 0 |  |
|63058_64ab9a7ef7b6c3492507_24_QF_UFDTLIA.smt2                |    4.429s | 67.292MiB| sat | 0 |  |
|41958_45c688a4814eb926c254_59_QF_UFNIA.smt2                  |    4.451s | 75.308MiB| sat | 0 |  |
|63058_64ab9a7ef7b6c3492507_23_QF_UFDTNIA.smt2                |    4.508s | 53.48MiB| sat | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_69_QF_UFDTNIA.smt2    |    4.646s | 43.812MiB| sat | 0 |  |
|66603_accdadf23a1cf70ae043_72_QF_UFDTNIA.smt2                |    4.813s | 86.996MiB| unsat | 0 |  |
|3106_1c933134166dbad31f79_41_QF_UFLIA.smt2                   |    4.816s | 51.748MiB| sat | 0 |  |
|44289_4066055e0f64d96da11a_15_QF_UFDTNIA.smt2                |    4.953s | 79.804MiB| sat | 0 |  |
|44289_4066055e0f64d96da11a_15_QF_UFDTLIA.smt2                |    4.997s | 79.24MiB| sat | 0 |  |
|66603_accdadf23a1cf70ae043_72_QF_UFDTLIA.smt2                |    5.028s | 106.0MiB| unsat | 0 |  |
|65782_cd31513fdcd15701933b_7_QF_UFNIA.smt2                   |    5.629s | 27.58MiB| unsat | 0 |  |
|17512_5c1021b0faa6b6e1791b_21_QF_UFDTLIA.smt2                |    5.631s | 68.768MiB| unsat | 0 |  |
|41958_45c688a4814eb926c254_60_QF_UFLIA.smt2                  |    5.778s | 62.608MiB| sat | 0 |  |
|41958_32933c5a1384696720a2_62_QF_UFDTLIA.smt2                |    5.927s | 47.188MiB| sat | 0 |  |
|72771_f9d228efc97cf1458e38_64_QF_UFDTLIA.smt2                |    6.023s | 33.536MiB| sat | 0 |  |
|72771_f9d228efc97cf1458e38_64_QF_UFDTNIA.smt2                |    6.030s | 31.548MiB| sat | 0 |  |
|39657_1c7158801cd59dc13f05_44_QF_UFLIA.smt2                  |    6.062s | 66.956MiB| sat | 0 |  |
|39657_2866defdd1f2434b69ab_48_QF_UFDTLIA.smt2                |    6.244s | 33.204MiB| sat | 0 |  |
|83314_a702bf8b823398c9e37a_1_UFNIA.smt2                      |    6.520s | 39.436MiB| sat | 0 |  |
|39657_2866defdd1f2434b69ab_47_QF_UFNIA.smt2                  |    6.860s | 35.368MiB| sat | 0 |  |
|63058_64ab9a7ef7b6c3492507_22_QF_UFDTNIA.smt2                |    7.001s | 47.636MiB| sat | 0 |  |
|66603_accdadf23a1cf70ae043_73_QF_UFDTNIA.smt2                |    7.254s | 97.08MiB| sat | 0 |  |
|17512_5c1021b0faa6b6e1791b_19_QF_UFDTNIA.smt2                |    7.302s | 25.4MiB| unsat | 0 |  |
|65782_cd31513fdcd15701933b_7_QF_UFDTNIA.smt2                 |    7.435s | 27.684MiB| unsat | 0 |  |
|39657_1c7158801cd59dc13f05_46_QF_UFDTNIA.smt2                |    7.713s | 61.46MiB| sat | 0 |  |
|72658_63104dadde9c6026353f_70_QF_UFLIA.smt2                  |    7.963s | 71.328MiB| sat | 0 |  |
|44289_e5a2e5c780236919ee6a_17_QF_UFLIA.smt2                  |    8.795s | 103.0MiB| sat | 0 |  |
|940_590f27b1c3c800d3243e_31_QF_UFDTNIA.smt2                  |    8.811s | 62.58MiB| sat | 0 |  |
|63058_55d6bef5390186355f11_26_QF_UFDTNIA.smt2                |    8.938s | 64.084MiB| sat | 0 |  |
|3106_1c933134166dbad31f79_41_QF_UFNIA.smt2                   |    9.433s | 53.992MiB| sat | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_65_QF_UFDTNIA.smt2    |    9.588s | 62.992MiB| sat | 0 |  |
|39657_1c7158801cd59dc13f05_46_QF_UFLIA.smt2                  |    9.644s | 75.168MiB| sat | 0 |  |
|44289_e5a2e5c780236919ee6a_18_QF_UFLIA.smt2                  |    9.807s | 94.448MiB| sat | 0 |  |
|66603_accdadf23a1cf70ae043_76_QF_UFDTNIA.smt2                |   10.088s | 144.0MiB| unsat | 0 |  |
|17512_5c1021b0faa6b6e1791b_21_QF_UFNIA.smt2                  |   10.104s | 31.752MiB| unsat | 0 |  |
|940_590f27b1c3c800d3243e_31_QF_UFNIA.smt2                    |   10.180s | 67.392MiB| sat | 0 |  |
|3106_afb7bc55417e43d7a22790c3576f04fc_37_QF_UFDTLIA.smt2     |   10.242s | 81.324MiB| sat | 0 |  |
|63058_64ab9a7ef7b6c3492507_24_QF_UFDTNIA.smt2                |   11.090s | 76.792MiB| sat | 0 |  |
|17512_5c1021b0faa6b6e1791b_21_QF_UFDTNIA.smt2                |   11.579s | 33.268MiB| unsat | 0 |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_11_QF_UFNIA.smt2      |   11.813s | 223.0MiB| sat | 0 |  |
|93493_27ab26d56d60426da02e50231269b6ff_51_QF_UFDTLIA.smt2    |   12.008s | 75.328MiB| sat | 0 |  |
|39657_2866defdd1f2434b69ab_48_QF_UFLIA.smt2                  |   13.011s | 44.276MiB| sat | 0 |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_13_QF_UFLIA.smt2      |   13.189s | 120.0MiB| sat | 0 |  |
|93493_27ab26d56d60426da02e50231269b6ff_51_QF_UFDTNIA.smt2    |   13.521s | 82.048MiB| sat | 0 |  |
|66603_accdadf23a1cf70ae043_75_QF_UFDTLIA.smt2                |   14.866s | 135.0MiB| sat | 0 |  |
|66603_accdadf23a1cf70ae043_72_QF_UFLIA.smt2                  |   15.301s | 559.0MiB| unsat | 0 |  |
|3106_afb7bc55417e43d7a22790c3576f04fc_37_QF_UFDTNIA.smt2     |   15.513s | 67.216MiB| sat | 0 |  |
|72771_f9d228efc97cf1458e38_64_QF_UFLIA.smt2                  |   16.132s | 48.132MiB| sat | 0 |  |
|39657_1c7158801cd59dc13f05_45_QF_UFDTNIA.smt2                |   16.762s | 64.376MiB| sat | 0 |  |
|3106_1c933134166dbad31f79_41_QF_UFDTNIA.smt2                 |   17.328s | 41.988MiB| sat | 0 |  |
|66603_accdadf23a1cf70ae043_76_QF_UFDTLIA.smt2                |   17.625s | 237.0MiB| unsat | 0 |  |
|63058_64ab9a7ef7b6c3492507_22_QF_UFLIA.smt2                  |   17.970s | 149.0MiB| sat | 0 |  |
|30078_f817a923328f75af7e60_27_QF_UFDTNIA.smt2                |   18.300s | 127.0MiB| unsat | 0 |  |
|30078_f817a923328f75af7e60_27_QF_UFDTLIA.smt2                |   18.811s | 151.0MiB| unsat | 0 |  |
|63058_55d6bef5390186355f11_26_QF_UFLIA.smt2                  |   18.883s | 207.0MiB| sat | 0 |  |
|39657_1c7158801cd59dc13f05_45_QF_UFLIA.smt2                  |   19.719s | 81.968MiB| sat | 0 |  |
|38347_092cc73601c78e45f4f9_55_QF_UFDTNIA.smt2                |   20.014s | 23.724MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_1_UFDTNIA.smt2                    |   20.017s | 48.2MiB| timeout | 0 |  |
|93493_1fdb6cc8eb9c4363b5838af9eb8c7f1f_53_QF_UFDTNIA.smt2    |   20.017s | 65.38MiB| timeout | 0 |  |
|38347_092cc73601c78e45f4f9_56_QF_UFNIA.smt2                  |   20.019s | 74.608MiB| timeout | 0 |  |
|39657_1c7158801cd59dc13f05_46_QF_UFNIA.smt2                  |   20.020s | 75.372MiB| timeout | 0 |  |
|3106_1c933134166dbad31f79_39_QF_UFLIA.smt2                   |   20.022s | 104.0MiB| timeout | 0 |  |
|3106_1c933134166dbad31f79_39_QF_UFNIA.smt2                   |   20.024s | 111.0MiB| timeout | 0 |  |
|66603_accdadf23a1cf70ae043_74_QF_UFDTLIA.smt2                |   20.026s | 130.0MiB| timeout | 0 |  |
|72658_63104dadde9c6026353f_70_QF_UFNIA.smt2                  |   20.028s | 38.876MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_67_QF_UFDTNIA.smt2    |   20.028s | 58.716MiB| timeout | 0 |  |
|52759_b3ecd2335fd16ec2eee2_9_UFNIA.smt2                      |   20.028s | 47.916MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_32_QF_UFNIA.smt2                    |   20.030s | 160.0MiB| timeout | 0 |  |
|3106_1c933134166dbad31f79_38_QF_UFNIA.smt2                   |   20.030s | 131.0MiB| timeout | 0 |  |
|63058_64ab9a7ef7b6c3492507_24_QF_UFLIA.smt2                  |   20.031s | 181.0MiB| timeout | 0 |  |
|52759_b3ecd2335fd16ec2eee2_9_UFLIA.smt2                      |   20.031s | 52.812MiB| timeout | 0 |  |
|44289_4066055e0f64d96da11a_14_QF_UFLIA.smt2                  |   20.031s | 108.0MiB| timeout | 0 |  |
|30078_f817a923328f75af7e60_28_QF_UFDTNIA.smt2                |   20.032s | 133.0MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_66_QF_UFNIA.smt2      |   20.032s | 50.632MiB| timeout | 0 |  |
|3106_1c933134166dbad31f79_39_QF_UFDTLIA.smt2                 |   20.032s | 69.496MiB| timeout | 0 |  |
|38347_525a1ca0331f2bcbf520_54_QF_UFNIA.smt2                  |   20.033s | 147.0MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_32_QF_UFDTNIA.smt2                  |   20.034s | 191.0MiB| timeout | 0 |  |
|63058_64ab9a7ef7b6c3492507_24_QF_UFNIA.smt2                  |   20.036s | 177.0MiB| timeout | 0 |  |
|39657_2866defdd1f2434b69ab_47_QF_UFDTLIA.smt2                |   20.037s | 38.568MiB| timeout | 0 |  |
|63058_64ab9a7ef7b6c3492507_23_QF_UFLIA.smt2                  |   20.037s | 155.0MiB| timeout | 0 |  |
|72658_63104dadde9c6026353f_71_QF_UFDTNIA.smt2                |   20.037s | 28.544MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_32_QF_UFLIA.smt2                    |   20.038s | 287.0MiB| timeout | 0 |  |
|3106_1c933134166dbad31f79_38_QF_UFLIA.smt2                   |   20.038s | 131.0MiB| timeout | 0 |  |
|63058_64ab9a7ef7b6c3492507_22_QF_UFNIA.smt2                  |   20.039s | 176.0MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_30_QF_UFLIA.smt2                    |   20.040s | 280.0MiB| timeout | 0 |  |
|93493_798593962ee29ad45ac8_52_QF_UFLIA.smt2                  |   20.043s | 182.0MiB| timeout | 0 |  |
|44788_1965f0d6d94d5d8054ba_34_QF_UFNIA.smt2                  |   20.044s | 31.896MiB| timeout | 0 |  |
|39657_1c7158801cd59dc13f05_45_QF_UFNIA.smt2                  |   20.044s | 81.536MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_2_UFNIA.smt2                      |   20.045s | 138.0MiB| timeout | 0 |  |
|41958_32933c5a1384696720a2_61_QF_UFLIA.smt2                  |   20.045s | 68.9MiB| timeout | 0 |  |
|38347_525a1ca0331f2bcbf520_54_QF_UFDTNIA.smt2                |   20.046s | 117.0MiB| timeout | 0 |  |
|39657_2866defdd1f2434b69ab_47_QF_UFLIA.smt2                  |   20.046s | 41.604MiB| timeout | 0 |  |
|93493_798593962ee29ad45ac8_52_QF_UFDTLIA.smt2                |   20.047s | 135.0MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_29_QF_UFDTNIA.smt2                  |   20.047s | 156.0MiB| timeout | 0 |  |
|38347_092cc73601c78e45f4f9_57_QF_UFDTNIA.smt2                |   20.048s | 26.82MiB| timeout | 0 |  |
|93493_5990a6bf5f2740164f77_50_QF_UFLIA.smt2                  |   20.048s | 72.524MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_30_QF_UFDTLIA.smt2                  |   20.049s | 256.0MiB| timeout | 0 |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_10_QF_UFDTNIA.smt2    |   20.051s | 86.292MiB| timeout | 0 |  |
|65782_cd31513fdcd15701933b_5_QF_UFNIA.smt2                   |   20.051s | 45.22MiB| timeout | 0 |  |
|41958_32933c5a1384696720a2_61_QF_UFNIA.smt2                  |   20.051s | 35.704MiB| timeout | 0 |  |
|66603_accdadf23a1cf70ae043_76_QF_UFLIA.smt2                  |   20.053s | 479.0MiB| timeout | 0 |  |
|41958_32933c5a1384696720a2_62_QF_UFDTNIA.smt2                |   20.054s | 28.104MiB| timeout | 0 |  |
|72771_f9d228efc97cf1458e38_64_QF_UFNIA.smt2                  |   20.055s | 48.66MiB| timeout | 0 |  |
|93493_1fdb6cc8eb9c4363b5838af9eb8c7f1f_53_QF_UFLIA.smt2      |   20.055s | 98.264MiB| timeout | 0 |  |
|3106_afb7bc55417e43d7a22790c3576f04fc_37_QF_UFNIA.smt2       |   20.055s | 118.0MiB| timeout | 0 |  |
|17512_5c1021b0faa6b6e1791b_20_QF_UFNIA.smt2                  |   20.055s | 35.752MiB| timeout | 0 |  |
|93493_4ea6163ed03941199c785278ccc42812_49_QF_UFDTLIA.smt2    |   20.056s | 167.0MiB| timeout | 0 |  |
|38347_092cc73601c78e45f4f9_57_QF_UFNIA.smt2                  |   20.057s | 32.584MiB| timeout | 0 |  |
|44788_1965f0d6d94d5d8054ba_35_QF_UFDTNIA.smt2                |   20.058s | 33.544MiB| timeout | 0 |  |
|41958_32933c5a1384696720a2_62_QF_UFLIA.smt2                  |   20.058s | 54.776MiB| timeout | 0 |  |
|44289_4066055e0f64d96da11a_14_QF_UFNIA.smt2                  |   20.059s | 110.0MiB| timeout | 0 |  |
|41958_32933c5a1384696720a2_63_QF_UFDTNIA.smt2                |   20.061s | 30.276MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_32_QF_UFDTLIA.smt2                  |   20.062s | 275.0MiB| timeout | 0 |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_10_QF_UFDTLIA.smt2    |   20.062s | 146.0MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_3_UFDTNIA.smt2                    |   20.063s | 41.176MiB| timeout | 0 |  |
|93493_27ab26d56d60426da02e50231269b6ff_51_QF_UFLIA.smt2      |   20.063s | 87.216MiB| timeout | 0 |  |
|44788_1965f0d6d94d5d8054ba_36_QF_UFDTNIA.smt2                |   20.065s | 35.504MiB| timeout | 0 |  |
|3106_1c933134166dbad31f79_39_QF_UFDTNIA.smt2                 |   20.066s | 69.324MiB| timeout | 0 |  |
|93493_1fdb6cc8eb9c4363b5838af9eb8c7f1f_53_QF_UFNIA.smt2      |   20.066s | 110.0MiB| timeout | 0 |  |
|30078_f817a923328f75af7e60_28_QF_UFNIA.smt2                  |   20.066s | 392.0MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_66_QF_UFDTNIA.smt2    |   20.066s | 49.008MiB| timeout | 0 |  |
|44289_e5a2e5c780236919ee6a_17_QF_UFNIA.smt2                  |   20.067s | 97.828MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_0_UFDTLIA.smt2                    |   20.067s | 92.548MiB| timeout | 0 |  |
|65782_cd31513fdcd15701933b_5_QF_UFDTNIA.smt2                 |   20.067s | 39.764MiB| timeout | 0 |  |
|52759_b3ecd2335fd16ec2eee2_9_UFDTNIA.smt2                    |   20.068s | 44.668MiB| timeout | 0 |  |
|93493_5990a6bf5f2740164f77_50_QF_UFNIA.smt2                  |   20.068s | 130.0MiB| timeout | 0 |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_10_QF_UFLIA.smt2      |   20.069s | 211.0MiB| timeout | 0 |  |
|3106_1c933134166dbad31f79_38_QF_UFDTLIA.smt2                 |   20.069s | 200.0MiB| timeout | 0 |  |
|38347_092cc73601c78e45f4f9_57_QF_UFDTLIA.smt2                |   20.069s | 29.7MiB| timeout | 0 |  |
|72658_63104dadde9c6026353f_71_QF_UFNIA.smt2                  |   20.069s | 25.24MiB| timeout | 0 |  |
|3106_afb7bc55417e43d7a22790c3576f04fc_37_QF_UFLIA.smt2       |   20.069s | 102.0MiB| timeout | 0 |  |
|41958_32933c5a1384696720a2_62_QF_UFNIA.smt2                  |   20.070s | 28.152MiB| timeout | 0 |  |
|66603_accdadf23a1cf70ae043_76_QF_UFNIA.smt2                  |   20.070s | 486.0MiB| timeout | 0 |  |
|38347_092cc73601c78e45f4f9_56_QF_UFDTLIA.smt2                |   20.070s | 134.0MiB| timeout | 0 |  |
|38347_092cc73601c78e45f4f9_57_QF_UFLIA.smt2                  |   20.070s | 32.772MiB| timeout | 0 |  |
|17512_5c1021b0faa6b6e1791b_20_QF_UFDTNIA.smt2                |   20.070s | 38.56MiB| timeout | 0 |  |
|72658_63104dadde9c6026353f_70_QF_UFDTNIA.smt2                |   20.071s | 29.772MiB| timeout | 0 |  |
|44289_b077fc096b3d41cba49f8628caff7fa5_16_QF_UFNIA.smt2      |   20.072s | 105.0MiB| timeout | 0 |  |
|72658_63104dadde9c6026353f_71_QF_UFLIA.smt2                  |   20.072s | 113.0MiB| timeout | 0 |  |
|93493_798593962ee29ad45ac8_52_QF_UFDTNIA.smt2                |   20.072s | 73.748MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_3_UFNIA.smt2                      |   20.072s | 67.148MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_65_QF_UFNIA.smt2      |   20.072s | 78.072MiB| timeout | 0 |  |
|38347_092cc73601c78e45f4f9_55_QF_UFNIA.smt2                  |   20.072s | 23.936MiB| timeout | 0 |  |
|41958_32933c5a1384696720a2_61_QF_UFDTNIA.smt2                |   20.072s | 35.74MiB| timeout | 0 |  |
|41958_32933c5a1384696720a2_63_QF_UFNIA.smt2                  |   20.073s | 30.952MiB| timeout | 0 |  |
|44289_b077fc096b3d41cba49f8628caff7fa5_16_QF_UFDTLIA.smt2    |   20.073s | 144.0MiB| timeout | 0 |  |
|39657_1c7158801cd59dc13f05_44_QF_UFNIA.smt2                  |   20.073s | 77.252MiB| timeout | 0 |  |
|38347_092cc73601c78e45f4f9_55_QF_UFDTLIA.smt2                |   20.074s | 24.928MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_4_UFDTNIA.smt2                    |   20.074s | 111.0MiB| timeout | 0 |  |
|41958_32933c5a1384696720a2_61_QF_UFDTLIA.smt2                |   20.075s | 69.568MiB| timeout | 0 |  |
|38347_092cc73601c78e45f4f9_55_QF_UFLIA.smt2                  |   20.076s | 26.508MiB| timeout | 0 |  |
|93493_798593962ee29ad45ac8_52_QF_UFNIA.smt2                  |   20.076s | 174.0MiB| timeout | 0 |  |
|93493_1fdb6cc8eb9c4363b5838af9eb8c7f1f_53_QF_UFDTLIA.smt2    |   20.077s | 87.464MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_66_QF_UFLIA.smt2      |   20.077s | 525.0MiB| timeout | 0 |  |
|66603_accdadf23a1cf70ae043_75_QF_UFDTNIA.smt2                |   20.078s | 119.0MiB| timeout | 0 |  |
|72658_63104dadde9c6026353f_71_QF_UFDTLIA.smt2                |   20.080s | 159.0MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_31_QF_UFDTLIA.smt2                  |   20.080s | 145.0MiB| timeout | 0 |  |
|17512_5c1021b0faa6b6e1791b_19_QF_UFDTLIA.smt2                |   20.081s | 85.612MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_67_QF_UFNIA.smt2      |   20.081s | 57.224MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_2_UFLIA.smt2                      |   20.083s | 168.0MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_0_UFLIA.smt2                      |   20.083s | 99.0MiB| timeout | 0 |  |
|44289_e5a2e5c780236919ee6a_18_QF_UFNIA.smt2                  |   20.084s | 98.444MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_31_QF_UFLIA.smt2                    |   20.084s | 134.0MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_33_QF_UFDTLIA.smt2                  |   20.084s | 256.0MiB| timeout | 0 |  |
|44788_1965f0d6d94d5d8054ba_36_QF_UFNIA.smt2                  |   20.085s | 39.072MiB| timeout | 0 |  |
|44289_b077fc096b3d41cba49f8628caff7fa5_16_QF_UFLIA.smt2      |   20.085s | 149.0MiB| timeout | 0 |  |
|65782_cd31513fdcd15701933b_5_QF_UFLIA.smt2                   |   20.086s | 110.0MiB| timeout | 0 |  |
|93493_4ea6163ed03941199c785278ccc42812_49_QF_UFNIA.smt2      |   20.086s | 159.0MiB| timeout | 0 |  |
|93493_4ea6163ed03941199c785278ccc42812_49_QF_UFDTNIA.smt2    |   20.086s | 172.0MiB| timeout | 0 |  |
|17512_5c1021b0faa6b6e1791b_20_QF_UFDTLIA.smt2                |   20.087s | 256.0MiB| timeout | 0 |  |
|41958_45c688a4814eb926c254_59_QF_UFLIA.smt2                  |   20.087s | 85.216MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_29_QF_UFNIA.smt2                    |   20.088s | 155.0MiB| timeout | 0 |  |
|38347_525a1ca0331f2bcbf520_54_QF_UFDTLIA.smt2                |   20.091s | 471.0MiB| timeout | 0 |  |
|93493_4ea6163ed03941199c785278ccc42812_49_QF_UFLIA.smt2      |   20.092s | 148.0MiB| timeout | 0 |  |
|63058_64ab9a7ef7b6c3492507_23_QF_UFNIA.smt2                  |   20.092s | 174.0MiB| timeout | 0 |  |
|93493_5990a6bf5f2740164f77_50_QF_UFDTLIA.smt2                |   20.094s | 171.0MiB| timeout | 0 |  |
|66603_accdadf23a1cf70ae043_74_QF_UFDTNIA.smt2                |   20.094s | 126.0MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_33_QF_UFLIA.smt2                    |   20.095s | 272.0MiB| timeout | 0 |  |
|17512_5c1021b0faa6b6e1791b_19_QF_UFLIA.smt2                  |   20.095s | 79.252MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_65_QF_UFDTLIA.smt2    |   20.095s | 969.0MiB| timeout | 0 |  |
|44788_1965f0d6d94d5d8054ba_36_QF_UFDTLIA.smt2                |   20.096s | 252.0MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_30_QF_UFDTNIA.smt2                  |   20.096s | 155.0MiB| timeout | 0 |  |
|52759_b3ecd2335fd16ec2eee2_9_UFDTLIA.smt2                    |   20.097s | 48.348MiB| timeout | 0 |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_12_QF_UFNIA.smt2      |   20.098s | 187.0MiB| timeout | 0 |  |
|63058_55d6bef5390186355f11_26_QF_UFNIA.smt2                  |   20.099s | 180.0MiB| timeout | 0 |  |
|30078_f817a923328f75af7e60_27_QF_UFLIA.smt2                  |   20.100s | 252.0MiB| timeout | 0 |  |
|66603_accdadf23a1cf70ae043_75_QF_UFNIA.smt2                  |   20.101s | 657.0MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_33_QF_UFDTNIA.smt2                  |   20.102s | 186.0MiB| timeout | 0 |  |
|30078_f817a923328f75af7e60_28_QF_UFDTLIA.smt2                |   20.102s | 150.0MiB| timeout | 0 |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_13_QF_UFNIA.smt2      |   20.102s | 154.0MiB| timeout | 0 |  |
|44788_1965f0d6d94d5d8054ba_35_QF_UFNIA.smt2                  |   20.103s | 44.776MiB| timeout | 0 |  |
|3106_1c933134166dbad31f79_38_QF_UFDTNIA.smt2                 |   20.103s | 205.0MiB| timeout | 0 |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_11_QF_UFLIA.smt2      |   20.104s | 225.0MiB| timeout | 0 |  |
|44788_1965f0d6d94d5d8054ba_34_QF_UFDTNIA.smt2                |   20.106s | 28.048MiB| timeout | 0 |  |
|39657_1c7158801cd59dc13f05_44_QF_UFDTNIA.smt2                |   20.107s | 80.684MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_33_QF_UFNIA.smt2                    |   20.107s | 154.0MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_2_UFDTLIA.smt2                    |   20.107s | 147.0MiB| timeout | 0 |  |
|44289_4066055e0f64d96da11a_15_QF_UFNIA.smt2                  |   20.107s | 310.0MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_1_UFLIA.smt2                      |   20.107s | 228.0MiB| timeout | 0 |  |
|52759_af0c476fe3b544b9a8507f3e42472c43_12_QF_UFLIA.smt2      |   20.110s | 172.0MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_4_UFNIA.smt2                      |   20.111s | 148.0MiB| timeout | 0 |  |
|66603_accdadf23a1cf70ae043_73_QF_UFDTLIA.smt2                |   20.112s | 239.0MiB| timeout | 0 |  |
|44289_4066055e0f64d96da11a_15_QF_UFLIA.smt2                  |   20.114s | 364.0MiB| timeout | 0 |  |
|93493_5990a6bf5f2740164f77_50_QF_UFDTNIA.smt2                |   20.114s | 182.0MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_69_QF_UFLIA.smt2      |   20.116s | 475.0MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_68_QF_UFDTLIA.smt2    |   20.118s | 481.0MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_29_QF_UFDTLIA.smt2                  |   20.118s | 266.0MiB| timeout | 0 |  |
|44788_1965f0d6d94d5d8054ba_36_QF_UFLIA.smt2                  |   20.119s | 264.0MiB| timeout | 0 |  |
|38347_525a1ca0331f2bcbf520_54_QF_UFLIA.smt2                  |   20.120s | 499.0MiB| timeout | 0 |  |
|66603_accdadf23a1cf70ae043_73_QF_UFLIA.smt2                  |   20.122s | 444.0MiB| timeout | 0 |  |
|52759_bec3a2272267494faeecb6bfaf253e3b_10_QF_UFNIA.smt2      |   20.122s | 213.0MiB| timeout | 0 |  |
|66603_accdadf23a1cf70ae043_72_QF_UFNIA.smt2                  |   20.122s | 596.0MiB| timeout | 0 |  |
|66603_accdadf23a1cf70ae043_73_QF_UFNIA.smt2                  |   20.123s | 446.0MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_66_QF_UFDTLIA.smt2    |   20.124s | 569.0MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_68_QF_UFLIA.smt2      |   20.124s | 483.0MiB| timeout | 0 |  |
|17512_5c1021b0faa6b6e1791b_20_QF_UFLIA.smt2                  |   20.126s | 325.0MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_68_QF_UFNIA.smt2      |   20.126s | 77.412MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_1_UFDTLIA.smt2                    |   20.126s | 270.0MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_30_QF_UFNIA.smt2                    |   20.127s | 170.0MiB| timeout | 0 |  |
|65782_cd31513fdcd15701933b_5_QF_UFDTLIA.smt2                 |   20.129s | 78.684MiB| timeout | 0 |  |
|38347_092cc73601c78e45f4f9_56_QF_UFLIA.smt2                  |   20.132s | 137.0MiB| timeout | 0 |  |
|940_590f27b1c3c800d3243e_29_QF_UFLIA.smt2                    |   20.132s | 268.0MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_3_UFLIA.smt2                      |   20.135s | 270.0MiB| timeout | 0 |  |
|93493_27ab26d56d60426da02e50231269b6ff_51_QF_UFNIA.smt2      |   20.136s | 98.108MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_69_QF_UFDTLIA.smt2    |   20.141s | 451.0MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_4_UFLIA.smt2                      |   20.146s | 549.0MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_4_UFDTLIA.smt2                    |   20.150s | 595.0MiB| timeout | 0 |  |
|30078_f817a923328f75af7e60_28_QF_UFLIA.smt2                  |   20.150s | 378.0MiB| timeout | 0 |  |
|30078_f817a923328f75af7e60_27_QF_UFNIA.smt2                  |   20.152s | 399.0MiB| timeout | 0 |  |
|66603_accdadf23a1cf70ae043_74_QF_UFLIA.smt2                  |   20.154s | 612.0MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_67_QF_UFDTLIA.smt2    |   20.157s | 476.0MiB| timeout | 0 |  |
|83314_a702bf8b823398c9e37a_3_UFDTLIA.smt2                    |   20.157s | 220.0MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_65_QF_UFLIA.smt2      |   20.168s | 1007.0MiB| timeout | 0 |  |
|66603_accdadf23a1cf70ae043_74_QF_UFNIA.smt2                  |   20.171s | 666.0MiB| timeout | 0 |  |
|66603_accdadf23a1cf70ae043_75_QF_UFLIA.smt2                  |   20.195s | 597.0MiB| timeout | 0 |  |
|25959_5dee2e2f6ef44465a2bea4b085818948_67_QF_UFLIA.smt2      |   20.212s | 488.0MiB| timeout | 0 |  |
