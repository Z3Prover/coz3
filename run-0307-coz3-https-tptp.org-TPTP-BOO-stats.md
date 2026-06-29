# .

* SAT 0
* UNSAT 0
* TIMEOUT 0
* UNKNOWN 0

* ERRORS 0

* SZS (TPTP) 150 (Success: 23, NoSuccess: 127) (Timeout:127, Unsatisfiable:18, Satisfiable:5)

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://tptp.org/TPTP/Distribution/TPTP-v9.2.1.tgz Problems/BOO | Source list: benchmarks-tptp.txt
Job tag: coz3-https-tptp.org-TPTP-BOO
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: d197cee018b458d95e22f3e575b3d5f536858bf2
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://tptp.org/TPTP/Distribution/TPTP-v9.2.1.tgz Problems/BOO
Z3 commit message: Fix TPTP front-end precedence and Int/Real coercion bugs

Three translation defects in tptp_frontend.cpp caused spurious sat/unsat
verdicts (reported as SZS BUG against annotated status):

- Parenthesized negation bound the whole disjunction: ( ~ p | q ) parsed
  as ~(p | q) instead of (~p) | q, flipping nearly every CNF/FOF clause.
  Negate only the next unary unit, then resume precedence parsing via a
  new parse_binary_rest helper.
- Quantifier bodies absorbed lower-precedence connectives: ! [X] : p(X) => g
  parsed as ! [X] : (p(X) => g). TPTP quantifiers bind tighter than the
  binary connectives, so parse the body at parse_expr(PREC_EQ).
- Mixed Int/Real equality coerced through an uninterpreted box function,
  severing arithmetic semantics and yielding spurious models. Use the
  arithmetic to_real/to_int conversions instead.

