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
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_SNIA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-16740866-files-QF_SNIA.tar.zst-do
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 6a62a531819d3c2aa21eaaffa8411aea4d9a2f40
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_SNIA.tar.zst?download=1
Z3 commit message: qsat: decide quantifier-free goals so qe2 returns sat instead of unknown (fixes iss-7027/small-30) (#9970)

## Summary

Fixes the `iss-7027/small-30` snapshot regression (`Z3Prover/bench`
discussion #2705) **at its root**, instead of working around it by
retuning the LP heuristics.

- **Benchmark:** `inputs/issues/iss-7027/small-30.smt2` —
`(check-sat-using qe2)` over a single `(distinct ...)` of 33 mixed
Int/Real terms.
- The recorded oracle was `unknown`; current `master` produces
`timeout`.

## Root cause

`unknown`/`timeout` are both wrong here: the formula is a `distinct`
over 33 terms (free Int/Real constants plus the literals `0`/`1`), which
is **trivially `sat`** — there are infinitely many distinct reals.

The real bug is in the `qsat` tactic that backs `qe2`. Running
quantifier elimination on a **quantifier-free** formula has nothing to
eliminate, so `qsat` left an undecided residual goal and
`check-sat-using` reported `unknown`. This reproduces on any ground
formula with free variables, e.g.:

```
(declare-fun a () Int)(assert (> a 0))(check-sat-using qe2)   ; -> unknown (should be sat)
```

For `small-30` the QE alternation additionally drove `theory_lra`
integer branch-and-bound down a non-terminating path, surfacing as a
`timeout` under the capture budget (the symptom the `random_hammers`
schedule change happened to expose).

## Fix

Under `check-sat` semantics, top-level free variables are implicitly
existentially quantified. So when the `qsat` input has no quantifiers,
decide satisfiability directly (route through the existing `qsat_sat`
path) instead of producing a residual goal. `qe2`/`qe` now return
`sat`/`unsat` for ground formulas.

QE of genuinely-quantified formulas is unchanged: `apply qe2` on a
quantified goal produces the same projected formula as before (verified
identical to `master`). Only the degenerate quantifier-free case is
affected.

This supersedes the previous approach in this PR (reverting the
`lp.random_hammers` default). That default is left **unchanged**
(`true`), preserving #9958's aggregate QF_LIA benefit. `small-30` now
returns `sat` in ~0.01s regardless of the heuristic schedule, because
the QE machinery no longer runs on this ground instance.

Two changes:
- `src/qe/qsat.cpp`: short-circuit quantifier-free input to the
satisfiability decision path.
- `Z3Prover/bench` `inputs/issues/iss-7027/small-30.expected.out`:
oracle updated `unknown` -> `sat` (to be committed alongside this fix).

## Validation

```
$ z3 small-30.smt2
sat            # ~0.01s

$ echo '(declare-fun a () Int)(assert (> a 0))(check-sat-using qe2)' | z3 -in
sat
$ echo '(declare-fun a () Int)(assert (and (> a 0)(< a 0)))(check-sat-using qe2)' | z3 -in
unsat
```

Full unit-test suite (`test-z3 /a`) passes (92/92). Quantified `qe2`
round-trips (`apply qe2`) match `master` byte-for-byte.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1553.corecstrs.readable.smt2 |    0.023s | 20.472MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/2009.corecstrs.readable.smt2 |    0.024s | 20.38MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1597.corecstrs.readable.smt2 |    0.025s | 20.38MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1635.corecstrs.readable.smt2 |    0.026s | 20.368MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/2007.corecstrs.readable.smt2 |    0.026s | 20.12MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1555.corecstrs.readable.smt2 |    0.026s | 20.476MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1590.corecstrs.readable.smt2 |    0.026s | 20.648MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1910.corecstrs.readable.smt2 |    0.026s | 20.664MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1633.corecstrs.readable.smt2 |    0.026s | 20.364MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1885.corecstrs.readable.smt2 |    0.027s | 20.608MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1552.corecstrs.readable.smt2 |    0.027s | 20.368MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1554.corecstrs.readable.smt2 |    0.027s | 20.412MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1572.corecstrs.readable.smt2 |    0.027s | 20.364MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1626.corecstrs.readable.smt2 |    0.028s | 20.364MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1997.corecstrs.readable.smt2 |    0.029s | 20.328MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1550.corecstrs.readable.smt2 |    0.029s | 20.372MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1627.corecstrs.readable.smt2 |    0.030s | 20.896MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1551.corecstrs.readable.smt2 |    0.030s | 20.524MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1567.corecstrs.readable.smt2 |    0.030s | 20.66MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/845.corecstrs.readable.smt2 |    0.031s | 20.396MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/851.corecstrs.readable.smt2 |    0.032s | 20.776MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20200224-Wu-PyExZ3/f72b09bc9444f702ad7cdf3a9075754e71d673f3d134d9f485f6608a.smt2 |    0.033s | 21.636MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20200224-Wu-PyExZ3/dddb9cc1f86d5738fd85ffa1b430c4e9df9771c0fffa945b9b414772.smt2 |    0.033s | 21.136MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/849.corecstrs.readable.smt2 |    0.034s | 20.512MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1594.corecstrs.readable.smt2 |    0.034s | 20.364MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1598.corecstrs.readable.smt2 |    0.034s | 20.296MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/823.corecstrs.readable.smt2 |    0.035s | 20.588MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1573.corecstrs.readable.smt2 |    0.036s | 20.836MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/2008.corecstrs.readable.smt2 |    0.039s | 20.388MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/824.corecstrs.readable.smt2 |    0.040s | 20.628MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1907.corecstrs.readable.smt2 |    0.041s | 20.372MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/808.corecstrs.readable.smt2 |    0.042s | 20.376MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/838.corecstrs.readable.smt2 |    0.042s | 20.588MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/811.corecstrs.readable.smt2 |    0.042s | 20.372MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/825.corecstrs.readable.smt2 |    0.043s | 20.616MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/843.corecstrs.readable.smt2 |    0.045s | 20.412MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/827.corecstrs.readable.smt2 |    0.045s | 20.536MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1911.corecstrs.readable.smt2 |    0.045s | 20.476MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/2010.corecstrs.readable.smt2 |    0.045s | 20.64MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/822.corecstrs.readable.smt2 |    0.046s | 20.548MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1909.corecstrs.readable.smt2 |    0.046s | 20.356MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1596.corecstrs.readable.smt2 |    0.046s | 20.4MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/820.corecstrs.readable.smt2 |    0.047s | 20.62MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/840.corecstrs.readable.smt2 |    0.047s | 20.408MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1595.corecstrs.readable.smt2 |    0.047s | 20.556MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/821.corecstrs.readable.smt2 |    0.048s | 20.648MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1575.corecstrs.readable.smt2 |    0.048s | 20.264MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20200224-Wu-PyExZ3/bdcb3877984a55b540face066045c4642cba1bc553af78576a41a76e.smt2 |    0.049s | 21.168MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1637.corecstrs.readable.smt2 |    0.049s | 20.4MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1557.corecstrs.readable.smt2 |    0.049s | 20.444MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20200224-Wu-PyExZ3/e007cfdcf4dd61007107222cbedbb46ad161a945ab5ee0a68d3f585a.smt2 |    0.050s | 21.14MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1576.corecstrs.readable.smt2 |    0.050s | 20.62MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1628.corecstrs.readable.smt2 |    0.050s | 20.36MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1886.corecstrs.readable.smt2 |    0.050s | 20.604MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/812.corecstrs.readable.smt2 |    0.051s | 20.448MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/809.corecstrs.readable.smt2 |    0.051s | 20.416MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1556.corecstrs.readable.smt2 |    0.051s | 20.64MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1566.corecstrs.readable.smt2 |    0.051s | 20.456MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1634.corecstrs.readable.smt2 |    0.051s | 20.376MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1884.corecstrs.readable.smt2 |    0.052s | 20.504MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/846.corecstrs.readable.smt2 |    0.053s | 20.364MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1589.corecstrs.readable.smt2 |    0.053s | 20.36MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1568.corecstrs.readable.smt2 |    0.053s | 20.264MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1636.corecstrs.readable.smt2 |    0.053s | 20.348MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1908.corecstrs.readable.smt2 |    0.053s | 20.388MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1588.corecstrs.readable.smt2 |    0.053s | 20.404MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20200224-Wu-PyExZ3/5735c9082c3f9cd487c6376032029bb499ba1f87113dc9ca03adc6bc.smt2 |    0.054s | 21.116MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/826.corecstrs.readable.smt2 |    0.054s | 20.58MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/839.corecstrs.readable.smt2 |    0.054s | 20.592MiB| sat | 0 |  |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1574.corecstrs.readable.smt2 |    0.055s | 20.74MiB| sat | 0 |  |  |
