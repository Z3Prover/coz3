# .

* SAT 0
* UNSAT 0
* TIMEOUT 0
* UNKNOWN 0

* ERRORS 0

* SZS (TPTP) 309 (Success: 78, NoSuccess: 231) (Timeout:231, Theorem:56, Unsatisfiable:20, Satisfiable:2)

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://tptp.org/TPTP/Distribution/TPTP-v9.2.1.tgz Problems/SCT | Source list: benchmarks-tptp.txt
Job tag: coz3-https-tptp.org-TPTP-SCT
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: d197cee018b458d95e22f3e575b3d5f536858bf2
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://tptp.org/TPTP/Distribution/TPTP-v9.2.1.tgz Problems/SCT
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
|SCT226_5.p                                                   |    0.045s | 20.504MiB| Theorem | 0 |  |  |
|SCT171+2.p                                                   |    0.049s | 20.82MiB| Theorem | 0 |  |  |
|SCT101-1.p                                                   |    0.053s | 21.06MiB| Satisfiable | 0 |  |  |
|SCT171^1.p                                                   |    0.065s | 20.108MiB| Theorem | 0 |  |  |
|SCT231_5.p                                                   |    0.067s | 20.652MiB| Theorem | 0 |  |  |
|SCT171+1.p                                                   |    0.071s | 20.104MiB| Theorem | 0 |  |  |
|SCT229_5.p                                                   |    0.071s | 19.788MiB| Theorem | 0 |  |  |
|SCT241_5.p                                                   |    0.072s | 23.48MiB| Theorem | 0 |  |  |
|SCT214_5.p                                                   |    0.073s | 22.872MiB| Theorem | 0 |  |  |
|SCT261_5.p                                                   |    0.075s | 19.932MiB| Theorem | 0 |  |  |
|SCT199_5.p                                                   |    0.076s | 27.764MiB| Theorem | 0 |  |  |
|SCT105+1.p                                                   |    0.078s | 20.344MiB| Theorem | 0 |  |  |
|SCT222_5.p                                                   |    0.082s | 19.808MiB| Theorem | 0 |  |  |
|SCT175_5.p                                                   |    0.082s | 22.148MiB| Theorem | 0 |  |  |
|SCT243_5.p                                                   |    0.084s | 23.492MiB| Theorem | 0 |  |  |
|SCT236_5.p                                                   |    0.086s | 25.06MiB| Theorem | 0 |  |  |
|SCT171_1.p                                                   |    0.087s | 20.128MiB| Theorem | 0 |  |  |
|SCT179_5.p                                                   |    0.088s | 21.696MiB| Theorem | 0 |  |  |
|SCT072-1.p                                                   |    0.088s | 20.924MiB| Satisfiable | 0 |  |  |
|SCT252_5.p                                                   |    0.089s | 21.592MiB| Theorem | 0 |  |  |
|SCT100-1.p                                                   |    0.096s | 20.416MiB| Unsatisfiable | 0 |  |  |
|SCT264_5.p                                                   |    0.096s | 22.46MiB| Theorem | 0 |  |  |
|SCT171_3.p                                                   |    0.103s | 21.8MiB| Theorem | 0 |  |  |
|SCT240_5.p                                                   |    0.105s | 29.76MiB| Theorem | 0 |  |  |
|SCT094-1.p                                                   |    0.114s | 21.272MiB| Unsatisfiable | 0 |  |  |
|SCT052-1.p                                                   |    0.116s | 20.62MiB| Unsatisfiable | 0 |  |  |
|SCT174_5.p                                                   |    0.116s | 25.884MiB| Theorem | 0 |  |  |
|SCT061-1.p                                                   |    0.117s | 20.492MiB| Unsatisfiable | 0 |  |  |
|SCT157+1.p                                                   |    0.117s | 25.844MiB| Theorem | 0 |  |  |
|SCT253_5.p                                                   |    0.117s | 21.532MiB| Theorem | 0 |  |  |
|SCT246_5.p                                                   |    0.120s | 21.692MiB| Theorem | 0 |  |  |
|SCT171^3.p                                                   |    0.121s | 21.38MiB| Theorem | 0 |  |  |
|SCT171^2.p                                                   |    0.122s | 20.612MiB| Theorem | 0 |  |  |
|SCT255_5.p                                                   |    0.123s | 22.664MiB| Theorem | 0 |  |  |
|SCT250_5.p                                                   |    0.125s | 21.644MiB| Theorem | 0 |  |  |
|SCT007-1.p                                                   |    0.135s | 26.524MiB| Unsatisfiable | 0 |  |  |
|SCT235_5.p                                                   |    0.136s | 24.92MiB| Theorem | 0 |  |  |
|SCT207_5.p                                                   |    0.141s | 28.816MiB| Theorem | 0 |  |  |
|SCT171+5.p                                                   |    0.144s | 20.288MiB| Theorem | 0 |  |  |
|SCT158+1.p                                                   |    0.147s | 25.904MiB| Theorem | 0 |  |  |
|SCT074-1.p                                                   |    0.151s | 26.624MiB| Unsatisfiable | 0 |  |  |
|SCT083-1.p                                                   |    0.152s | 26.612MiB| Unsatisfiable | 0 |  |  |
|SCT176_5.p                                                   |    0.153s | 19.884MiB| Theorem | 0 |  |  |
|SCT247_5.p                                                   |    0.158s | 24.72MiB| Theorem | 0 |  |  |
|SCT242_5.p                                                   |    0.159s | 23.94MiB| Theorem | 0 |  |  |
|SCT155+1.p                                                   |    0.165s | 25.86MiB| Theorem | 0 |  |  |
|SCT233_5.p                                                   |    0.165s | 34.328MiB| Theorem | 0 |  |  |
|SCT073-1.p                                                   |    0.170s | 27.46MiB| Unsatisfiable | 0 |  |  |
|SCT002-1.p                                                   |    0.173s | 27.84MiB| Unsatisfiable | 0 |  |  |
|SCT064-1.p                                                   |    0.174s | 25.64MiB| Unsatisfiable | 0 |  |  |
|SCT031-1.p                                                   |    0.181s | 25.344MiB| Unsatisfiable | 0 |  |  |
|SCT171_2.p                                                   |    0.181s | 20.924MiB| Theorem | 0 |  |  |
|SCT171+3.p                                                   |    0.181s | 21.576MiB| Theorem | 0 |  |  |
|SCT209_5.p                                                   |    0.185s | 29.68MiB| Theorem | 0 |  |  |
|SCT071-1.p                                                   |    0.186s | 31.228MiB| Unsatisfiable | 0 |  |  |
|SCT197_5.p                                                   |    0.203s | 28.576MiB| Theorem | 0 |  |  |
|SCT086-1.p                                                   |    0.205s | 25.576MiB| Unsatisfiable | 0 |  |  |
|SCT171+6.p                                                   |    0.207s | 21.04MiB| Theorem | 0 |  |  |
|SCT149+1.p                                                   |    0.209s | 21.02MiB| Theorem | 0 |  |  |
|SCT085-1.p                                                   |    0.231s | 24.696MiB| Unsatisfiable | 0 |  |  |
|SCT080-1.p                                                   |    0.235s | 25.784MiB| Unsatisfiable | 0 |  |  |
|SCT223_5.p                                                   |    0.236s | 47.2MiB| Theorem | 0 |  |  |
|SCT076-1.p                                                   |    0.237s | 26.556MiB| Unsatisfiable | 0 |  |  |
|SCT203_5.p                                                   |    0.239s | 27.816MiB| Theorem | 0 |  |  |
|SCT027-1.p                                                   |    0.240s | 25.304MiB| Unsatisfiable | 0 |  |  |
|SCT171+7.p                                                   |    0.253s | 21.924MiB| Theorem | 0 |  |  |
|SCT212_5.p                                                   |    0.255s | 35.024MiB| Theorem | 0 |  |  |
|SCT123+1.p                                                   |    0.295s | 44.024MiB| Theorem | 0 |  |  |
|SCT126+1.p                                                   |    0.306s | 41.572MiB| Theorem | 0 |  |  |
|SCT224_5.p                                                   |    0.360s | 67.812MiB| Theorem | 0 |  |  |
|SCT097-1.p                                                   |    0.377s | 34.072MiB| Unsatisfiable | 0 |  |  |
|SCT075-1.p                                                   |    0.528s | 49.544MiB| Unsatisfiable | 0 |  |  |
|SCT205_5.p                                                   |    0.591s | 48.36MiB| Theorem | 0 |  |  |
|SCT152+1.p                                                   |    0.658s | 51.7MiB| Theorem | 0 |  |  |
|SCT151+1.p                                                   |    0.659s | 51.188MiB| Theorem | 0 |  |  |
|SCT185_5.p                                                   |    1.430s | 63.424MiB| Theorem | 0 |  |  |
|SCT184_5.p                                                   |    1.730s | 63.3MiB| Theorem | 0 |  |  |
|SCT026-1.p                                                   |    4.034s | 246.0MiB| Unsatisfiable | 0 |  |  |
|SCT195_5.p                                                   |   20.068s | 311.0MiB| Timeout | 0 |  |  |
|SCT260_5.p                                                   |   20.075s | 417.0MiB| Timeout | 0 |  |  |
|SCT093-1.p                                                   |   20.077s | 167.0MiB| Timeout | 0 |  |  |
|SCT138+1.p                                                   |   20.085s | 164.0MiB| Timeout | 0 |  |  |
|SCT139+1.p                                                   |   20.099s | 112.0MiB| Timeout | 0 |  |  |
|SCT137+1.p                                                   |   20.102s | 103.0MiB| Timeout | 0 |  |  |
|SCT141+1.p                                                   |   20.103s | 161.0MiB| Timeout | 0 |  |  |
|SCT178_5.p                                                   |   20.114s | 161.0MiB| Timeout | 0 |  |  |
|SCT170+7.p                                                   |   20.114s | 76.928MiB| Timeout | 0 |  |  |
|SCT170+6.p                                                   |   20.116s | 74.916MiB| Timeout | 0 |  |  |
|SCT092-1.p                                                   |   20.120s | 181.0MiB| Timeout | 0 |  |  |
|SCT143+1.p                                                   |   20.128s | 85.236MiB| Timeout | 0 |  |  |
|SCT029-1.p                                                   |   20.136s | 122.0MiB| Timeout | 0 |  |  |
|SCT132+1.p                                                   |   20.137s | 163.0MiB| Timeout | 0 |  |  |
|SCT146+1.p                                                   |   20.139s | 160.0MiB| Timeout | 0 |  |  |
|SCT078-1.p                                                   |   20.141s | 302.0MiB| Timeout | 0 |  |  |
|SCT088-1.p                                                   |   20.149s | 313.0MiB| Timeout | 0 |  |  |
|SCT098-1.p                                                   |   20.176s | 216.0MiB| Timeout | 0 |  |  |
|SCT213_5.p                                                   |   20.177s | 343.0MiB| Timeout | 0 |  |  |
|SCT136+1.p                                                   |   20.180s | 117.0MiB| Timeout | 0 |  |  |
|SCT089-1.p                                                   |   20.181s | 340.0MiB| Timeout | 0 |  |  |
|SCT180_5.p                                                   |   20.184s | 129.0MiB| Timeout | 0 |  |  |
|SCT107+1.p                                                   |   20.187s | 227.0MiB| Timeout | 0 |  |  |
|SCT008-1.p                                                   |   20.189s | 447.0MiB| Timeout | 0 |  |  |
|SCT153+1.p                                                   |   20.197s | 299.0MiB| Timeout | 0 |  |  |
|SCT081-1.p                                                   |   20.200s | 102.0MiB| Timeout | 0 |  |  |
|SCT042-1.p                                                   |   20.201s | 277.0MiB| Timeout | 0 |  |  |
|SCT211_5.p                                                   |   20.205s | 158.0MiB| Timeout | 0 |  |  |
|SCT116+1.p                                                   |   20.214s | 156.0MiB| Timeout | 0 |  |  |
|SCT232_5.p                                                   |   20.217s | 305.0MiB| Timeout | 0 |  |  |
|SCT127+1.p                                                   |   20.217s | 112.0MiB| Timeout | 0 |  |  |
|SCT048-1.p                                                   |   20.224s | 294.0MiB| Timeout | 0 |  |  |
|SCT208_5.p                                                   |   20.225s | 151.0MiB| Timeout | 0 |  |  |
|SCT257_5.p                                                   |   20.228s | 411.0MiB| Timeout | 0 |  |  |
|SCT187_5.p                                                   |   20.238s | 361.0MiB| Timeout | 0 |  |  |
|SCT244_5.p                                                   |   20.245s | 176.0MiB| Timeout | 0 |  |  |
|SCT023-1.p                                                   |   20.246s | 409.0MiB| Timeout | 0 |  |  |
|SCT114+1.p                                                   |   20.246s | 156.0MiB| Timeout | 0 |  |  |
|SCT238_5.p                                                   |   20.246s | 237.0MiB| Timeout | 0 |  |  |
|SCT046-1.p                                                   |   20.254s | 268.0MiB| Timeout | 0 |  |  |
|SCT170_3.p                                                   |   20.268s | 285.0MiB| Timeout | 0 |  |  |
|SCT188_5.p                                                   |   20.270s | 234.0MiB| Timeout | 0 |  |  |
|SCT059-1.p                                                   |   20.273s | 218.0MiB| Timeout | 0 |  |  |
|SCT145+1.p                                                   |   20.274s | 349.0MiB| Timeout | 0 |  |  |
|SCT041-1.p                                                   |   20.274s | 273.0MiB| Timeout | 0 |  |  |
|SCT147+1.p                                                   |   20.275s | 228.0MiB| Timeout | 0 |  |  |
|SCT217_5.p                                                   |   20.277s | 273.0MiB| Timeout | 0 |  |  |
|SCT169+7.p                                                   |   20.278s | 272.0MiB| Timeout | 0 |  |  |
|SCT082-1.p                                                   |   20.279s | 205.0MiB| Timeout | 0 |  |  |
|SCT106+1.p                                                   |   20.283s | 283.0MiB| Timeout | 0 |  |  |
|SCT170_1.p                                                   |   20.286s | 436.0MiB| Timeout | 0 |  |  |
|SCT033-1.p                                                   |   20.293s | 382.0MiB| Timeout | 0 |  |  |
|SCT043-1.p                                                   |   20.293s | 411.0MiB| Timeout | 0 |  |  |
|SCT124+1.p                                                   |   20.296s | 196.0MiB| Timeout | 0 |  |  |
|SCT104+1.p                                                   |   20.301s | 239.0MiB| Timeout | 0 |  |  |
|SCT249_5.p                                                   |   20.303s | 248.0MiB| Timeout | 0 |  |  |
|SCT044-1.p                                                   |   20.303s | 414.0MiB| Timeout | 0 |  |  |
|SCT204_5.p                                                   |   20.305s | 504.0MiB| Timeout | 0 |  |  |
|SCT177_5.p                                                   |   20.306s | 268.0MiB| Timeout | 0 |  |  |
|SCT099-1.p                                                   |   20.309s | 238.0MiB| Timeout | 0 |  |  |
|SCT038-1.p                                                   |   20.311s | 362.0MiB| Timeout | 0 |  |  |
|SCT206_5.p                                                   |   20.323s | 191.0MiB| Timeout | 0 |  |  |
|SCT148+1.p                                                   |   20.323s | 832.0MiB| Timeout | 0 |  |  |
|SCT186_5.p                                                   |   20.324s | 238.0MiB| Timeout | 0 |  |  |
|SCT077-1.p                                                   |   20.326s | 267.0MiB| Timeout | 0 |  |  |
|SCT156+1.p                                                   |   20.336s | 639.0MiB| Timeout | 0 |  |  |
|SCT079-1.p                                                   |   20.336s | 281.0MiB| Timeout | 0 |  |  |
|SCT193_5.p                                                   |   20.342s | 458.0MiB| Timeout | 0 |  |  |
|SCT237_5.p                                                   |   20.347s | 305.0MiB| Timeout | 0 |  |  |
|SCT047-1.p                                                   |   20.349s | 260.0MiB| Timeout | 0 |  |  |
|SCT012-1.p                                                   |   20.349s | 473.0MiB| Timeout | 0 |  |  |
|SCT125+1.p                                                   |   20.358s | 250.0MiB| Timeout | 0 |  |  |
|SCT133+1.p                                                   |   20.361s | 395.0MiB| Timeout | 0 |  |  |
|SCT169_20.p                                                  |   20.362s | 335.0MiB| Timeout | 0 |  |  |
|SCT063-1.p                                                   |   20.363s | 696.0MiB| Timeout | 0 |  |  |
|SCT049-1.p                                                   |   20.368s | 280.0MiB| Timeout | 0 |  |  |
|SCT034-1.p                                                   |   20.373s | 501.0MiB| Timeout | 0 |  |  |
|SCT030-1.p                                                   |   20.376s | 554.0MiB| Timeout | 0 |  |  |
|SCT069-1.p                                                   |   20.383s | 430.0MiB| Timeout | 0 |  |  |
|SCT066-1.p                                                   |   20.384s | 340.0MiB| Timeout | 0 |  |  |
|SCT245_5.p                                                   |   20.392s | 274.0MiB| Timeout | 0 |  |  |
|SCT196_5.p                                                   |   20.392s | 481.0MiB| Timeout | 0 |  |  |
|SCT025-1.p                                                   |   20.394s | 329.0MiB| Timeout | 0 |  |  |
|SCT102+1.p                                                   |   20.399s | 240.0MiB| Timeout | 0 |  |  |
|SCT024-1.p                                                   |   20.401s | 533.0MiB| Timeout | 0 |  |  |
|SCT140+1.p                                                   |   20.402s | 505.0MiB| Timeout | 0 |  |  |
|SCT054-1.p                                                   |   20.404s | 952.0MiB| Timeout | 0 |  |  |
|SCT171_10.p                                                  |   20.409s | 421.0MiB| Timeout | 0 |  |  |
|SCT144+1.p                                                   |   20.410s | 388.0MiB| Timeout | 0 |  |  |
|SCT022-1.p                                                   |   20.411s | 402.0MiB| Timeout | 0 |  |  |
|SCT234_5.p                                                   |   20.411s | 307.0MiB| Timeout | 0 |  |  |
|SCT171_30.p                                                  |   20.416s | 588.0MiB| Timeout | 0 |  |  |
|SCT161+1.p                                                   |   20.416s | 368.0MiB| Timeout | 0 |  |  |
|SCT039-1.p                                                   |   20.417s | 413.0MiB| Timeout | 0 |  |  |
|SCT067-1.p                                                   |   20.418s | 427.0MiB| Timeout | 0 |  |  |
|SCT216_5.p                                                   |   20.418s | 549.0MiB| Timeout | 0 |  |  |
|SCT225_5.p                                                   |   20.419s | 432.0MiB| Timeout | 0 |  |  |
|SCT129+1.p                                                   |   20.420s | 462.0MiB| Timeout | 0 |  |  |
|SCT068-1.p                                                   |   20.426s | 434.0MiB| Timeout | 0 |  |  |
|SCT256_5.p                                                   |   20.429s | 574.0MiB| Timeout | 0 |  |  |
|SCT262_5.p                                                   |   20.429s | 535.0MiB| Timeout | 0 |  |  |
|SCT182_5.p                                                   |   20.430s | 702.0MiB| Timeout | 0 |  |  |
|SCT239_5.p                                                   |   20.433s | 352.0MiB| Timeout | 0 |  |  |
|SCT191_5.p                                                   |   20.437s | 339.0MiB| Timeout | 0 |  |  |
|SCT135+1.p                                                   |   20.440s | 417.0MiB| Timeout | 0 |  |  |
|SCT084-1.p                                                   |   20.441s | 372.0MiB| Timeout | 0 |  |  |
|SCT020-1.p                                                   |   20.444s | 523.0MiB| Timeout | 0 |  |  |
|SCT021-1.p                                                   |   20.449s | 383.0MiB| Timeout | 0 |  |  |
|SCT032-1.p                                                   |   20.455s | 510.0MiB| Timeout | 0 |  |  |
|SCT017-1.p                                                   |   20.459s | 483.0MiB| Timeout | 0 |  |  |
|SCT130+1.p                                                   |   20.463s | 373.0MiB| Timeout | 0 |  |  |
|SCT001-1.p                                                   |   20.464s | 444.0MiB| Timeout | 0 |  |  |
|SCT110+1.p                                                   |   20.465s | 450.0MiB| Timeout | 0 |  |  |
|SCT251_5.p                                                   |   20.475s | 442.0MiB| Timeout | 0 |  |  |
|SCT172_5.p                                                   |   20.476s | 343.0MiB| Timeout | 0 |  |  |
|SCT210_5.p                                                   |   20.477s | 421.0MiB| Timeout | 0 |  |  |
|SCT108+1.p                                                   |   20.478s | 395.0MiB| Timeout | 0 |  |  |
|SCT170^3.p                                                   |   20.479s | 345.0MiB| Timeout | 0 |  |  |
|SCT201_5.p                                                   |   20.483s | 547.0MiB| Timeout | 0 |  |  |
|SCT103+1.p                                                   |   20.484s | 420.0MiB| Timeout | 0 |  |  |
|SCT258_5.p                                                   |   20.491s | 489.0MiB| Timeout | 0 |  |  |
|SCT202_5.p                                                   |   20.492s | 382.0MiB| Timeout | 0 |  |  |
|SCT119+1.p                                                   |   20.493s | 932.0MiB| Timeout | 0 |  |  |
|SCT090-1.p                                                   |   20.494s | 498.0MiB| Timeout | 0 |  |  |
|SCT265_5.p                                                   |   20.496s | 474.0MiB| Timeout | 0 |  |  |
|SCT070-1.p                                                   |   20.497s | 400.0MiB| Timeout | 0 |  |  |
|SCT045-1.p                                                   |   20.498s | 431.0MiB| Timeout | 0 |  |  |
|SCT004-1.p                                                   |   20.502s | 664.0MiB| Timeout | 0 |  |  |
|SCT171_20.p                                                  |   20.505s | 891.0MiB| Timeout | 0 |  |  |
|SCT169_10.p                                                  |   20.509s | 990.0MiB| Timeout | 0 |  |  |
|SCT259_5.p                                                   |   20.512s | 443.0MiB| Timeout | 0 |  |  |
|SCT053-1.p                                                   |   20.518s | 395.0MiB| Timeout | 0 |  |  |
|SCT058-1.p                                                   |   20.519s | 405.0MiB| Timeout | 0 |  |  |
|SCT263_5.p                                                   |   20.521s | 426.0MiB| Timeout | 0 |  |  |
|SCT035-1.p                                                   |   20.522s | 570.0MiB| Timeout | 0 |  |  |
|SCT215_5.p                                                   |   20.524s | 838.0MiB| Timeout | 0 |  |  |
|SCT162+1.p                                                   |   20.524s | 696.0MiB| Timeout | 0 |  |  |
|SCT254_5.p                                                   |   20.527s | 619.0MiB| Timeout | 0 |  |  |
|SCT037-1.p                                                   |   20.531s | 591.0MiB| Timeout | 0 |  |  |
|SCT091-1.p                                                   |   20.535s | 572.0MiB| Timeout | 0 |  |  |
|SCT163+1.p                                                   |   20.540s | 683.0MiB| Timeout | 0 |  |  |
|SCT192_5.p                                                   |   20.540s | 452.0MiB| Timeout | 0 |  |  |
|SCT154+1.p                                                   |   20.545s | 555.0MiB| Timeout | 0 |  |  |
|SCT003-1.p                                                   |   20.548s | 453.0MiB| Timeout | 0 |  |  |
|SCT014-1.p                                                   |   20.550s | 491.0MiB| Timeout | 0 |  |  |
|SCT220_5.p                                                   |   20.552s | 601.0MiB| Timeout | 0 |  |  |
|SCT096-1.p                                                   |   20.552s | 648.0MiB| Timeout | 0 |  |  |
|SCT169_3.p                                                   |   20.562s | 1284.0MiB| Timeout | 0 |  |  |
|SCT010-1.p                                                   |   20.569s | 595.0MiB| Timeout | 0 |  |  |
|SCT015-1.p                                                   |   20.570s | 527.0MiB| Timeout | 0 |  |  |
|SCT128+1.p                                                   |   20.571s | 830.0MiB| Timeout | 0 |  |  |
|SCT122+1.p                                                   |   20.577s | 584.0MiB| Timeout | 0 |  |  |
|SCT019-1.p                                                   |   20.579s | 620.0MiB| Timeout | 0 |  |  |
|SCT065-1.p                                                   |   20.585s | 468.0MiB| Timeout | 0 |  |  |
|SCT111+1.p                                                   |   20.587s | 681.0MiB| Timeout | 0 |  |  |
|SCT118+1.p                                                   |   20.587s | 442.0MiB| Timeout | 0 |  |  |
|SCT120+1.p                                                   |   20.589s | 529.0MiB| Timeout | 0 |  |  |
|SCT170+2.p                                                   |   20.590s | 1280.0MiB| Timeout | 0 |  |  |
|SCT142+1.p                                                   |   20.590s | 828.0MiB| Timeout | 0 |  |  |
|SCT036-1.p                                                   |   20.591s | 596.0MiB| Timeout | 0 |  |  |
|SCT198_5.p                                                   |   20.593s | 1641.0MiB| Timeout | 0 |  |  |
|SCT194_5.p                                                   |   20.595s | 460.0MiB| Timeout | 0 |  |  |
|SCT011-1.p                                                   |   20.608s | 548.0MiB| Timeout | 0 |  |  |
|SCT051-1.p                                                   |   20.609s | 467.0MiB| Timeout | 0 |  |  |
|SCT087-1.p                                                   |   20.610s | 579.0MiB| Timeout | 0 |  |  |
|SCT170_20.p                                                  |   20.613s | 1358.0MiB| Timeout | 0 |  |  |
|SCT016-1.p                                                   |   20.618s | 508.0MiB| Timeout | 0 |  |  |
|SCT050-1.p                                                   |   20.618s | 913.0MiB| Timeout | 0 |  |  |
|SCT121+1.p                                                   |   20.621s | 587.0MiB| Timeout | 0 |  |  |
|SCT169+2.p                                                   |   20.623s | 583.0MiB| Timeout | 0 |  |  |
|SCT150+1.p                                                   |   20.628s | 772.0MiB| Timeout | 0 |  |  |
|SCT160+1.p                                                   |   20.629s | 609.0MiB| Timeout | 0 |  |  |
|SCT095-1.p                                                   |   20.630s | 671.0MiB| Timeout | 0 |  |  |
|SCT170+1.p                                                   |   20.633s | 1060.0MiB| Timeout | 0 |  |  |
|SCT221_5.p                                                   |   20.636s | 572.0MiB| Timeout | 0 |  |  |
|SCT169_1.p                                                   |   20.639s | 742.0MiB| Timeout | 0 |  |  |
|SCT169^2.p                                                   |   20.640s | 541.0MiB| Timeout | 0 |  |  |
|SCT169+6.p                                                   |   20.641s | 1085.0MiB| Timeout | 0 |  |  |
|SCT181_5.p                                                   |   20.644s | 630.0MiB| Timeout | 0 |  |  |
|SCT169_2.p                                                   |   20.646s | 562.0MiB| Timeout | 0 |  |  |
|SCT013-1.p                                                   |   20.650s | 825.0MiB| Timeout | 0 |  |  |
|SCT169^3.p                                                   |   20.660s | 525.0MiB| Timeout | 0 |  |  |
|SCT170+5.p                                                   |   20.663s | 641.0MiB| Timeout | 0 |  |  |
|SCT170_5.p                                                   |   20.671s | 969.0MiB| Timeout | 0 |  |  |
|SCT159+1.p                                                   |   20.671s | 850.0MiB| Timeout | 0 |  |  |
|SCT190_5.p                                                   |   20.673s | 885.0MiB| Timeout | 0 |  |  |
|SCT169+5.p                                                   |   20.673s | 1043.0MiB| Timeout | 0 |  |  |
|SCT009-1.p                                                   |   20.682s | 894.0MiB| Timeout | 0 |  |  |
|SCT170_30.p                                                  |   20.684s | 1208.0MiB| Timeout | 0 |  |  |
|SCT006-1.p                                                   |   20.686s | 976.0MiB| Timeout | 0 |  |  |
|SCT134+1.p                                                   |   20.694s | 634.0MiB| Timeout | 0 |  |  |
|SCT183_5.p                                                   |   20.694s | 683.0MiB| Timeout | 0 |  |  |
|SCT170+3.p                                                   |   20.706s | 1208.0MiB| Timeout | 0 |  |  |
|SCT018-1.p                                                   |   20.713s | 1224.0MiB| Timeout | 0 |  |  |
|SCT005-1.p                                                   |   20.722s | 592.0MiB| Timeout | 0 |  |  |
|SCT200_5.p                                                   |   20.724s | 1150.0MiB| Timeout | 0 |  |  |
|SCT170^1.p                                                   |   20.730s | 1405.0MiB| Timeout | 0 |  |  |
|SCT219_5.p                                                   |   20.736s | 763.0MiB| Timeout | 0 |  |  |
|SCT170^2.p                                                   |   20.761s | 763.0MiB| Timeout | 0 |  |  |
|SCT230_5.p                                                   |   20.762s | 1251.0MiB| Timeout | 0 |  |  |
|SCT189_5.p                                                   |   20.765s | 1623.0MiB| Timeout | 0 |  |  |
|SCT164+1.p                                                   |   20.780s | 973.0MiB| Timeout | 0 |  |  |
|SCT167+1.p                                                   |   20.786s | 997.0MiB| Timeout | 0 |  |  |
|SCT169^1.p                                                   |   20.789s | 1825.0MiB| Timeout | 0 |  |  |
|SCT131+1.p                                                   |   20.800s | 947.0MiB| Timeout | 0 |  |  |
|SCT218_5.p                                                   |   20.801s | 1791.0MiB| Timeout | 0 |  |  |
|SCT056-1.p                                                   |   20.837s | 726.0MiB| Timeout | 0 |  |  |
|SCT169+3.p                                                   |   20.845s | 1006.0MiB| Timeout | 0 |  |  |
|SCT028-1.p                                                   |   20.865s | 932.0MiB| Timeout | 0 |  |  |
|SCT040-1.p                                                   |   20.878s | 677.0MiB| Timeout | 0 |  |  |
|SCT117+1.p                                                   |   20.883s | 985.0MiB| Timeout | 0 |  |  |
|SCT170_2.p                                                   |   20.890s | 1308.0MiB| Timeout | 0 |  |  |
|SCT168+1.p                                                   |   20.893s | 974.0MiB| Timeout | 0 |  |  |
|SCT169_5.p                                                   |   20.928s | 1306.0MiB| Timeout | 0 |  |  |
|SCT062-1.p                                                   |   20.933s | 907.0MiB| Timeout | 0 |  |  |
|SCT109+1.p                                                   |   20.937s | 1031.0MiB| Timeout | 0 |  |  |
|SCT055-1.p                                                   |   20.939s | 964.0MiB| Timeout | 0 |  |  |
|SCT060-1.p                                                   |   20.953s | 1320.0MiB| Timeout | 0 |  |  |
|SCT115+1.p                                                   |   20.966s | 1494.0MiB| Timeout | 0 |  |  |
|SCT165+1.p                                                   |   20.986s | 972.0MiB| Timeout | 0 |  |  |
|SCT166+1.p                                                   |   20.987s | 975.0MiB| Timeout | 0 |  |  |
|SCT057-1.p                                                   |   20.997s | 962.0MiB| Timeout | 0 |  |  |
|SCT169+1.p                                                   |   21.000s | 918.0MiB| Timeout | 0 |  |  |
|SCT112+1.p                                                   |   21.046s | 1384.0MiB| Timeout | 0 |  |  |
|SCT113+1.p                                                   |   21.086s | 1384.0MiB| Timeout | 0 |  |  |
|SCT169_30.p                                                  |   21.116s | 1686.0MiB| Timeout | 0 |  |  |
|SCT228_5.p                                                   |   21.138s | 1705.0MiB| Timeout | 0 |  |  |
|SCT173_5.p                                                   |   21.147s | 1530.0MiB| Timeout | 0 |  |  |
|SCT227_5.p                                                   |   21.189s | 1892.0MiB| Timeout | 0 |  |  |
|SCT170_10.p                                                  |   21.193s | 1950.0MiB| Timeout | 0 |  |  |
|SCT248_5.p                                                   |   21.258s | 2371.0MiB| Timeout | 0 |  |  |
