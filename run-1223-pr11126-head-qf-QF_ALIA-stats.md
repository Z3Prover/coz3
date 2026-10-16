# .

* SAT 61
* UNSAT 71
* TIMEOUT 44
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-10-16 00:37:38 UTC
Job description: PR11126 candidate QF_ALIA: paired comparison against baseline run 38481337387; same pinned master plus PR commits
Job tag: pr11126-head-qf-QF_ALIA
Runner: rise-runner-2
Z3 repo: levnach/z3
Z3 commit: bb027e1104bc379deaaf084e6f878ef4a213bf4a
Z3 branch: bb027e1104bc379deaaf084e6f878ef4a213bf4a
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_ALIA.tar.zst?download=1
Z3 commit message: perf: apply Z3 PR 11126 onto pinned master

Benchmark-only candidate based on master f05a5c6518028169dee82d8bb3108e71a80f3b5a. Applies PR commits 483ca99b8742930b4147b7445529c898ca159663, 611545acc6f4fd9c49fcb4b53e734f5a0cdada0e, and 129ad1e04ef520973327de75d205e6c81cc0490c without modifying the PR branch.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
Copilot-Session: 6ba1bb05-6b4e-4cd9-866f-5b956b261736
</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | INFO |
|------------|----------:|---------:|-------------:| ----------:|------|
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00004_001.cvc.smt2 |    0.022s | 20.62MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00004_001.cvc.smt2 |    0.023s | 20.86MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00007_001.cvc.smt2 |    0.024s | 20.612MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00002_001.cvc.smt2 |    0.024s | 20.86MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00002_001.cvc.smt2 |    0.024s | 20.608MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/cvc/read2.smt2                       |    0.025s | 20.62MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00003_001.cvc.smt2 |    0.025s | 20.86MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00013_001.cvc.smt2 |    0.025s | 20.608MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00005_001.cvc.smt2 |    0.025s | 20.608MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00008_001.cvc.smt2 |    0.025s | 20.592MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00011_001.cvc.smt2 |    0.026s | 20.604MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00013_001.cvc.smt2 |    0.027s | 21.116MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00015_001.cvc.smt2 |    0.028s | 20.876MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_ffa5fa.smt2                |    0.032s | 19.848MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_5b181b.smt2                |    0.034s | 20.112MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_cb19c7.smt2                |    0.034s | 20.36MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00010_001.cvc.smt2 |    0.034s | 20.824MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00001_001.cvc.smt2 |    0.034s | 20.364MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00008_001.cvc.smt2 |    0.034s | 20.808MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00007_001.cvc.smt2 |    0.035s | 20.612MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/misc/queue-th2-6.smt2 |    0.036s | 20.096MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00001_001.cvc.smt2 |    0.036s | 20.604MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_ed9849.smt2                |    0.037s | 21.12MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_13f61c.smt2                |    0.037s | 21.136MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00015_001.cvc.smt2 |    0.037s | 21.072MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00005_001.cvc.smt2 |    0.037s | 20.86MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00009_001.cvc.smt2 |    0.037s | 20.856MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00006_001.cvc.smt2 |    0.037s | 20.364MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/misc/stack-invalid-6.smt2 |    0.038s | 20.864MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-safe-5.smt2 |    0.038s | 20.868MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_fdec13.smt2                |    0.038s | 21.128MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_7fd2c4.smt2                |    0.038s | 20.368MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00009_001.cvc.smt2 |    0.038s | 20.612MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00012_001.cvc.smt2 |    0.039s | 21.116MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-invalid-5.smt2 |    0.040s | 21.368MiB| sat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00011_001.cvc.smt2 |    0.040s | 20.864MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00012_001.cvc.smt2 |    0.040s | 20.604MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00006_001.cvc.smt2 |    0.041s | 20.588MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_174f4d.smt2                |    0.042s | 20.856MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/misc/stack-th1-6.smt2 |    0.043s | 20.348MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/misc/stack-th2-6.smt2 |    0.043s | 20.096MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00003_001.cvc.smt2 |    0.044s | 20.864MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_bia_np_sf_ai_00014_001.cvc.smt2 |    0.044s | 20.812MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-safe-10.smt2 |    0.045s | 21.64MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_408ff0.smt2                |    0.045s | 21.204MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00014_001.cvc.smt2 |    0.045s | 20.868MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/misc/queue-th1-6.smt2 |    0.047s | 20.352MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-invalid-10.smt2 |    0.048s | 22.148MiB| sat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_d421cb.smt2                |    0.049s | 20.876MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/ios/ios_t1_ios_np_sf_ai_00010_001.cvc.smt2 |    0.049s | 20.376MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/cvc/pp-dmem2.smt2                    |    0.050s | 22.148MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/cvc/pp-dmem-a.smt2                   |    0.053s | 22.344MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_3031c9.smt2                |    0.054s | 23.004MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/cvc/pp-bloaddata.smt2                |    0.055s | 22.152MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/AllInterval-015.c_AllErrorsAtOnce_Iteration2_0.smt2 |    0.056s | 23.324MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.5.smt2        |    0.057s | 21.636MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug-5.smt2 |    0.058s | 21.772MiB| sat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_f5059f.smt2                |    0.058s | 22.58MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.6.smt2        |    0.058s | 21.892MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-mutex-5.smt2 |    0.059s | 21.372MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_22b1f2.smt2                |    0.059s | 22.576MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-invalid-20.smt2 |    0.060s | 24.456MiB| sat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_509c40.smt2                |    0.062s | 22.576MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/piVC/piVC_46582a.smt2                |    0.066s | 22.524MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-invalid-15.smt2 |    0.067s | 23.172MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.5.smt2             |    0.068s | 21.12MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-safe-15.smt2 |    0.074s | 22.128MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/cvc/pp-bloaddata-a.smt2              |    0.074s | 22.152MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.6.smt2             |    0.088s | 21.624MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug2-10.smt2 |    0.090s | 22.652MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/AllInterval-019.c_AllErrorsAtOnce_Iteration2_0.smt2 |    0.116s | 24.628MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.7.smt2        |    0.119s | 22.4MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.8.smt2        |    0.122s | 22.668MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/pointer/pointer-safe-20.smt2 |    0.132s | 22.92MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.9.smt2        |    0.132s | 22.912MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug-10.smt2 |    0.140s | 22.912MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.7.smt2             |    0.194s | 21.636MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.10.smt2       |    0.231s | 23.68MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.12.smt2       |    0.246s | 24.456MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug2-15.smt2 |    0.271s | 25.472MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.11.smt2       |    0.310s | 23.936MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.8.smt2             |    0.322s | 22.14MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.13.smt2       |    0.488s | 24.944MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.14.smt2       |    0.576s | 25.224MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.15.smt2            |    0.670s | 26.116MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.9.smt2             |    0.674s | 22.416MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.15.smt2       |    0.743s | 25.16MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_6.smt2 |    0.915s | 34.02MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_1.smt2 |    0.920s | 34.08MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_0.smt2 |    0.931s | 34.048MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_3.smt2 |    0.933s | 34.116MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_4.smt2 |    0.935s | 34.24MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_sync.i_2.smt2 |    0.936s | 34.304MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.18.smt2       |    0.993s | 29.432MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_stateful-1.i_4.smt2 |    1.073s | 33.1MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_stateful-1.i_1.smt2 |    1.081s | 33.484MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_stateful-1.i_3.smt2 |    1.082s | 33.336MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_stateful-1.i_2.smt2 |    1.087s | 32.924MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.10.smt2            |    1.088s | 23.416MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-mutex-10.smt2 |    1.108s | 22.928MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.16.smt2            |    1.185s | 27.78MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.20.smt2            |    1.411s | 29.012MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug2-20.smt2 |    1.422s | 29.012MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.16.smt2       |    1.664s | 27.904MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug-15.smt2 |    1.683s | 26.508MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.18.smt2            |    1.733s | 28.808MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.11.smt2            |    1.832s | 23.932MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.19.smt2            |    1.864s | 29.608MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.22.smt2            |    1.906s | 30.976MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_5.smt2 |    1.920s | 45.656MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.17.smt2            |    2.345s | 27.268MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_6.smt2 |    2.774s | 57.996MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_7.smt2 |    2.780s | 57.692MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.12.smt2            |    2.909s | 24.196MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.23.smt2            |    3.157s | 33.016MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_lazy.i_7.smt2 |    3.666s | 36.068MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_lazy.i_6.smt2 |    3.703s | 35.94MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_lazy.i_3.smt2 |    3.714s | 36.032MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.21.smt2            |    4.827s | 32.392MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.13.smt2            |    5.694s | 25.448MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.21.smt2       |    5.798s | 32.84MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.14.smt2            |    5.955s | 25.728MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.17.smt2       |    6.558s | 30.496MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-mutex-15.smt2 |    7.264s | 26.548MiB| unsat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.19.smt2       |    7.569s | 32.076MiB| sat | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-bug-20.smt2 |    8.907s | 33.404MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.24.smt2            |   12.654s | 36.992MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.20.smt2       |   13.801s | 33.832MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_1.smt2 |   15.903s | 59.776MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_2.smt2 |   15.942s | 59.748MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.25.smt2            |   17.924s | 39.904MiB| sat | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/AllInterval-011.c_AllErrorsAtOnce_Iteration2_0.smt2 |   18.810s | 29.572MiB| sat | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.24.smt2       |   20.013s | 36.088MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_szymanski.i_0.smt2 |   20.015s | 70.392MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_peterson.i_0.smt2 |   20.015s | 64.788MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_read_write_lock-2.i_0.smt2 |   20.016s | 62.892MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_lamport.i_0.smt2 |   20.016s | 71.564MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_time_var_mutex.i_0.smt2 |   20.018s | 81.512MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_fib-2.i_0.smt2 |   20.021s | 100.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_fib-1.i_0.smt2 |   20.021s | 106.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.27.smt2            |   20.021s | 37.724MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.23.smt2       |   20.021s | 37.2MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/array_benchmarks/qlock/qlock-mutex-20.smt2 |   20.022s | 31.364MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.30.smt2            |   20.022s | 39.496MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.26.smt2       |   20.022s | 37.04MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_2.smt2 |   20.023s | 123.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.28.smt2            |   20.023s | 38.764MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_6.smt2 |   20.024s | 126.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/CostasArray-15.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.024s | 28.888MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.30.smt2       |   20.026s | 38.66MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_queue-1.i_0.smt2 |   20.027s | 104.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.25.smt2       |   20.027s | 36.428MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.22.smt2       |   20.027s | 36.192MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_dekker.i_0.smt2 |   20.028s | 63.504MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.29.smt2       |   20.029s | 36.072MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.26.smt2            |   20.029s | 39.904MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.28.smt2       |   20.029s | 39.016MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/AllInterval-016.c_AllErrorsAtOnce_Iteration2_0.smt2 |   20.031s | 26.724MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.induction.27.smt2       |   20.031s | 35.824MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_fib_longer-2.i_0.smt2 |   20.032s | 125.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_5.smt2 |   20.032s | 112.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/qlock2/qlock.base.29.smt2            |   20.032s | 39.592MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_1.smt2 |   20.034s | 103.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_0.smt2 |   20.034s | 120.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_fib_longer-1.i_0.smt2 |   20.034s | 126.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_9.smt2 |   20.035s | 104.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_10.smt2 |   20.035s | 123.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_7.smt2 |   20.036s | 129.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-3.i_7.smt2 |   20.039s | 176.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_8.smt2 |   20.040s | 125.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-3.i_6.smt2 |   20.041s | 99.34MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_11.smt2 |   20.042s | 103.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_read_write_lock-1.i_0.smt2 |   20.043s | 58.184MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_4.smt2 |   20.043s | 122.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/cs_queue-2.i_0.smt2 |   20.044s | 105.0MiB| timeout | 0 |  |
|non-incremental/QF_ALIA/20230321-UltimateAutomizerSvcomp2023/tree-4.i_3.smt2 |   20.047s | 110.0MiB| timeout | 0 |  |
