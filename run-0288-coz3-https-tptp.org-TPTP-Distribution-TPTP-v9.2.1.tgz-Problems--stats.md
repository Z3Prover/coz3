# .

* SAT 0
* UNSAT 0
* TIMEOUT 92
* UNKNOWN 0

* ERRORS 58 (parser:53, error:5)

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://tptp.org/TPTP/Distribution/TPTP-v9.2.1.tgz Problems/BOO | Source list: benchmarks-tptp.txt
Job tag: coz3-https-tptp.org-TPTP-Distribution-TPTP-v9.2.1.tgz-Problems-
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 56bf04e30a9cfd601b1dcb4159604e81929a02ee
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://tptp.org/TPTP/Distribution/TPTP-v9.2.1.tgz Problems/BOO
Z3 commit message: Fix qe-lite de Bruijn reindexing after bounded quantifier expansion (#9996)

`qe-lite` could produce malformed formulas when expanding bounded
quantifiers under nested binders, leaving outer de Bruijn indices
unshifted after eliminating an inner quantifier (e.g., `(:var 1)`
escaping capture). This change fixes index normalization in that rewrite
path and adds a regression for the reported forall/exists arithmetic
case.

- **Rewrite correctness in bounded quantifier expansion**
- In `src/qe/lite/qe_lite_tactic.cpp`, after substituting bounded
variables in payload conjuncts, apply `inv_var_shifter(num_decls)` so
outer bound variables are reindexed relative to the removed binder.
- This preserves quantifier structure correctness when
`try_expand_bounded_quantifier` eliminates an inner quantifier.

- **Regression coverage for the reported pattern**
- In `src/test/smt_context.cpp`, add a focused quantified arithmetic
formula matching the bug shape:
    - outer `forall (x, x4)`
    - inner `exists (y)`
    - mixed inequalities that trigger qe-lite bounded expansion
- Assert the formula is unsatisfiable, preventing reintroduction of
invalid index handling in this path.

```c++
inst = vs(p, subst_map.size(), subst_map.data());
shift(inst, num_decls, inst); // reindex outer de Bruijn vars after eliminating inner quantifier
```

---------

Co-authored-by: copilot-swe-agent[bot] <198982749+Copilot@users.noreply.github.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|BOO012-4.p                                                   |    0.020s | 19.576MiB| parser | 103 |  |  |
|BOO008-4.p                                                   |    0.020s | 19.596MiB| parser | 103 |  |  |
|BOO014-1.p                                                   |    0.021s | 19.648MiB| parser | 103 |  |  |
|BOO004-4.p                                                   |    0.022s | 19.636MiB| parser | 103 |  |  |
|BOO015-4.p                                                   |    0.022s | 19.452MiB| parser | 103 |  |  |
|BOO006-1.p                                                   |    0.022s | 19.668MiB| parser | 103 |  |  |
|BOO007-2.p                                                   |    0.022s | 19.596MiB| parser | 103 |  |  |
|BOO036-1.p                                                   |    0.024s | 19.592MiB| parser | 103 |  |  |
|BOO011-2.p                                                   |    0.031s | 19.636MiB| parser | 103 |  |  |
|BOO012-3.p                                                   |    0.031s | 19.616MiB| parser | 103 |  |  |
|BOO037-3.p                                                   |    0.035s | 19.596MiB| parser | 103 |  |  |
|BOO011-1.p                                                   |    0.035s | 19.488MiB| parser | 103 |  |  |
|BOO014-4.p                                                   |    0.038s | 19.592MiB| parser | 103 |  |  |
|BOO014-2.p                                                   |    0.041s | 19.62MiB| parser | 103 |  |  |
|BOO008-2.p                                                   |    0.042s | 19.856MiB| parser | 103 |  |  |
|BOO007-4.p                                                   |    0.042s | 19.6MiB| parser | 103 |  |  |
|BOO013-1.p                                                   |    0.043s | 19.748MiB| parser | 103 |  |  |
|BOO003-1.p                                                   |    0.043s | 19.684MiB| parser | 103 |  |  |
|BOO003-4.p                                                   |    0.044s | 20.1MiB| parser | 103 |  |  |
|BOO014-3.p                                                   |    0.044s | 19.644MiB| parser | 103 |  |  |
|BOO006-4.p                                                   |    0.044s | 19.616MiB| parser | 103 |  |  |
|BOO016-2.p                                                   |    0.045s | 19.84MiB| parser | 103 |  |  |
|BOO012-1.p                                                   |    0.045s | 19.648MiB| parser | 103 |  |  |
|BOO009-1.p                                                   |    0.046s | 19.912MiB| parser | 103 |  |  |
|BOO003-2.p                                                   |    0.046s | 19.592MiB| parser | 103 |  |  |
|BOO017-1.p                                                   |    0.046s | 19.932MiB| parser | 103 |  |  |
|BOO018-4.p                                                   |    0.046s | 19.548MiB| parser | 103 |  |  |
|BOO037-1.p                                                   |    0.046s | 19.6MiB| parser | 103 |  |  |
|BOO008-3.p                                                   |    0.047s | 20.4MiB| error | 0 |  |  |
|BOO008-1.p                                                   |    0.047s | 19.9MiB| parser | 103 |  |  |
|BOO010-4.p                                                   |    0.047s | 19.676MiB| parser | 103 |  |  |
|BOO015-1.p                                                   |    0.048s | 19.616MiB| parser | 103 |  |  |
|BOO007-1.p                                                   |    0.048s | 19.604MiB| parser | 103 |  |  |
|BOO012-2.p                                                   |    0.048s | 19.444MiB| parser | 103 |  |  |
|BOO015-2.p                                                   |    0.049s | 19.508MiB| parser | 103 |  |  |
|BOO004-1.p                                                   |    0.049s | 19.876MiB| parser | 103 |  |  |
|BOO034-1.p                                                   |    0.050s | 19.604MiB| parser | 103 |  |  |
|BOO013-2.p                                                   |    0.050s | 19.604MiB| parser | 103 |  |  |
|BOO021-1.p                                                   |    0.051s | 20.512MiB| error | 0 |  |  |
|BOO005-4.p                                                   |    0.052s | 19.66MiB| parser | 103 |  |  |
|BOO001-1.p                                                   |    0.052s | 19.548MiB| parser | 103 |  |  |
|BOO004-2.p                                                   |    0.052s | 19.588MiB| parser | 103 |  |  |
|BOO009-4.p                                                   |    0.052s | 19.636MiB| parser | 103 |  |  |
|BOO010-2.p                                                   |    0.053s | 19.6MiB| parser | 103 |  |  |
|BOO010-1.p                                                   |    0.053s | 19.492MiB| parser | 103 |  |  |
|BOO013-3.p                                                   |    0.054s | 19.856MiB| parser | 103 |  |  |
|BOO005-1.p                                                   |    0.054s | 19.448MiB| parser | 103 |  |  |
|BOO017-2.p                                                   |    0.055s | 20.124MiB| parser | 103 |  |  |
|BOO006-2.p                                                   |    0.055s | 19.888MiB| parser | 103 |  |  |
|BOO037-2.p                                                   |    0.055s | 19.696MiB| parser | 103 |  |  |
|BOO005-2.p                                                   |    0.058s | 19.796MiB| parser | 103 |  |  |
|BOO016-1.p                                                   |    0.058s | 19.596MiB| parser | 103 |  |  |
|BOO013-4.p                                                   |    0.058s | 19.628MiB| parser | 103 |  |  |
|BOO011-4.p                                                   |    0.059s | 19.648MiB| parser | 103 |  |  |
|BOO009-2.p                                                   |    0.063s | 19.428MiB| parser | 103 |  |  |
|BOO004-10.p                                                  |    0.113s | 29.056MiB| error | 0 |  |  |
|BOO022-1.p                                                   |    0.143s | 34.94MiB| error | 0 |  |  |
|BOO029-1.p                                                   |    4.708s | 601.0MiB| error | 0 |  |  |
|BOO086-1.p                                                   |   20.021s | 111.0MiB| timeout | 0 |  |  |
|BOO043-1.p                                                   |   20.023s | 114.0MiB| timeout | 0 |  |  |
|BOO059-1.p                                                   |   20.025s | 115.0MiB| timeout | 0 |  |  |
|BOO103-1.p                                                   |   20.025s | 115.0MiB| timeout | 0 |  |  |
|BOO077-1.p                                                   |   20.026s | 187.0MiB| timeout | 0 |  |  |
|BOO094-1.p                                                   |   20.029s | 110.0MiB| timeout | 0 |  |  |
|BOO058-1.p                                                   |   20.036s | 108.0MiB| timeout | 0 |  |  |
|BOO057-1.p                                                   |   20.040s | 112.0MiB| timeout | 0 |  |  |
|BOO105-1.p                                                   |   20.040s | 81.812MiB| timeout | 0 |  |  |
|BOO060-1.p                                                   |   20.040s | 107.0MiB| timeout | 0 |  |  |
|BOO100-1.p                                                   |   20.041s | 118.0MiB| timeout | 0 |  |  |
|BOO056-1.p                                                   |   20.041s | 114.0MiB| timeout | 0 |  |  |
|BOO073-1.p                                                   |   20.042s | 201.0MiB| timeout | 0 |  |  |
|BOO091-1.p                                                   |   20.043s | 109.0MiB| timeout | 0 |  |  |
|BOO061-1.p                                                   |   20.043s | 107.0MiB| timeout | 0 |  |  |
|BOO068-1.p                                                   |   20.044s | 125.0MiB| timeout | 0 |  |  |
|BOO078-1.p                                                   |   20.045s | 113.0MiB| timeout | 0 |  |  |
|BOO040-1.p                                                   |   20.046s | 118.0MiB| timeout | 0 |  |  |
|BOO014-10.p                                                  |   20.046s | 329.0MiB| timeout | 0 |  |  |
|BOO072-1.p                                                   |   20.047s | 120.0MiB| timeout | 0 |  |  |
|BOO071-1.p                                                   |   20.048s | 119.0MiB| timeout | 0 |  |  |
|BOO079-1.p                                                   |   20.048s | 114.0MiB| timeout | 0 |  |  |
|BOO104-1.p                                                   |   20.049s | 108.0MiB| timeout | 0 |  |  |
|BOO075-1.p                                                   |   20.050s | 116.0MiB| timeout | 0 |  |  |
|BOO055-1.p                                                   |   20.050s | 117.0MiB| timeout | 0 |  |  |
|BOO106-1.p                                                   |   20.050s | 113.0MiB| timeout | 0 |  |  |
|BOO082-1.p                                                   |   20.050s | 119.0MiB| timeout | 0 |  |  |
|BOO099-1.p                                                   |   20.050s | 126.0MiB| timeout | 0 |  |  |
|BOO093-1.p                                                   |   20.051s | 109.0MiB| timeout | 0 |  |  |
|BOO088-1.p                                                   |   20.051s | 237.0MiB| timeout | 0 |  |  |
|BOO092-1.p                                                   |   20.051s | 109.0MiB| timeout | 0 |  |  |
|BOO067-1.p                                                   |   20.051s | 122.0MiB| timeout | 0 |  |  |
|BOO101-1.p                                                   |   20.051s | 123.0MiB| timeout | 0 |  |  |
|BOO042-1.p                                                   |   20.052s | 116.0MiB| timeout | 0 |  |  |
|BOO048-1.p                                                   |   20.052s | 104.0MiB| timeout | 0 |  |  |
|BOO070-1.p                                                   |   20.053s | 127.0MiB| timeout | 0 |  |  |
|BOO035-1.p                                                   |   20.053s | 110.0MiB| timeout | 0 |  |  |
|BOO044-1.p                                                   |   20.053s | 108.0MiB| timeout | 0 |  |  |
|BOO053-1.p                                                   |   20.053s | 115.0MiB| timeout | 0 |  |  |
|BOO050-1.p                                                   |   20.053s | 102.0MiB| timeout | 0 |  |  |
|BOO041-1.p                                                   |   20.054s | 105.0MiB| timeout | 0 |  |  |
|BOO051-10.p                                                  |   20.054s | 119.0MiB| timeout | 0 |  |  |
|BOO083-1.p                                                   |   20.054s | 119.0MiB| timeout | 0 |  |  |
|BOO102-1.p                                                   |   20.054s | 187.0MiB| timeout | 0 |  |  |
|BOO069-1.p                                                   |   20.054s | 143.0MiB| timeout | 0 |  |  |
|BOO046-10.p                                                  |   20.055s | 106.0MiB| timeout | 0 |  |  |
|BOO098-1.p                                                   |   20.055s | 112.0MiB| timeout | 0 |  |  |
|BOO081-1.p                                                   |   20.056s | 115.0MiB| timeout | 0 |  |  |
|BOO038-1.p                                                   |   20.056s | 192.0MiB| timeout | 0 |  |  |
|BOO084-1.p                                                   |   20.056s | 110.0MiB| timeout | 0 |  |  |
|BOO054-1.p                                                   |   20.056s | 121.0MiB| timeout | 0 |  |  |
|BOO090-1.p                                                   |   20.057s | 106.0MiB| timeout | 0 |  |  |
|BOO085-1.p                                                   |   20.057s | 189.0MiB| timeout | 0 |  |  |
|BOO020-1.p                                                   |   20.057s | 125.0MiB| timeout | 0 |  |  |
|BOO087-1.p                                                   |   20.058s | 237.0MiB| timeout | 0 |  |  |
|BOO052-1.p                                                   |   20.059s | 110.0MiB| timeout | 0 |  |  |
|BOO107-1.p                                                   |   20.059s | 110.0MiB| timeout | 0 |  |  |
|BOO039-1.p                                                   |   20.059s | 120.0MiB| timeout | 0 |  |  |
|BOO097-1.p                                                   |   20.059s | 131.0MiB| timeout | 0 |  |  |
|BOO080-1.p                                                   |   20.060s | 114.0MiB| timeout | 0 |  |  |
|BOO051-1.p                                                   |   20.060s | 120.0MiB| timeout | 0 |  |  |
|BOO076-1.p                                                   |   20.061s | 115.0MiB| timeout | 0 |  |  |
|BOO047-10.p                                                  |   20.062s | 107.0MiB| timeout | 0 |  |  |
|BOO015-10.p                                                  |   20.063s | 200.0MiB| timeout | 0 |  |  |
|BOO074-1.p                                                   |   20.065s | 112.0MiB| timeout | 0 |  |  |
|BOO095-1.p                                                   |   20.065s | 119.0MiB| timeout | 0 |  |  |
|BOO057-10.p                                                  |   20.065s | 113.0MiB| timeout | 0 |  |  |
|BOO008-10.p                                                  |   20.065s | 229.0MiB| timeout | 0 |  |  |
|BOO108-1.p                                                   |   20.067s | 112.0MiB| timeout | 0 |  |  |
|BOO046-1.p                                                   |   20.069s | 111.0MiB| timeout | 0 |  |  |
|BOO089-1.p                                                   |   20.069s | 107.0MiB| timeout | 0 |  |  |
|BOO047-1.p                                                   |   20.070s | 122.0MiB| timeout | 0 |  |  |
|BOO056-10.p                                                  |   20.071s | 115.0MiB| timeout | 0 |  |  |
|BOO096-1.p                                                   |   20.072s | 123.0MiB| timeout | 0 |  |  |
|BOO017-10.p                                                  |   20.074s | 355.0MiB| timeout | 0 |  |  |
|BOO045-1.p                                                   |   20.076s | 238.0MiB| timeout | 0 |  |  |
|BOO030-1.p                                                   |   20.090s | 478.0MiB| timeout | 0 |  |  |
|BOO049-1.p                                                   |   20.098s | 651.0MiB| timeout | 0 |  |  |
|BOO033-1.p                                                   |   20.112s | 481.0MiB| timeout | 0 |  |  |
|BOO109+1.p                                                   |   20.140s | 1114.0MiB| timeout | 0 |  |  |
|BOO032-1.p                                                   |   20.145s | 1089.0MiB| timeout | 0 |  |  |
|BOO066-1.p                                                   |   20.173s | 1173.0MiB| timeout | 0 |  |  |
|BOO031-1.p                                                   |   20.179s | 1099.0MiB| timeout | 0 |  |  |
|BOO019-1.p                                                   |   20.303s | 2862.0MiB| timeout | 0 |  |  |
|BOO028-1.p                                                   |   20.310s | 2305.0MiB| timeout | 0 |  |  |
|BOO026-1.p                                                   |   20.372s | 2585.0MiB| timeout | 0 |  |  |
|BOO024-1.p                                                   |   20.385s | 3099.0MiB| timeout | 0 |  |  |
|BOO002-2.p                                                   |   20.442s | 3942.0MiB| timeout | 0 |  |  |
|BOO002-1.p                                                   |   20.491s | 3814.0MiB| timeout | 0 |  |  |
|BOO023-1.p                                                   |   20.498s | 4423.0MiB| timeout | 0 |  |  |
|BOO025-1.p                                                   |   20.501s | 3692.0MiB| timeout | 0 |  |  |
|BOO027-1.p                                                   |   20.530s | 4775.0MiB| timeout | 0 |  |  |
