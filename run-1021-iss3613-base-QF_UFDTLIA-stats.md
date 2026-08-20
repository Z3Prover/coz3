# .

* SAT 37
* UNSAT 7
* TIMEOUT 32
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-08-20 23:57:21 UTC
Job description: Baseline for Z3Prover/bench#3613 A/B: master (414189804, exact parent of iss3613-bool_var2bound-dense). QF_UFDTLIA.
Job tag: iss3613-base-QF_UFDTLIA
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 414189804519311450363b3858ed4a15501a4afe
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_UFDTLIA.tar.zst?download=1
Z3 commit message: Fix intblast translation of BV logical shift right (#10596)

Z3 could report `sat` for an `unsat` quantified bit-vector formula when
using `smt.bv.solver=2`. The issue came from translating symbolic
`bvlshr` into an arithmetic `lshr` term in plugin mode, where the
arithmetic axioms were insufficient under quantifiers.

- **BV-to-Int translation**
- Use the existing finite ITE expansion for `bvlshr` while running as
the intblast plugin.
  - Keep the symbolic arithmetic `lshr` path for non-plugin translation.

```cpp
case OP_BLSHR:
    if (!m_is_plugin && !a.is_numeral(arg(0)) && !a.is_numeral(arg(1)))
        r = a.mk_lshr(bv.get_bv_size(e), arg(0), arg(1));
    else {
        expr* x = arg(0), * y = umod(e, 1);
        r = a.mk_int(0);
        for (unsigned i = 0; i < bv.get_bv_size(e); ++i)
            r = if_eq(y, i, a.mk_idiv(x, a.mk_int(rational::power_of_two(i))), r);
    }
```

- **Regression coverage**
- Added a focused `smt_context` regression for the reported quantified
BV shift formula with `smt.bv.solver=2`.

<!-- START COPILOT CODING AGENT SUFFIX -->

- Fixes #10595

---------

Co-authored-by: copilot-swe-agent[bot] <198982749+Copilot@users.noreply.github.com>
Co-authored-by: NikolajBjorner <3085284+NikolajBjorner@users.noreply.github.com>
</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | INFO |
|------------|----------:|---------:|-------------:| ----------:|------|
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_6_QF_UFDTLIA.smt2 |    0.088s | 22.616MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_40_QF_UFDTLIA.smt2 |    0.126s | 24.468MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/63058_aa742630eef64f949de269382c1f9035_25_UFDTLIA.smt2 |    0.135s | 25.58MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/44788_1965f0d6d94d5d8054ba_34_QF_UFDTLIA.smt2 |    0.151s | 24.216MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_58_QF_UFDTLIA.smt2 |    0.265s | 29.476MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/52759_af0c476fe3b544b9a8507f3e42472c43_12_QF_UFDTLIA.smt2 |    0.287s | 34.94MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_7_QF_UFDTLIA.smt2 |    0.296s | 26.588MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_8_QF_UFDTLIA.smt2 |    0.298s | 26.728MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/11775_ad46e5b8db4748c51973_43_QF_UFDTLIA.smt2 |    0.322s | 34.576MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/52759_af0c476fe3b544b9a8507f3e42472c43_13_QF_UFDTLIA.smt2 |    0.375s | 31.64MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/41958_45c688a4814eb926c254_60_QF_UFDTLIA.smt2 |    0.409s | 34.972MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/44788_1965f0d6d94d5d8054ba_35_QF_UFDTLIA.smt2 |    0.517s | 27.844MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/11775_ad46e5b8db4748c51973_42_QF_UFDTLIA.smt2 |    0.563s | 37.468MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/72658_63104dadde9c6026353f_70_QF_UFDTLIA.smt2 |    0.799s | 56.932MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/41958_45c688a4814eb926c254_59_QF_UFDTLIA.smt2 |    0.817s | 40.0MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_41_QF_UFDTLIA.smt2 |    0.873s | 40.08MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/44289_e5a2e5c780236919ee6a_17_QF_UFDTLIA.smt2 |    1.048s | 49.568MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/44289_4066055e0f64d96da11a_14_QF_UFDTLIA.smt2 |    1.057s | 44.256MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/39657_1c7158801cd59dc13f05_44_QF_UFDTLIA.smt2 |    1.172s | 53.372MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/39657_1c7158801cd59dc13f05_46_QF_UFDTLIA.smt2 |    1.173s | 52.72MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/39657_1c7158801cd59dc13f05_45_QF_UFDTLIA.smt2 |    1.234s | 56.18MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/63058_55d6bef5390186355f11_26_QF_UFDTLIA.smt2 |    1.255s | 50.82MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/44289_e5a2e5c780236919ee6a_18_QF_UFDTLIA.smt2 |    1.309s | 51.9MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/41958_32933c5a1384696720a2_63_QF_UFDTLIA.smt2 |    1.469s | 34.084MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/63058_64ab9a7ef7b6c3492507_23_QF_UFDTLIA.smt2 |    1.510s | 51.116MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/63058_64ab9a7ef7b6c3492507_22_QF_UFDTLIA.smt2 |    1.693s | 47.128MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/52759_bec3a2272267494faeecb6bfaf253e3b_11_QF_UFDTLIA.smt2 |    1.824s | 71.16MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/63058_64ab9a7ef7b6c3492507_24_QF_UFDTLIA.smt2 |    2.160s | 67.04MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/44289_4066055e0f64d96da11a_15_QF_UFDTLIA.smt2 |    2.566s | 79.232MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/17512_5c1021b0faa6b6e1791b_21_QF_UFDTLIA.smt2 |    2.733s | 68.724MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/41958_32933c5a1384696720a2_62_QF_UFDTLIA.smt2 |    2.754s | 47.332MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/72771_f9d228efc97cf1458e38_64_QF_UFDTLIA.smt2 |    2.857s | 33.452MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_72_QF_UFDTLIA.smt2 |    2.873s | 106.0MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/39657_2866defdd1f2434b69ab_48_QF_UFDTLIA.smt2 |    3.172s | 33.012MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/3106_afb7bc55417e43d7a22790c3576f04fc_37_QF_UFDTLIA.smt2 |    5.888s | 81.272MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/93493_27ab26d56d60426da02e50231269b6ff_51_QF_UFDTLIA.smt2 |    6.111s | 75.236MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_75_QF_UFDTLIA.smt2 |    7.765s | 135.0MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_76_QF_UFDTLIA.smt2 |    8.778s | 238.0MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/52759_bec3a2272267494faeecb6bfaf253e3b_10_QF_UFDTLIA.smt2 |   11.061s | 181.0MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/30078_f817a923328f75af7e60_27_QF_UFDTLIA.smt2 |   11.932s | 151.0MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/93493_1fdb6cc8eb9c4363b5838af9eb8c7f1f_53_QF_UFDTLIA.smt2 |   12.181s | 90.484MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_39_QF_UFDTLIA.smt2 |   13.292s | 86.644MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/39657_2866defdd1f2434b69ab_47_QF_UFDTLIA.smt2 |   13.975s | 40.412MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_74_QF_UFDTLIA.smt2 |   14.360s | 142.0MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_30_QF_UFDTLIA.smt2 |   20.035s | 265.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/44289_b077fc096b3d41cba49f8628caff7fa5_16_QF_UFDTLIA.smt2 |   20.038s | 155.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_55_QF_UFDTLIA.smt2 |   20.045s | 25.316MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/66603_accdadf23a1cf70ae043_73_QF_UFDTLIA.smt2 |   20.048s | 251.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/72658_63104dadde9c6026353f_71_QF_UFDTLIA.smt2 |   20.049s | 169.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/65782_cd31513fdcd15701933b_5_QF_UFDTLIA.smt2 |   20.051s | 134.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_56_QF_UFDTLIA.smt2 |   20.053s | 164.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/38347_092cc73601c78e45f4f9_57_QF_UFDTLIA.smt2 |   20.055s | 32.072MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/41958_32933c5a1384696720a2_61_QF_UFDTLIA.smt2 |   20.056s | 78.3MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/44788_1965f0d6d94d5d8054ba_36_QF_UFDTLIA.smt2 |   20.058s | 395.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_2_UFDTLIA.smt2 |   20.064s | 175.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_31_QF_UFDTLIA.smt2 |   20.065s | 161.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/17512_5c1021b0faa6b6e1791b_19_QF_UFDTLIA.smt2 |   20.068s | 101.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_0_UFDTLIA.smt2 |   20.072s | 125.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_3_UFDTLIA.smt2 |   20.078s | 257.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/17512_5c1021b0faa6b6e1791b_20_QF_UFDTLIA.smt2 |   20.080s | 309.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/93493_798593962ee29ad45ac8_52_QF_UFDTLIA.smt2 |   20.080s | 137.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/30078_f817a923328f75af7e60_28_QF_UFDTLIA.smt2 |   20.083s | 157.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_1_UFDTLIA.smt2 |   20.086s | 294.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/93493_4ea6163ed03941199c785278ccc42812_49_QF_UFDTLIA.smt2 |   20.087s | 178.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/3106_1c933134166dbad31f79_38_QF_UFDTLIA.smt2 |   20.093s | 217.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/93493_5990a6bf5f2740164f77_50_QF_UFDTLIA.smt2 |   20.094s | 193.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_67_QF_UFDTLIA.smt2 |   20.095s | 934.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_33_QF_UFDTLIA.smt2 |   20.100s | 264.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_29_QF_UFDTLIA.smt2 |   20.102s | 290.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/940_590f27b1c3c800d3243e_32_QF_UFDTLIA.smt2 |   20.105s | 293.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/83314_a702bf8b823398c9e37a_4_UFDTLIA.smt2 |   20.105s | 639.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_68_QF_UFDTLIA.smt2 |   20.109s | 537.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_66_QF_UFDTLIA.smt2 |   20.115s | 625.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_69_QF_UFDTLIA.smt2 |   20.129s | 495.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/25959_5dee2e2f6ef44465a2bea4b085818948_65_QF_UFDTLIA.smt2 |   20.155s | 969.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDTLIA/20230314-Jaroslav-Bendik-Certora/38347_525a1ca0331f2bcbf520_54_QF_UFDTLIA.smt2 |   20.169s | 1289.0MiB| timeout | 0 |  |
