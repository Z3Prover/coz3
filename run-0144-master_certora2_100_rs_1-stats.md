# .

* SAT 89
* UNSAT 85
* TIMEOUT 17
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: check against certora
Job tag: master_certora2_100_rs_1
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 75981a5d3b3c216142cd69b8c4d4e0a15ecf2e72
Z3 branch: master
Z3 options: "-T:100 smt.random_seed=1"
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
|auk-0068.smt2                                                |    0.027s | 20.356MiB| unsat | 0 |  |  |
|auk-0178.smt2                                                |    0.030s | 20.572MiB| sat | 0 |  |  |
|auk-0161.smt2                                                |    0.045s | 20.676MiB| sat | 0 |  |  |
|auk-0035.smt2                                                |    0.053s | 20.848MiB| unsat | 0 |  |  |
|auk-0066.smt2                                                |    0.055s | 20.432MiB| sat | 0 |  |  |
|auk-0046.smt2                                                |    0.060s | 20.396MiB| unsat | 0 |  |  |
|auk-0038.smt2                                                |    0.061s | 20.376MiB| unsat | 0 |  |  |
|auk-0054.smt2                                                |    0.067s | 19.844MiB| unsat | 0 |  |  |
|auk-0060.smt2                                                |    0.067s | 21.116MiB| unsat | 0 |  |  |
|auk-0067.smt2                                                |    0.067s | 20.976MiB| sat | 0 |  |  |
|auk-0057.smt2                                                |    0.069s | 20.588MiB| unsat | 0 |  |  |
|auk-0168.smt2                                                |    0.075s | 20.896MiB| sat | 0 |  |  |
|auk-0069.smt2                                                |    0.077s | 21.084MiB| unsat | 0 |  |  |
|auk-0179.smt2                                                |    0.078s | 21.128MiB| unsat | 0 |  |  |
|auk-0028.smt2                                                |    0.084s | 23.24MiB| unsat | 0 |  |  |
|auk-0041.smt2                                                |    0.085s | 21.112MiB| unsat | 0 |  |  |
|auk-0040.smt2                                                |    0.085s | 20.996MiB| unsat | 0 |  |  |
|auk-0031.smt2                                                |    0.085s | 20.924MiB| unsat | 0 |  |  |
|auk-0160.smt2                                                |    0.086s | 20.976MiB| unsat | 0 |  |  |
|auk-0042.smt2                                                |    0.091s | 20.82MiB| sat | 0 |  |  |
|auk-0063.smt2                                                |    0.092s | 21.492MiB| unsat | 0 |  |  |
|auk-0050.smt2                                                |    0.093s | 21.176MiB| unsat | 0 |  |  |
|auk-0138.smt2                                                |    0.093s | 21.624MiB| unsat | 0 |  |  |
|auk-0093.smt2                                                |    0.094s | 24.452MiB| sat | 0 |  |  |
|auk-0043.smt2                                                |    0.095s | 25.124MiB| unsat | 0 |  |  |
|auk-0065.smt2                                                |    0.095s | 21.116MiB| unsat | 0 |  |  |
|auk-0032.smt2                                                |    0.097s | 21.096MiB| unsat | 0 |  |  |
|auk-0170.smt2                                                |    0.098s | 21.624MiB| unsat | 0 |  |  |
|auk-0039.smt2                                                |    0.103s | 21.388MiB| unsat | 0 |  |  |
|auk-0159.smt2                                                |    0.103s | 21.588MiB| unsat | 0 |  |  |
|auk-0044.smt2                                                |    0.103s | 21.548MiB| unsat | 0 |  |  |
|auk-0061.smt2                                                |    0.107s | 21.492MiB| unsat | 0 |  |  |
|auk-0012.smt2                                                |    0.110s | 21.52MiB| unsat | 0 |  |  |
|auk-0051.smt2                                                |    0.117s | 21.688MiB| unsat | 0 |  |  |
|auk-0155.smt2                                                |    0.117s | 26.132MiB| sat | 0 |  |  |
|auk-0058.smt2                                                |    0.119s | 25.472MiB| sat | 0 |  |  |
|auk-0166.smt2                                                |    0.120s | 21.728MiB| sat | 0 |  |  |
|auk-0162.smt2                                                |    0.126s | 25.192MiB| sat | 0 |  |  |
|auk-0129.smt2                                                |    0.128s | 25.792MiB| sat | 0 |  |  |
|auk-0167.smt2                                                |    0.129s | 25.896MiB| sat | 0 |  |  |
|auk-0136.smt2                                                |    0.131s | 21.672MiB| sat | 0 |  |  |
|auk-0052.smt2                                                |    0.132s | 25.832MiB| unsat | 0 |  |  |
|auk-0097.smt2                                                |    0.132s | 23.788MiB| sat | 0 |  |  |
|auk-0152.smt2                                                |    0.132s | 25.672MiB| sat | 0 |  |  |
|auk-0174.smt2                                                |    0.134s | 26.44MiB| sat | 0 |  |  |
|auk-0158.smt2                                                |    0.135s | 25.632MiB| sat | 0 |  |  |
|auk-0143.smt2                                                |    0.135s | 21.812MiB| unsat | 0 |  |  |
|auk-0034.smt2                                                |    0.137s | 21.564MiB| unsat | 0 |  |  |
|auk-0121.smt2                                                |    0.138s | 27.368MiB| sat | 0 |  |  |
|auk-0064.smt2                                                |    0.138s | 21.396MiB| sat | 0 |  |  |
|auk-0107.smt2                                                |    0.140s | 27.412MiB| sat | 0 |  |  |
|auk-0175.smt2                                                |    0.141s | 27.104MiB| sat | 0 |  |  |
|auk-0059.smt2                                                |    0.142s | 23.792MiB| unsat | 0 |  |  |
|auk-0171.smt2                                                |    0.142s | 26.5MiB| sat | 0 |  |  |
|auk-0157.smt2                                                |    0.143s | 25.664MiB| sat | 0 |  |  |
|auk-0055.smt2                                                |    0.143s | 25.488MiB| sat | 0 |  |  |
|auk-0021.smt2                                                |    0.150s | 24.372MiB| unsat | 0 |  |  |
|auk-0013.smt2                                                |    0.153s | 28.88MiB| unsat | 0 |  |  |
|auk-0114.smt2                                                |    0.153s | 26.464MiB| sat | 0 |  |  |
|auk-0019.smt2                                                |    0.155s | 26.776MiB| unsat | 0 |  |  |
|auk-0118.smt2                                                |    0.156s | 27.592MiB| sat | 0 |  |  |
|auk-0024.smt2                                                |    0.157s | 26.476MiB| unsat | 0 |  |  |
|auk-0033.smt2                                                |    0.158s | 26.772MiB| sat | 0 |  |  |
|auk-0127.smt2                                                |    0.159s | 27.232MiB| sat | 0 |  |  |
|auk-0105.smt2                                                |    0.159s | 27.576MiB| sat | 0 |  |  |
|auk-0100.smt2                                                |    0.160s | 25.316MiB| sat | 0 |  |  |
|auk-0104.smt2                                                |    0.163s | 24.752MiB| sat | 0 |  |  |
|auk-0110.smt2                                                |    0.163s | 25.588MiB| sat | 0 |  |  |
|auk-0176.smt2                                                |    0.163s | 26.956MiB| sat | 0 |  |  |
|auk-0173.smt2                                                |    0.164s | 26.472MiB| sat | 0 |  |  |
|auk-0165.smt2                                                |    0.165s | 25.932MiB| sat | 0 |  |  |
|auk-0106.smt2                                                |    0.165s | 27.372MiB| sat | 0 |  |  |
|auk-0172.smt2                                                |    0.166s | 26.456MiB| sat | 0 |  |  |
|auk-0108.smt2                                                |    0.167s | 29.116MiB| sat | 0 |  |  |
|auk-0048.smt2                                                |    0.167s | 26.224MiB| unsat | 0 |  |  |
|auk-0018.smt2                                                |    0.169s | 26.824MiB| unsat | 0 |  |  |
|auk-0029.smt2                                                |    0.169s | 26.972MiB| unsat | 0 |  |  |
|auk-0062.smt2                                                |    0.170s | 25.452MiB| unsat | 0 |  |  |
|auk-0125.smt2                                                |    0.172s | 27.492MiB| sat | 0 |  |  |
|auk-0103.smt2                                                |    0.173s | 28.628MiB| sat | 0 |  |  |
|auk-0026.smt2                                                |    0.173s | 25.76MiB| unsat | 0 |  |  |
|auk-0008.smt2                                                |    0.175s | 28.848MiB| unsat | 0 |  |  |
|auk-0156.smt2                                                |    0.179s | 25.892MiB| sat | 0 |  |  |
|auk-0016.smt2                                                |    0.180s | 26.028MiB| unsat | 0 |  |  |
|auk-0163.smt2                                                |    0.181s | 26.664MiB| sat | 0 |  |  |
|auk-0003.smt2                                                |    0.181s | 29.112MiB| unsat | 0 |  |  |
|auk-0124.smt2                                                |    0.181s | 27.568MiB| sat | 0 |  |  |
|auk-0020.smt2                                                |    0.182s | 26.488MiB| unsat | 0 |  |  |
|auk-0169.smt2                                                |    0.183s | 27.68MiB| unsat | 0 |  |  |
|auk-0111.smt2                                                |    0.186s | 28.924MiB| sat | 0 |  |  |
|auk-0014.smt2                                                |    0.187s | 28.864MiB| unsat | 0 |  |  |
|auk-0030.smt2                                                |    0.189s | 26.276MiB| unsat | 0 |  |  |
|auk-0112.smt2                                                |    0.195s | 27.576MiB| sat | 0 |  |  |
|auk-0164.smt2                                                |    0.195s | 26.368MiB| sat | 0 |  |  |
|auk-0045.smt2                                                |    0.199s | 26.92MiB| unsat | 0 |  |  |
|auk-0102.smt2                                                |    0.200s | 27.792MiB| sat | 0 |  |  |
|auk-0037.smt2                                                |    0.201s | 26.788MiB| unsat | 0 |  |  |
|auk-0017.smt2                                                |    0.206s | 28.084MiB| unsat | 0 |  |  |
|auk-0154.smt2                                                |    0.206s | 27.008MiB| sat | 0 |  |  |
|auk-0177.smt2                                                |    0.208s | 27.804MiB| sat | 0 |  |  |
|auk-0053.smt2                                                |    0.209s | 22.452MiB| sat | 0 |  |  |
|auk-0047.smt2                                                |    0.211s | 27.304MiB| unsat | 0 |  |  |
|auk-0153.smt2                                                |    0.214s | 27.652MiB| sat | 0 |  |  |
|auk-0126.smt2                                                |    0.231s | 28.836MiB| sat | 0 |  |  |
|auk-0139.smt2                                                |    0.240s | 22.632MiB| unsat | 0 |  |  |
|auk-0180.smt2                                                |    0.270s | 28.996MiB| unsat | 0 |  |  |
|auk-0122.smt2                                                |    0.271s | 32.124MiB| sat | 0 |  |  |
|auk-0113.smt2                                                |    0.273s | 29.94MiB| sat | 0 |  |  |
|auk-0150.smt2                                                |    0.290s | 27.74MiB| unsat | 0 |  |  |
|auk-0009.smt2                                                |    0.292s | 21.5MiB| sat | 0 |  |  |
|auk-0022.smt2                                                |    0.302s | 29.22MiB| sat | 0 |  |  |
|auk-0116.smt2                                                |    0.304s | 31.676MiB| sat | 0 |  |  |
|auk-0117.smt2                                                |    0.312s | 32.272MiB| sat | 0 |  |  |
|auk-0010.smt2                                                |    0.313s | 35.228MiB| unsat | 0 |  |  |
|auk-0119.smt2                                                |    0.323s | 34.14MiB| sat | 0 |  |  |
|auk-0123.smt2                                                |    0.342s | 32.04MiB| sat | 0 |  |  |
|auk-0096.smt2                                                |    0.356s | 29.492MiB| sat | 0 |  |  |
|auk-0070.smt2                                                |    0.369s | 31.964MiB| sat | 0 |  |  |
|auk-0115.smt2                                                |    0.378s | 36.024MiB| sat | 0 |  |  |
|auk-0036.smt2                                                |    0.389s | 22.792MiB| sat | 0 |  |  |
|auk-0128.smt2                                                |    0.400s | 34.116MiB| sat | 0 |  |  |
|auk-0056.smt2                                                |    0.401s | 27.908MiB| sat | 0 |  |  |
|auk-0130.smt2                                                |    0.405s | 34.268MiB| sat | 0 |  |  |
|auk-0023.smt2                                                |    0.482s | 29.688MiB| unsat | 0 |  |  |
|auk-0049.smt2                                                |    0.524s | 22.624MiB| unsat | 0 |  |  |
|auk-0006.smt2                                                |    0.525s | 36.62MiB| unsat | 0 |  |  |
|auk-0002.smt2                                                |    0.526s | 37.38MiB| unsat | 0 |  |  |
|auk-0072.smt2                                                |    0.651s | 32.876MiB| sat | 0 |  |  |
|auk-0078.smt2                                                |    0.692s | 32.968MiB| sat | 0 |  |  |
|auk-0120.smt2                                                |    0.749s | 32.604MiB| sat | 0 |  |  |
|auk-0135.smt2                                                |    0.777s | 28.944MiB| unsat | 0 |  |  |
|auk-0004.smt2                                                |    0.828s | 37.256MiB| unsat | 0 |  |  |
|auk-0190.smt2                                                |    0.866s | 32.592MiB| unsat | 0 |  |  |
|auk-0134.smt2                                                |    0.889s | 33.2MiB| sat | 0 |  |  |
|auk-0133.smt2                                                |    0.901s | 42.916MiB| sat | 0 |  |  |
|auk-0091.smt2                                                |    0.909s | 44.312MiB| sat | 0 |  |  |
|auk-0109.smt2                                                |    0.934s | 96.004MiB| sat | 0 |  |  |
|auk-0094.smt2                                                |    0.965s | 33.1MiB| sat | 0 |  |  |
|auk-0027.smt2                                                |    1.034s | 29.296MiB| unsat | 0 |  |  |
|auk-0080.smt2                                                |    1.088s | 34.888MiB| sat | 0 |  |  |
|auk-0101.smt2                                                |    1.139s | 32.916MiB| unsat | 0 |  |  |
|auk-0132.smt2                                                |    1.194s | 58.944MiB| sat | 0 |  |  |
|auk-0073.smt2                                                |    1.385s | 48.072MiB| sat | 0 |  |  |
|auk-0071.smt2                                                |    1.421s | 48.056MiB| sat | 0 |  |  |
|auk-0076.smt2                                                |    1.430s | 48.176MiB| sat | 0 |  |  |
|auk-0075.smt2                                                |    1.437s | 48.16MiB| sat | 0 |  |  |
|auk-0149.smt2                                                |    1.636s | 30.604MiB| unsat | 0 |  |  |
|auk-0151.smt2                                                |    1.696s | 23.312MiB| unsat | 0 |  |  |
|auk-0131.smt2                                                |    1.816s | 94.46MiB| sat | 0 |  |  |
|auk-0025.smt2                                                |    2.123s | 34.04MiB| unsat | 0 |  |  |
|auk-0145.smt2                                                |    2.169s | 31.316MiB| unsat | 0 |  |  |
|auk-0077.smt2                                                |    2.241s | 43.048MiB| sat | 0 |  |  |
|auk-0142.smt2                                                |    2.309s | 35.688MiB| unsat | 0 |  |  |
|auk-0146.smt2                                                |    2.434s | 30.384MiB| unsat | 0 |  |  |
|auk-0095.smt2                                                |    2.889s | 39.152MiB| unsat | 0 |  |  |
|auk-0083.smt2                                                |    3.464s | 100.0MiB| sat | 0 |  |  |
|auk-0082.smt2                                                |    3.489s | 100.0MiB| sat | 0 |  |  |
|auk-0148.smt2                                                |    3.799s | 35.748MiB| unsat | 0 |  |  |
|auk-0144.smt2                                                |    4.174s | 37.844MiB| unsat | 0 |  |  |
|auk-0183.smt2                                                |    4.786s | 60.668MiB| unsat | 0 |  |  |
|auk-0182.smt2                                                |    6.032s | 59.66MiB| unsat | 0 |  |  |
|auk-0005.smt2                                                |    6.312s | 39.516MiB| unsat | 0 |  |  |
|auk-0181.smt2                                                |   12.379s | 63.54MiB| unsat | 0 |  |  |
|auk-0084.smt2                                                |   12.670s | 56.216MiB| unsat | 0 |  |  |
|auk-0186.smt2                                                |   12.835s | 63.9MiB| unsat | 0 |  |  |
|auk-0137.smt2                                                |   13.622s | 31.96MiB| sat | 0 |  |  |
|auk-0007.smt2                                                |   14.959s | 42.596MiB| sat | 0 |  |  |
|auk-0098.smt2                                                |   20.778s | 189.0MiB| unsat | 0 |  |  |
|auk-0074.smt2                                                |   24.767s | 32.492MiB| unsat | 0 |  |  |
|auk-0140.smt2                                                |   26.620s | 41.432MiB| unsat | 0 |  |  |
|auk-0090.smt2                                                |   27.673s | 61.672MiB| unsat | 0 |  |  |
|auk-0099.smt2                                                |   28.769s | 224.0MiB| unsat | 0 |  |  |
|auk-0079.smt2                                                |   32.536s | 56.66MiB| sat | 0 |  |  |
|auk-0085.smt2                                                |   40.773s | 91.288MiB| unsat | 0 |  |  |
|auk-0188.smt2                                                |  100.036s | 55.36MiB| timeout | 0 |  |  |
|auk-0089.smt2                                                |  100.045s | 76.06MiB| timeout | 0 |  |  |
|auk-0147.smt2                                                |  100.046s | 53.604MiB| timeout | 0 |  |  |
|auk-0191.smt2                                                |  100.052s | 45.1MiB| timeout | 0 |  |  |
|auk-0189.smt2                                                |  100.052s | 66.772MiB| timeout | 0 |  |  |
|auk-0087.smt2                                                |  100.053s | 67.204MiB| timeout | 0 |  |  |
|auk-0185.smt2                                                |  100.054s | 55.008MiB| timeout | 0 |  |  |
|auk-0184.smt2                                                |  100.058s | 59.004MiB| timeout | 0 |  |  |
|auk-0187.smt2                                                |  100.062s | 66.516MiB| timeout | 0 |  |  |
|auk-0086.smt2                                                |  100.064s | 118.0MiB| timeout | 0 |  |  |
|auk-0088.smt2                                                |  100.065s | 170.0MiB| timeout | 0 |  |  |
|auk-0141.smt2                                                |  100.069s | 77.148MiB| timeout | 0 |  |  |
|auk-0092.smt2                                                |  100.070s | 125.0MiB| timeout | 0 |  |  |
|auk-0001.smt2                                                |  100.071s | 33.312MiB| timeout | 0 |  |  |
|auk-0011.smt2                                                |  100.071s | 52.064MiB| timeout | 0 |  |  |
|auk-0015.smt2                                                |  100.075s | 57.648MiB| timeout | 0 |  |  |
|auk-0081.smt2                                                |  100.079s | 35.312MiB| timeout | 0 |  |  |
