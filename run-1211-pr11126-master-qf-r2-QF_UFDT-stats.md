# .

* SAT 4
* UNSAT 6
* TIMEOUT 193
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-10-15 17:33:20 UTC
Job description: PR11126 baseline QF_UFDT: runner-queue recovery; one suite at a time; pinned master f05a5c651802
Job tag: pr11126-master-qf-r2-QF_UFDT
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: f05a5c6518028169dee82d8bb3108e71a80f3b5a
Z3 branch: f05a5c6518028169dee82d8bb3108e71a80f3b5a
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_UFDT.tar.zst?download=1
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
|non-incremental/QF_UFDT/20170428-Barrett/cdt-cade2015/data/afp/coinductive_list/x2015_09_10_17_01_00_473_1716450.smt_in.smt2 |    0.034s | 19.588MiB| sat | 0 |  |
|non-incremental/QF_UFDT/20170428-Barrett/cdt-cade2015/data/distro/stream_processor/x2015_09_10_16_45_47_401_987519.smt_in.smt2 |    0.034s | 19.848MiB| sat | 0 |  |
|non-incremental/QF_UFDT/20170428-Barrett/cdt-cade2015/data/distro/stream_processor/x2015_09_10_16_45_54_875_1000989.smt_in.smt2 |    0.034s | 20.356MiB| sat | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e10.smt2     |    3.189s | 29.508MiB| unsat | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e01.smt2     |    8.925s | 32.204MiB| unsat | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k00.smt2     |   10.640s | 39.844MiB| sat | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e04.smt2     |   12.331s | 37.864MiB| unsat | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e44.smt2     |   14.082s | 38.524MiB| unsat | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e08.smt2     |   14.742s | 38.216MiB| unsat | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e00.smt2     |   15.835s | 32.472MiB| unsat | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e87.smt2     |   20.012s | 42.304MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k41.smt2     |   20.013s | 36.636MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k79.smt2     |   20.013s | 35.008MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e94.smt2     |   20.013s | 33.744MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e61.smt2     |   20.013s | 38.164MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e23.smt2     |   20.013s | 36.092MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k77.smt2     |   20.013s | 39.244MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k15.smt2     |   20.013s | 35.964MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e17.smt2     |   20.014s | 39.924MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e49.smt2     |   20.014s | 42.652MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e59.smt2     |   20.014s | 36.744MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e22.smt2     |   20.014s | 33.784MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k27.smt2     |   20.014s | 40.476MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e58.smt2     |   20.014s | 34.896MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e13.smt2     |   20.014s | 36.596MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k23.smt2     |   20.014s | 34.576MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e16.smt2     |   20.014s | 36.176MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e56.smt2     |   20.014s | 36.776MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k10.smt2     |   20.014s | 51.128MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k73.smt2     |   20.014s | 32.448MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e80.smt2     |   20.014s | 41.496MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k08.smt2     |   20.015s | 46.96MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e47.smt2     |   20.015s | 35.436MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k62.smt2     |   20.015s | 40.132MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e72.smt2     |   20.015s | 40.868MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e88.smt2     |   20.015s | 46.704MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e37.smt2     |   20.015s | 39.82MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e33.smt2     |   20.015s | 36.776MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k33.smt2     |   20.015s | 40.4MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k74.smt2     |   20.015s | 37.068MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e99.smt2     |   20.015s | 43.188MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k02.smt2     |   20.015s | 35.628MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e93.smt2     |   20.015s | 42.056MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k60.smt2     |   20.015s | 36.184MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e98.smt2     |   20.015s | 35.064MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k09.smt2     |   20.015s | 39.16MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e71.smt2     |   20.015s | 39.992MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k18.smt2     |   20.016s | 32.104MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e09.smt2     |   20.016s | 36.488MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k05.smt2     |   20.016s | 46.02MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k70.smt2     |   20.016s | 38.764MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e85.smt2     |   20.016s | 38.012MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e19.smt2     |   20.016s | 32.42MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k69.smt2     |   20.016s | 55.212MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e70.smt2     |   20.016s | 43.632MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e42.smt2     |   20.016s | 34.448MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k80.smt2     |   20.017s | 64.492MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k29.smt2     |   20.017s | 40.612MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e40.smt2     |   20.017s | 38.632MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e84.smt2     |   20.017s | 36.904MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k47.smt2     |   20.018s | 80.364MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e34.smt2     |   20.019s | 33.736MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e82.smt2     |   20.019s | 38.328MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e75.smt2     |   20.019s | 36.512MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k72.smt2     |   20.019s | 98.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k16.smt2     |   20.019s | 34.48MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e24.smt2     |   20.020s | 40.76MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k57.smt2     |   20.020s | 89.32MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e64.smt2     |   20.020s | 39.24MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k38.smt2     |   20.020s | 37.32MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k32.smt2     |   20.021s | 32.736MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e54.smt2     |   20.021s | 35.212MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e48.smt2     |   20.021s | 34.188MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e78.smt2     |   20.022s | 34.552MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k21.smt2     |   20.022s | 39.048MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e31.smt2     |   20.022s | 35.984MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e39.smt2     |   20.023s | 38.584MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k66.smt2     |   20.023s | 30.672MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e12.smt2     |   20.023s | 33.66MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e83.smt2     |   20.024s | 32.388MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e21.smt2     |   20.024s | 38.316MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e65.smt2     |   20.024s | 39.936MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k01.smt2     |   20.024s | 36.452MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e14.smt2     |   20.024s | 38.872MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e81.smt2     |   20.024s | 41.176MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e02.smt2     |   20.024s | 33.956MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k40.smt2     |   20.024s | 36.188MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e73.smt2     |   20.025s | 39.972MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k22.smt2     |   20.025s | 30.608MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k35.smt2     |   20.025s | 44.04MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e25.smt2     |   20.025s | 35.344MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e18.smt2     |   20.025s | 34.696MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k88.smt2     |   20.025s | 123.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e79.smt2     |   20.025s | 41.204MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k06.smt2     |   20.025s | 47.164MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e74.smt2     |   20.025s | 40.1MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e55.smt2     |   20.026s | 33.316MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e92.smt2     |   20.026s | 41.632MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e35.smt2     |   20.026s | 37.868MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k03.smt2     |   20.026s | 41.468MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e36.smt2     |   20.026s | 32.392MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k68.smt2     |   20.026s | 38.556MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e45.smt2     |   20.026s | 37.604MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k07.smt2     |   20.026s | 47.208MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k89.smt2     |   20.026s | 103.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e69.smt2     |   20.026s | 42.808MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e11.smt2     |   20.026s | 37.66MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e57.smt2     |   20.026s | 38.964MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k34.smt2     |   20.026s | 36.296MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k63.smt2     |   20.026s | 88.552MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e53.smt2     |   20.027s | 38.836MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e15.smt2     |   20.027s | 33.372MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e32.smt2     |   20.027s | 37.468MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e89.smt2     |   20.027s | 39.504MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k67.smt2     |   20.027s | 33.872MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k37.smt2     |   20.027s | 72.432MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k46.smt2     |   20.028s | 80.872MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k13.smt2     |   20.028s | 31.176MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k12.smt2     |   20.028s | 39.252MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e67.smt2     |   20.028s | 42.404MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k90.smt2     |   20.028s | 36.344MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e43.smt2     |   20.028s | 39.756MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e52.smt2     |   20.028s | 38.568MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e62.smt2     |   20.028s | 36.192MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k56.smt2     |   20.028s | 36.296MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e63.smt2     |   20.028s | 41.752MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e46.smt2     |   20.028s | 35.272MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k25.smt2     |   20.028s | 35.66MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k82.smt2     |   20.028s | 182.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k49.smt2     |   20.029s | 34.74MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e03.smt2     |   20.029s | 38.496MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k20.smt2     |   20.029s | 32.848MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k50.smt2     |   20.029s | 40.26MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e90.smt2     |   20.029s | 39.596MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k30.smt2     |   20.029s | 32.208MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k58.smt2     |   20.029s | 34.052MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k75.smt2     |   20.029s | 38.596MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k59.smt2     |   20.030s | 32.892MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k54.smt2     |   20.030s | 189.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e96.smt2     |   20.030s | 180.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k17.smt2     |   20.030s | 46.048MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k11.smt2     |   20.030s | 41.848MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k14.smt2     |   20.030s | 31.356MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k36.smt2     |   20.030s | 34.228MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k43.smt2     |   20.030s | 75.536MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e06.smt2     |   20.030s | 33.98MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k64.smt2     |   20.030s | 34.6MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e66.smt2     |   20.031s | 42.768MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k28.smt2     |   20.031s | 33.756MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k81.smt2     |   20.031s | 180.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k76.smt2     |   20.031s | 38.992MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e68.smt2     |   20.031s | 38.472MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e77.smt2     |   20.031s | 40.928MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k48.smt2     |   20.031s | 79.336MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e95.smt2     |   20.031s | 42.292MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e29.smt2     |   20.031s | 35.088MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e76.smt2     |   20.031s | 39.568MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e51.smt2     |   20.032s | 36.988MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k44.smt2     |   20.032s | 188.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k83.smt2     |   20.032s | 181.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k61.smt2     |   20.032s | 38.652MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e41.smt2     |   20.032s | 41.64MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k42.smt2     |   20.032s | 43.176MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k24.smt2     |   20.033s | 32.932MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e20.smt2     |   20.033s | 41.384MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k95.smt2     |   20.033s | 104.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e27.smt2     |   20.034s | 37.312MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k19.smt2     |   20.034s | 32.596MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e91.smt2     |   20.035s | 33.952MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k04.smt2     |   20.035s | 45.364MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k86.smt2     |   20.035s | 180.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k26.smt2     |   20.035s | 36.344MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e50.smt2     |   20.035s | 40.276MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e30.smt2     |   20.036s | 33.956MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e07.smt2     |   20.036s | 35.944MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k51.smt2     |   20.037s | 35.944MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e86.smt2     |   20.037s | 42.488MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k65.smt2     |   20.037s | 39.428MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e26.smt2     |   20.038s | 36.768MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k91.smt2     |   20.038s | 181.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k78.smt2     |   20.038s | 155.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k93.smt2     |   20.039s | 182.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k94.smt2     |   20.039s | 37.312MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e38.smt2     |   20.039s | 38.212MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k97.smt2     |   20.040s | 183.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k31.smt2     |   20.040s | 32.144MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k52.smt2     |   20.040s | 39.252MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k92.smt2     |   20.040s | 78.016MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k99.smt2     |   20.043s | 182.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k85.smt2     |   20.043s | 181.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e05.smt2     |   20.043s | 41.176MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k96.smt2     |   20.043s | 183.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e60.smt2     |   20.043s | 41.692MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k39.smt2     |   20.043s | 75.484MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e28.smt2     |   20.044s | 35.6MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k71.smt2     |   20.045s | 89.076MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k55.smt2     |   20.046s | 195.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k98.smt2     |   20.047s | 181.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k84.smt2     |   20.047s | 181.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k45.smt2     |   20.048s | 192.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k53.smt2     |   20.048s | 182.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_k87.smt2     |   20.048s | 180.0MiB| timeout | 0 |  |
|non-incremental/QF_UFDT/20210312-Bouvier/vlsat3_e97.smt2     |   20.053s | 192.0MiB| timeout | 0 |  |