Add regression cases to src/test/tptp.cpp covering all three fixes.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|BOO011-2.p                                                   |    0.028s | 20.36MiB| Unsatisfiable | 0 |  |  |
|BOO018-4.p                                                   |    0.031s | 20.34MiB| Unsatisfiable | 0 |  |  |
|BOO013-4.p                                                   |    0.035s | 21.392MiB| Unsatisfiable | 0 |  |  |
|BOO013-2.p                                                   |    0.050s | 21.38MiB| Unsatisfiable | 0 |  |  |
|BOO011-4.p                                                   |    0.051s | 20.172MiB| Unsatisfiable | 0 |  |  |
|BOO011-1.p                                                   |    0.051s | 21.148MiB| Unsatisfiable | 0 |  |  |
|BOO021-1.p                                                   |    0.056s | 20.744MiB| Unsatisfiable | 0 |  |  |
|BOO037-3.p                                                   |    0.057s | 21.236MiB| Satisfiable | 0 |  |  |
|BOO012-4.p                                                   |    0.059s | 21.436MiB| Unsatisfiable | 0 |  |  |
|BOO003-1.p                                                   |    0.059s | 22.26MiB| Unsatisfiable | 0 |  |  |
|BOO037-2.p                                                   |    0.059s | 20.884MiB| Satisfiable | 0 |  |  |
|BOO008-3.p                                                   |    0.064s | 21.088MiB| Satisfiable | 0 |  |  |
|BOO012-2.p                                                   |    0.073s | 21.796MiB| Unsatisfiable | 0 |  |  |
|BOO036-1.p                                                   |    0.075s | 21.16MiB| Satisfiable | 0 |  |  |
|BOO001-1.p                                                   |    0.080s | 21.532MiB| Unsatisfiable | 0 |  |  |
|BOO004-1.p                                                   |    0.088s | 22.328MiB| Unsatisfiable | 0 |  |  |
|BOO004-10.p                                                  |    0.107s | 29.16MiB| Unsatisfiable | 0 |  |  |
|BOO022-1.p                                                   |    0.170s | 34.92MiB| Unsatisfiable | 0 |  |  |
|BOO005-1.p                                                   |    3.185s | 71.188MiB| Unsatisfiable | 0 |  |  |
|BOO012-3.p                                                   |    3.226s | 113.0MiB| Unsatisfiable | 0 |  |  |
|BOO029-1.p                                                   |    5.312s | 601.0MiB| Unsatisfiable | 0 |  |  |
|BOO006-1.p                                                   |   11.928s | 256.0MiB| Unsatisfiable | 0 |  |  |
|BOO037-1.p                                                   |   19.457s | 333.0MiB| Satisfiable | 0 |  |  |
|BOO088-1.p                                                   |   20.033s | 238.0MiB| Timeout | 0 |  |  |
|BOO074-1.p                                                   |   20.038s | 111.0MiB| Timeout | 0 |  |  |
|BOO070-1.p                                                   |   20.040s | 127.0MiB| Timeout | 0 |  |  |
|BOO041-1.p                                                   |   20.040s | 106.0MiB| Timeout | 0 |  |  |
|BOO071-1.p                                                   |   20.041s | 119.0MiB| Timeout | 0 |  |  |
|BOO101-1.p                                                   |   20.042s | 112.0MiB| Timeout | 0 |  |  |
|BOO068-1.p                                                   |   20.043s | 125.0MiB| Timeout | 0 |  |  |
|BOO079-1.p                                                   |   20.043s | 114.0MiB| Timeout | 0 |  |  |
|BOO061-1.p                                                   |   20.045s | 107.0MiB| Timeout | 0 |  |  |
|BOO020-1.p                                                   |   20.045s | 124.0MiB| Timeout | 0 |  |  |
|BOO098-1.p                                                   |   20.045s | 112.0MiB| Timeout | 0 |  |  |
|BOO106-1.p                                                   |   20.046s | 113.0MiB| Timeout | 0 |  |  |
|BOO085-1.p                                                   |   20.047s | 109.0MiB| Timeout | 0 |  |  |
|BOO046-10.p                                                  |   20.048s | 106.0MiB| Timeout | 0 |  |  |
|BOO039-1.p                                                   |   20.051s | 118.0MiB| Timeout | 0 |  |  |
|BOO084-1.p                                                   |   20.052s | 111.0MiB| Timeout | 0 |  |  |
|BOO090-1.p                                                   |   20.055s | 107.0MiB| Timeout | 0 |  |  |
|BOO060-1.p                                                   |   20.056s | 106.0MiB| Timeout | 0 |  |  |
|BOO095-1.p                                                   |   20.056s | 119.0MiB| Timeout | 0 |  |  |
|BOO048-1.p                                                   |   20.056s | 104.0MiB| Timeout | 0 |  |  |
|BOO059-1.p                                                   |   20.057s | 103.0MiB| Timeout | 0 |  |  |
|BOO073-1.p                                                   |   20.057s | 105.0MiB| Timeout | 0 |  |  |
|BOO076-1.p                                                   |   20.058s | 115.0MiB| Timeout | 0 |  |  |
|BOO104-1.p                                                   |   20.061s | 108.0MiB| Timeout | 0 |  |  |
|BOO055-1.p                                                   |   20.062s | 117.0MiB| Timeout | 0 |  |  |
|BOO052-1.p                                                   |   20.064s | 110.0MiB| Timeout | 0 |  |  |
|BOO008-10.p                                                  |   20.064s | 191.0MiB| Timeout | 0 |  |  |
|BOO047-1.p                                                   |   20.065s | 107.0MiB| Timeout | 0 |  |  |
|BOO067-1.p                                                   |   20.065s | 65.028MiB| Timeout | 0 |  |  |
|BOO054-1.p                                                   |   20.065s | 121.0MiB| Timeout | 0 |  |  |
|BOO050-1.p                                                   |   20.067s | 102.0MiB| Timeout | 0 |  |  |
|BOO103-1.p                                                   |   20.070s | 107.0MiB| Timeout | 0 |  |  |
|BOO097-1.p                                                   |   20.072s | 131.0MiB| Timeout | 0 |  |  |
|BOO086-1.p                                                   |   20.073s | 111.0MiB| Timeout | 0 |  |  |
|BOO080-1.p                                                   |   20.075s | 113.0MiB| Timeout | 0 |  |  |
|BOO058-1.p                                                   |   20.075s | 108.0MiB| Timeout | 0 |  |  |
|BOO057-10.p                                                  |   20.076s | 113.0MiB| Timeout | 0 |  |  |
|BOO056-1.p                                                   |   20.077s | 102.0MiB| Timeout | 0 |  |  |
|BOO046-1.p                                                   |   20.078s | 102.0MiB| Timeout | 0 |  |  |
|BOO083-1.p                                                   |   20.078s | 104.0MiB| Timeout | 0 |  |  |
|BOO047-10.p                                                  |   20.078s | 107.0MiB| Timeout | 0 |  |  |
|BOO042-1.p                                                   |   20.079s | 116.0MiB| Timeout | 0 |  |  |
|BOO099-1.p                                                   |   20.079s | 116.0MiB| Timeout | 0 |  |  |
|BOO040-1.p                                                   |   20.079s | 107.0MiB| Timeout | 0 |  |  |
|BOO092-1.p                                                   |   20.082s | 109.0MiB| Timeout | 0 |  |  |
|BOO072-1.p                                                   |   20.085s | 120.0MiB| Timeout | 0 |  |  |
|BOO075-1.p                                                   |   20.086s | 114.0MiB| Timeout | 0 |  |  |
|BOO081-1.p                                                   |   20.088s | 116.0MiB| Timeout | 0 |  |  |
|BOO035-1.p                                                   |   20.088s | 110.0MiB| Timeout | 0 |  |  |
|BOO102-1.p                                                   |   20.088s | 114.0MiB| Timeout | 0 |  |  |
|BOO093-1.p                                                   |   20.089s | 109.0MiB| Timeout | 0 |  |  |
|BOO044-1.p                                                   |   20.090s | 107.0MiB| Timeout | 0 |  |  |
|BOO108-1.p                                                   |   20.090s | 112.0MiB| Timeout | 0 |  |  |
|BOO096-1.p                                                   |   20.091s | 123.0MiB| Timeout | 0 |  |  |
|BOO089-1.p                                                   |   20.091s | 107.0MiB| Timeout | 0 |  |  |
|BOO053-1.p                                                   |   20.092s | 105.0MiB| Timeout | 0 |  |  |
|BOO051-1.p                                                   |   20.092s | 113.0MiB| Timeout | 0 |  |  |
|BOO091-1.p                                                   |   20.093s | 109.0MiB| Timeout | 0 |  |  |
|BOO107-1.p                                                   |   20.095s | 110.0MiB| Timeout | 0 |  |  |
|BOO057-1.p                                                   |   20.096s | 112.0MiB| Timeout | 0 |  |  |
|BOO105-1.p                                                   |   20.096s | 81.36MiB| Timeout | 0 |  |  |
|BOO094-1.p                                                   |   20.098s | 105.0MiB| Timeout | 0 |  |  |
|BOO043-1.p                                                   |   20.098s | 105.0MiB| Timeout | 0 |  |  |
|BOO077-1.p                                                   |   20.099s | 112.0MiB| Timeout | 0 |  |  |
|BOO078-1.p                                                   |   20.101s | 114.0MiB| Timeout | 0 |  |  |
|BOO051-10.p                                                  |   20.101s | 119.0MiB| Timeout | 0 |  |  |
|BOO014-10.p                                                  |   20.101s | 312.0MiB| Timeout | 0 |  |  |
|BOO056-10.p                                                  |   20.102s | 110.0MiB| Timeout | 0 |  |  |
|BOO100-1.p                                                   |   20.103s | 117.0MiB| Timeout | 0 |  |  |
|BOO082-1.p                                                   |   20.106s | 119.0MiB| Timeout | 0 |  |  |
|BOO087-1.p                                                   |   20.112s | 237.0MiB| Timeout | 0 |  |  |
|BOO015-10.p                                                  |   20.115s | 165.0MiB| Timeout | 0 |  |  |
|BOO069-1.p                                                   |   20.131s | 143.0MiB| Timeout | 0 |  |  |
|BOO038-1.p                                                   |   20.137s | 191.0MiB| Timeout | 0 |  |  |
|BOO017-10.p                                                  |   20.139s | 327.0MiB| Timeout | 0 |  |  |
|BOO045-1.p                                                   |   20.144s | 238.0MiB| Timeout | 0 |  |  |
|BOO014-3.p                                                   |   20.159s | 487.0MiB| Timeout | 0 |  |  |
|BOO007-1.p                                                   |   20.165s | 363.0MiB| Timeout | 0 |  |  |
|BOO014-1.p                                                   |   20.179s | 323.0MiB| Timeout | 0 |  |  |
|BOO012-1.p                                                   |   20.202s | 566.0MiB| Timeout | 0 |  |  |
|BOO049-1.p                                                   |   20.207s | 532.0MiB| Timeout | 0 |  |  |
|BOO032-1.p                                                   |   20.213s | 496.0MiB| Timeout | 0 |  |  |
|BOO033-1.p                                                   |   20.215s | 402.0MiB| Timeout | 0 |  |  |
|BOO008-1.p                                                   |   20.216s | 502.0MiB| Timeout | 0 |  |  |
|BOO016-1.p                                                   |   20.237s | 544.0MiB| Timeout | 0 |  |  |
|BOO015-1.p                                                   |   20.246s | 601.0MiB| Timeout | 0 |  |  |
|BOO030-1.p                                                   |   20.246s | 478.0MiB| Timeout | 0 |  |  |
|BOO010-1.p                                                   |   20.248s | 646.0MiB| Timeout | 0 |  |  |
|BOO009-1.p                                                   |   20.259s | 630.0MiB| Timeout | 0 |  |  |
|BOO017-1.p                                                   |   20.274s | 607.0MiB| Timeout | 0 |  |  |
|BOO109+1.p                                                   |   20.288s | 751.0MiB| Timeout | 0 |  |  |
|BOO013-1.p                                                   |   20.289s | 764.0MiB| Timeout | 0 |  |  |
|BOO005-4.p                                                   |   20.318s | 1087.0MiB| Timeout | 0 |  |  |
|BOO066-1.p                                                   |   20.321s | 804.0MiB| Timeout | 0 |  |  |
|BOO006-2.p                                                   |   20.333s | 822.0MiB| Timeout | 0 |  |  |
|BOO005-2.p                                                   |   20.336s | 841.0MiB| Timeout | 0 |  |  |
|BOO009-2.p                                                   |   20.369s | 1145.0MiB| Timeout | 0 |  |  |
|BOO006-4.p                                                   |   20.380s | 1116.0MiB| Timeout | 0 |  |  |
|BOO015-4.p                                                   |   20.395s | 1794.0MiB| Timeout | 0 |  |  |
|BOO010-2.p                                                   |   20.398s | 1153.0MiB| Timeout | 0 |  |  |
|BOO031-1.p                                                   |   20.408s | 1098.0MiB| Timeout | 0 |  |  |
|BOO017-2.p                                                   |   20.416s | 1110.0MiB| Timeout | 0 |  |  |
|BOO013-3.p                                                   |   20.418s | 1234.0MiB| Timeout | 0 |  |  |
|BOO016-2.p                                                   |   20.429s | 1194.0MiB| Timeout | 0 |  |  |
|BOO024-1.p                                                   |   20.438s | 1630.0MiB| Timeout | 0 |  |  |
|BOO015-2.p                                                   |   20.444s | 1964.0MiB| Timeout | 0 |  |  |
|BOO007-2.p                                                   |   20.449s | 2333.0MiB| Timeout | 0 |  |  |
|BOO009-4.p                                                   |   20.453s | 1749.0MiB| Timeout | 0 |  |  |
|BOO028-1.p                                                   |   20.457s | 2072.0MiB| Timeout | 0 |  |  |
|BOO026-1.p                                                   |   20.492s | 1592.0MiB| Timeout | 0 |  |  |
|BOO010-4.p                                                   |   20.505s | 1751.0MiB| Timeout | 0 |  |  |
|BOO003-2.p                                                   |   20.509s | 2657.0MiB| Timeout | 0 |  |  |
|BOO019-1.p                                                   |   20.509s | 2468.0MiB| Timeout | 0 |  |  |
|BOO004-2.p                                                   |   20.511s | 2676.0MiB| Timeout | 0 |  |  |
|BOO014-4.p                                                   |   20.512s | 1792.0MiB| Timeout | 0 |  |  |
|BOO004-4.p                                                   |   20.521s | 2440.0MiB| Timeout | 0 |  |  |
|BOO014-2.p                                                   |   20.555s | 1934.0MiB| Timeout | 0 |  |  |
|BOO002-1.p                                                   |   20.569s | 3107.0MiB| Timeout | 0 |  |  |
|BOO008-2.p                                                   |   20.569s | 2096.0MiB| Timeout | 0 |  |  |
|BOO008-4.p                                                   |   20.582s | 3218.0MiB| Timeout | 0 |  |  |
|BOO003-4.p                                                   |   20.599s | 2419.0MiB| Timeout | 0 |  |  |
|BOO034-1.p                                                   |   20.638s | 3202.0MiB| Timeout | 0 |  |  |
|BOO002-2.p                                                   |   20.685s | 3096.0MiB| Timeout | 0 |  |  |
|BOO007-4.p                                                   |   20.699s | 3221.0MiB| Timeout | 0 |  |  |
|BOO025-1.p                                                   |   20.709s | 3231.0MiB| Timeout | 0 |  |  |
|BOO023-1.p                                                   |   20.716s | 3402.0MiB| Timeout | 0 |  |  |
|BOO027-1.p                                                   |   20.774s | 4776.0MiB| Timeout | 0 |  |  |
