# .

* SAT 58
* UNSAT 17
* TIMEOUT 82
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-08-20 22:08:36 UTC
Job description: Z3Prover/bench#3613: theory_lra m_bool_var2bound u_map -> dense ptr_vector. QF_ANIA.
Job tag: iss3613-QF_ANIA
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 7ba26830413225af507adb14cb99c16e129dbc5b
Z3 branch: iss3613-bool_var2bound-dense
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_ANIA.tar.zst?download=1
Z3 commit message: theory_lra: index m_bool_var2bound by bool_var instead of hashing it

m_bool_var2bound was a u_map<api_bound*>. u_hash is the identity, so
every key lands at slot (bv & mask), and internalize_atom does
erase(bv) immediately before insert(bv, b). core_hashtable::insert
only stops its linear probe at a FREE slot -- a deleted slot is
remembered as a reuse candidate but does not terminate the scan.

bool_vars are recycled on backtracking, so on quantifier- and
theory-driven workloads the same small id range is internalized over
and over. The live keys then occupy a contiguous prefix of the table
that is permanently used-or-deleted, and each insert walks that whole
prefix looking for a free slot. The operation becomes
O(#distinct bool_vars) instead of O(1).

On inputs/issues/iss-2178/bug-1.smt2 (AUFLIRA, Z3Prover/bench#3613)
this made the insert alone 18.8% of all instructions: 147,668 inserts
costing 11,279 instructions each. Replaying the real key trace through
the probing logic gives 207,448,738 probes, 1,404.8 per insert, for a
table holding only 8,699 live keys.

bool_var ids are dense small unsigned integers, and smt already keeps
several bool_var-indexed vectors, so index a ptr_vector directly.
api_bound* is never null when stored, which makes the
find/contains -> get(bv, nullptr) rewrite exact.

Measured on bug-1.smt2, same tree and flags, identical search
(conflicts, decisions, mk-bool-var, quant-instantiations, rlimit-count
and memory all unchanged):

  instructions, rlimit=3000000   8,872,944,804 -> 7,202,192,073  -18.8%
  wall, rlimit= 3,000,000              1.55s -> 1.40s   -10%
  wall, rlimit=20,000,000             17.48s -> 12.84s  -27%
  wall, rlimit=60,000,000             71.41s -> 43.44s  -39%

No memory regression: max-memory 187.47 -> 186.29 MB.

A differential run over 600 benchmarks from Z3Prover/bench at
rlimit=2000000, comparing full stdout and stderr, shows no differences.

src/sat/smt/arith_solver.h has the same u_map and the same
erase-then-insert pattern; it is left alone here.
</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | INFO |
|------------|----------:|---------:|-------------:| ----------:|------|
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_4.smt2 |    0.031s | 20.764MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_3.smt2 |    0.044s | 20.216MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_0.smt2 |    0.048s | 20.88MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20240413-AutomizerLoopAcceleration/in-de42.c_AllErrorsAtOnce_Iteration7_0.smt2 |    0.053s | 20.62MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_9.smt2 |    0.061s | 20.612MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_13.smt2 |    0.063s | 20.296MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_0.smt2 |    0.066s | 20.368MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_5.smt2 |    0.066s | 20.368MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_7.smt2 |    0.066s | 20.62MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_7.smt2 |    0.067s | 20.396MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_1.smt2 |    0.071s | 20.472MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_12.smt2 |    0.073s | 20.352MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_2.smt2 |    0.075s | 20.916MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_10.smt2 |    0.078s | 20.64MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_1.smt2 |    0.079s | 20.868MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_4.smt2 |    0.081s | 20.396MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_2.smt2 |    0.081s | 20.468MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_14.smt2 |    0.082s | 20.396MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_6.smt2 |    0.082s | 20.892MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_6.smt2 |    0.083s | 20.628MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_5.smt2 |    0.084s | 21.14MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/3.smt2 |    0.085s | 21.132MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_8.smt2 |    0.085s | 20.768MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_3.smt2 |    0.087s | 20.608MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/3.smt2 |    0.092s | 21.164MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_9.smt2 |    0.093s | 20.656MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/3.smt2 |    0.097s | 21.38MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_dekker_true-unreach-call.i.smt2 |    0.099s | 23.268MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_lazy_false-unreach-call.i.smt2 |    0.100s | 22.272MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/4.smt2 |    0.100s | 22.152MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_1.smt2 |    0.100s | 21.944MiB| sat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_peterson_true-unreach-call.i.smt2 |    0.106s | 22.784MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_lamport_true-unreach-call.i.smt2 |    0.110s | 22.84MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_2.smt2 |    0.124s | 21.416MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_5.smt2 |    0.144s | 21.8MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/5.smt2 |    0.186s | 23.44MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/3.smt2 |    0.195s | 23.068MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/4.smt2 |    0.199s | 22.724MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_9.smt2 |    0.204s | 22.448MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_3.smt2 |    0.220s | 23.448MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_3.smt2 |    0.273s | 23.924MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_0.smt2 |    0.285s | 23.54MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_4.smt2 |    0.297s | 23.18MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_7.smt2 |    0.390s | 23.436MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/lcm2.c_AllErrorsAtOnce_Iteration3_0.smt2 |    0.413s | 22.444MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/5.smt2 |    0.503s | 24.456MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/6.smt2 |    0.669s | 25.492MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/4.smt2 |    0.721s | 26.956MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20240413-AutomizerLoopAcceleration/ddlm2013.i_AllErrorsAtOnce_Iteration5_0.smt2 |    0.812s | 21.94MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/7.smt2 |    1.045s | 29.332MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy2.i.cil.c_4.smt2 |    1.173s | 25.26MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy2.i.cil.c_3.smt2 |    1.201s | 25.168MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/9.smt2 |    1.729s | 32.456MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_6.smt2 |    1.770s | 34.904MiB| sat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_fib_true-unreach-call.i.smt2 |    1.868s | 22.76MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_8.smt2 |    1.902s | 26.428MiB| sat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_fib_false-unreach-call.i.smt2 |    1.922s | 22.584MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/7.smt2 |    2.187s | 30.696MiB| sat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer2/linux-stable-1575714-1-150_1a-drivers--net--wireless--b43--b43.ko-entry_point_ldv-val-v0.8_false-unreach-call.cil.out.c_TraceCheck_Iteration4_0.smt2 |    2.898s | 293.0MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/4.smt2 |    3.435s | 32.376MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/6.smt2 |    5.105s | 36.08MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_9.smt2 |    5.543s | 39.632MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_12.smt2 |    5.706s | 39.648MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/s3_clnt.blast.04_false-unreach-call.i.cil.c_AllErrorsAtOnce_Iteration6_TraceCheck_0.smt2 |    5.788s | 47.872MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_6.smt2 |    6.029s | 39.548MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/8.smt2 |    6.191s | 36.104MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_10.smt2 |    6.600s | 42.368MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_13.smt2 |    7.064s | 42.568MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_4.smt2 |    7.299s | 42.472MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_7.smt2 |    7.324s | 42.724MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/5.smt2 |    7.345s | 42.488MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer2/linux-3.12-rc1.tar.xz-144_2a-drivers--media--usb--stk1160--stk1160.ko-entry_point_false-unreach-call.cil.out.c_TraceCheck_Iteration9_0.smt2 |   14.721s | 673.0MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/diskperf_false-unreach-call.i.cil.c_1.smt2 |   15.454s | 51.556MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg40_true-unreach-call.i_AllErrorsAtOnce_Iteration17_TraceCheck_0.smt2 |   17.744s | 97.844MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/8.smt2 |   19.688s | 57.056MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/6.smt2 |   20.016s | 65.872MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_11.smt2 |   20.026s | 62.184MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_14.smt2 |   20.028s | 60.92MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_2.smt2 |   20.030s | 111.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/parport.i.cil-2.c_0.smt2 |   20.030s | 54.672MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/10.smt2 |   20.031s | 59.9MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_3.smt2 |   20.031s | 105.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rangesum40_false-unreach-call.i_AllErrorsAtOnce_Iteration19_TraceCheck_0.smt2 |   20.032s | 101.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_5.smt2 |   20.032s | 109.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/usb_urb-drivers-net-can-usb-ems_usb.ko_false-unreach-call.cil.out.i_2.smt2 |   20.032s | 208.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/diskperf_false-unreach-call.i.cil.c_9.smt2 |   20.032s | 94.748MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_4.smt2 |   20.034s | 103.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/Haystacks-07.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.045s | 33.804MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy2.i.cil.c_1.smt2 |   20.045s | 63.212MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/sum_array-2-2.i_AllErrorsAtOnce_Iteration8_0.smt2 |   20.046s | 33.908MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-3.c_1.smt2 |   20.046s | 66.688MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-2.c_2.smt2 |   20.046s | 55.672MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/11.smt2 |   20.047s | 54.384MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_aso_false-unreach-call.2.M4.c_0.smt2 |   20.047s | 91.272MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/Haystacks-12.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.047s | 80.828MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/8.smt2 |   20.050s | 98.812MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_6.smt2 |   20.050s | 113.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-1.c_0.smt2 |   20.050s | 56.224MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/Haystacks-14.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.050s | 114.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_12.smt2 |   20.051s | 110.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-2.c_3.smt2 |   20.051s | 78.136MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/11.smt2 |   20.053s | 73.508MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/6.smt2 |   20.053s | 68.208MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_11.smt2 |   20.053s | 105.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/9.smt2 |   20.054s | 64.156MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_1.smt2 |   20.054s | 105.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/9.smt2 |   20.057s | 116.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy_true-unreach-call_true-valid-memsafety.i.cil.c_8.smt2 |   20.057s | 66.236MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/s3_clnt.blast.01_false-unreach-call.i.cil.c_AllErrorsAtOnce_Iteration11_TraceCheck_0.smt2 |   20.057s | 42.704MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/sum02-1.c_AllErrorsAtOnce_Iteration3_0.smt2 |   20.057s | 21.392MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/7.smt2 |   20.058s | 77.196MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/parport_true-unreach-call.i.cil.c_6.smt2 |   20.059s | 57.552MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/11.smt2 |   20.060s | 54.18MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/11.smt2 |   20.063s | 124.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/10.smt2 |   20.064s | 155.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/10.smt2 |   20.064s | 98.38MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_1.smt2 |   20.064s | 103.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/parport.i.cil-1.c_0.smt2 |   20.064s | 51.984MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-2.c_1.smt2 |   20.065s | 77.108MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_10.smt2 |   20.066s | 64.152MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_5.smt2 |   20.066s | 114.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/9.smt2 |   20.067s | 84.304MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/8.smt2 |   20.067s | 73.28MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/parport_false-unreach-call.i.cil.c_6.smt2 |   20.068s | 56.644MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-1.c_1.smt2 |   20.069s | 79.852MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/10.smt2 |   20.070s | 50.824MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/5.smt2 |   20.070s | 57.548MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_8.smt2 |   20.070s | 66.948MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_6.smt2 |   20.071s | 104.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/usb_urb-drivers-hid-usbhid-usbmouse.ko_false-unreach-call.cil.out.i_AllErrorsAtOnce_Iteration21_TraceCheck_0.smt2 |   20.071s | 62.676MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/sum_array-2.i_AllErrorsAtOnce_Iteration8_0.smt2 |   20.071s | 31.536MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-1.c_4.smt2 |   20.072s | 68.128MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_4.smt2 |   20.073s | 111.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/diskperf.i.cil-1.c_0.smt2 |   20.073s | 108.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_7.smt2 |   20.074s | 110.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_5.smt2 |   20.074s | 61.408MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_8.smt2 |   20.074s | 61.536MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_11.smt2 |   20.074s | 68.58MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_13.smt2 |   20.075s | 115.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_10.smt2 |   20.075s | 111.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/7.smt2 |   20.076s | 81.42MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/usb_urb-drivers-net-can-usb-ems_usb.ko_false-unreach-call.cil.out.i_1.smt2 |   20.076s | 146.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_2.smt2 |   20.076s | 110.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_3.smt2 |   20.076s | 187.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy_false-unreach-call.i.cil.c_8.smt2 |   20.076s | 72.888MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-3.c_5.smt2 |   20.076s | 66.312MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/kbfiltr_false-unreach-call.i.cil.c_5.smt2 |   20.077s | 102.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-3.c_4.smt2 |   20.077s | 66.412MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/kbfiltr_false-unreach-call.i.cil.c_3.smt2 |   20.078s | 106.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_2.smt2 |   20.080s | 112.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_aso_false-unreach-call.2.M4.c_2.smt2 |   20.080s | 125.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-1.c_5.smt2 |   20.081s | 80.76MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_1.smt2 |   20.083s | 182.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/diskperf_true-unreach-call.i.cil.c_2.smt2 |   20.084s | 124.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-2.c_4.smt2 |   20.084s | 77.248MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_aso_false-unreach-call.2.M4.c_3.smt2 |   20.121s | 483.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/diskperf_true-unreach-call.i.cil.c_1.smt2 |   20.129s | 684.0MiB| timeout | 0 |  |
