# .

* SAT 58
* UNSAT 16
* TIMEOUT 83
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-08-20 23:46:50 UTC
Job description: Baseline for Z3Prover/bench#3613 A/B: master (414189804, exact parent of iss3613-bool_var2bound-dense). QF_ANIA.
Job tag: iss3613-base-QF_ANIA
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 414189804519311450363b3858ed4a15501a4afe
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_ANIA.tar.zst?download=1
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
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_0.smt2 |    0.026s | 20.344MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_6.smt2 |    0.029s | 20.64MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_5.smt2 |    0.036s | 20.368MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_3.smt2 |    0.042s | 20.248MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_10.smt2 |    0.043s | 20.416MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_14.smt2 |    0.048s | 20.372MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_0.smt2 |    0.048s | 20.892MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_7.smt2 |    0.048s | 20.612MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_4.smt2 |    0.050s | 20.408MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_13.smt2 |    0.053s | 20.364MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20240413-AutomizerLoopAcceleration/in-de42.c_AllErrorsAtOnce_Iteration7_0.smt2 |    0.056s | 20.648MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_1.smt2 |    0.057s | 20.704MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_3.smt2 |    0.059s | 20.6MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_2.smt2 |    0.061s | 20.932MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_2.smt2 |    0.061s | 20.532MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_9.smt2 |    0.064s | 20.416MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_5.smt2 |    0.065s | 20.616MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_8.smt2 |    0.068s | 20.88MiB| sat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_lazy_false-unreach-call.i.smt2 |    0.072s | 22.152MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_9.smt2 |    0.075s | 20.676MiB| sat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_lamport_true-unreach-call.i.smt2 |    0.082s | 22.804MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_peterson_true-unreach-call.i.smt2 |    0.083s | 22.896MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_7.smt2 |    0.085s | 20.624MiB| sat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_dekker_true-unreach-call.i.smt2 |    0.088s | 23.288MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_6.smt2 |    0.094s | 20.364MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/3.smt2 |    0.098s | 21.632MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/4.smt2 |    0.107s | 22.152MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_5.smt2 |    0.108s | 21.792MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_12.smt2 |    0.113s | 20.612MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/sum10_true-unreach-call_true-termination.i_1.smt2 |    0.114s | 20.668MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_1.smt2 |    0.118s | 21.804MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg20_true-unreach-call.i_4.smt2 |    0.120s | 20.772MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/3.smt2 |    0.122s | 21.612MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/3.smt2 |    0.138s | 21.132MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_2.smt2 |    0.160s | 21.624MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/3.smt2 |    0.168s | 22.964MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_9.smt2 |    0.185s | 22.376MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_3.smt2 |    0.194s | 23.904MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/4.smt2 |    0.196s | 22.92MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/5.smt2 |    0.253s | 23.508MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_3.smt2 |    0.291s | 23.692MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_0.smt2 |    0.310s | 23.492MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_7.smt2 |    0.345s | 23.428MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_4.smt2 |    0.414s | 23.284MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/lcm2.c_AllErrorsAtOnce_Iteration3_0.smt2 |    0.437s | 22.364MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/5.smt2 |    0.455s | 24.512MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/6.smt2 |    0.484s | 25.464MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/4.smt2 |    0.802s | 27.148MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20240413-AutomizerLoopAcceleration/ddlm2013.i_AllErrorsAtOnce_Iteration5_0.smt2 |    0.837s | 21.832MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/7.smt2 |    1.079s | 29.352MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy2.i.cil.c_3.smt2 |    1.132s | 25.216MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy2.i.cil.c_4.smt2 |    1.221s | 25.104MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/9.smt2 |    1.712s | 32.652MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_8.smt2 |    1.904s | 26.508MiB| sat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_fib_false-unreach-call.i.smt2 |    1.984s | 22.572MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer/cs_fib_true-unreach-call.i.smt2 |    1.989s | 22.848MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_6.smt2 |    2.102s | 35.256MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/7.smt2 |    2.225s | 30.808MiB| sat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer2/linux-stable-1575714-1-150_1a-drivers--net--wireless--b43--b43.ko-entry_point_ldv-val-v0.8_false-unreach-call.cil.out.c_TraceCheck_Iteration4_0.smt2 |    3.164s | 293.0MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/4.smt2 |    3.385s | 32.528MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_9.smt2 |    5.091s | 40.0MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/6.smt2 |    5.218s | 36.336MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_6.smt2 |    5.270s | 39.964MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_12.smt2 |    5.366s | 40.112MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/8.smt2 |    5.891s | 36.328MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/s3_clnt.blast.04_false-unreach-call.i.cil.c_AllErrorsAtOnce_Iteration6_TraceCheck_0.smt2 |    6.078s | 47.38MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_13.smt2 |    6.457s | 42.708MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_10.smt2 |    6.526s | 42.856MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_7.smt2 |    6.583s | 42.676MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_4.smt2 |    6.704s | 42.828MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/5.smt2 |    7.416s | 42.616MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/UltimateAutomizer2/linux-3.12-rc1.tar.xz-144_2a-drivers--media--usb--stk1160--stk1160.ko-entry_point_false-unreach-call.cil.out.c_TraceCheck_Iteration9_0.smt2 |   14.191s | 669.0MiB| unsat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/diskperf_false-unreach-call.i.cil.c_1.smt2 |   16.140s | 51.744MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/8.smt2 |   19.716s | 57.112MiB| sat | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/s3_clnt.blast.01_false-unreach-call.i.cil.c_AllErrorsAtOnce_Iteration11_TraceCheck_0.smt2 |   20.020s | 42.724MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_11.smt2 |   20.022s | 61.896MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_aso_false-unreach-call.2.M4.c_0.smt2 |   20.023s | 92.644MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_14.smt2 |   20.025s | 60.672MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rangesum40_false-unreach-call.i_AllErrorsAtOnce_Iteration19_TraceCheck_0.smt2 |   20.025s | 105.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/diskperf_false-unreach-call.i.cil.c_9.smt2 |   20.025s | 94.44MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/7.smt2 |   20.026s | 81.888MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_3.smt2 |   20.026s | 105.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/parport.i.cil-2.c_0.smt2 |   20.026s | 48.784MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_5.smt2 |   20.028s | 108.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/11.smt2 |   20.029s | 54.296MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_2.smt2 |   20.032s | 111.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/5.smt2 |   20.033s | 57.688MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_11.smt2 |   20.034s | 105.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/kbfiltr_false-unreach-call.i.cil.c_5.smt2 |   20.036s | 103.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_1.smt2 |   20.036s | 105.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_8.smt2 |   20.041s | 67.544MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_11.smt2 |   20.041s | 69.524MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_6.smt2 |   20.043s | 103.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_4.smt2 |   20.043s | 111.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_aso_false-unreach-call.2.M4.c_2.smt2 |   20.043s | 126.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_4.smt2 |   20.045s | 103.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy_true-unreach-call_true-valid-memsafety.i.cil.c_8.smt2 |   20.045s | 66.776MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_8.smt2 |   20.045s | 62.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/sum_array-2-2.i_AllErrorsAtOnce_Iteration8_0.smt2 |   20.045s | 34.368MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_2.smt2 |   20.046s | 112.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/parport_false-unreach-call.i.cil.c_6.smt2 |   20.046s | 56.872MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_5.smt2 |   20.047s | 62.52MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_2.smt2 |   20.048s | 110.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_13.smt2 |   20.052s | 114.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/Haystacks-07.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.054s | 34.06MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_7.smt2 |   20.055s | 110.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy_false-unreach-call.i.cil.c_8.smt2 |   20.055s | 73.364MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_10.smt2 |   20.056s | 111.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_6.smt2 |   20.056s | 112.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_true-unreach-call.i.cil.c_12.smt2 |   20.056s | 110.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/usb_urb-drivers-hid-usbhid-usbmouse.ko_false-unreach-call.cil.out.i_AllErrorsAtOnce_Iteration21_TraceCheck_0.smt2 |   20.060s | 63.324MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-2.c_3.smt2 |   20.061s | 78.476MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-2.c_2.smt2 |   20.065s | 56.04MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/9.smt2 |   20.069s | 64.424MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/11.smt2 |   20.072s | 54.624MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/8.smt2 |   20.073s | 72.768MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/sum02-1.c_AllErrorsAtOnce_Iteration3_0.smt2 |   20.073s | 21.488MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/floppy2_true-unreach-call_true-termination.i.cil.c_10.smt2 |   20.074s | 64.504MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/usb_urb-drivers-net-can-usb-ems_usb.ko_false-unreach-call.cil.out.i_2.smt2 |   20.074s | 211.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_1.smt2 |   20.078s | 103.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/usb_urb-drivers-net-can-usb-ems_usb.ko_false-unreach-call.cil.out.i_1.smt2 |   20.079s | 148.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/10.smt2 |   20.082s | 158.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/kbfiltr_false-unreach-call.i.cil.c_3.smt2 |   20.082s | 106.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/diskperf_true-unreach-call.i.cil.c_2.smt2 |   20.083s | 124.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-1.c_1.smt2 |   20.083s | 80.336MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-1.c_5.smt2 |   20.083s | 80.404MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/9.smt2 |   20.086s | 85.092MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/same/10.smt2 |   20.087s | 51.516MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/10.smt2 |   20.087s | 63.776MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-3.c_4.smt2 |   20.090s | 66.988MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/sum_array-2.i_AllErrorsAtOnce_Iteration8_0.smt2 |   20.091s | 31.488MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/10.smt2 |   20.094s | 99.144MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-2.c_4.smt2 |   20.094s | 79.192MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_aso_false-unreach-call.2.M4.c_3.smt2 |   20.096s | 466.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-2.c_1.smt2 |   20.096s | 76.908MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy2.i.cil.c_1.smt2 |   20.098s | 62.32MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/7.smt2 |   20.099s | 79.576MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/parport.i.cil-1.c_0.smt2 |   20.104s | 52.464MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_1.smt2 |   20.105s | 180.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-1.c_0.smt2 |   20.105s | 56.38MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-1.c_4.smt2 |   20.107s | 68.58MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/unsound/diff/11.smt2 |   20.108s | 74.22MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/cdaudio_false-unreach-call.i.cil.c_5.smt2 |   20.108s | 113.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/Haystacks-14.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.111s | 115.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/diskperf.i.cil-1.c_0.smt2 |   20.111s | 107.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/diff/6.smt2 |   20.112s | 69.824MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/parport_true-unreach-call.i.cil.c_6.smt2 |   20.112s | 58.54MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/8.smt2 |   20.113s | 99.852MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/Haystacks-12.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.116s | 80.832MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/6.smt2 |   20.117s | 66.32MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-3.c_1.smt2 |   20.119s | 66.332MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20230321-UltimateAutomizerSvcomp2023/floppy.i.cil-3.c_5.smt2 |   20.119s | 66.98MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/avg40_true-unreach-call.i_AllErrorsAtOnce_Iteration17_TraceCheck_0.smt2 |   20.120s | 77.42MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/9.smt2 |   20.124s | 115.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20211213-GrandProduct-Ozdemir/sound/same/11.smt2 |   20.126s | 126.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/diskperf_true-unreach-call.i.cil.c_1.smt2 |   20.130s | 681.0MiB| timeout | 0 |  |
|non-incremental/QF_ANIA/20190429-UltimateAutomizerSvcomp2019/rekh_ctm_false-unreach-call.3_true-termination.c_3.smt2 |   20.131s | 172.0MiB| timeout | 0 |  |
