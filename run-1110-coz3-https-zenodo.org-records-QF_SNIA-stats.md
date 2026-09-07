# .

* SAT 70
* UNSAT 0
* TIMEOUT 0
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-09-07 01:54:55 UTC
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_SNIA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-QF_SNIA
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 53621bed781b1f80d49d57b65e13b6ef814e563a
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_SNIA.tar.zst?download=1
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
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1909.corecstrs.readable.smt2 |    0.024s | 20.34MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1589.corecstrs.readable.smt2 |    0.026s | 20.372MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1907.corecstrs.readable.smt2 |    0.026s | 20.308MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1884.corecstrs.readable.smt2 |    0.026s | 20.4MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1555.corecstrs.readable.smt2 |    0.029s | 20.404MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1556.corecstrs.readable.smt2 |    0.029s | 20.66MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1594.corecstrs.readable.smt2 |    0.029s | 20.612MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20200224-Wu-PyExZ3/5735c9082c3f9cd487c6376032029bb499ba1f87113dc9ca03adc6bc.smt2 |    0.030s | 20.892MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20200224-Wu-PyExZ3/f72b09bc9444f702ad7cdf3a9075754e71d673f3d134d9f485f6608a.smt2 |    0.030s | 21.28MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1566.corecstrs.readable.smt2 |    0.030s | 20.384MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1554.corecstrs.readable.smt2 |    0.031s | 20.408MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/2007.corecstrs.readable.smt2 |    0.031s | 20.404MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20200224-Wu-PyExZ3/bdcb3877984a55b540face066045c4642cba1bc553af78576a41a76e.smt2 |    0.032s | 20.912MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1596.corecstrs.readable.smt2 |    0.032s | 20.4MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1590.corecstrs.readable.smt2 |    0.032s | 20.412MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1997.corecstrs.readable.smt2 |    0.033s | 20.404MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1574.corecstrs.readable.smt2 |    0.034s | 20.412MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1572.corecstrs.readable.smt2 |    0.034s | 20.356MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/826.corecstrs.readable.smt2 |    0.035s | 20.472MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/812.corecstrs.readable.smt2 |    0.035s | 20.588MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/843.corecstrs.readable.smt2 |    0.035s | 20.4MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/821.corecstrs.readable.smt2 |    0.035s | 20.868MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1908.corecstrs.readable.smt2 |    0.035s | 20.448MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1550.corecstrs.readable.smt2 |    0.035s | 20.62MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1626.corecstrs.readable.smt2 |    0.035s | 20.368MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1886.corecstrs.readable.smt2 |    0.035s | 20.364MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1573.corecstrs.readable.smt2 |    0.036s | 20.616MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1588.corecstrs.readable.smt2 |    0.036s | 20.42MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20200224-Wu-PyExZ3/e007cfdcf4dd61007107222cbedbb46ad161a945ab5ee0a68d3f585a.smt2 |    0.038s | 20.92MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1627.corecstrs.readable.smt2 |    0.038s | 20.38MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1575.corecstrs.readable.smt2 |    0.039s | 20.408MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/825.corecstrs.readable.smt2 |    0.040s | 20.612MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1633.corecstrs.readable.smt2 |    0.040s | 20.364MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1598.corecstrs.readable.smt2 |    0.041s | 20.372MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/809.corecstrs.readable.smt2 |    0.042s | 20.604MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/839.corecstrs.readable.smt2 |    0.042s | 20.368MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/838.corecstrs.readable.smt2 |    0.044s | 20.572MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/820.corecstrs.readable.smt2 |    0.044s | 20.62MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/2010.corecstrs.readable.smt2 |    0.045s | 20.364MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1551.corecstrs.readable.smt2 |    0.045s | 20.328MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/822.corecstrs.readable.smt2 |    0.046s | 20.896MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1634.corecstrs.readable.smt2 |    0.046s | 20.368MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/2008.corecstrs.readable.smt2 |    0.046s | 20.372MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1557.corecstrs.readable.smt2 |    0.046s | 20.868MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1597.corecstrs.readable.smt2 |    0.046s | 20.4MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1628.corecstrs.readable.smt2 |    0.046s | 20.408MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1576.corecstrs.readable.smt2 |    0.046s | 21.064MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/824.corecstrs.readable.smt2 |    0.047s | 21.008MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/808.corecstrs.readable.smt2 |    0.047s | 20.42MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1568.corecstrs.readable.smt2 |    0.047s | 20.692MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/2009.corecstrs.readable.smt2 |    0.047s | 20.384MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/827.corecstrs.readable.smt2 |    0.048s | 20.872MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1636.corecstrs.readable.smt2 |    0.048s | 20.408MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1635.corecstrs.readable.smt2 |    0.048s | 20.716MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1911.corecstrs.readable.smt2 |    0.048s | 21.028MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1553.corecstrs.readable.smt2 |    0.048s | 20.62MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20200224-Wu-PyExZ3/dddb9cc1f86d5738fd85ffa1b430c4e9df9771c0fffa945b9b414772.smt2 |    0.049s | 21.216MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/846.corecstrs.readable.smt2 |    0.049s | 20.396MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1637.corecstrs.readable.smt2 |    0.049s | 20.66MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/851.corecstrs.readable.smt2 |    0.050s | 20.42MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/849.corecstrs.readable.smt2 |    0.050s | 20.364MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1910.corecstrs.readable.smt2 |    0.050s | 20.452MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1885.corecstrs.readable.smt2 |    0.050s | 20.372MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/845.corecstrs.readable.smt2 |    0.051s | 20.628MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/811.corecstrs.readable.smt2 |    0.052s | 20.808MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1567.corecstrs.readable.smt2 |    0.052s | 20.492MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1552.corecstrs.readable.smt2 |    0.052s | 20.42MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1595.corecstrs.readable.smt2 |    0.052s | 20.368MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/840.corecstrs.readable.smt2 |    0.053s | 20.724MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/823.corecstrs.readable.smt2 |    0.054s | 20.412MiB| sat | 0 |  |
