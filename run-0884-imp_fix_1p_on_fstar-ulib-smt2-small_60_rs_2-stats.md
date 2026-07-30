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
Job description: fstar-ulib-smt2-small, imp_fix, propagate_fixed_rows=true, -T:60, seed 2
Job tag: imp_fix_1p_on_fstar-ulib-smt2-small_60_rs_2
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 552abc0c032f57e86e040aa42d868ea19a34c7ed
Z3 branch: imp_fix
Z3 options: "-T:60 smt.random_seed=2 smt.arith.nl.propagate_fixed_rows=true"
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
|queries-FStar.UInt-264.smt2                                  |    1.097s | 85.328MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-285.smt2                           |    1.225s | 144.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-193.smt2                           |    1.228s | 141.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Parametricity-9.smt2                   |    1.331s | 155.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Parametricity-10.smt2                  |    1.348s | 154.0MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Fermat-54.smt2                            |    1.359s | 157.0MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Typing-69.smt2                      |    1.365s | 158.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-173.smt2                           |    1.377s | 142.0MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Fermat-22.smt2                            |    1.378s | 157.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-202.smt2                           |    1.397s | 63.356MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-321.smt2                           |    1.402s | 146.0MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Fermat-8.smt2                             |    1.406s | 157.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-304.smt2                           |    1.423s | 147.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-182.smt2                           |    1.444s | 143.0MiB| error-1 | 1 |  |  |
|queries-FStar.InteractiveHelpers.PostProcess-5.smt2          |    1.451s | 165.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-221.smt2                           |    1.535s | 145.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-64.smt2                            |    1.541s | 141.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-197.smt2                           |    1.569s | 146.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-269.smt2                           |    1.595s | 145.0MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Typing-95.smt2                      |    1.607s | 170.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-49.smt2                            |    1.618s | 141.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-69.smt2                            |    1.634s | 142.0MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Typing-136.smt2                     |    1.641s | 168.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-288.smt2                           |    1.704s | 148.0MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Typing-139.smt2                     |    1.781s | 192.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-232.smt2                           |    1.815s | 144.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Typeclasses-9.smt2                     |    1.823s | 160.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-278.smt2                           |    1.876s | 151.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-61.smt2                            |    1.916s | 142.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-200.smt2                           |    2.022s | 144.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-258.smt2                           |    2.068s | 149.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-313.smt2                           |    2.090s | 156.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-264.smt2                           |    2.160s | 155.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-17.smt2                            |    2.185s | 143.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-275.smt2                           |    2.203s | 152.0MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Fermat-52.smt2                            |    2.210s | 39.256MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-44.smt2                            |    2.213s | 147.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-159.smt2                           |    2.427s | 147.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-66.smt2                            |    2.476s | 145.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-165.smt2                           |    2.484s | 147.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-266.smt2                           |    2.508s | 149.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-188.smt2                           |    2.538s | 44.152MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-224.smt2                           |    2.628s | 149.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-228.smt2                           |    2.761s | 149.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-234.smt2                           |    2.942s | 58.252MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-155.smt2                           |    3.211s | 146.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-239.smt2                           |    3.288s | 150.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-280.smt2                           |    3.305s | 160.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-310.smt2                           |    3.855s | 163.0MiB| error-1 | 1 |  |  |
|queries-LowStar.Monotonic.Buffer-286.smt2                    |    4.616s | 159.0MiB| error-1 | 1 |  |  |
