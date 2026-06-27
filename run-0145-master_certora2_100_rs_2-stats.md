# .

* SAT 89
* UNSAT 86
* TIMEOUT 16
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: check against certora
Job tag: master_certora2_100_rs_2
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 75981a5d3b3c216142cd69b8c4d4e0a15ecf2e72
Z3 branch: master
Z3 options: "-T:100 smt.random_seed=2"
Z3 inputs: inputs/certora2
Z3 commit message: Remove leaked `check-assignment` output from debug GCC CMake runs (#9978)

The `Ubuntu build - cmake - debugGcc` job was failing because the solver
could emit an unexpected `check-assignment` line before normal
satisfiability output. This change removes that stray output so debug
GCC runs no longer contaminate expected CLI/results streams.

- **Root cause**
- `src/math/lp/nra_solver.cpp` printed `check-assignment` from
`solver::check_assignment()` via `IF_VERBOSE(0, ...)`.
- Verbosity level `0` made this effectively unconditional in the failing
path, so debug builds could leak internal diagnostics into user-visible
output.

- **Change**
- Remove the `check-assignment` print from the exception path in
`lp::solver::check_assignment()`.
- Preserve all existing control flow and error handling; only the
unintended output side effect is removed.

- **Effect**
  - Debug GCC CMake builds keep their normal `sat`/`unsat` output shape.
- Internal solver diagnostics no longer interfere with output-sensitive
CI checks.

```c++
catch (z3_exception &) {
    statistics &st = m_imp->m_nla_core.lp_settings().stats().m_st;
    m_imp->m_nlsat->collect_statistics(st);
    if (m_imp->m_limit.is_canceled()) {
        return l_undef;
    }
    else {
        throw;
    }
}
```

---------

Co-authored-by: copilot-swe-agent[bot] <198982749+Copilot@users.noreply.github.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|auk-0066.smt2                                                |    0.027s | 20.352MiB| sat | 0 |  |  |
|auk-0046.smt2                                                |    0.034s | 20.58MiB| unsat | 0 |  |  |
|auk-0160.smt2                                                |    0.040s | 21.136MiB| unsat | 0 |  |  |
|auk-0054.smt2                                                |    0.045s | 20.032MiB| unsat | 0 |  |  |
|auk-0041.smt2                                                |    0.055s | 21.132MiB| unsat | 0 |  |  |
|auk-0060.smt2                                                |    0.057s | 21.144MiB| unsat | 0 |  |  |
|auk-0168.smt2                                                |    0.058s | 21.12MiB| sat | 0 |  |  |
|auk-0161.smt2                                                |    0.059s | 20.488MiB| sat | 0 |  |  |
|auk-0031.smt2                                                |    0.059s | 20.924MiB| unsat | 0 |  |  |
|auk-0065.smt2                                                |    0.061s | 21.476MiB| unsat | 0 |  |  |
|auk-0067.smt2                                                |    0.063s | 21.12MiB| sat | 0 |  |  |
|auk-0170.smt2                                                |    0.064s | 21.684MiB| unsat | 0 |  |  |
|auk-0038.smt2                                                |    0.065s | 19.616MiB| unsat | 0 |  |  |
|auk-0069.smt2                                                |    0.065s | 21.12MiB| unsat | 0 |  |  |
|auk-0138.smt2                                                |    0.065s | 21.14MiB| unsat | 0 |  |  |
|auk-0178.smt2                                                |    0.065s | 20.596MiB| sat | 0 |  |  |
|auk-0159.smt2                                                |    0.066s | 21.496MiB| unsat | 0 |  |  |
|auk-0042.smt2                                                |    0.066s | 21.068MiB| sat | 0 |  |  |
|auk-0063.smt2                                                |    0.066s | 21.464MiB| unsat | 0 |  |  |
|auk-0097.smt2                                                |    0.072s | 23.596MiB| sat | 0 |  |  |
|auk-0179.smt2                                                |    0.074s | 20.956MiB| unsat | 0 |  |  |
|auk-0057.smt2                                                |    0.082s | 20.56MiB| unsat | 0 |  |  |
|auk-0040.smt2                                                |    0.082s | 21.188MiB| unsat | 0 |  |  |
|auk-0166.smt2                                                |    0.084s | 21.776MiB| sat | 0 |  |  |
|auk-0050.smt2                                                |    0.085s | 21.244MiB| unsat | 0 |  |  |
|auk-0044.smt2                                                |    0.087s | 21.544MiB| unsat | 0 |  |  |
|auk-0059.smt2                                                |    0.088s | 23.732MiB| unsat | 0 |  |  |
|auk-0093.smt2                                                |    0.088s | 25.084MiB| sat | 0 |  |  |
|auk-0061.smt2                                                |    0.091s | 21.484MiB| unsat | 0 |  |  |
|auk-0028.smt2                                                |    0.094s | 23.264MiB| unsat | 0 |  |  |
|auk-0009.smt2                                                |    0.094s | 21.484MiB| sat | 0 |  |  |
|auk-0068.smt2                                                |    0.101s | 20.416MiB| unsat | 0 |  |  |
|auk-0039.smt2                                                |    0.104s | 21.468MiB| unsat | 0 |  |  |
|auk-0052.smt2                                                |    0.104s | 25.792MiB| unsat | 0 |  |  |
|auk-0032.smt2                                                |    0.104s | 21.22MiB| unsat | 0 |  |  |
|auk-0055.smt2                                                |    0.107s | 25.564MiB| sat | 0 |  |  |
|auk-0064.smt2                                                |    0.111s | 21.64MiB| sat | 0 |  |  |
|auk-0048.smt2                                                |    0.115s | 26.152MiB| unsat | 0 |  |  |
|auk-0104.smt2                                                |    0.117s | 24.736MiB| sat | 0 |  |  |
|auk-0035.smt2                                                |    0.119s | 20.736MiB| unsat | 0 |  |  |
|auk-0162.smt2                                                |    0.120s | 25.176MiB| sat | 0 |  |  |
|auk-0100.smt2                                                |    0.120s | 25.416MiB| sat | 0 |  |  |
|auk-0026.smt2                                                |    0.121s | 25.84MiB| unsat | 0 |  |  |
|auk-0157.smt2                                                |    0.123s | 25.812MiB| sat | 0 |  |  |
|auk-0012.smt2                                                |    0.125s | 21.4MiB| unsat | 0 |  |  |
|auk-0036.smt2                                                |    0.125s | 21.784MiB| sat | 0 |  |  |
|auk-0165.smt2                                                |    0.125s | 26.044MiB| sat | 0 |  |  |
|auk-0003.smt2                                                |    0.125s | 28.868MiB| unsat | 0 |  |  |
|auk-0058.smt2                                                |    0.127s | 25.484MiB| sat | 0 |  |  |
|auk-0114.smt2                                                |    0.127s | 26.5MiB| sat | 0 |  |  |
|auk-0158.smt2                                                |    0.128s | 25.736MiB| sat | 0 |  |  |
|auk-0051.smt2                                                |    0.128s | 21.696MiB| unsat | 0 |  |  |
|auk-0129.smt2                                                |    0.128s | 25.748MiB| sat | 0 |  |  |
|auk-0043.smt2                                                |    0.130s | 25.116MiB| unsat | 0 |  |  |
|auk-0107.smt2                                                |    0.130s | 27.4MiB| sat | 0 |  |  |
|auk-0024.smt2                                                |    0.133s | 26.524MiB| unsat | 0 |  |  |
|auk-0121.smt2                                                |    0.136s | 27.284MiB| sat | 0 |  |  |
|auk-0033.smt2                                                |    0.137s | 27.056MiB| sat | 0 |  |  |
|auk-0175.smt2                                                |    0.137s | 27.14MiB| sat | 0 |  |  |
|auk-0021.smt2                                                |    0.138s | 24.376MiB| unsat | 0 |  |  |
|auk-0174.smt2                                                |    0.139s | 26.308MiB| sat | 0 |  |  |
|auk-0019.smt2                                                |    0.140s | 26.784MiB| unsat | 0 |  |  |
|auk-0143.smt2                                                |    0.143s | 21.784MiB| unsat | 0 |  |  |
|auk-0106.smt2                                                |    0.143s | 27.42MiB| sat | 0 |  |  |
|auk-0034.smt2                                                |    0.143s | 21.536MiB| unsat | 0 |  |  |
|auk-0110.smt2                                                |    0.144s | 25.688MiB| sat | 0 |  |  |
|auk-0037.smt2                                                |    0.145s | 26.84MiB| unsat | 0 |  |  |
|auk-0173.smt2                                                |    0.147s | 26.5MiB| sat | 0 |  |  |
|auk-0127.smt2                                                |    0.149s | 27.356MiB| sat | 0 |  |  |
|auk-0013.smt2                                                |    0.151s | 28.884MiB| unsat | 0 |  |  |
|auk-0163.smt2                                                |    0.151s | 26.872MiB| sat | 0 |  |  |
|auk-0171.smt2                                                |    0.153s | 26.448MiB| sat | 0 |  |  |
|auk-0018.smt2                                                |    0.156s | 26.792MiB| unsat | 0 |  |  |
|auk-0108.smt2                                                |    0.159s | 29.116MiB| sat | 0 |  |  |
|auk-0029.smt2                                                |    0.160s | 27.132MiB| unsat | 0 |  |  |
|auk-0045.smt2                                                |    0.161s | 26.148MiB| unsat | 0 |  |  |
|auk-0167.smt2                                                |    0.163s | 25.884MiB| sat | 0 |  |  |
|auk-0176.smt2                                                |    0.163s | 27.176MiB| sat | 0 |  |  |
|auk-0047.smt2                                                |    0.164s | 26.12MiB| unsat | 0 |  |  |
|auk-0125.smt2                                                |    0.164s | 27.94MiB| sat | 0 |  |  |
|auk-0177.smt2                                                |    0.164s | 27.908MiB| sat | 0 |  |  |
|auk-0172.smt2                                                |    0.165s | 26.448MiB| sat | 0 |  |  |
|auk-0105.smt2                                                |    0.166s | 27.676MiB| sat | 0 |  |  |
|auk-0136.smt2                                                |    0.166s | 21.652MiB| sat | 0 |  |  |
|auk-0112.smt2                                                |    0.167s | 27.8MiB| sat | 0 |  |  |
|auk-0118.smt2                                                |    0.167s | 27.656MiB| sat | 0 |  |  |
|auk-0030.smt2                                                |    0.172s | 26.956MiB| unsat | 0 |  |  |
|auk-0062.smt2                                                |    0.173s | 25.544MiB| unsat | 0 |  |  |
|auk-0124.smt2                                                |    0.175s | 27.684MiB| sat | 0 |  |  |
|auk-0154.smt2                                                |    0.178s | 26.908MiB| sat | 0 |  |  |
|auk-0102.smt2                                                |    0.184s | 27.98MiB| sat | 0 |  |  |
|auk-0169.smt2                                                |    0.186s | 27.504MiB| unsat | 0 |  |  |
|auk-0008.smt2                                                |    0.188s | 28.852MiB| unsat | 0 |  |  |
|auk-0014.smt2                                                |    0.189s | 28.992MiB| unsat | 0 |  |  |
|auk-0016.smt2                                                |    0.190s | 26.088MiB| unsat | 0 |  |  |
|auk-0155.smt2                                                |    0.191s | 25.916MiB| sat | 0 |  |  |
|auk-0111.smt2                                                |    0.194s | 28.94MiB| sat | 0 |  |  |
|auk-0152.smt2                                                |    0.196s | 25.612MiB| sat | 0 |  |  |
|auk-0156.smt2                                                |    0.201s | 25.964MiB| sat | 0 |  |  |
|auk-0164.smt2                                                |    0.206s | 26.456MiB| sat | 0 |  |  |
|auk-0020.smt2                                                |    0.211s | 26.464MiB| unsat | 0 |  |  |
|auk-0103.smt2                                                |    0.213s | 29.088MiB| sat | 0 |  |  |
|auk-0017.smt2                                                |    0.214s | 27.936MiB| unsat | 0 |  |  |
|auk-0049.smt2                                                |    0.220s | 22.264MiB| unsat | 0 |  |  |
|auk-0126.smt2                                                |    0.230s | 28.96MiB| sat | 0 |  |  |
|auk-0113.smt2                                                |    0.230s | 29.86MiB| sat | 0 |  |  |
|auk-0153.smt2                                                |    0.249s | 27.672MiB| sat | 0 |  |  |
|auk-0139.smt2                                                |    0.264s | 22.928MiB| unsat | 0 |  |  |
|auk-0122.smt2                                                |    0.276s | 31.604MiB| sat | 0 |  |  |
|auk-0123.smt2                                                |    0.283s | 32.116MiB| sat | 0 |  |  |
|auk-0130.smt2                                                |    0.284s | 33.836MiB| sat | 0 |  |  |
|auk-0190.smt2                                                |    0.292s | 29.7MiB| unsat | 0 |  |  |
|auk-0128.smt2                                                |    0.307s | 33.748MiB| sat | 0 |  |  |
|auk-0023.smt2                                                |    0.311s | 29.672MiB| unsat | 0 |  |  |
|auk-0150.smt2                                                |    0.320s | 27.948MiB| unsat | 0 |  |  |
|auk-0070.smt2                                                |    0.321s | 32.048MiB| sat | 0 |  |  |
|auk-0119.smt2                                                |    0.327s | 34.912MiB| sat | 0 |  |  |
|auk-0002.smt2                                                |    0.327s | 35.98MiB| unsat | 0 |  |  |
|auk-0115.smt2                                                |    0.331s | 36.116MiB| sat | 0 |  |  |
|auk-0116.smt2                                                |    0.336s | 31.672MiB| sat | 0 |  |  |
|auk-0117.smt2                                                |    0.350s | 31.696MiB| sat | 0 |  |  |
|auk-0180.smt2                                                |    0.364s | 30.28MiB| unsat | 0 |  |  |
|auk-0056.smt2                                                |    0.440s | 27.844MiB| sat | 0 |  |  |
|auk-0135.smt2                                                |    0.523s | 29.42MiB| unsat | 0 |  |  |
|auk-0134.smt2                                                |    0.546s | 33.564MiB| sat | 0 |  |  |
|auk-0010.smt2                                                |    0.605s | 37.988MiB| unsat | 0 |  |  |
|auk-0096.smt2                                                |    0.635s | 29.952MiB| sat | 0 |  |  |
|auk-0072.smt2                                                |    0.666s | 32.896MiB| sat | 0 |  |  |
|auk-0078.smt2                                                |    0.675s | 32.9MiB| sat | 0 |  |  |
|auk-0120.smt2                                                |    0.685s | 32.42MiB| sat | 0 |  |  |
|auk-0079.smt2                                                |    0.741s | 42.076MiB| sat | 0 |  |  |
|auk-0091.smt2                                                |    0.763s | 43.424MiB| sat | 0 |  |  |
|auk-0004.smt2                                                |    0.814s | 37.088MiB| unsat | 0 |  |  |
|auk-0151.smt2                                                |    0.855s | 22.948MiB| unsat | 0 |  |  |
|auk-0109.smt2                                                |    0.936s | 95.976MiB| sat | 0 |  |  |
|auk-0133.smt2                                                |    0.944s | 43.448MiB| sat | 0 |  |  |
|auk-0053.smt2                                                |    0.944s | 23.372MiB| sat | 0 |  |  |
|auk-0077.smt2                                                |    0.952s | 41.552MiB| sat | 0 |  |  |
|auk-0145.smt2                                                |    1.024s | 31.792MiB| unsat | 0 |  |  |
|auk-0006.smt2                                                |    1.214s | 38.944MiB| unsat | 0 |  |  |
|auk-0075.smt2                                                |    1.368s | 47.988MiB| sat | 0 |  |  |
|auk-0071.smt2                                                |    1.382s | 48.108MiB| sat | 0 |  |  |
|auk-0073.smt2                                                |    1.393s | 47.924MiB| sat | 0 |  |  |
|auk-0025.smt2                                                |    1.415s | 33.112MiB| unsat | 0 |  |  |
|auk-0076.smt2                                                |    1.421s | 48.204MiB| sat | 0 |  |  |
|auk-0149.smt2                                                |    1.427s | 29.592MiB| unsat | 0 |  |  |
|auk-0132.smt2                                                |    1.653s | 59.764MiB| sat | 0 |  |  |
|auk-0146.smt2                                                |    1.828s | 30.144MiB| unsat | 0 |  |  |
|auk-0094.smt2                                                |    1.862s | 32.732MiB| sat | 0 |  |  |
|auk-0080.smt2                                                |    1.923s | 33.928MiB| sat | 0 |  |  |
|auk-0011.smt2                                                |    2.061s | 39.692MiB| unsat | 0 |  |  |
|auk-0131.smt2                                                |    2.085s | 96.488MiB| sat | 0 |  |  |
|auk-0022.smt2                                                |    2.327s | 32.132MiB| sat | 0 |  |  |
|auk-0101.smt2                                                |    2.415s | 33.936MiB| unsat | 0 |  |  |
|auk-0083.smt2                                                |    3.424s | 100.0MiB| sat | 0 |  |  |
|auk-0082.smt2                                                |    3.477s | 100.0MiB| sat | 0 |  |  |
|auk-0090.smt2                                                |    3.799s | 53.596MiB| unsat | 0 |  |  |
|auk-0148.smt2                                                |    3.958s | 36.148MiB| unsat | 0 |  |  |
|auk-0144.smt2                                                |    4.372s | 35.36MiB| unsat | 0 |  |  |
|auk-0142.smt2                                                |    4.791s | 35.812MiB| unsat | 0 |  |  |
|auk-0095.smt2                                                |    5.218s | 41.324MiB| unsat | 0 |  |  |
|auk-0182.smt2                                                |    6.494s | 63.152MiB| unsat | 0 |  |  |
|auk-0185.smt2                                                |    7.680s | 46.564MiB| unsat | 0 |  |  |
|auk-0027.smt2                                                |    7.684s | 30.844MiB| unsat | 0 |  |  |
|auk-0087.smt2                                                |   10.165s | 58.968MiB| unsat | 0 |  |  |
|auk-0141.smt2                                                |   12.417s | 44.98MiB| unsat | 0 |  |  |
|auk-0001.smt2                                                |   14.770s | 23.94MiB| sat | 0 |  |  |
|auk-0084.smt2                                                |   15.438s | 58.136MiB| unsat | 0 |  |  |
|auk-0007.smt2                                                |   22.462s | 45.328MiB| sat | 0 |  |  |
|auk-0098.smt2                                                |   26.273s | 183.0MiB| unsat | 0 |  |  |
|auk-0099.smt2                                                |   26.772s | 211.0MiB| unsat | 0 |  |  |
|auk-0140.smt2                                                |   37.398s | 44.012MiB| unsat | 0 |  |  |
|auk-0074.smt2                                                |   49.083s | 32.336MiB| unsat | 0 |  |  |
|auk-0089.smt2                                                |   63.436s | 73.848MiB| unsat | 0 |  |  |
|auk-0183.smt2                                                |   63.555s | 88.208MiB| unsat | 0 |  |  |
|auk-0137.smt2                                                |  100.015s | 52.616MiB| timeout | 0 |  |  |
|auk-0186.smt2                                                |  100.018s | 69.52MiB| timeout | 0 |  |  |
|auk-0088.smt2                                                |  100.023s | 105.0MiB| timeout | 0 |  |  |
|auk-0081.smt2                                                |  100.029s | 35.304MiB| timeout | 0 |  |  |
|auk-0184.smt2                                                |  100.029s | 49.724MiB| timeout | 0 |  |  |
|auk-0188.smt2                                                |  100.030s | 61.716MiB| timeout | 0 |  |  |
|auk-0085.smt2                                                |  100.035s | 100.0MiB| timeout | 0 |  |  |
|auk-0147.smt2                                                |  100.042s | 52.656MiB| timeout | 0 |  |  |
|auk-0005.smt2                                                |  100.048s | 50.804MiB| timeout | 0 |  |  |
|auk-0086.smt2                                                |  100.049s | 111.0MiB| timeout | 0 |  |  |
|auk-0187.smt2                                                |  100.050s | 69.912MiB| timeout | 0 |  |  |
|auk-0181.smt2                                                |  100.050s | 75.464MiB| timeout | 0 |  |  |
|auk-0015.smt2                                                |  100.051s | 56.576MiB| timeout | 0 |  |  |
|auk-0189.smt2                                                |  100.052s | 68.868MiB| timeout | 0 |  |  |
|auk-0191.smt2                                                |  100.053s | 40.396MiB| timeout | 0 |  |  |
|auk-0092.smt2                                                |  100.054s | 145.0MiB| timeout | 0 |  |  |
