# .

* SAT 5
* UNSAT 12
* TIMEOUT 0
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-09-06 23:25:26 UTC
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_AUFNIA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-QF_AUFNIA
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 53621bed781b1f80d49d57b65e13b6ef814e563a
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_AUFNIA.tar.zst?download=1
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
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_clnt.blast.03_false-unreach-call.i.cil.c_71.smt2 |    0.046s | 23.484MiB| unsat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.07_false-unreach-call.i.cil.c_0.smt2 |    0.048s | 22.912MiB| unsat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_clnt.blast.01_false-unreach-call.i.cil.c_57.smt2 |    0.049s | 22.932MiB| unsat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.10_true-unreach-call.i.cil.c_0.smt2 |    0.062s | 24.444MiB| unsat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.08_true-unreach-call.i.cil.c_1140.smt2 |    0.065s | 25.424MiB| unsat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_clnt.blast.02_false-unreach-call.i.cil.c_71.smt2 |    0.067s | 22.676MiB| unsat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.12_true-unreach-call.i.cil.c_0.smt2 |    0.077s | 25.644MiB| unsat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.15_true-unreach-call.i.cil.c_1173.smt2 |    0.083s | 27.052MiB| unsat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.14_true-unreach-call.i.cil.c_959.smt2 |    0.087s | 26.9MiB| unsat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.06_true-unreach-call.i.cil.c_2803.smt2 |    0.105s | 28.992MiB| unsat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.16_true-unreach-call.i.cil.c_2036.smt2 |    0.113s | 28.112MiB| unsat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.06_true-unreach-call.i.cil.c.smt2 |    0.115s | 30.432MiB| unsat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.02_false-unreach-call.i.cil.c_0.smt2 |    0.471s | 29.056MiB| sat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.04_false-unreach-call.i.cil.c_0.smt2 |    0.494s | 29.028MiB| sat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.01_false-unreach-call.i.cil.c_0.smt2 |    0.677s | 29.284MiB| sat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_srvr.blast.11_false-unreach-call.i.cil.c_0.smt2 |    1.785s | 35.092MiB| sat | 0 |  |
|non-incremental/QF_AUFNIA/UltimateAutomizer/s3_clnt.blast.01_false-unreach-call.i.cil.c_0.smt2 |   17.938s | 62.612MiB| sat | 0 |  |
