# .

* SAT 0
* UNSAT 0
* TIMEOUT 0
* UNKNOWN 0

* ERRORS 50 (error-1:50)

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: fstar-ulib-smt2-small, imp_fix, propagate_fixed_rows=true, -T:60, seed 1
Job tag: imp_fix_1p_on_fstar-ulib-smt2-small_60_rs_1
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 552abc0c032f57e86e040aa42d868ea19a34c7ed
Z3 branch: imp_fix
Z3 options: "-T:60 smt.random_seed=1 smt.arith.nl.propagate_fixed_rows=true"
Z3 inputs: inputs/fstar-ulib-smt2-small
Z3 commit message: nla: record why the two fstar regressions are not worth tuning away

queries-Pulse.Lib.HashTable.Spec-1 and FStar.Matrix-2 cost more with
propagate_fixed_rows on. Neither is a defect in the derivation; both are
second-order interactions with the bounded nlsat escalation in check(),
and they pull in opposite directions.

On HashTable.Spec the escalation is pure waste. All 11 nra calls come from
the should_run_bounded_nlsat() site, and with arith.nl.nra=false the file
is still unsat and the pass is a 15% win: 223336 rlimit on master, 472374
with the pass, 189730 with the pass and no nra. The pass has already
derived what monomial_bounds and add_bounds would have, so no_effect()
holds while monomials still need refining, and m_nlsat_delay starts at 0,
so nlsat runs at the first opportunity. On Matrix-2 the escalation is
load-bearing instead: with arith.nl.nra=false that file goes from 765543
to unknown.

Both global levers were measured and both fail the same way, fixing
HashTable.Spec and turning Matrix-2 from unsat into unknown:

  initial m_nlsat_delay   0 (now)      5        10       50    exponential
    HashTable.Spec         472374   557081   189730   189730   429941
    Matrix-2              1603093  unknown  unknown  unknown   unknown

  bounded nlsat rlimit   100000 (now)   25000     5000
    HashTable.Spec             472374   382601   194730
    Matrix-2                  1603213  2307249  unknown

There is no local signal to separate the two cases either: at the
escalation point the bounded runs are equally productive, 6 conflicts and
5 inconclusive on HashTable.Spec against 6 and 9 on Matrix-2. What differs
is whether the rest of the search would have closed the goal anyway, which
is not knowable there.

Comment only, no functional change.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|queries-FStar.UInt-264.smt2                                  |    1.119s | 85.056MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-193.smt2                           |    1.248s | 141.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-285.smt2                           |    1.279s | 144.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Parametricity-10.smt2                  |    1.285s | 155.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Parametricity-9.smt2                   |    1.337s | 155.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-202.smt2                           |    1.353s | 63.456MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Typing-69.smt2                      |    1.378s | 158.0MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Fermat-22.smt2                            |    1.381s | 157.0MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Fermat-8.smt2                             |    1.385s | 157.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-173.smt2                           |    1.387s | 142.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-304.smt2                           |    1.390s | 147.0MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Fermat-54.smt2                            |    1.413s | 157.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-321.smt2                           |    1.417s | 146.0MiB| error-1 | 1 |  |  |
|queries-FStar.InteractiveHelpers.PostProcess-5.smt2          |    1.439s | 165.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-221.smt2                           |    1.503s | 144.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-182.smt2                           |    1.517s | 143.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-64.smt2                            |    1.518s | 141.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-197.smt2                           |    1.541s | 146.0MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Fermat-52.smt2                            |    1.554s | 38.232MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Typing-136.smt2                     |    1.568s | 168.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-269.smt2                           |    1.602s | 145.0MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Typing-95.smt2                      |    1.608s | 170.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-69.smt2                            |    1.631s | 142.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-49.smt2                            |    1.674s | 141.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-288.smt2                           |    1.693s | 148.0MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Typing-139.smt2                     |    1.762s | 192.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-232.smt2                           |    1.783s | 144.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Typeclasses-9.smt2                     |    1.814s | 160.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-61.smt2                            |    1.817s | 142.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-278.smt2                           |    1.886s | 151.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-200.smt2                           |    1.982s | 144.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-258.smt2                           |    2.016s | 149.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-313.smt2                           |    2.065s | 155.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-275.smt2                           |    2.159s | 152.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-264.smt2                           |    2.203s | 155.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-44.smt2                            |    2.215s | 146.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-17.smt2                            |    2.217s | 143.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-66.smt2                            |    2.507s | 145.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-165.smt2                           |    2.546s | 147.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-159.smt2                           |    2.551s | 147.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-188.smt2                           |    2.551s | 44.204MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-266.smt2                           |    2.596s | 149.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-224.smt2                           |    2.701s | 149.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-228.smt2                           |    2.738s | 149.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-234.smt2                           |    3.067s | 57.984MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-155.smt2                           |    3.220s | 147.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-280.smt2                           |    3.239s | 159.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-239.smt2                           |    3.310s | 150.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-310.smt2                           |    3.563s | 163.0MiB| error-1 | 1 |  |  |
|queries-LowStar.Monotonic.Buffer-286.smt2                    |    4.423s | 159.0MiB| error-1 | 1 |  |  |
