# .

* SAT 10
* UNSAT 36
* TIMEOUT 29
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-09-06 23:19:31 UTC
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_AUFBV.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-QF_AUFBV
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 53621bed781b1f80d49d57b65e13b6ef814e563a
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_AUFBV.tar.zst?download=1
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
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.mod_div10.short.smt2 |    0.028s | 19.992MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.sq_inner13.short.smt2 |    0.034s | 28.54MiB| sat | 0 |  |
|non-incremental/QF_AUFBV/20210301-Alive2/gzip/250_gzip.smt2  |    0.037s | 21.388MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.mod_div1.short.smt2 |    0.045s | 20.34MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_full_add1.short.smt2 |    0.045s | 19.9MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.sq_inner12.short.smt2 |    0.046s | 21.9MiB| sat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_twin_mul_init.short.smt2 |    0.048s | 20.6MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_twin_mul_init1.short.smt2 |    0.049s | 20.256MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_twin_mul_init2.short.smt2 |    0.049s | 20.672MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.mul_inner4.short.smt2 |    0.051s | 29.3MiB| sat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.sq_inner22.short.smt2 |    0.051s | 21.896MiB| sat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.sq_inner23.short.smt2 |    0.052s | 28.468MiB| sat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.mul_inner3.short.smt2 |    0.053s | 22.176MiB| sat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.mod_div14.short.smt2 |    0.054s | 21.18MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.group_red1.short.smt2 |    0.056s | 28.788MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.mod_div12.short.smt2 |    0.067s | 22.752MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/20231002-nysm/sha512_block_armv8_one_block_correct_fn_calls_equal.smt2 |    0.076s | 22.16MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/20210301-Alive2-partial-undef/gzip/322_gzip.smt2 |    0.078s | 22.968MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/20210301-Alive2-partial-undef/gzip/332_gzip.smt2 |    0.084s | 23.224MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/20210301-Alive2/gzip/272_gzip.smt2  |    0.087s | 23.124MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_full_add4.short.smt2 |    0.089s | 32.364MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/20231002-nysm/sha512_block_data_order_one_block_correct_fn_calls_equal.smt2 |    0.089s | 21.668MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_full_add2.short.smt2 |    0.094s | 30.62MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_mul_aux3.short.smt2 |    0.105s | 32.56MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_full_add3.short.smt2 |    0.116s | 35.344MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_mul_aux4.short.smt2 |    0.122s | 32.412MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_mul_aux2.short.smt2 |    0.122s | 32.508MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.signHash4.short.smt2 |    0.181s | 63.516MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.signHash5.short.smt2 |    0.187s | 63.448MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.signHash3.short.smt2 |    0.204s | 63.708MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_full_sub2.short.smt2 |    0.322s | 33.868MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_full_sub1.short.smt2 |    0.346s | 34.24MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_full_sub3.short.smt2 |    0.347s | 33.856MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/2019-Gonzalvez/opStructure_MBA_6.smt2 |    0.366s | 45.052MiB| sat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_twin_mul_aux11.short.smt2 |    0.532s | 93.92MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/20231002-nysm/sha512_block_data_order_loop_inductive_invariantLoopInductive.smt2 |    0.554s | 34.868MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_twin_mul_aux1.short.smt2 |    0.570s | 93.668MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.ec_twin_mul_aux12.short.smt2 |    0.575s | 93.768MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/2019-Gonzalvez/opStructure_MBA_7.smt2 |    0.921s | 75.192MiB| sat | 0 |  |
|non-incremental/QF_AUFBV/ecc/com.galois.ecc.P384ECC64.verifySignature.short.smt2 |    4.019s | 69.676MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/20210301-Alive2-partial-undef/gcc/204_gcc.smt2 |    4.372s | 35.548MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/20231002-nysm/sha512_block_armv8_one_block_correct_fn_calls_equal_no_rewrite.smt2 |    5.720s | 135.0MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/picorv32-check-compact-mem.smt2 |    9.231s | 134.0MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/20210301-Alive2/ph7/708_ph7.smt2    |   11.346s | 169.0MiB| sat | 0 |  |
|non-incremental/QF_AUFBV/20210301-Alive2-partial-undef/sqlite3/891_sqlite3.smt2 |   12.526s | 51.684MiB| sat | 0 |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/zipcpu-busdelay-compact-mem.smt2 |   16.101s | 147.0MiB| unsat | 0 |  |
|non-incremental/QF_AUFBV/20210301-Alive2/sqlite3/906_sqlite3.smt2 |   20.021s | 97.0MiB| timeout | 0 |  |
|non-incremental/QF_AUFBV/2019-Gonzalvez/opStructure_NPT_2.smt2 |   20.022s | 102.0MiB| timeout | 0 |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/zipcpu-zipmmu-compact-mem.smt2 |   20.025s | 154.0MiB| timeout | 0 |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2019A/picorv32_mutAX_QF_AUFBV_NONINCR.smt2 |   20.028s | 208.0MiB| timeout | 0 |  |
|non-incremental/QF_AUFBV/2019-Gonzalvez/opStructure_NPT_1.smt2 |   20.033s | 142.0MiB| timeout | 0 |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2019A/picorv32_mutCX_QF_AUFBV_NONINCR.smt2 |   20.033s | 225.0MiB| timeout | 0 |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/VexRiscv-regch0-15-compact-mem.smt2 |   20.036s | 71.196MiB| timeout | 0 |  |
|non-incremental/QF_AUFBV/20210301-Alive2/sqlite3/823_sqlite3.smt2 |   20.040s | 328.0MiB| timeout | 0 |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2019A/picorv32_mutCY_QF_AUFBV_NONINCR.smt2 |   20.042s | 143.0MiB| timeout | 0 |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2019A/picorv32_mutAY_QF_AUFBV_NONINCR.smt2 |   20.042s | 125.0MiB| timeout | 0 |  |
|non-incremental/QF_AUFBV/20210301-Alive2/gcc/033_gcc.smt2    |   20.043s | 104.0MiB| timeout | 0 |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/VexRiscv-regch0-30-compact-mem.smt2 |   20.043s | 135.0MiB| timeout | 0 |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/VexRiscv-regch0-20-compact-mem.smt2 |   20.044s | 91.936MiB| timeout | 0 |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/picorv32-pcregs-compact-mem.smt2 |   20.047s | 113.0MiB| timeout | 0 |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2019A/picorv32_mutBY_QF_AUFBV_NONINCR.smt2 |   20.050s | 149.0MiB| timeout | 0 |  |
|non-incremental/QF_AUFBV/20210301-Alive2/gcc/073_gcc.smt2    |   20.053s | 285.0MiB| timeout | 0 |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2019A/picorv32_mutBX_QF_AUFBV_NONINCR.smt2 |   20.062s | 225.0MiB| timeout | 0 |  |
|non-incremental/QF_AUFBV/20231002-nysm/sha512_block_data_order_one_block_correct_fn_calls_equal_no_rewrite.smt2 |   20.078s | 985.0MiB| timeout | 0 |  |
|non-incremental/QF_AUFBV/20231002-nysm/sha512_block_data_order_loop_inductive_invariant_no_rewriteLoopInductive.smt2 |   20.107s | 719.0MiB| timeout | 0 |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/ponylink-slaveTXlen-sat-compact-mem.smt2 |   20.114s | 1160.0MiB| timeout | 0 |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/zipcpu-pfcache-compact-mem.smt2 |   20.124s | 997.0MiB| timeout | 0 |  |
|non-incremental/QF_AUFBV/2019-Wolf-fmbench/2018E/ponylink-slaveTXlen-unsat-compact-mem.smt2 |   20.150s | 1234.0MiB| timeout | 0 |  |
|non-incremental/QF_AUFBV/20231002-nysm/sha512_block_armv8_loop_inductive_invariant_no_rewriteLoopInductive.smt2 |   20.174s | 1605.0MiB| timeout | 0 |  |
|non-incremental/QF_AUFBV/2019-Gonzalvez/opStructure_C_16_32_2_2.smt2 |   20.218s | 2041.0MiB| timeout | 0 |  |
|non-incremental/QF_AUFBV/2019-Gonzalvez/opStructure_O_16_32_2_2.smt2 |   20.273s | 3699.0MiB| timeout | 0 |  |
|non-incremental/QF_AUFBV/2019-Gonzalvez/opStructure_O_32_16_2_2.smt2 |   20.276s | 3699.0MiB| timeout | 0 |  |
|non-incremental/QF_AUFBV/2019-Gonzalvez/opStructure_O_32_32_2_2.smt2 |   20.294s | 3723.0MiB| timeout | 0 |  |
|non-incremental/QF_AUFBV/2019-Gonzalvez/opStructure_O_16_24_2_2.smt2 |   20.298s | 3694.0MiB| timeout | 0 |  |
|non-incremental/QF_AUFBV/20231002-nysm/sha512_block_armv8_loop_inductive_invariantLoopInductive.smt2 |   20.313s | 2548.0MiB| timeout | 0 |  |
