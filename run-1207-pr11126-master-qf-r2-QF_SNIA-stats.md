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
Experiment date and time: 2026-10-15 15:36:17 UTC
Job description: PR11126 baseline QF_SNIA: retry after runner queue expiry; one suite at a time; pinned master f05a5c651802
Job tag: pr11126-master-qf-r2-QF_SNIA
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: f05a5c6518028169dee82d8bb3108e71a80f3b5a
Z3 branch: f05a5c6518028169dee82d8bb3108e71a80f3b5a
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_SNIA.tar.zst?download=1
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
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/840.corecstrs.readable.smt2 |    0.023s | 20.436MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/2010.corecstrs.readable.smt2 |    0.024s | 20.388MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/2008.corecstrs.readable.smt2 |    0.024s | 20.468MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1636.corecstrs.readable.smt2 |    0.024s | 20.356MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1551.corecstrs.readable.smt2 |    0.024s | 20.512MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20200224-Wu-PyExZ3/5735c9082c3f9cd487c6376032029bb499ba1f87113dc9ca03adc6bc.smt2 |    0.025s | 21.648MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1886.corecstrs.readable.smt2 |    0.025s | 20.56MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1573.corecstrs.readable.smt2 |    0.025s | 20.612MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1626.corecstrs.readable.smt2 |    0.025s | 20.612MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/839.corecstrs.readable.smt2 |    0.025s | 20.872MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/845.corecstrs.readable.smt2 |    0.025s | 21.128MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20200224-Wu-PyExZ3/bdcb3877984a55b540face066045c4642cba1bc553af78576a41a76e.smt2 |    0.026s | 21.692MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20200224-Wu-PyExZ3/dddb9cc1f86d5738fd85ffa1b430c4e9df9771c0fffa945b9b414772.smt2 |    0.026s | 21.372MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1635.corecstrs.readable.smt2 |    0.026s | 20.66MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/2009.corecstrs.readable.smt2 |    0.026s | 20.244MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1557.corecstrs.readable.smt2 |    0.026s | 20.608MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1588.corecstrs.readable.smt2 |    0.026s | 20.608MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/851.corecstrs.readable.smt2 |    0.026s | 20.62MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/809.corecstrs.readable.smt2 |    0.026s | 20.884MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1633.corecstrs.readable.smt2 |    0.027s | 20.592MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1997.corecstrs.readable.smt2 |    0.027s | 20.604MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1553.corecstrs.readable.smt2 |    0.027s | 20.392MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1597.corecstrs.readable.smt2 |    0.027s | 20.624MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1627.corecstrs.readable.smt2 |    0.027s | 20.612MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1598.corecstrs.readable.smt2 |    0.027s | 20.332MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/849.corecstrs.readable.smt2 |    0.027s | 20.36MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/843.corecstrs.readable.smt2 |    0.027s | 20.656MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20200224-Wu-PyExZ3/f72b09bc9444f702ad7cdf3a9075754e71d673f3d134d9f485f6608a.smt2 |    0.028s | 21.632MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1884.corecstrs.readable.smt2 |    0.028s | 20.64MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1572.corecstrs.readable.smt2 |    0.028s | 20.604MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1885.corecstrs.readable.smt2 |    0.028s | 20.632MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1910.corecstrs.readable.smt2 |    0.028s | 20.648MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1637.corecstrs.readable.smt2 |    0.028s | 20.616MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1575.corecstrs.readable.smt2 |    0.028s | 20.608MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1574.corecstrs.readable.smt2 |    0.028s | 20.732MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1550.corecstrs.readable.smt2 |    0.028s | 20.936MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1907.corecstrs.readable.smt2 |    0.028s | 20.876MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1628.corecstrs.readable.smt2 |    0.028s | 20.596MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/808.corecstrs.readable.smt2 |    0.028s | 20.868MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/827.corecstrs.readable.smt2 |    0.028s | 20.964MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/2007.corecstrs.readable.smt2 |    0.029s | 20.876MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1567.corecstrs.readable.smt2 |    0.029s | 20.356MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1555.corecstrs.readable.smt2 |    0.029s | 20.876MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/838.corecstrs.readable.smt2 |    0.029s | 20.604MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1911.corecstrs.readable.smt2 |    0.030s | 20.824MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1909.corecstrs.readable.smt2 |    0.030s | 20.936MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1590.corecstrs.readable.smt2 |    0.031s | 20.6MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1634.corecstrs.readable.smt2 |    0.041s | 20.468MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1556.corecstrs.readable.smt2 |    0.043s | 20.72MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1594.corecstrs.readable.smt2 |    0.043s | 20.88MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1589.corecstrs.readable.smt2 |    0.043s | 20.556MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1908.corecstrs.readable.smt2 |    0.043s | 20.724MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1554.corecstrs.readable.smt2 |    0.043s | 20.604MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1595.corecstrs.readable.smt2 |    0.044s | 20.628MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1576.corecstrs.readable.smt2 |    0.044s | 20.416MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/820.corecstrs.readable.smt2 |    0.044s | 20.624MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1552.corecstrs.readable.smt2 |    0.045s | 20.608MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1566.corecstrs.readable.smt2 |    0.045s | 21.076MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1596.corecstrs.readable.smt2 |    0.045s | 20.588MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/846.corecstrs.readable.smt2 |    0.045s | 20.876MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/small/1568.corecstrs.readable.smt2 |    0.046s | 20.86MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20200224-Wu-PyExZ3/e007cfdcf4dd61007107222cbedbb46ad161a945ab5ee0a68d3f585a.smt2 |    0.049s | 20.976MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/823.corecstrs.readable.smt2 |    0.052s | 20.512MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/824.corecstrs.readable.smt2 |    0.060s | 21.12MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/826.corecstrs.readable.smt2 |    0.083s | 20.62MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/821.corecstrs.readable.smt2 |    0.084s | 21.092MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/822.corecstrs.readable.smt2 |    0.085s | 20.576MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/811.corecstrs.readable.smt2 |    0.085s | 20.856MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/812.corecstrs.readable.smt2 |    0.090s | 20.872MiB| sat | 0 |  |
|non-incremental/QF_SNIA/20180523-Reynolds/kaluza/sat/big/825.corecstrs.readable.smt2 |    0.092s | 20.904MiB| sat | 0 |  |
