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
Job tag: capture_pointer_certora2_100_rs_3
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 1f67f6a2344fc8508ad25381686ce3901cf28ae8
Z3 branch: capture_pointer
Z3 options: "-T:100 smt.random_seed=3"
Z3 inputs: inputs/certora2
Z3 commit message: Remove stray check-assignment debug print polluting solver output

The IF_VERBOSE(0, ...) in solver::check_assignment unconditionally wrote
"check-assignment" to stdout, corrupting solver output and breaking the
3042.smt2 regression (extra line before 'sat').

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|auk-0178.smt2                                                |    0.027s | 20.904MiB| sat | 0 |  |  |
|auk-0060.smt2                                                |    0.038s | 21.132MiB| unsat | 0 |  |  |
|auk-0179.smt2                                                |    0.042s | 21.172MiB| unsat | 0 |  |  |
|auk-0042.smt2                                                |    0.051s | 21.204MiB| sat | 0 |  |  |
|auk-0054.smt2                                                |    0.057s | 19.644MiB| unsat | 0 |  |  |
|auk-0038.smt2                                                |    0.058s | 19.668MiB| unsat | 0 |  |  |
|auk-0161.smt2                                                |    0.059s | 20.352MiB| sat | 0 |  |  |
|auk-0066.smt2                                                |    0.060s | 20.412MiB| sat | 0 |  |  |
|auk-0068.smt2                                                |    0.064s | 20.264MiB| unsat | 0 |  |  |
|auk-0168.smt2                                                |    0.067s | 20.676MiB| sat | 0 |  |  |
|auk-0138.smt2                                                |    0.068s | 21.056MiB| unsat | 0 |  |  |
|auk-0035.smt2                                                |    0.069s | 20.904MiB| unsat | 0 |  |  |
|auk-0067.smt2                                                |    0.070s | 21.04MiB| sat | 0 |  |  |
|auk-0160.smt2                                                |    0.073s | 20.932MiB| unsat | 0 |  |  |
|auk-0069.smt2                                                |    0.073s | 20.928MiB| unsat | 0 |  |  |
|auk-0057.smt2                                                |    0.074s | 20.62MiB| unsat | 0 |  |  |
|auk-0040.smt2                                                |    0.076s | 21.168MiB| unsat | 0 |  |  |
|auk-0028.smt2                                                |    0.077s | 23.224MiB| unsat | 0 |  |  |
|auk-0097.smt2                                                |    0.081s | 23.632MiB| sat | 0 |  |  |
|auk-0031.smt2                                                |    0.083s | 20.872MiB| unsat | 0 |  |  |
|auk-0012.smt2                                                |    0.084s | 21.44MiB| unsat | 0 |  |  |
|auk-0034.smt2                                                |    0.084s | 21.536MiB| unsat | 0 |  |  |
|auk-0041.smt2                                                |    0.084s | 21.104MiB| unsat | 0 |  |  |
|auk-0065.smt2                                                |    0.086s | 21.24MiB| unsat | 0 |  |  |
|auk-0046.smt2                                                |    0.086s | 20.444MiB| unsat | 0 |  |  |
|auk-0059.smt2                                                |    0.093s | 23.56MiB| unsat | 0 |  |  |
|auk-0039.smt2                                                |    0.097s | 21.528MiB| unsat | 0 |  |  |
|auk-0044.smt2                                                |    0.102s | 21.648MiB| unsat | 0 |  |  |
|auk-0170.smt2                                                |    0.103s | 21.644MiB| unsat | 0 |  |  |
|auk-0050.smt2                                                |    0.104s | 21.104MiB| unsat | 0 |  |  |
|auk-0159.smt2                                                |    0.108s | 21.512MiB| unsat | 0 |  |  |
|auk-0055.smt2                                                |    0.112s | 25.528MiB| sat | 0 |  |  |
|auk-0061.smt2                                                |    0.112s | 21.392MiB| unsat | 0 |  |  |
|auk-0032.smt2                                                |    0.115s | 21.156MiB| unsat | 0 |  |  |
|auk-0093.smt2                                                |    0.116s | 24.636MiB| sat | 0 |  |  |
|auk-0063.smt2                                                |    0.117s | 21.68MiB| unsat | 0 |  |  |
|auk-0024.smt2                                                |    0.118s | 26.516MiB| unsat | 0 |  |  |
|auk-0058.smt2                                                |    0.119s | 25.476MiB| sat | 0 |  |  |
|auk-0064.smt2                                                |    0.121s | 21.632MiB| sat | 0 |  |  |
|auk-0110.smt2                                                |    0.124s | 25.528MiB| sat | 0 |  |  |
|auk-0152.smt2                                                |    0.125s | 25.724MiB| sat | 0 |  |  |
|auk-0136.smt2                                                |    0.126s | 21.512MiB| sat | 0 |  |  |
|auk-0026.smt2                                                |    0.127s | 25.712MiB| unsat | 0 |  |  |
|auk-0121.smt2                                                |    0.130s | 27.076MiB| sat | 0 |  |  |
|auk-0043.smt2                                                |    0.131s | 25.124MiB| unsat | 0 |  |  |
|auk-0158.smt2                                                |    0.135s | 25.704MiB| sat | 0 |  |  |
|auk-0052.smt2                                                |    0.135s | 25.996MiB| unsat | 0 |  |  |
|auk-0174.smt2                                                |    0.136s | 26.348MiB| sat | 0 |  |  |
|auk-0157.smt2                                                |    0.137s | 26.052MiB| sat | 0 |  |  |
|auk-0162.smt2                                                |    0.138s | 25.18MiB| sat | 0 |  |  |
|auk-0020.smt2                                                |    0.139s | 26.384MiB| unsat | 0 |  |  |
|auk-0167.smt2                                                |    0.140s | 26.192MiB| sat | 0 |  |  |
|auk-0033.smt2                                                |    0.141s | 26.76MiB| sat | 0 |  |  |
|auk-0049.smt2                                                |    0.142s | 22.212MiB| unsat | 0 |  |  |
|auk-0114.smt2                                                |    0.142s | 26.356MiB| sat | 0 |  |  |
|auk-0172.smt2                                                |    0.144s | 26.472MiB| sat | 0 |  |  |
|auk-0165.smt2                                                |    0.145s | 26.448MiB| sat | 0 |  |  |
|auk-0129.smt2                                                |    0.146s | 25.676MiB| sat | 0 |  |  |
|auk-0104.smt2                                                |    0.146s | 25.128MiB| sat | 0 |  |  |
|auk-0100.smt2                                                |    0.146s | 25.456MiB| sat | 0 |  |  |
|auk-0051.smt2                                                |    0.147s | 22.116MiB| unsat | 0 |  |  |
|auk-0036.smt2                                                |    0.147s | 21.948MiB| sat | 0 |  |  |
|auk-0166.smt2                                                |    0.147s | 22.064MiB| sat | 0 |  |  |
|auk-0048.smt2                                                |    0.147s | 26.052MiB| unsat | 0 |  |  |
|auk-0173.smt2                                                |    0.148s | 26.452MiB| sat | 0 |  |  |
|auk-0112.smt2                                                |    0.151s | 27.808MiB| sat | 0 |  |  |
|auk-0107.smt2                                                |    0.154s | 27.516MiB| sat | 0 |  |  |
|auk-0163.smt2                                                |    0.154s | 26.532MiB| sat | 0 |  |  |
|auk-0019.smt2                                                |    0.155s | 26.784MiB| unsat | 0 |  |  |
|auk-0171.smt2                                                |    0.156s | 26.352MiB| sat | 0 |  |  |
|auk-0124.smt2                                                |    0.156s | 27.352MiB| sat | 0 |  |  |
|auk-0003.smt2                                                |    0.158s | 28.784MiB| unsat | 0 |  |  |
|auk-0047.smt2                                                |    0.159s | 26.108MiB| unsat | 0 |  |  |
|auk-0021.smt2                                                |    0.160s | 24.264MiB| unsat | 0 |  |  |
|auk-0125.smt2                                                |    0.160s | 27.496MiB| sat | 0 |  |  |
|auk-0062.smt2                                                |    0.162s | 25.456MiB| unsat | 0 |  |  |
|auk-0106.smt2                                                |    0.162s | 27.396MiB| sat | 0 |  |  |
|auk-0127.smt2                                                |    0.163s | 27.196MiB| sat | 0 |  |  |
|auk-0118.smt2                                                |    0.164s | 27.504MiB| sat | 0 |  |  |
|auk-0013.smt2                                                |    0.164s | 28.928MiB| unsat | 0 |  |  |
|auk-0155.smt2                                                |    0.164s | 26.332MiB| sat | 0 |  |  |
|auk-0143.smt2                                                |    0.165s | 21.752MiB| unsat | 0 |  |  |
|auk-0030.smt2                                                |    0.166s | 25.964MiB| unsat | 0 |  |  |
|auk-0037.smt2                                                |    0.167s | 26.696MiB| unsat | 0 |  |  |
|auk-0105.smt2                                                |    0.168s | 27.532MiB| sat | 0 |  |  |
|auk-0175.smt2                                                |    0.169s | 26.988MiB| sat | 0 |  |  |
|auk-0018.smt2                                                |    0.170s | 26.712MiB| unsat | 0 |  |  |
|auk-0016.smt2                                                |    0.171s | 26.176MiB| unsat | 0 |  |  |
|auk-0154.smt2                                                |    0.175s | 26.86MiB| sat | 0 |  |  |
|auk-0156.smt2                                                |    0.175s | 26.204MiB| sat | 0 |  |  |
|auk-0045.smt2                                                |    0.179s | 27.176MiB| unsat | 0 |  |  |
|auk-0102.smt2                                                |    0.180s | 27.848MiB| sat | 0 |  |  |
|auk-0169.smt2                                                |    0.186s | 27.476MiB| unsat | 0 |  |  |
|auk-0176.smt2                                                |    0.187s | 27.092MiB| sat | 0 |  |  |
|auk-0139.smt2                                                |    0.191s | 22.96MiB| unsat | 0 |  |  |
|auk-0177.smt2                                                |    0.192s | 27.704MiB| sat | 0 |  |  |
|auk-0029.smt2                                                |    0.193s | 28.08MiB| unsat | 0 |  |  |
|auk-0153.smt2                                                |    0.195s | 27.664MiB| sat | 0 |  |  |
|auk-0108.smt2                                                |    0.200s | 29.024MiB| sat | 0 |  |  |
|auk-0014.smt2                                                |    0.202s | 28.76MiB| unsat | 0 |  |  |
|auk-0126.smt2                                                |    0.205s | 28.84MiB| sat | 0 |  |  |
|auk-0103.smt2                                                |    0.206s | 28.584MiB| sat | 0 |  |  |
|auk-0008.smt2                                                |    0.209s | 28.748MiB| unsat | 0 |  |  |
|auk-0053.smt2                                                |    0.211s | 22.404MiB| sat | 0 |  |  |
|auk-0150.smt2                                                |    0.212s | 27.304MiB| unsat | 0 |  |  |
|auk-0164.smt2                                                |    0.213s | 26.492MiB| sat | 0 |  |  |
|auk-0111.smt2                                                |    0.213s | 28.968MiB| sat | 0 |  |  |
|auk-0113.smt2                                                |    0.262s | 30.08MiB| sat | 0 |  |  |
|auk-0117.smt2                                                |    0.272s | 31.984MiB| sat | 0 |  |  |
|auk-0122.smt2                                                |    0.276s | 31.864MiB| sat | 0 |  |  |
|auk-0128.smt2                                                |    0.277s | 32.804MiB| sat | 0 |  |  |
|auk-0017.smt2                                                |    0.287s | 27.868MiB| unsat | 0 |  |  |
|auk-0123.smt2                                                |    0.292s | 32.028MiB| sat | 0 |  |  |
|auk-0119.smt2                                                |    0.294s | 34.184MiB| sat | 0 |  |  |
|auk-0116.smt2                                                |    0.301s | 31.668MiB| sat | 0 |  |  |
|auk-0130.smt2                                                |    0.305s | 34.4MiB| sat | 0 |  |  |
|auk-0115.smt2                                                |    0.317s | 35.812MiB| sat | 0 |  |  |
|auk-0009.smt2                                                |    0.344s | 21.672MiB| sat | 0 |  |  |
|auk-0120.smt2                                                |    0.377s | 32.536MiB| sat | 0 |  |  |
|auk-0180.smt2                                                |    0.391s | 29.288MiB| unsat | 0 |  |  |
|auk-0007.smt2                                                |    0.404s | 37.584MiB| sat | 0 |  |  |
|auk-0002.smt2                                                |    0.409s | 35.656MiB| unsat | 0 |  |  |
|auk-0056.smt2                                                |    0.410s | 28.456MiB| sat | 0 |  |  |
|auk-0096.smt2                                                |    0.412s | 29.4MiB| sat | 0 |  |  |
|auk-0023.smt2                                                |    0.441s | 29.696MiB| unsat | 0 |  |  |
|auk-0077.smt2                                                |    0.477s | 40.232MiB| sat | 0 |  |  |
|auk-0135.smt2                                                |    0.502s | 28.972MiB| unsat | 0 |  |  |
|auk-0190.smt2                                                |    0.642s | 32.716MiB| unsat | 0 |  |  |
|auk-0010.smt2                                                |    0.656s | 38.304MiB| unsat | 0 |  |  |
|auk-0072.smt2                                                |    0.664s | 32.832MiB| sat | 0 |  |  |
|auk-0078.smt2                                                |    0.669s | 32.924MiB| sat | 0 |  |  |
|auk-0006.smt2                                                |    0.674s | 37.432MiB| unsat | 0 |  |  |
|auk-0080.smt2                                                |    0.751s | 34.98MiB| sat | 0 |  |  |
|auk-0022.smt2                                                |    0.821s | 29.6MiB| sat | 0 |  |  |
|auk-0101.smt2                                                |    0.857s | 32.872MiB| unsat | 0 |  |  |
|auk-0133.smt2                                                |    0.872s | 42.92MiB| sat | 0 |  |  |
|auk-0091.smt2                                                |    0.892s | 44.056MiB| sat | 0 |  |  |
|auk-0025.smt2                                                |    0.916s | 32.524MiB| unsat | 0 |  |  |
|auk-0109.smt2                                                |    0.928s | 95.912MiB| sat | 0 |  |  |
|auk-0005.smt2                                                |    1.118s | 38.044MiB| unsat | 0 |  |  |
|auk-0145.smt2                                                |    1.173s | 31.56MiB| unsat | 0 |  |  |
|auk-0132.smt2                                                |    1.190s | 58.204MiB| sat | 0 |  |  |
|auk-0004.smt2                                                |    1.233s | 37.732MiB| unsat | 0 |  |  |
|auk-0149.smt2                                                |    1.260s | 29.968MiB| unsat | 0 |  |  |
|auk-0134.smt2                                                |    1.273s | 33.32MiB| sat | 0 |  |  |
|auk-0071.smt2                                                |    1.374s | 48.12MiB| sat | 0 |  |  |
|auk-0076.smt2                                                |    1.391s | 48.132MiB| sat | 0 |  |  |
|auk-0073.smt2                                                |    1.400s | 48.288MiB| sat | 0 |  |  |
|auk-0075.smt2                                                |    1.402s | 48.036MiB| sat | 0 |  |  |
|auk-0151.smt2                                                |    1.636s | 23.256MiB| unsat | 0 |  |  |
|auk-0131.smt2                                                |    1.899s | 95.492MiB| sat | 0 |  |  |
|auk-0094.smt2                                                |    2.136s | 33.908MiB| sat | 0 |  |  |
|auk-0144.smt2                                                |    2.348s | 34.952MiB| unsat | 0 |  |  |
|auk-0148.smt2                                                |    2.450s | 35.708MiB| unsat | 0 |  |  |
|auk-0079.smt2                                                |    2.611s | 45.928MiB| sat | 0 |  |  |
|auk-0011.smt2                                                |    2.657s | 39.452MiB| unsat | 0 |  |  |
|auk-0095.smt2                                                |    2.947s | 38.56MiB| unsat | 0 |  |  |
|auk-0146.smt2                                                |    3.401s | 30.98MiB| unsat | 0 |  |  |
|auk-0083.smt2                                                |    3.430s | 100.0MiB| sat | 0 |  |  |
|auk-0082.smt2                                                |    3.440s | 100.0MiB| sat | 0 |  |  |
|auk-0142.smt2                                                |    3.880s | 36.276MiB| unsat | 0 |  |  |
|auk-0181.smt2                                                |    4.417s | 60.92MiB| unsat | 0 |  |  |
|auk-0027.smt2                                                |    4.439s | 29.924MiB| unsat | 0 |  |  |
|auk-0183.smt2                                                |    7.088s | 62.588MiB| unsat | 0 |  |  |
|auk-0140.smt2                                                |    8.656s | 37.564MiB| unsat | 0 |  |  |
|auk-0084.smt2                                                |    9.677s | 54.896MiB| unsat | 0 |  |  |
|auk-0098.smt2                                                |   17.184s | 180.0MiB| unsat | 0 |  |  |
|auk-0099.smt2                                                |   20.504s | 184.0MiB| unsat | 0 |  |  |
|auk-0074.smt2                                                |   34.126s | 33.58MiB| unsat | 0 |  |  |
|auk-0070.smt2                                                |   38.896s | 33.224MiB| sat | 0 |  |  |
|auk-0137.smt2                                                |   43.443s | 38.544MiB| sat | 0 |  |  |
|auk-0087.smt2                                                |   47.949s | 71.548MiB| unsat | 0 |  |  |
|auk-0184.smt2                                                |   56.273s | 46.664MiB| unsat | 0 |  |  |
|auk-0182.smt2                                                |   78.572s | 87.364MiB| unsat | 0 |  |  |
|auk-0186.smt2                                                |   95.266s | 73.38MiB| unsat | 0 |  |  |
|auk-0187.smt2                                                |  100.017s | 64.104MiB| timeout | 0 |  |  |
|auk-0147.smt2                                                |  100.019s | 56.972MiB| timeout | 0 |  |  |
|auk-0191.smt2                                                |  100.038s | 37.6MiB| timeout | 0 |  |  |
|auk-0001.smt2                                                |  100.047s | 27.94MiB| timeout | 0 |  |  |
|auk-0081.smt2                                                |  100.048s | 34.612MiB| timeout | 0 |  |  |
|auk-0189.smt2                                                |  100.049s | 68.748MiB| timeout | 0 |  |  |
|auk-0015.smt2                                                |  100.050s | 51.5MiB| timeout | 0 |  |  |
|auk-0141.smt2                                                |  100.051s | 90.632MiB| timeout | 0 |  |  |
|auk-0092.smt2                                                |  100.057s | 137.0MiB| timeout | 0 |  |  |
|auk-0185.smt2                                                |  100.057s | 58.068MiB| timeout | 0 |  |  |
|auk-0188.smt2                                                |  100.059s | 57.716MiB| timeout | 0 |  |  |
|auk-0085.smt2                                                |  100.067s | 232.0MiB| timeout | 0 |  |  |
|auk-0090.smt2                                                |  100.067s | 68.888MiB| timeout | 0 |  |  |
|auk-0089.smt2                                                |  100.075s | 69.74MiB| timeout | 0 |  |  |
|auk-0088.smt2                                                |  100.077s | 120.0MiB| timeout | 0 |  |  |
|auk-0086.smt2                                                |  100.083s | 143.0MiB| timeout | 0 |  |  |
