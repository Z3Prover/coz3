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
Job tag: capture_pointer_certora2_100_rs_2
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 1f67f6a2344fc8508ad25381686ce3901cf28ae8
Z3 branch: capture_pointer
Z3 options: "-T:100 smt.random_seed=2"
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
|auk-0054.smt2                                                |    0.028s | 19.876MiB| unsat | 0 |  |  |
|auk-0067.smt2                                                |    0.032s | 20.96MiB| sat | 0 |  |  |
|auk-0046.smt2                                                |    0.033s | 20.42MiB| unsat | 0 |  |  |
|auk-0042.smt2                                                |    0.035s | 21.0MiB| sat | 0 |  |  |
|auk-0160.smt2                                                |    0.037s | 20.92MiB| unsat | 0 |  |  |
|auk-0168.smt2                                                |    0.046s | 21.572MiB| sat | 0 |  |  |
|auk-0031.smt2                                                |    0.048s | 20.956MiB| unsat | 0 |  |  |
|auk-0060.smt2                                                |    0.062s | 20.948MiB| unsat | 0 |  |  |
|auk-0057.smt2                                                |    0.069s | 20.744MiB| unsat | 0 |  |  |
|auk-0038.smt2                                                |    0.075s | 19.612MiB| unsat | 0 |  |  |
|auk-0034.smt2                                                |    0.075s | 21.58MiB| unsat | 0 |  |  |
|auk-0066.smt2                                                |    0.077s | 20.352MiB| sat | 0 |  |  |
|auk-0161.smt2                                                |    0.079s | 20.288MiB| sat | 0 |  |  |
|auk-0068.smt2                                                |    0.080s | 20.288MiB| unsat | 0 |  |  |
|auk-0050.smt2                                                |    0.084s | 21.172MiB| unsat | 0 |  |  |
|auk-0138.smt2                                                |    0.084s | 21.048MiB| unsat | 0 |  |  |
|auk-0178.smt2                                                |    0.085s | 20.612MiB| sat | 0 |  |  |
|auk-0036.smt2                                                |    0.086s | 21.844MiB| sat | 0 |  |  |
|auk-0166.smt2                                                |    0.087s | 21.812MiB| sat | 0 |  |  |
|auk-0041.smt2                                                |    0.087s | 21.12MiB| unsat | 0 |  |  |
|auk-0179.smt2                                                |    0.091s | 21.004MiB| unsat | 0 |  |  |
|auk-0069.smt2                                                |    0.093s | 21.032MiB| unsat | 0 |  |  |
|auk-0028.smt2                                                |    0.094s | 23.048MiB| unsat | 0 |  |  |
|auk-0040.smt2                                                |    0.097s | 21.112MiB| unsat | 0 |  |  |
|auk-0093.smt2                                                |    0.102s | 25.152MiB| sat | 0 |  |  |
|auk-0063.smt2                                                |    0.103s | 21.592MiB| unsat | 0 |  |  |
|auk-0170.smt2                                                |    0.103s | 21.656MiB| unsat | 0 |  |  |
|auk-0061.smt2                                                |    0.106s | 21.312MiB| unsat | 0 |  |  |
|auk-0159.smt2                                                |    0.107s | 21.616MiB| unsat | 0 |  |  |
|auk-0065.smt2                                                |    0.107s | 21.296MiB| unsat | 0 |  |  |
|auk-0035.smt2                                                |    0.112s | 20.852MiB| unsat | 0 |  |  |
|auk-0044.smt2                                                |    0.117s | 21.54MiB| unsat | 0 |  |  |
|auk-0019.smt2                                                |    0.117s | 26.98MiB| unsat | 0 |  |  |
|auk-0043.smt2                                                |    0.118s | 25.036MiB| unsat | 0 |  |  |
|auk-0032.smt2                                                |    0.120s | 21.16MiB| unsat | 0 |  |  |
|auk-0048.smt2                                                |    0.120s | 26.184MiB| unsat | 0 |  |  |
|auk-0009.smt2                                                |    0.120s | 21.428MiB| sat | 0 |  |  |
|auk-0039.smt2                                                |    0.121s | 21.188MiB| unsat | 0 |  |  |
|auk-0012.smt2                                                |    0.129s | 21.388MiB| unsat | 0 |  |  |
|auk-0024.smt2                                                |    0.130s | 26.508MiB| unsat | 0 |  |  |
|auk-0121.smt2                                                |    0.131s | 27.252MiB| sat | 0 |  |  |
|auk-0176.smt2                                                |    0.131s | 27.016MiB| sat | 0 |  |  |
|auk-0064.smt2                                                |    0.131s | 21.772MiB| sat | 0 |  |  |
|auk-0055.smt2                                                |    0.132s | 25.488MiB| sat | 0 |  |  |
|auk-0020.smt2                                                |    0.133s | 26.448MiB| unsat | 0 |  |  |
|auk-0058.smt2                                                |    0.133s | 25.552MiB| sat | 0 |  |  |
|auk-0097.smt2                                                |    0.139s | 23.616MiB| sat | 0 |  |  |
|auk-0158.smt2                                                |    0.140s | 25.828MiB| sat | 0 |  |  |
|auk-0143.smt2                                                |    0.140s | 21.772MiB| unsat | 0 |  |  |
|auk-0152.smt2                                                |    0.141s | 25.708MiB| sat | 0 |  |  |
|auk-0162.smt2                                                |    0.141s | 25.34MiB| sat | 0 |  |  |
|auk-0052.smt2                                                |    0.142s | 25.896MiB| unsat | 0 |  |  |
|auk-0026.smt2                                                |    0.144s | 25.836MiB| unsat | 0 |  |  |
|auk-0106.smt2                                                |    0.145s | 27.484MiB| sat | 0 |  |  |
|auk-0171.smt2                                                |    0.146s | 26.484MiB| sat | 0 |  |  |
|auk-0104.smt2                                                |    0.147s | 24.704MiB| sat | 0 |  |  |
|auk-0157.smt2                                                |    0.151s | 25.736MiB| sat | 0 |  |  |
|auk-0129.smt2                                                |    0.153s | 25.804MiB| sat | 0 |  |  |
|auk-0164.smt2                                                |    0.157s | 26.484MiB| sat | 0 |  |  |
|auk-0155.smt2                                                |    0.158s | 26.012MiB| sat | 0 |  |  |
|auk-0062.smt2                                                |    0.159s | 25.496MiB| unsat | 0 |  |  |
|auk-0033.smt2                                                |    0.160s | 27.072MiB| sat | 0 |  |  |
|auk-0051.smt2                                                |    0.161s | 21.74MiB| unsat | 0 |  |  |
|auk-0059.smt2                                                |    0.161s | 23.62MiB| unsat | 0 |  |  |
|auk-0165.smt2                                                |    0.161s | 25.992MiB| sat | 0 |  |  |
|auk-0173.smt2                                                |    0.162s | 26.288MiB| sat | 0 |  |  |
|auk-0156.smt2                                                |    0.162s | 25.912MiB| sat | 0 |  |  |
|auk-0174.smt2                                                |    0.164s | 26.38MiB| sat | 0 |  |  |
|auk-0105.smt2                                                |    0.165s | 27.544MiB| sat | 0 |  |  |
|auk-0163.smt2                                                |    0.165s | 26.556MiB| sat | 0 |  |  |
|auk-0018.smt2                                                |    0.165s | 26.728MiB| unsat | 0 |  |  |
|auk-0021.smt2                                                |    0.166s | 24.316MiB| unsat | 0 |  |  |
|auk-0016.smt2                                                |    0.166s | 26.088MiB| unsat | 0 |  |  |
|auk-0100.smt2                                                |    0.166s | 25.28MiB| sat | 0 |  |  |
|auk-0114.smt2                                                |    0.166s | 26.56MiB| sat | 0 |  |  |
|auk-0103.smt2                                                |    0.168s | 29.056MiB| sat | 0 |  |  |
|auk-0037.smt2                                                |    0.169s | 26.8MiB| unsat | 0 |  |  |
|auk-0172.smt2                                                |    0.169s | 26.532MiB| sat | 0 |  |  |
|auk-0175.smt2                                                |    0.169s | 27.0MiB| sat | 0 |  |  |
|auk-0127.smt2                                                |    0.171s | 27.356MiB| sat | 0 |  |  |
|auk-0003.smt2                                                |    0.172s | 28.94MiB| unsat | 0 |  |  |
|auk-0008.smt2                                                |    0.173s | 28.876MiB| unsat | 0 |  |  |
|auk-0014.smt2                                                |    0.174s | 28.748MiB| unsat | 0 |  |  |
|auk-0125.smt2                                                |    0.174s | 28.072MiB| sat | 0 |  |  |
|auk-0045.smt2                                                |    0.174s | 26.12MiB| unsat | 0 |  |  |
|auk-0013.smt2                                                |    0.177s | 28.772MiB| unsat | 0 |  |  |
|auk-0107.smt2                                                |    0.177s | 27.452MiB| sat | 0 |  |  |
|auk-0118.smt2                                                |    0.178s | 27.504MiB| sat | 0 |  |  |
|auk-0110.smt2                                                |    0.178s | 25.524MiB| sat | 0 |  |  |
|auk-0102.smt2                                                |    0.181s | 28.004MiB| sat | 0 |  |  |
|auk-0167.smt2                                                |    0.184s | 25.912MiB| sat | 0 |  |  |
|auk-0153.smt2                                                |    0.184s | 27.544MiB| sat | 0 |  |  |
|auk-0136.smt2                                                |    0.188s | 21.64MiB| sat | 0 |  |  |
|auk-0029.smt2                                                |    0.195s | 27.224MiB| unsat | 0 |  |  |
|auk-0111.smt2                                                |    0.202s | 29.016MiB| sat | 0 |  |  |
|auk-0112.smt2                                                |    0.204s | 27.684MiB| sat | 0 |  |  |
|auk-0030.smt2                                                |    0.206s | 26.924MiB| unsat | 0 |  |  |
|auk-0169.smt2                                                |    0.208s | 27.432MiB| unsat | 0 |  |  |
|auk-0047.smt2                                                |    0.212s | 26.2MiB| unsat | 0 |  |  |
|auk-0124.smt2                                                |    0.212s | 27.76MiB| sat | 0 |  |  |
|auk-0108.smt2                                                |    0.215s | 28.988MiB| sat | 0 |  |  |
|auk-0177.smt2                                                |    0.217s | 27.568MiB| sat | 0 |  |  |
|auk-0154.smt2                                                |    0.223s | 26.82MiB| sat | 0 |  |  |
|auk-0126.smt2                                                |    0.223s | 28.964MiB| sat | 0 |  |  |
|auk-0113.smt2                                                |    0.230s | 30.036MiB| sat | 0 |  |  |
|auk-0017.smt2                                                |    0.232s | 28.112MiB| unsat | 0 |  |  |
|auk-0139.smt2                                                |    0.242s | 22.82MiB| unsat | 0 |  |  |
|auk-0049.smt2                                                |    0.251s | 22.152MiB| unsat | 0 |  |  |
|auk-0150.smt2                                                |    0.270s | 27.96MiB| unsat | 0 |  |  |
|auk-0130.smt2                                                |    0.310s | 33.88MiB| sat | 0 |  |  |
|auk-0023.smt2                                                |    0.316s | 29.708MiB| unsat | 0 |  |  |
|auk-0123.smt2                                                |    0.316s | 32.136MiB| sat | 0 |  |  |
|auk-0128.smt2                                                |    0.321s | 33.688MiB| sat | 0 |  |  |
|auk-0070.smt2                                                |    0.322s | 31.936MiB| sat | 0 |  |  |
|auk-0122.smt2                                                |    0.322s | 31.612MiB| sat | 0 |  |  |
|auk-0116.smt2                                                |    0.330s | 31.888MiB| sat | 0 |  |  |
|auk-0002.smt2                                                |    0.340s | 35.852MiB| unsat | 0 |  |  |
|auk-0190.smt2                                                |    0.347s | 29.62MiB| unsat | 0 |  |  |
|auk-0115.smt2                                                |    0.352s | 36.14MiB| sat | 0 |  |  |
|auk-0119.smt2                                                |    0.355s | 34.96MiB| sat | 0 |  |  |
|auk-0180.smt2                                                |    0.369s | 30.304MiB| unsat | 0 |  |  |
|auk-0117.smt2                                                |    0.429s | 31.608MiB| sat | 0 |  |  |
|auk-0056.smt2                                                |    0.435s | 27.808MiB| sat | 0 |  |  |
|auk-0134.smt2                                                |    0.522s | 33.576MiB| sat | 0 |  |  |
|auk-0135.smt2                                                |    0.522s | 29.308MiB| unsat | 0 |  |  |
|auk-0010.smt2                                                |    0.597s | 37.872MiB| unsat | 0 |  |  |
|auk-0078.smt2                                                |    0.652s | 32.996MiB| sat | 0 |  |  |
|auk-0120.smt2                                                |    0.681s | 32.288MiB| sat | 0 |  |  |
|auk-0096.smt2                                                |    0.684s | 30.032MiB| sat | 0 |  |  |
|auk-0079.smt2                                                |    0.686s | 42.044MiB| sat | 0 |  |  |
|auk-0072.smt2                                                |    0.690s | 32.76MiB| sat | 0 |  |  |
|auk-0091.smt2                                                |    0.811s | 43.28MiB| sat | 0 |  |  |
|auk-0004.smt2                                                |    0.843s | 37.088MiB| unsat | 0 |  |  |
|auk-0151.smt2                                                |    0.905s | 23.044MiB| unsat | 0 |  |  |
|auk-0053.smt2                                                |    0.906s | 23.176MiB| sat | 0 |  |  |
|auk-0077.smt2                                                |    0.933s | 41.396MiB| sat | 0 |  |  |
|auk-0133.smt2                                                |    0.943s | 43.344MiB| sat | 0 |  |  |
|auk-0109.smt2                                                |    0.945s | 96.104MiB| sat | 0 |  |  |
|auk-0145.smt2                                                |    0.995s | 31.876MiB| unsat | 0 |  |  |
|auk-0006.smt2                                                |    1.221s | 38.636MiB| unsat | 0 |  |  |
|auk-0073.smt2                                                |    1.376s | 47.932MiB| sat | 0 |  |  |
|auk-0075.smt2                                                |    1.381s | 48.004MiB| sat | 0 |  |  |
|auk-0071.smt2                                                |    1.391s | 48.132MiB| sat | 0 |  |  |
|auk-0076.smt2                                                |    1.398s | 48.128MiB| sat | 0 |  |  |
|auk-0149.smt2                                                |    1.405s | 29.544MiB| unsat | 0 |  |  |
|auk-0025.smt2                                                |    1.447s | 32.868MiB| unsat | 0 |  |  |
|auk-0132.smt2                                                |    1.627s | 59.736MiB| sat | 0 |  |  |
|auk-0146.smt2                                                |    1.792s | 30.164MiB| unsat | 0 |  |  |
|auk-0094.smt2                                                |    1.871s | 32.932MiB| sat | 0 |  |  |
|auk-0080.smt2                                                |    1.971s | 34.108MiB| sat | 0 |  |  |
|auk-0131.smt2                                                |    2.001s | 96.308MiB| sat | 0 |  |  |
|auk-0011.smt2                                                |    2.036s | 39.08MiB| unsat | 0 |  |  |
|auk-0022.smt2                                                |    2.358s | 31.952MiB| sat | 0 |  |  |
|auk-0101.smt2                                                |    2.385s | 33.584MiB| unsat | 0 |  |  |
|auk-0083.smt2                                                |    3.470s | 100.0MiB| sat | 0 |  |  |
|auk-0082.smt2                                                |    3.542s | 100.0MiB| sat | 0 |  |  |
|auk-0090.smt2                                                |    3.651s | 53.328MiB| unsat | 0 |  |  |
|auk-0148.smt2                                                |    3.713s | 36.004MiB| unsat | 0 |  |  |
|auk-0144.smt2                                                |    4.358s | 35.252MiB| unsat | 0 |  |  |
|auk-0142.smt2                                                |    4.653s | 35.784MiB| unsat | 0 |  |  |
|auk-0095.smt2                                                |    4.866s | 41.124MiB| unsat | 0 |  |  |
|auk-0182.smt2                                                |    6.266s | 63.228MiB| unsat | 0 |  |  |
|auk-0185.smt2                                                |    7.686s | 46.372MiB| unsat | 0 |  |  |
|auk-0027.smt2                                                |    7.803s | 31.228MiB| unsat | 0 |  |  |
|auk-0087.smt2                                                |    9.977s | 59.0MiB| unsat | 0 |  |  |
|auk-0141.smt2                                                |   12.420s | 44.304MiB| unsat | 0 |  |  |
|auk-0001.smt2                                                |   15.018s | 23.944MiB| sat | 0 |  |  |
|auk-0084.smt2                                                |   15.364s | 58.264MiB| unsat | 0 |  |  |
|auk-0007.smt2                                                |   22.401s | 45.352MiB| sat | 0 |  |  |
|auk-0098.smt2                                                |   24.550s | 183.0MiB| unsat | 0 |  |  |
|auk-0099.smt2                                                |   26.485s | 211.0MiB| unsat | 0 |  |  |
|auk-0140.smt2                                                |   36.952s | 43.756MiB| unsat | 0 |  |  |
|auk-0074.smt2                                                |   50.780s | 32.332MiB| unsat | 0 |  |  |
|auk-0183.smt2                                                |   61.641s | 88.068MiB| unsat | 0 |  |  |
|auk-0089.smt2                                                |   61.713s | 73.588MiB| unsat | 0 |  |  |
|auk-0137.smt2                                                |  100.030s | 52.508MiB| timeout | 0 |  |  |
|auk-0081.smt2                                                |  100.035s | 35.356MiB| timeout | 0 |  |  |
|auk-0015.smt2                                                |  100.037s | 56.3MiB| timeout | 0 |  |  |
|auk-0181.smt2                                                |  100.043s | 75.78MiB| timeout | 0 |  |  |
|auk-0191.smt2                                                |  100.046s | 40.452MiB| timeout | 0 |  |  |
|auk-0188.smt2                                                |  100.047s | 61.16MiB| timeout | 0 |  |  |
|auk-0189.smt2                                                |  100.054s | 69.112MiB| timeout | 0 |  |  |
|auk-0187.smt2                                                |  100.055s | 70.184MiB| timeout | 0 |  |  |
|auk-0005.smt2                                                |  100.057s | 50.808MiB| timeout | 0 |  |  |
|auk-0086.smt2                                                |  100.057s | 111.0MiB| timeout | 0 |  |  |
|auk-0184.smt2                                                |  100.058s | 49.792MiB| timeout | 0 |  |  |
|auk-0088.smt2                                                |  100.059s | 105.0MiB| timeout | 0 |  |  |
|auk-0186.smt2                                                |  100.059s | 69.364MiB| timeout | 0 |  |  |
|auk-0085.smt2                                                |  100.065s | 100.0MiB| timeout | 0 |  |  |
|auk-0147.smt2                                                |  100.065s | 53.228MiB| timeout | 0 |  |  |
|auk-0092.smt2                                                |  100.076s | 144.0MiB| timeout | 0 |  |  |
