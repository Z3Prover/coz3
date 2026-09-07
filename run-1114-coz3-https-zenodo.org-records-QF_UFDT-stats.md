# .

* SAT 3
* UNSAT 1
* TIMEOUT 199
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-09-07 02:07:23 UTC
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_UFDT.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-QF_UFDT
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 53621bed781b1f80d49d57b65e13b6ef814e563a
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_UFDT.tar.zst?download=1
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
|non-incremental/QF_UFDT/20170428-Barrett/cdt-cade2015/data/afp/coinductive_list/x2015_09_10_17_01_00_473_1716450.smt_in.smt2 |    0.050s | 19.54MiB| sat | 0 |  |
|non-incremental/QF_UFDT/20170428-Barrett/cdt-cade2015/data/distro/stream_processor/x2015_09_10_16_45_54_875_1000989.smt_in.smt2 |    0.050s | 20.06MiB| sat | 0 |  |
|non-incremental/QF_UFDT/20170428-Barrett/cdt-cade2015/data/distro/stream_processor/x2015_09_10_16_45_47_401_987519.smt_in.smt2 |    0.051s | 19.888MiB| sat | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e10.smt2     |   17.574s | 34.212MiB| unsat | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k02.smt2     |   20.015s | 35.332MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e28.smt2     |   20.016s | 34.108MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k09.smt2     |   20.019s | 38.344MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k79.smt2     |   20.019s | 33.696MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e95.smt2     |   20.019s | 40.252MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e80.smt2     |   20.020s | 37.328MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e69.smt2     |   20.021s | 40.556MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e74.smt2     |   20.021s | 38.416MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k20.smt2     |   20.022s | 32.24MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e16.smt2     |   20.022s | 35.512MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k50.smt2     |   20.022s | 39.276MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e35.smt2     |   20.023s | 38.332MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e22.smt2     |   20.023s | 32.636MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k51.smt2     |   20.024s | 35.452MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e30.smt2     |   20.024s | 29.476MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k01.smt2     |   20.025s | 35.072MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k24.smt2     |   20.026s | 31.664MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e20.smt2     |   20.027s | 33.876MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k06.smt2     |   20.027s | 46.292MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k77.smt2     |   20.028s | 38.332MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k95.smt2     |   20.033s | 101.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k54.smt2     |   20.033s | 189.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k00.smt2     |   20.033s | 37.772MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k32.smt2     |   20.033s | 32.26MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e53.smt2     |   20.034s | 40.692MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e06.smt2     |   20.035s | 31.928MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k42.smt2     |   20.035s | 40.576MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k93.smt2     |   20.037s | 181.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e03.smt2     |   20.038s | 32.892MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k34.smt2     |   20.038s | 35.392MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e54.smt2     |   20.040s | 32.828MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k76.smt2     |   20.041s | 38.144MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e94.smt2     |   20.041s | 33.284MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k69.smt2     |   20.041s | 54.28MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e63.smt2     |   20.042s | 37.9MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k55.smt2     |   20.042s | 195.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e23.smt2     |   20.043s | 33.012MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e88.smt2     |   20.043s | 31.148MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e41.smt2     |   20.044s | 32.86MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e98.smt2     |   20.044s | 33.948MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k25.smt2     |   20.044s | 34.584MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e81.smt2     |   20.044s | 30.144MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e37.smt2     |   20.045s | 38.796MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e73.smt2     |   20.046s | 38.824MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e51.smt2     |   20.046s | 34.164MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k46.smt2     |   20.047s | 79.18MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e55.smt2     |   20.047s | 31.528MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e65.smt2     |   20.047s | 37.408MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k11.smt2     |   20.047s | 40.82MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k27.smt2     |   20.048s | 39.224MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k66.smt2     |   20.048s | 30.072MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k86.smt2     |   20.049s | 178.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e44.smt2     |   20.049s | 29.576MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e27.smt2     |   20.049s | 37.068MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e58.smt2     |   20.050s | 34.1MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e33.smt2     |   20.050s | 35.548MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e43.smt2     |   20.050s | 27.864MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k68.smt2     |   20.050s | 37.744MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e70.smt2     |   20.051s | 34.932MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e24.smt2     |   20.052s | 34.104MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e92.smt2     |   20.053s | 34.52MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k74.smt2     |   20.053s | 35.916MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k75.smt2     |   20.053s | 37.976MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k40.smt2     |   20.053s | 35.628MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e48.smt2     |   20.053s | 31.764MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k05.smt2     |   20.053s | 44.776MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e11.smt2     |   20.055s | 35.18MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k49.smt2     |   20.055s | 33.7MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e67.smt2     |   20.055s | 28.676MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k47.smt2     |   20.056s | 79.72MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e38.smt2     |   20.056s | 32.476MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e72.smt2     |   20.056s | 39.14MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e36.smt2     |   20.056s | 31.196MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k72.smt2     |   20.057s | 98.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e09.smt2     |   20.057s | 36.704MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e56.smt2     |   20.058s | 35.244MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k16.smt2     |   20.058s | 33.24MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k31.smt2     |   20.059s | 30.832MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e14.smt2     |   20.059s | 32.6MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k67.smt2     |   20.059s | 32.804MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k39.smt2     |   20.059s | 74.188MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e46.smt2     |   20.059s | 35.22MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e18.smt2     |   20.059s | 31.956MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k56.smt2     |   20.060s | 34.932MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e49.smt2     |   20.060s | 33.612MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k89.smt2     |   20.061s | 102.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k14.smt2     |   20.061s | 30.408MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k60.smt2     |   20.061s | 35.452MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k38.smt2     |   20.062s | 36.1MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e89.smt2     |   20.062s | 37.9MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k65.smt2     |   20.062s | 38.476MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e13.smt2     |   20.062s | 30.752MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k92.smt2     |   20.062s | 75.028MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e82.smt2     |   20.063s | 37.44MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k48.smt2     |   20.063s | 79.152MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e39.smt2     |   20.063s | 34.328MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e64.smt2     |   20.063s | 37.272MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e15.smt2     |   20.063s | 31.416MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k59.smt2     |   20.063s | 32.248MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k21.smt2     |   20.063s | 38.2MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e75.smt2     |   20.064s | 29.968MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e57.smt2     |   20.064s | 34.316MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e02.smt2     |   20.064s | 30.92MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k30.smt2     |   20.065s | 31.088MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e86.smt2     |   20.065s | 28.812MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k12.smt2     |   20.065s | 38.232MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e32.smt2     |   20.065s | 33.924MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e78.smt2     |   20.065s | 33.684MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e87.smt2     |   20.065s | 36.7MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e99.smt2     |   20.065s | 38.488MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e31.smt2     |   20.066s | 32.908MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e17.smt2     |   20.066s | 38.88MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k61.smt2     |   20.067s | 38.164MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k15.smt2     |   20.067s | 34.952MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e76.smt2     |   20.068s | 35.732MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k19.smt2     |   20.068s | 31.9MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k84.smt2     |   20.068s | 179.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e21.smt2     |   20.068s | 32.184MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k73.smt2     |   20.069s | 31.828MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e29.smt2     |   20.069s | 34.064MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k13.smt2     |   20.069s | 30.344MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k08.smt2     |   20.069s | 46.576MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k83.smt2     |   20.070s | 179.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k64.smt2     |   20.071s | 33.844MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e85.smt2     |   20.071s | 37.768MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k90.smt2     |   20.071s | 34.616MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k22.smt2     |   20.071s | 30.156MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k63.smt2     |   20.071s | 89.632MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e71.smt2     |   20.072s | 32.504MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e68.smt2     |   20.072s | 29.032MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k44.smt2     |   20.072s | 184.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e47.smt2     |   20.074s | 32.564MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k17.smt2     |   20.075s | 45.544MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k70.smt2     |   20.075s | 37.948MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k53.smt2     |   20.075s | 176.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k43.smt2     |   20.075s | 74.248MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k23.smt2     |   20.077s | 33.672MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k07.smt2     |   20.077s | 46.628MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e01.smt2     |   20.078s | 31.996MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k57.smt2     |   20.079s | 88.468MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k98.smt2     |   20.079s | 180.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k26.smt2     |   20.080s | 35.392MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e77.smt2     |   20.080s | 34.6MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k10.smt2     |   20.081s | 50.472MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e91.smt2     |   20.081s | 35.632MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k82.smt2     |   20.082s | 179.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e66.smt2     |   20.082s | 38.336MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e62.smt2     |   20.084s | 34.232MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e52.smt2     |   20.084s | 37.176MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e07.smt2     |   20.085s | 35.12MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k97.smt2     |   20.086s | 183.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e93.smt2     |   20.086s | 31.08MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e83.smt2     |   20.089s | 30.384MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e40.smt2     |   20.090s | 34.616MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k03.smt2     |   20.091s | 40.08MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e00.smt2     |   20.092s | 31.664MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k85.smt2     |   20.093s | 179.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k99.smt2     |   20.093s | 182.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k18.smt2     |   20.093s | 30.664MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k29.smt2     |   20.093s | 39.296MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k96.smt2     |   20.094s | 182.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k91.smt2     |   20.095s | 179.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e08.smt2     |   20.095s | 30.576MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e61.smt2     |   20.096s | 34.84MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e34.smt2     |   20.098s | 32.676MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e12.smt2     |   20.098s | 32.152MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e59.smt2     |   20.099s | 34.656MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e84.smt2     |   20.100s | 32.98MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e04.smt2     |   20.100s | 37.508MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k04.smt2     |   20.102s | 44.48MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k41.smt2     |   20.103s | 35.328MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e50.smt2     |   20.103s | 32.66MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e19.smt2     |   20.104s | 31.764MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e45.smt2     |   20.104s | 33.796MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e42.smt2     |   20.105s | 31.388MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e97.smt2     |   20.105s | 183.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k28.smt2     |   20.106s | 33.164MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k52.smt2     |   20.107s | 38.176MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e26.smt2     |   20.107s | 32.704MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k94.smt2     |   20.109s | 35.888MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k58.smt2     |   20.109s | 33.104MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e79.smt2     |   20.109s | 39.916MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e60.smt2     |   20.110s | 38.476MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k71.smt2     |   20.110s | 88.74MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k36.smt2     |   20.110s | 32.424MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e05.smt2     |   20.111s | 35.628MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e90.smt2     |   20.112s | 38.444MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k35.smt2     |   20.113s | 42.288MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e25.smt2     |   20.114s | 34.184MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k80.smt2     |   20.114s | 65.012MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k37.smt2     |   20.115s | 72.024MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k33.smt2     |   20.115s | 39.132MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k62.smt2     |   20.115s | 39.064MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k88.smt2     |   20.124s | 122.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e96.smt2     |   20.125s | 177.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k81.smt2     |   20.130s | 178.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k78.smt2     |   20.130s | 146.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k45.smt2     |   20.134s | 189.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k87.smt2     |   20.138s | 178.0MiB| timeout | 0 |  |
