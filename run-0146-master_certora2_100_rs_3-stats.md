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
Job tag: master_certora2_100_rs_3
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 75981a5d3b3c216142cd69b8c4d4e0a15ecf2e72
Z3 branch: master
Z3 options: "-T:100 smt.random_seed=3"
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
|auk-0066.smt2                                                |    0.031s | 20.612MiB| sat | 0 |  |  |
|auk-0046.smt2                                                |    0.031s | 20.404MiB| unsat | 0 |  |  |
|auk-0057.smt2                                                |    0.040s | 20.76MiB| unsat | 0 |  |  |
|auk-0160.smt2                                                |    0.040s | 21.056MiB| unsat | 0 |  |  |
|auk-0041.smt2                                                |    0.047s | 21.14MiB| unsat | 0 |  |  |
|auk-0068.smt2                                                |    0.057s | 20.34MiB| unsat | 0 |  |  |
|auk-0042.smt2                                                |    0.060s | 20.908MiB| sat | 0 |  |  |
|auk-0179.smt2                                                |    0.060s | 21.132MiB| unsat | 0 |  |  |
|auk-0060.smt2                                                |    0.062s | 20.912MiB| unsat | 0 |  |  |
|auk-0059.smt2                                                |    0.066s | 23.636MiB| unsat | 0 |  |  |
|auk-0035.smt2                                                |    0.068s | 20.888MiB| unsat | 0 |  |  |
|auk-0032.smt2                                                |    0.070s | 21.192MiB| unsat | 0 |  |  |
|auk-0050.smt2                                                |    0.074s | 20.952MiB| unsat | 0 |  |  |
|auk-0038.smt2                                                |    0.079s | 19.628MiB| unsat | 0 |  |  |
|auk-0034.smt2                                                |    0.082s | 21.568MiB| unsat | 0 |  |  |
|auk-0012.smt2                                                |    0.083s | 21.78MiB| unsat | 0 |  |  |
|auk-0040.smt2                                                |    0.083s | 21.104MiB| unsat | 0 |  |  |
|auk-0054.smt2                                                |    0.085s | 19.624MiB| unsat | 0 |  |  |
|auk-0178.smt2                                                |    0.085s | 20.504MiB| sat | 0 |  |  |
|auk-0170.smt2                                                |    0.086s | 21.556MiB| unsat | 0 |  |  |
|auk-0161.smt2                                                |    0.086s | 20.396MiB| sat | 0 |  |  |
|auk-0055.smt2                                                |    0.087s | 25.484MiB| sat | 0 |  |  |
|auk-0097.smt2                                                |    0.092s | 23.708MiB| sat | 0 |  |  |
|auk-0067.smt2                                                |    0.092s | 20.884MiB| sat | 0 |  |  |
|auk-0063.smt2                                                |    0.094s | 21.636MiB| unsat | 0 |  |  |
|auk-0168.smt2                                                |    0.095s | 20.928MiB| sat | 0 |  |  |
|auk-0138.smt2                                                |    0.096s | 21.444MiB| unsat | 0 |  |  |
|auk-0065.smt2                                                |    0.097s | 21.312MiB| unsat | 0 |  |  |
|auk-0031.smt2                                                |    0.098s | 21.076MiB| unsat | 0 |  |  |
|auk-0069.smt2                                                |    0.100s | 21.064MiB| unsat | 0 |  |  |
|auk-0064.smt2                                                |    0.100s | 21.648MiB| sat | 0 |  |  |
|auk-0036.smt2                                                |    0.104s | 22.02MiB| sat | 0 |  |  |
|auk-0044.smt2                                                |    0.104s | 21.504MiB| unsat | 0 |  |  |
|auk-0039.smt2                                                |    0.105s | 21.272MiB| unsat | 0 |  |  |
|auk-0093.smt2                                                |    0.106s | 24.792MiB| sat | 0 |  |  |
|auk-0061.smt2                                                |    0.107s | 21.268MiB| unsat | 0 |  |  |
|auk-0166.smt2                                                |    0.108s | 21.92MiB| sat | 0 |  |  |
|auk-0048.smt2                                                |    0.111s | 26.076MiB| unsat | 0 |  |  |
|auk-0058.smt2                                                |    0.116s | 25.716MiB| sat | 0 |  |  |
|auk-0155.smt2                                                |    0.117s | 26.288MiB| sat | 0 |  |  |
|auk-0159.smt2                                                |    0.119s | 21.684MiB| unsat | 0 |  |  |
|auk-0136.smt2                                                |    0.119s | 21.656MiB| sat | 0 |  |  |
|auk-0003.smt2                                                |    0.119s | 28.82MiB| unsat | 0 |  |  |
|auk-0028.smt2                                                |    0.124s | 23.488MiB| unsat | 0 |  |  |
|auk-0110.smt2                                                |    0.125s | 25.472MiB| sat | 0 |  |  |
|auk-0162.smt2                                                |    0.129s | 25.316MiB| sat | 0 |  |  |
|auk-0033.smt2                                                |    0.131s | 26.8MiB| sat | 0 |  |  |
|auk-0018.smt2                                                |    0.133s | 26.856MiB| unsat | 0 |  |  |
|auk-0024.smt2                                                |    0.135s | 26.508MiB| unsat | 0 |  |  |
|auk-0100.smt2                                                |    0.135s | 25.236MiB| sat | 0 |  |  |
|auk-0043.smt2                                                |    0.136s | 25.044MiB| unsat | 0 |  |  |
|auk-0106.smt2                                                |    0.137s | 27.452MiB| sat | 0 |  |  |
|auk-0062.smt2                                                |    0.138s | 25.472MiB| unsat | 0 |  |  |
|auk-0019.smt2                                                |    0.138s | 26.732MiB| unsat | 0 |  |  |
|auk-0121.smt2                                                |    0.139s | 27.096MiB| sat | 0 |  |  |
|auk-0107.smt2                                                |    0.141s | 27.296MiB| sat | 0 |  |  |
|auk-0114.smt2                                                |    0.142s | 26.404MiB| sat | 0 |  |  |
|auk-0158.smt2                                                |    0.143s | 25.716MiB| sat | 0 |  |  |
|auk-0154.smt2                                                |    0.143s | 26.88MiB| sat | 0 |  |  |
|auk-0108.smt2                                                |    0.149s | 29.2MiB| sat | 0 |  |  |
|auk-0143.smt2                                                |    0.150s | 21.904MiB| unsat | 0 |  |  |
|auk-0129.smt2                                                |    0.150s | 25.732MiB| sat | 0 |  |  |
|auk-0167.smt2                                                |    0.151s | 26.248MiB| sat | 0 |  |  |
|auk-0163.smt2                                                |    0.152s | 26.5MiB| sat | 0 |  |  |
|auk-0016.smt2                                                |    0.154s | 25.944MiB| unsat | 0 |  |  |
|auk-0156.smt2                                                |    0.157s | 26.188MiB| sat | 0 |  |  |
|auk-0020.smt2                                                |    0.158s | 26.456MiB| unsat | 0 |  |  |
|auk-0021.smt2                                                |    0.158s | 24.26MiB| unsat | 0 |  |  |
|auk-0104.smt2                                                |    0.158s | 25.208MiB| sat | 0 |  |  |
|auk-0052.smt2                                                |    0.160s | 25.856MiB| unsat | 0 |  |  |
|auk-0176.smt2                                                |    0.160s | 27.26MiB| sat | 0 |  |  |
|auk-0124.smt2                                                |    0.161s | 27.384MiB| sat | 0 |  |  |
|auk-0171.smt2                                                |    0.162s | 26.38MiB| sat | 0 |  |  |
|auk-0152.smt2                                                |    0.164s | 25.648MiB| sat | 0 |  |  |
|auk-0173.smt2                                                |    0.164s | 26.52MiB| sat | 0 |  |  |
|auk-0125.smt2                                                |    0.165s | 27.472MiB| sat | 0 |  |  |
|auk-0008.smt2                                                |    0.166s | 28.948MiB| unsat | 0 |  |  |
|auk-0175.smt2                                                |    0.166s | 27.036MiB| sat | 0 |  |  |
|auk-0105.smt2                                                |    0.168s | 27.632MiB| sat | 0 |  |  |
|auk-0102.smt2                                                |    0.169s | 27.76MiB| sat | 0 |  |  |
|auk-0164.smt2                                                |    0.169s | 26.308MiB| sat | 0 |  |  |
|auk-0051.smt2                                                |    0.170s | 22.064MiB| unsat | 0 |  |  |
|auk-0127.smt2                                                |    0.170s | 27.472MiB| sat | 0 |  |  |
|auk-0157.smt2                                                |    0.170s | 26.132MiB| sat | 0 |  |  |
|auk-0165.smt2                                                |    0.172s | 26.428MiB| sat | 0 |  |  |
|auk-0049.smt2                                                |    0.177s | 22.04MiB| unsat | 0 |  |  |
|auk-0174.smt2                                                |    0.177s | 26.42MiB| sat | 0 |  |  |
|auk-0112.smt2                                                |    0.178s | 27.78MiB| sat | 0 |  |  |
|auk-0014.smt2                                                |    0.178s | 28.928MiB| unsat | 0 |  |  |
|auk-0111.smt2                                                |    0.179s | 28.976MiB| sat | 0 |  |  |
|auk-0017.smt2                                                |    0.181s | 28.02MiB| unsat | 0 |  |  |
|auk-0103.smt2                                                |    0.181s | 28.612MiB| sat | 0 |  |  |
|auk-0026.smt2                                                |    0.184s | 25.892MiB| unsat | 0 |  |  |
|auk-0037.smt2                                                |    0.186s | 26.808MiB| unsat | 0 |  |  |
|auk-0047.smt2                                                |    0.188s | 26.232MiB| unsat | 0 |  |  |
|auk-0126.smt2                                                |    0.191s | 29.056MiB| sat | 0 |  |  |
|auk-0013.smt2                                                |    0.192s | 28.876MiB| unsat | 0 |  |  |
|auk-0153.smt2                                                |    0.192s | 27.596MiB| sat | 0 |  |  |
|auk-0169.smt2                                                |    0.195s | 27.66MiB| unsat | 0 |  |  |
|auk-0118.smt2                                                |    0.196s | 27.744MiB| sat | 0 |  |  |
|auk-0045.smt2                                                |    0.200s | 27.184MiB| unsat | 0 |  |  |
|auk-0139.smt2                                                |    0.212s | 22.916MiB| unsat | 0 |  |  |
|auk-0117.smt2                                                |    0.215s | 32.092MiB| sat | 0 |  |  |
|auk-0029.smt2                                                |    0.216s | 27.952MiB| unsat | 0 |  |  |
|auk-0150.smt2                                                |    0.222s | 27.312MiB| unsat | 0 |  |  |
|auk-0177.smt2                                                |    0.224s | 27.488MiB| sat | 0 |  |  |
|auk-0030.smt2                                                |    0.227s | 26.16MiB| unsat | 0 |  |  |
|auk-0053.smt2                                                |    0.237s | 22.388MiB| sat | 0 |  |  |
|auk-0172.smt2                                                |    0.242s | 26.5MiB| sat | 0 |  |  |
|auk-0122.smt2                                                |    0.256s | 31.892MiB| sat | 0 |  |  |
|auk-0123.smt2                                                |    0.271s | 32.028MiB| sat | 0 |  |  |
|auk-0116.smt2                                                |    0.288s | 31.708MiB| sat | 0 |  |  |
|auk-0130.smt2                                                |    0.288s | 34.468MiB| sat | 0 |  |  |
|auk-0128.smt2                                                |    0.298s | 32.764MiB| sat | 0 |  |  |
|auk-0113.smt2                                                |    0.298s | 29.992MiB| sat | 0 |  |  |
|auk-0119.smt2                                                |    0.319s | 34.056MiB| sat | 0 |  |  |
|auk-0115.smt2                                                |    0.320s | 35.664MiB| sat | 0 |  |  |
|auk-0120.smt2                                                |    0.354s | 32.484MiB| sat | 0 |  |  |
|auk-0009.smt2                                                |    0.358s | 21.676MiB| sat | 0 |  |  |
|auk-0056.smt2                                                |    0.401s | 28.308MiB| sat | 0 |  |  |
|auk-0007.smt2                                                |    0.402s | 37.62MiB| sat | 0 |  |  |
|auk-0002.smt2                                                |    0.413s | 35.748MiB| unsat | 0 |  |  |
|auk-0096.smt2                                                |    0.422s | 29.36MiB| sat | 0 |  |  |
|auk-0180.smt2                                                |    0.432s | 29.508MiB| unsat | 0 |  |  |
|auk-0023.smt2                                                |    0.465s | 29.72MiB| unsat | 0 |  |  |
|auk-0077.smt2                                                |    0.512s | 40.236MiB| sat | 0 |  |  |
|auk-0135.smt2                                                |    0.518s | 29.144MiB| unsat | 0 |  |  |
|auk-0006.smt2                                                |    0.647s | 37.416MiB| unsat | 0 |  |  |
|auk-0190.smt2                                                |    0.663s | 32.82MiB| unsat | 0 |  |  |
|auk-0010.smt2                                                |    0.680s | 38.2MiB| unsat | 0 |  |  |
|auk-0078.smt2                                                |    0.687s | 33.048MiB| sat | 0 |  |  |
|auk-0072.smt2                                                |    0.705s | 32.932MiB| sat | 0 |  |  |
|auk-0080.smt2                                                |    0.782s | 34.944MiB| sat | 0 |  |  |
|auk-0091.smt2                                                |    0.827s | 44.088MiB| sat | 0 |  |  |
|auk-0022.smt2                                                |    0.855s | 29.676MiB| sat | 0 |  |  |
|auk-0101.smt2                                                |    0.915s | 32.76MiB| unsat | 0 |  |  |
|auk-0133.smt2                                                |    0.919s | 42.908MiB| sat | 0 |  |  |
|auk-0109.smt2                                                |    0.953s | 96.044MiB| sat | 0 |  |  |
|auk-0025.smt2                                                |    0.965s | 32.568MiB| unsat | 0 |  |  |
|auk-0005.smt2                                                |    1.089s | 37.756MiB| unsat | 0 |  |  |
|auk-0145.smt2                                                |    1.202s | 31.616MiB| unsat | 0 |  |  |
|auk-0132.smt2                                                |    1.224s | 57.988MiB| sat | 0 |  |  |
|auk-0004.smt2                                                |    1.304s | 37.688MiB| unsat | 0 |  |  |
|auk-0134.smt2                                                |    1.311s | 33.46MiB| sat | 0 |  |  |
|auk-0149.smt2                                                |    1.336s | 29.964MiB| unsat | 0 |  |  |
|auk-0071.smt2                                                |    1.370s | 48.072MiB| sat | 0 |  |  |
|auk-0073.smt2                                                |    1.375s | 47.916MiB| sat | 0 |  |  |
|auk-0075.smt2                                                |    1.388s | 47.96MiB| sat | 0 |  |  |
|auk-0076.smt2                                                |    1.440s | 48.144MiB| sat | 0 |  |  |
|auk-0151.smt2                                                |    1.691s | 23.468MiB| unsat | 0 |  |  |
|auk-0131.smt2                                                |    2.036s | 95.488MiB| sat | 0 |  |  |
|auk-0094.smt2                                                |    2.147s | 34.048MiB| sat | 0 |  |  |
|auk-0144.smt2                                                |    2.482s | 34.996MiB| unsat | 0 |  |  |
|auk-0148.smt2                                                |    2.570s | 35.988MiB| unsat | 0 |  |  |
|auk-0079.smt2                                                |    2.698s | 46.16MiB| sat | 0 |  |  |
|auk-0011.smt2                                                |    2.709s | 39.452MiB| unsat | 0 |  |  |
|auk-0095.smt2                                                |    3.255s | 38.468MiB| unsat | 0 |  |  |
|auk-0146.smt2                                                |    3.440s | 30.836MiB| unsat | 0 |  |  |
|auk-0083.smt2                                                |    3.538s | 100.0MiB| sat | 0 |  |  |
|auk-0082.smt2                                                |    3.560s | 100.0MiB| sat | 0 |  |  |
|auk-0142.smt2                                                |    4.072s | 36.56MiB| unsat | 0 |  |  |
|auk-0027.smt2                                                |    4.429s | 30.144MiB| unsat | 0 |  |  |
|auk-0181.smt2                                                |    4.716s | 60.804MiB| unsat | 0 |  |  |
|auk-0183.smt2                                                |    7.433s | 62.704MiB| unsat | 0 |  |  |
|auk-0140.smt2                                                |    8.897s | 37.376MiB| unsat | 0 |  |  |
|auk-0084.smt2                                                |    9.908s | 54.808MiB| unsat | 0 |  |  |
|auk-0098.smt2                                                |   16.484s | 180.0MiB| unsat | 0 |  |  |
|auk-0099.smt2                                                |   22.211s | 184.0MiB| unsat | 0 |  |  |
|auk-0074.smt2                                                |   34.690s | 33.708MiB| unsat | 0 |  |  |
|auk-0070.smt2                                                |   38.601s | 32.776MiB| sat | 0 |  |  |
|auk-0137.smt2                                                |   44.719s | 38.536MiB| sat | 0 |  |  |
|auk-0087.smt2                                                |   48.547s | 71.544MiB| unsat | 0 |  |  |
|auk-0184.smt2                                                |   54.269s | 46.036MiB| unsat | 0 |  |  |
|auk-0182.smt2                                                |   78.331s | 87.676MiB| unsat | 0 |  |  |
|auk-0186.smt2                                                |   95.949s | 73.788MiB| unsat | 0 |  |  |
|auk-0081.smt2                                                |  100.014s | 34.816MiB| timeout | 0 |  |  |
|auk-0015.smt2                                                |  100.015s | 51.688MiB| timeout | 0 |  |  |
|auk-0090.smt2                                                |  100.018s | 68.736MiB| timeout | 0 |  |  |
|auk-0185.smt2                                                |  100.021s | 58.024MiB| timeout | 0 |  |  |
|auk-0088.smt2                                                |  100.022s | 120.0MiB| timeout | 0 |  |  |
|auk-0001.smt2                                                |  100.033s | 27.792MiB| timeout | 0 |  |  |
|auk-0187.smt2                                                |  100.037s | 64.24MiB| timeout | 0 |  |  |
|auk-0089.smt2                                                |  100.037s | 69.588MiB| timeout | 0 |  |  |
|auk-0191.smt2                                                |  100.046s | 37.944MiB| timeout | 0 |  |  |
|auk-0189.smt2                                                |  100.048s | 68.392MiB| timeout | 0 |  |  |
|auk-0085.smt2                                                |  100.058s | 232.0MiB| timeout | 0 |  |  |
|auk-0188.smt2                                                |  100.075s | 57.4MiB| timeout | 0 |  |  |
|auk-0147.smt2                                                |  100.075s | 55.268MiB| timeout | 0 |  |  |
|auk-0141.smt2                                                |  100.080s | 90.268MiB| timeout | 0 |  |  |
|auk-0086.smt2                                                |  100.085s | 143.0MiB| timeout | 0 |  |  |
|auk-0092.smt2                                                |  100.088s | 137.0MiB| timeout | 0 |  |  |
