# .

* SAT 9
* UNSAT 34
* TIMEOUT 32
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_AUFBV.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-16740866-files-QF_AUFBV.tar.zst-d
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: ef66acc6b54144f10d6f64e9b557142ff438a31a
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_AUFBV.tar.zst?download=1
Z3 commit message: change calculation of threads to use total threads indicated by parameter or processor count, subtract from worker threads based on backbone and core threads

Signed-off-by: Nikolaj Bjorner <nbjorner@microsoft.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.mod_div14.short.smt2 |    0.035s | 21.184MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.sq_inner23.short.smt2 |    0.039s | 28.596MiB| sat | 0 |  |  |
|non-incremental/QF_AUFBV/20210301-Alive2/gzip/250_gzip.smt2  |    0.044s | 21.424MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.mod_div1.short.smt2 |    0.044s | 20.092MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.mod_div12.short.smt2 |    0.044s | 22.516MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_full_add1.short.smt2 |    0.045s | 20.016MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_twin_mul_init2.short.smt2 |    0.049s | 20.376MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.sq_inner13.short.smt2 |    0.049s | 28.532MiB| sat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.mod_div10.short.smt2 |    0.050s | 19.924MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.sq_inner22.short.smt2 |    0.053s | 21.78MiB| sat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.mul_inner3.short.smt2 |    0.054s | 22.236MiB| sat | 0 |  |  |
|non-incremental/QF_AUFBV/20210301-Alive2-partial-undef/gzip/322_gzip.smt2 |    0.057s | 23.04MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.group_red1.short.smt2 |    0.057s | 28.86MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_twin_mul_init1.short.smt2 |    0.058s | 20.172MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_twin_mul_init.short.smt2 |    0.059s | 20.404MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.sq_inner12.short.smt2 |    0.061s | 21.692MiB| sat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.mul_inner4.short.smt2 |    0.063s | 28.816MiB| sat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_full_sub2.short.smt2 |    0.066s | 33.416MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_full_sub3.short.smt2 |    0.072s | 33.576MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/20210301-Alive2/gzip/272_gzip.smt2  |    0.074s | 23.156MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_full_add4.short.smt2 |    0.092s | 30.972MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/20231002-nysm/sha512_block_armv8_one_block_correct_fn_calls_equal.smt2 |    0.093s | 24.128MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_full_add2.short.smt2 |    0.094s | 30.148MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/20210301-Alive2-partial-undef/gzip/332_gzip.smt2 |    0.097s | 23.232MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_full_sub1.short.smt2 |    0.097s | 33.268MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_mul_aux3.short.smt2 |    0.118s | 32.644MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_mul_aux4.short.smt2 |    0.122s | 32.38MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_mul_aux2.short.smt2 |    0.132s | 32.508MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/20231002-nysm/sha512_block_data_order_one_block_correct_fn_calls_equal.smt2 |    0.133s | 25.508MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.signHash5.short.smt2 |    0.160s | 63.676MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.signHash3.short.smt2 |    0.165s | 63.78MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.signHash4.short.smt2 |    0.181s | 63.732MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_full_add3.short.smt2 |    0.188s | 45.604MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Gonzalvez/opStructure_MBA_6.smt2 |    0.328s | 44.8MiB| sat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_twin_mul_aux1.short.smt2 |    0.362s | 93.648MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_twin_mul_aux12.short.smt2 |    0.369s | 93.88MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_twin_mul_aux11.short.smt2 |    0.380s | 93.824MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/20231002-nysm/sha512_block_data_order_loop_inductive_invariantLoopInductive.smt2 |    0.617s | 46.864MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Gonzalvez/opStructure_MBA_7.smt2 |    0.832s | 73.18MiB| sat | 0 |  |  |
|non-incremental/QF_AUFBV/20210301-Alive2/ph7/708_ph7.smt2    |    1.550s | 131.0MiB| sat | 0 |  |  |
|non-incremental/QF_AUFBV/20210301-Alive2-partial-undef/gcc/204_gcc.smt2 |    4.232s | 35.164MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.verifySignature.short.smt2 |    4.743s | 69.412MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/picorv32-check-compact-mem.smt2 |    8.095s | 131.0MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/picorv32-pcregs-compact-mem.smt2 |   20.020s | 111.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/VexRiscv-regch0-30-compact-mem.smt2 |   20.021s | 138.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/zipcpu-zipmmu-compact-mem.smt2 |   20.023s | 156.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/20210301-Alive2-partial-undef/sqlite3/891_sqlite3.smt2 |   20.028s | 71.812MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2019A/picorv32_mutCY_QF_AUFBV_NONINCR.smt2 |   20.028s | 143.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/20210301-Alive2/gcc/033_gcc.smt2    |   20.036s | 143.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Gonzalvez/opStructure_NPT_2.smt2 |   20.036s | 102.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/zipcpu-busdelay-compact-mem.smt2 |   20.038s | 144.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/VexRiscv-regch0-15-compact-mem.smt2 |   20.041s | 70.272MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2019A/picorv32_mutBX_QF_AUFBV_NONINCR.smt2 |   20.043s | 220.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2019A/picorv32_mutAY_QF_AUFBV_NONINCR.smt2 |   20.047s | 124.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2019A/picorv32_mutBY_QF_AUFBV_NONINCR.smt2 |   20.049s | 144.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/20210301-Alive2/sqlite3/906_sqlite3.smt2 |   20.050s | 121.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/VexRiscv-regch0-20-compact-mem.smt2 |   20.050s | 86.972MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/20210301-Alive2/gcc/073_gcc.smt2    |   20.054s | 388.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2019A/picorv32_mutAX_QF_AUFBV_NONINCR.smt2 |   20.055s | 201.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2019A/picorv32_mutCX_QF_AUFBV_NONINCR.smt2 |   20.060s | 219.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Gonzalvez/opStructure_NPT_1.smt2 |   20.063s | 160.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/20210301-Alive2/sqlite3/823_sqlite3.smt2 |   20.070s | 281.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/20231002-nysm/sha512_block_armv8_one_block_correct_fn_calls_equal_no_rewrite.smt2 |   20.082s | 523.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/20231002-nysm/sha512_block_data_order_one_block_correct_fn_calls_equal_no_rewrite.smt2 |   20.082s | 528.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/20231002-nysm/sha512_block_data_order_loop_inductive_invariant_no_rewriteLoopInductive.smt2 |   20.088s | 532.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/20231002-nysm/sha512_block_armv8_loop_inductive_invariant_no_rewriteLoopInductive.smt2 |   20.134s | 1133.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/zipcpu-pfcache-compact-mem.smt2 |   20.150s | 994.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/ponylink-slaveTXlen-sat-compact-mem.smt2 |   20.158s | 1162.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/ponylink-slaveTXlen-unsat-compact-mem.smt2 |   20.216s | 1705.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Gonzalvez/opStructure_C_16_32_2_2.smt2 |   20.261s | 2041.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/20231002-nysm/sha512_block_armv8_loop_inductive_invariantLoopInductive.smt2 |   20.273s | 2525.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Gonzalvez/opStructure_O_16_32_2_2.smt2 |   20.290s | 3621.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Gonzalvez/opStructure_O_32_16_2_2.smt2 |   20.350s | 3596.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Gonzalvez/opStructure_O_16_24_2_2.smt2 |   20.382s | 3776.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Gonzalvez/opStructure_O_32_32_2_2.smt2 |   20.674s | 6917.0MiB| timeout | 0 |  |  |
