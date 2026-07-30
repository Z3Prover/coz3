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
Job tag: imp_fix_on_fstar-ulib-smt2-small_60_rs_2
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 2516a29d5380f5aeb55232afc0bc1798ea7f3e43
Z3 branch: imp_fix
Z3 options: "-T:60 smt.random_seed=2 smt.arith.nl.propagate_fixed_rows=true"
Z3 inputs: inputs/fstar-ulib-smt2-small
Z3 commit message: nla: install values implied by rows with a single non-fixed column

A tableau row is an equality sum_j a_j x_j = 0. If every column in it is
fixed except one, the row determines that column by constant folding:
x_k = -(sum_{j != k} a_j v_j) / a_k. No simplex is involved.

Only columns occurring in a monomial are installed, because the point is
the effect on nonlinear reasoning rather than tighter arithmetic in
general: monomial_bounds::is_linear propagates a monic with at most one
non-fixed factor as a linear equality, which removes it from nonlinear
reasoning, so fixing one column can linearize every monomial it occurs in
at once.

These rows are common rather than exotic. On z3test/regressions/fstar's
FStar.UInt128-1 the tableau has 198 rows, the median row has 4 entries of
which 57% are already fixed, and 24 rows have exactly one non-fixed
column. z3 installs none of them: lar_solver only analyzes rows in
touched_rows(), i.e. rows changed by a pivot, and
theory_lra::bound_is_interesting keeps an implied bound only when it
matches an existing unassigned atom, so an implied equality with no
corresponding literal never reaches the column. Installing them takes that
file's linear monics from 6/19 to 14/19 and the file from 504361 to 147848
rlimit.

Off by default behind arith.nl.propagate_fixed_rows pending the A/B on the
benchmark sets. Measured on an earlier prototype over 20 random seeds on
z3test/regressions/fstar it solved 311/380 against 305 for master, with no
sat/unsat mismatch in 2560 paired answers, and on a 255 instance sample of
QF_NIA it was 194 against 196.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|queries-FStar.UInt-264.smt2                                  |    1.103s | 85.272MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-285.smt2                           |    1.230s | 144.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-193.smt2                           |    1.254s | 141.0MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Typing-69.smt2                      |    1.345s | 158.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Parametricity-10.smt2                  |    1.347s | 154.0MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Fermat-22.smt2                            |    1.354s | 157.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Parametricity-9.smt2                   |    1.357s | 155.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-202.smt2                           |    1.368s | 63.412MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-173.smt2                           |    1.370s | 142.0MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Fermat-8.smt2                             |    1.395s | 157.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-304.smt2                           |    1.413s | 147.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-321.smt2                           |    1.426s | 146.0MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Fermat-54.smt2                            |    1.430s | 157.0MiB| error-1 | 1 |  |  |
|queries-FStar.InteractiveHelpers.PostProcess-5.smt2          |    1.483s | 165.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-182.smt2                           |    1.492s | 144.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-197.smt2                           |    1.555s | 146.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-221.smt2                           |    1.561s | 144.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-64.smt2                            |    1.566s | 141.0MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Fermat-52.smt2                            |    1.598s | 38.228MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-269.smt2                           |    1.603s | 145.0MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Typing-136.smt2                     |    1.623s | 168.0MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Typing-95.smt2                      |    1.624s | 170.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-49.smt2                            |    1.682s | 141.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-69.smt2                            |    1.692s | 142.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-288.smt2                           |    1.761s | 148.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-232.smt2                           |    1.780s | 144.0MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Typing-139.smt2                     |    1.785s | 192.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-278.smt2                           |    1.836s | 151.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Typeclasses-9.smt2                     |    1.849s | 160.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-61.smt2                            |    1.955s | 142.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-258.smt2                           |    2.004s | 149.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-200.smt2                           |    2.094s | 144.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-264.smt2                           |    2.111s | 155.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-275.smt2                           |    2.140s | 152.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-313.smt2                           |    2.152s | 156.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-17.smt2                            |    2.207s | 143.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-44.smt2                            |    2.262s | 146.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-159.smt2                           |    2.446s | 147.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-66.smt2                            |    2.492s | 145.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-165.smt2                           |    2.494s | 148.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-188.smt2                           |    2.551s | 44.22MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-266.smt2                           |    2.577s | 149.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-228.smt2                           |    2.710s | 149.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-224.smt2                           |    2.711s | 149.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-234.smt2                           |    2.933s | 58.104MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-155.smt2                           |    3.192s | 146.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-239.smt2                           |    3.231s | 150.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-280.smt2                           |    3.386s | 160.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-310.smt2                           |    3.532s | 163.0MiB| error-1 | 1 |  |  |
|queries-LowStar.Monotonic.Buffer-286.smt2                    |    4.569s | 159.0MiB| error-1 | 1 |  |  |
