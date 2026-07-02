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
Job tag: coz3-gen-https-zenodo.org-records-QF_AUFBV
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: e7e686127bbc65834c09fa85686fbe8e1c497adf
Z3 branch: generation
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_AUFBV.tar.zst?download=1
Z3 commit message: fix signature regression for unit test

Signed-off-by: Nikolaj Bjorner <nbjorner@microsoft.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|non-incremental/QF_AUFBV/20210301-Alive2/gzip/250_gzip.smt2  |    0.038s | 21.384MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_twin_mul_init.short.smt2 |    0.041s | 20.372MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.mod_div12.short.smt2 |    0.044s | 22.644MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.mod_div10.short.smt2 |    0.046s | 20.34MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.mod_div1.short.smt2 |    0.047s | 20.596MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_full_add1.short.smt2 |    0.047s | 19.848MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_twin_mul_init2.short.smt2 |    0.047s | 20.364MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_twin_mul_init1.short.smt2 |    0.050s | 20.232MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.sq_inner13.short.smt2 |    0.051s | 28.348MiB| sat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.mod_div14.short.smt2 |    0.052s | 21.264MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.sq_inner23.short.smt2 |    0.053s | 28.304MiB| sat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.sq_inner22.short.smt2 |    0.054s | 21.9MiB| sat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.sq_inner12.short.smt2 |    0.055s | 21.784MiB| sat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.group_red1.short.smt2 |    0.057s | 28.84MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.mul_inner3.short.smt2 |    0.057s | 22.156MiB| sat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_full_add4.short.smt2 |    0.063s | 31.112MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.mul_inner4.short.smt2 |    0.064s | 28.844MiB| sat | 0 |  |  |
|non-incremental/QF_AUFBV/20210301-Alive2/gzip/272_gzip.smt2  |    0.070s | 23.132MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/20210301-Alive2-partial-undef/gzip/322_gzip.smt2 |    0.082s | 22.912MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_full_sub3.short.smt2 |    0.088s | 33.436MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_full_sub1.short.smt2 |    0.090s | 33.44MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_full_sub2.short.smt2 |    0.097s | 33.44MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_full_add2.short.smt2 |    0.103s | 31.076MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/20231002-nysm/sha512_block_data_order_one_block_correct_fn_calls_equal.smt2 |    0.105s | 25.5MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/20231002-nysm/sha512_block_armv8_one_block_correct_fn_calls_equal.smt2 |    0.107s | 24.256MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/20210301-Alive2-partial-undef/gzip/332_gzip.smt2 |    0.107s | 23.196MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_mul_aux4.short.smt2 |    0.117s | 32.36MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_mul_aux2.short.smt2 |    0.118s | 32.388MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_mul_aux3.short.smt2 |    0.118s | 32.38MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.signHash4.short.smt2 |    0.164s | 64.008MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.signHash5.short.smt2 |    0.167s | 63.644MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.signHash3.short.smt2 |    0.176s | 63.588MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_full_add3.short.smt2 |    0.212s | 45.612MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Gonzalvez/opStructure_MBA_6.smt2 |    0.320s | 44.688MiB| sat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_twin_mul_aux11.short.smt2 |    0.386s | 96.98MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_twin_mul_aux12.short.smt2 |    0.415s | 96.976MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_twin_mul_aux1.short.smt2 |    0.416s | 96.976MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/20231002-nysm/sha512_block_data_order_loop_inductive_invariantLoopInductive.smt2 |    0.610s | 46.832MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Gonzalvez/opStructure_MBA_7.smt2 |    0.864s | 73.128MiB| sat | 0 |  |  |
|non-incremental/QF_AUFBV/20210301-Alive2/ph7/708_ph7.smt2    |    1.519s | 131.0MiB| sat | 0 |  |  |
|non-incremental/QF_AUFBV/20210301-Alive2-partial-undef/gcc/204_gcc.smt2 |    4.600s | 35.468MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.verifySignature.short.smt2 |    4.652s | 69.452MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/picorv32-check-compact-mem.smt2 |    9.690s | 138.0MiB| unsat | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/VexRiscv-regch0-20-compact-mem.smt2 |   20.017s | 88.156MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/picorv32-pcregs-compact-mem.smt2 |   20.021s | 116.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2019A/picorv32_mutBY_QF_AUFBV_NONINCR.smt2 |   20.026s | 150.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Gonzalvez/opStructure_NPT_1.smt2 |   20.032s | 161.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Gonzalvez/opStructure_NPT_2.smt2 |   20.034s | 102.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/20210301-Alive2/gcc/033_gcc.smt2    |   20.036s | 143.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/20210301-Alive2/sqlite3/906_sqlite3.smt2 |   20.038s | 122.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/VexRiscv-regch0-15-compact-mem.smt2 |   20.039s | 71.3MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2019A/picorv32_mutCY_QF_AUFBV_NONINCR.smt2 |   20.040s | 149.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2019A/picorv32_mutAX_QF_AUFBV_NONINCR.smt2 |   20.042s | 208.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/zipcpu-busdelay-compact-mem.smt2 |   20.045s | 152.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/20210301-Alive2-partial-undef/sqlite3/891_sqlite3.smt2 |   20.046s | 73.924MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/VexRiscv-regch0-30-compact-mem.smt2 |   20.047s | 138.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2019A/picorv32_mutCX_QF_AUFBV_NONINCR.smt2 |   20.047s | 227.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2019A/picorv32_mutAY_QF_AUFBV_NONINCR.smt2 |   20.049s | 131.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/zipcpu-zipmmu-compact-mem.smt2 |   20.050s | 159.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/20210301-Alive2/gcc/073_gcc.smt2    |   20.065s | 389.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/20231002-nysm/sha512_block_armv8_one_block_correct_fn_calls_equal_no_rewrite.smt2 |   20.069s | 523.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2019A/picorv32_mutBX_QF_AUFBV_NONINCR.smt2 |   20.073s | 228.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/20210301-Alive2/sqlite3/823_sqlite3.smt2 |   20.080s | 283.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/20231002-nysm/sha512_block_data_order_loop_inductive_invariant_no_rewriteLoopInductive.smt2 |   20.088s | 532.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/20231002-nysm/sha512_block_data_order_one_block_correct_fn_calls_equal_no_rewrite.smt2 |   20.100s | 528.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/20231002-nysm/sha512_block_armv8_loop_inductive_invariant_no_rewriteLoopInductive.smt2 |   20.152s | 1130.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/ponylink-slaveTXlen-sat-compact-mem.smt2 |   20.157s | 1192.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/zipcpu-pfcache-compact-mem.smt2 |   20.167s | 1011.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/ponylink-slaveTXlen-unsat-compact-mem.smt2 |   20.196s | 1734.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Gonzalvez/opStructure_C_16_32_2_2.smt2 |   20.229s | 2041.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Gonzalvez/opStructure_O_16_32_2_2.smt2 |   20.309s | 3621.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/20231002-nysm/sha512_block_armv8_loop_inductive_invariantLoopInductive.smt2 |   20.331s | 2526.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Gonzalvez/opStructure_O_16_24_2_2.smt2 |   20.340s | 3613.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Gonzalvez/opStructure_O_32_16_2_2.smt2 |   20.453s | 4459.0MiB| timeout | 0 |  |  |
|non-incremental/QF_AUFBV/2019-Gonzalvez/opStructure_O_32_32_2_2.smt2 |   20.491s | 6917.0MiB| timeout | 0 |  |  |
