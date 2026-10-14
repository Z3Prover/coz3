# .

* SAT 61
* UNSAT 15
* TIMEOUT 81
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-10-14 05:24:58 UTC
Job description: PR11126 full QF baseline: pinned master f05a5c651802; paired smoke integrity gate passed | Benchmark suite: https://zenodo.org/records/16740866/files/QF_ANIA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: pr11126-master-qf-https-zenodo.org-records-QF_ANIA
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: f05a5c6518028169dee82d8bb3108e71a80f3b5a
Z3 branch: f05a5c6518028169dee82d8bb3108e71a80f3b5a
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_ANIA.tar.zst?download=1
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
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_8.smt2 |    0.034s | 21.12MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_5.smt2 |    0.034s | 20.876MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_14.smt2 |    0.034s | 20.608MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_1.smt2 |    0.034s | 20.612MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_10.smt2 |    0.035s | 20.612MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_4.smt2 |    0.037s | 20.868MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20240413-AutomizerLoopAcceleration/in-de42.c_AllErrorsAtOnce_Iteration7_0.smt2 |    0.039s | 20.892MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_2.smt2 |    0.040s | 20.612MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_6.smt2 |    0.041s | 20.856MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_7.smt2 |    0.041s | 20.616MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_6.smt2 |    0.042s | 20.864MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_9.smt2 |    0.042s | 21.12MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_4.smt2 |    0.042s | 20.86MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_3.smt2 |    0.042s | 20.856MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/3.smt2 |    0.042s | 21.352MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_0.smt2 |    0.043s | 20.864MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_1.smt2 |    0.043s | 21.12MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_7.smt2 |    0.044s | 21.112MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_5.smt2 |    0.044s | 20.576MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_12.smt2 |    0.045s | 20.856MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_13.smt2 |    0.047s | 20.616MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_3.smt2 |    0.048s | 21.116MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_9.smt2 |    0.049s | 20.852MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_2.smt2 |    0.049s | 20.872MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_0.smt2 |    0.054s | 21.372MiB| sat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_lamport_true-unreach-call.i.smt2 |    0.058s | 23.316MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/3.smt2 |    0.058s | 21.392MiB| sat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_lazy_false-unreach-call.i.smt2 |    0.059s | 22.652MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_peterson_true-unreach-call.i.smt2 |    0.061s | 23.292MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/3.smt2 |    0.066s | 21.38MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_1.smt2 |    0.069s | 22.348MiB| sat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_dekker_true-unreach-call.i.smt2 |    0.073s | 23.524MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/4.smt2 |    0.077s | 22.396MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_2.smt2 |    0.084s | 21.644MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_9.smt2 |    0.106s | 22.804MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_5.smt2 |    0.106s | 22.4MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_3.smt2 |    0.139s | 23.94MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/3.smt2 |    0.141s | 23.42MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/4.smt2 |    0.155s | 23.172MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/5.smt2 |    0.177s | 23.68MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_3.smt2 |    0.198s | 23.944MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_7.smt2 |    0.243s | 23.428MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_0.smt2 |    0.249s | 23.692MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_4.smt2 |    0.357s | 23.688MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/5.smt2 |    0.394s | 24.704MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/6.smt2 |    0.449s | 25.988MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy2.i.cil.c_3.smt2 |    0.484s | 24.46MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy2.i.cil.c_4.smt2 |    0.501s | 24.716MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/4.smt2 |    0.656s | 27.264MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_6.smt2 |    1.433s | 34.816MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/9.smt2 |    1.447s | 32.648MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/6.smt2 |    1.703s | 29.064MiB| sat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_fib_true-unreach-call.i.smt2 |    1.877s | 23.052MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/7.smt2 |    1.880s | 30.7MiB| sat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_fib_false-unreach-call.i.smt2 |    1.896s | 22.396MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer2/linux-stable-1575714-1-150_1a-drivers--net--wireless--b43--b43.ko-entry_point_ldv-val-v0.8_false-unreach-call.cil.out.c_TraceCheck_Iteration4_0.smt2 |    2.193s | 293.0MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_8.smt2 |    2.664s | 26.508MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/4.smt2 |    2.903s | 31.592MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/7.smt2 |    3.152s | 35.752MiB| sat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer2/linux-3.12-rc1.tar.xz-144_2a-drivers--media--usb--stk1160--stk1160.ko-entry_point_false-unreach-call.cil.out.c_TraceCheck_Iteration9_0.smt2 |    3.179s | 292.0MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_6.smt2 |    4.234s | 39.636MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_12.smt2 |    4.250s | 39.64MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_9.smt2 |    4.335s | 39.384MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/s3_clnt.blast.04_false-unreach-call.i.cil.c_AllErrorsAtOnce_Iteration6_TraceCheck_0.smt2 |    4.822s | 42.192MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_4.smt2 |    5.374s | 42.468MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_7.smt2 |    5.414s | 42.716MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_13.smt2 |    5.418s | 42.536MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_10.smt2 |    5.461s | 42.46MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/5.smt2 |    8.477s | 47.12MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/8.smt2 |    8.619s | 38.12MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg40_true-unreach-call.i_AllErrorsAtOnce_Iteration17_TraceCheck_0.smt2 |   10.820s | 88.944MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/diskperf_false-unreach-call.i.cil.c_1.smt2 |   12.082s | 51.276MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rangesum40_false-unreach-call.i_AllErrorsAtOnce_Iteration19_TraceCheck_0.smt2 |   13.115s | 96.4MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/parport.i.cil-2.c_0.smt2 |   15.086s | 59.172MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/8.smt2 |   15.680s | 52.208MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/s3_clnt.blast.01_false-unreach-call.i.cil.c_AllErrorsAtOnce_Iteration11_TraceCheck_0.smt2 |   19.138s | 49.128MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/lcm2.c_AllErrorsAtOnce_Iteration3_0.smt2 |   20.015s | 26.812MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/sum_array-2.i_AllErrorsAtOnce_Iteration8_0.smt2 |   20.015s | 31.712MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/10.smt2 |   20.019s | 70.532MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/6.smt2 |   20.019s | 81.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-1.c_4.smt2 |   20.020s | 77.528MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-3.c_4.smt2 |   20.020s | 82.748MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-3.c_1.smt2 |   20.020s | 82.048MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/5.smt2 |   20.021s | 63.632MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/9.smt2 |   20.022s | 98.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/11.smt2 |   20.023s | 55.592MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/sum_array-2-2.i_AllErrorsAtOnce_Iteration8_0.smt2 |   20.025s | 32.04MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/sum02-1.c_AllErrorsAtOnce_Iteration3_0.smt2 |   20.025s | 21.628MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/kbfiltr_false-unreach-call.i.cil.c_3.smt2 |   20.026s | 107.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_5.smt2 |   20.026s | 74.964MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/diskperf.i.cil-1.c_0.smt2 |   20.026s | 108.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/11.smt2 |   20.026s | 76.68MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20240413-AutomizerLoopAcceleration/ddlm2013.i_AllErrorsAtOnce_Iteration5_0.smt2 |   20.026s | 25.844MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_11.smt2 |   20.027s | 105.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_3.smt2 |   20.027s | 105.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_1.smt2 |   20.028s | 105.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/parport_false-unreach-call.i.cil.c_6.smt2 |   20.028s | 61.552MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/parport_true-unreach-call.i.cil.c_6.smt2 |   20.028s | 58.448MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy2.i.cil.c_1.smt2 |   20.028s | 66.344MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-2.c_2.smt2 |   20.028s | 58.048MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-2.c_3.smt2 |   20.028s | 82.792MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/6.smt2 |   20.028s | 78.528MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy_false-unreach-call.i.cil.c_8.smt2 |   20.029s | 75.028MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_8.smt2 |   20.029s | 70.376MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_10.smt2 |   20.029s | 67.276MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_8.smt2 |   20.029s | 75.22MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-2.c_1.smt2 |   20.029s | 82.728MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/Haystacks-07.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.029s | 34.708MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/11.smt2 |   20.029s | 98.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/9.smt2 |   20.029s | 127.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/parport.i.cil-1.c_0.smt2 |   20.030s | 55.7MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-1.c_0.smt2 |   20.030s | 58.18MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-2.c_4.smt2 |   20.030s | 82.68MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/8.smt2 |   20.030s | 110.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/diskperf_true-unreach-call.i.cil.c_2.smt2 |   20.031s | 125.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_5.smt2 |   20.031s | 114.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/10.smt2 |   20.031s | 52.86MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/9.smt2 |   20.031s | 82.256MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/10.smt2 |   20.031s | 88.736MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_aso_false-unreach-call.2.M4.c_0.smt2 |   20.032s | 131.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_11.smt2 |   20.032s | 73.204MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_12.smt2 |   20.033s | 111.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_10.smt2 |   20.033s | 112.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/10.smt2 |   20.033s | 103.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_aso_false-unreach-call.2.M4.c_2.smt2 |   20.034s | 124.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_11.smt2 |   20.034s | 75.024MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_7.smt2 |   20.034s | 111.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/Haystacks-12.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.034s | 80.676MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/7.smt2 |   20.034s | 87.92MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_2.smt2 |   20.035s | 123.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy_true-unreach-call_true-valid-memsafety.i.cil.c_8.smt2 |   20.035s | 66.404MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_4.smt2 |   20.035s | 112.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/usb_urb-drivers-hid-usbhid-usbmouse.ko_false-unreach-call.cil.out.i_AllErrorsAtOnce_Iteration21_TraceCheck_0.smt2 |   20.036s | 79.156MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_1.smt2 |   20.036s | 103.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_14.smt2 |   20.036s | 74.704MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/8.smt2 |   20.036s | 73.344MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/7.smt2 |   20.036s | 84.712MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/11.smt2 |   20.036s | 131.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_2.smt2 |   20.037s | 110.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/diskperf_false-unreach-call.i.cil.c_9.smt2 |   20.037s | 95.572MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_5.smt2 |   20.038s | 109.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_6.smt2 |   20.038s | 114.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/kbfiltr_false-unreach-call.i.cil.c_5.smt2 |   20.038s | 104.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_13.smt2 |   20.038s | 116.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_6.smt2 |   20.040s | 104.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_2.smt2 |   20.040s | 112.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/Haystacks-14.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.040s | 117.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_3.smt2 |   20.042s | 231.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_4.smt2 |   20.043s | 104.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_1.smt2 |   20.045s | 193.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-1.c_1.smt2 |   20.047s | 81.616MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/usb_urb-drivers-net-can-usb-ems_usb.ko_false-unreach-call.cil.out.i_2.smt2 |   20.048s | 209.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/usb_urb-drivers-net-can-usb-ems_usb.ko_false-unreach-call.cil.out.i_1.smt2 |   20.048s | 146.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-1.c_5.smt2 |   20.049s | 81.796MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-3.c_5.smt2 |   20.050s | 81.992MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/diskperf_true-unreach-call.i.cil.c_1.smt2 |   20.075s | 684.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_aso_false-unreach-call.2.M4.c_3.smt2 |   20.078s | 500.0MiB| timeout | 0 |  |
