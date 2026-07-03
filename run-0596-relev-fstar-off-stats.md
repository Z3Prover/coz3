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
Job description: Relevancy watch-filter A/B on quantified F* VCs: FILTER OFF. fstar-ulib-smt2-small, -T:60. Branch perf/relevancy-watch-filter-toggle.
Job tag: relev-fstar-off
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 435e4d7897d6754db17c817642ac60bd7c6a3637
Z3 branch: perf/relevancy-watch-filter-toggle
Z3 options: "-T:60 smt.relevancy.watch_filter=false"
Z3 inputs: inputs/fstar-ulib-smt2-small
Z3 commit message: smt: add smt.relevancy.watch_filter toggle for the watch-list filter

Make the relevancy_propagator watch-list membership filter (m_is_watched)
runtime-toggleable via a new boolean parameter smt.relevancy.watch_filter
(default true). When false, get_watches() always probes m_watches, giving
the pre-filter behavior. This enables A/B performance comparison of the
same binary (filter on vs off) under the Ramon benchmark harness.

Behavior-preserving either way: verified identical results on QF_LIA with
the filter on and off.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|queries-FStar.ModifiesGen-193.smt2                           |    1.228s | 142.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-285.smt2                           |    1.230s | 144.0MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Typing-69.smt2                      |    1.312s | 158.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Parametricity-10.smt2                  |    1.320s | 154.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-202.smt2                           |    1.325s | 59.392MiB| error-1 | 1 |  |  |
|queries-FStar.UInt-264.smt2                                  |    1.346s | 87.32MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Fermat-52.smt2                            |    1.358s | 36.2MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Parametricity-9.smt2                   |    1.359s | 154.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-304.smt2                           |    1.376s | 146.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-321.smt2                           |    1.390s | 146.0MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Fermat-54.smt2                            |    1.392s | 157.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-173.smt2                           |    1.394s | 142.0MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Fermat-8.smt2                             |    1.397s | 157.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-182.smt2                           |    1.424s | 144.0MiB| error-1 | 1 |  |  |
|queries-FStar.InteractiveHelpers.PostProcess-5.smt2          |    1.463s | 164.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-64.smt2                            |    1.481s | 141.0MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Fermat-22.smt2                            |    1.489s | 157.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-221.smt2                           |    1.496s | 144.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-197.smt2                           |    1.506s | 145.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-269.smt2                           |    1.564s | 144.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-49.smt2                            |    1.590s | 141.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-69.smt2                            |    1.603s | 141.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-288.smt2                           |    1.625s | 148.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-61.smt2                            |    1.694s | 141.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-278.smt2                           |    1.699s | 150.0MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Typing-136.smt2                     |    1.719s | 296.0MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Typing-95.smt2                      |    1.733s | 298.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Typeclasses-9.smt2                     |    1.816s | 160.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-232.smt2                           |    1.860s | 143.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-200.smt2                           |    1.883s | 143.0MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Typing-139.smt2                     |    1.896s | 272.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-17.smt2                            |    1.901s | 142.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-313.smt2                           |    1.916s | 155.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-44.smt2                            |    1.945s | 145.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-264.smt2                           |    1.951s | 154.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-275.smt2                           |    2.014s | 156.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-188.smt2                           |    2.094s | 42.876MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-66.smt2                            |    2.262s | 145.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-159.smt2                           |    2.309s | 147.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-165.smt2                           |    2.320s | 147.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-234.smt2                           |    2.395s | 56.44MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-224.smt2                           |    2.484s | 147.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-266.smt2                           |    2.598s | 150.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-258.smt2                           |    2.671s | 158.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-228.smt2                           |    2.709s | 148.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-239.smt2                           |    2.723s | 150.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-280.smt2                           |    2.994s | 157.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-155.smt2                           |    3.310s | 150.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-310.smt2                           |    3.584s | 165.0MiB| error-1 | 1 |  |  |
|queries-LowStar.Monotonic.Buffer-286.smt2                    |    4.280s | 140.0MiB| error-1 | 1 |  |  |
