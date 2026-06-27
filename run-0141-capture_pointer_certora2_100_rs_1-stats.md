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
Job tag: capture_pointer_certora2_100_rs_1
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 1f67f6a2344fc8508ad25381686ce3901cf28ae8
Z3 branch: capture_pointer
Z3 options: "-T:100 smt.random_seed=1"
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
|auk-0060.smt2                                                |    0.039s | 21.36MiB| unsat | 0 |  |  |
|auk-0050.smt2                                                |    0.043s | 21.36MiB| unsat | 0 |  |  |
|auk-0178.smt2                                                |    0.048s | 20.632MiB| sat | 0 |  |  |
|auk-0054.smt2                                                |    0.057s | 19.632MiB| unsat | 0 |  |  |
|auk-0166.smt2                                                |    0.058s | 21.74MiB| sat | 0 |  |  |
|auk-0068.smt2                                                |    0.058s | 20.464MiB| unsat | 0 |  |  |
|auk-0028.smt2                                                |    0.059s | 23.236MiB| unsat | 0 |  |  |
|auk-0067.smt2                                                |    0.059s | 20.776MiB| sat | 0 |  |  |
|auk-0038.smt2                                                |    0.061s | 19.64MiB| unsat | 0 |  |  |
|auk-0039.smt2                                                |    0.062s | 21.584MiB| unsat | 0 |  |  |
|auk-0161.smt2                                                |    0.063s | 20.412MiB| sat | 0 |  |  |
|auk-0179.smt2                                                |    0.064s | 20.88MiB| unsat | 0 |  |  |
|auk-0031.smt2                                                |    0.069s | 21.188MiB| unsat | 0 |  |  |
|auk-0066.smt2                                                |    0.077s | 20.616MiB| sat | 0 |  |  |
|auk-0042.smt2                                                |    0.077s | 20.864MiB| sat | 0 |  |  |
|auk-0138.smt2                                                |    0.077s | 21.496MiB| unsat | 0 |  |  |
|auk-0040.smt2                                                |    0.080s | 21.284MiB| unsat | 0 |  |  |
|auk-0097.smt2                                                |    0.081s | 23.736MiB| sat | 0 |  |  |
|auk-0160.smt2                                                |    0.082s | 21.252MiB| unsat | 0 |  |  |
|auk-0168.smt2                                                |    0.085s | 21.176MiB| sat | 0 |  |  |
|auk-0069.smt2                                                |    0.085s | 20.912MiB| unsat | 0 |  |  |
|auk-0057.smt2                                                |    0.086s | 20.616MiB| unsat | 0 |  |  |
|auk-0065.smt2                                                |    0.091s | 21.164MiB| unsat | 0 |  |  |
|auk-0041.smt2                                                |    0.093s | 21.444MiB| unsat | 0 |  |  |
|auk-0046.smt2                                                |    0.096s | 20.388MiB| unsat | 0 |  |  |
|auk-0032.smt2                                                |    0.097s | 21.072MiB| unsat | 0 |  |  |
|auk-0063.smt2                                                |    0.103s | 21.688MiB| unsat | 0 |  |  |
|auk-0159.smt2                                                |    0.105s | 21.468MiB| unsat | 0 |  |  |
|auk-0012.smt2                                                |    0.106s | 21.384MiB| unsat | 0 |  |  |
|auk-0061.smt2                                                |    0.106s | 21.46MiB| unsat | 0 |  |  |
|auk-0093.smt2                                                |    0.109s | 24.472MiB| sat | 0 |  |  |
|auk-0035.smt2                                                |    0.113s | 20.728MiB| unsat | 0 |  |  |
|auk-0121.smt2                                                |    0.114s | 27.292MiB| sat | 0 |  |  |
|auk-0170.smt2                                                |    0.119s | 21.648MiB| unsat | 0 |  |  |
|auk-0043.smt2                                                |    0.120s | 25.068MiB| unsat | 0 |  |  |
|auk-0026.smt2                                                |    0.120s | 25.788MiB| unsat | 0 |  |  |
|auk-0064.smt2                                                |    0.121s | 21.608MiB| sat | 0 |  |  |
|auk-0044.smt2                                                |    0.122s | 21.508MiB| unsat | 0 |  |  |
|auk-0059.smt2                                                |    0.122s | 23.652MiB| unsat | 0 |  |  |
|auk-0034.smt2                                                |    0.124s | 21.376MiB| unsat | 0 |  |  |
|auk-0051.smt2                                                |    0.129s | 21.88MiB| unsat | 0 |  |  |
|auk-0024.smt2                                                |    0.130s | 26.6MiB| unsat | 0 |  |  |
|auk-0052.smt2                                                |    0.131s | 25.756MiB| unsat | 0 |  |  |
|auk-0055.smt2                                                |    0.131s | 25.74MiB| sat | 0 |  |  |
|auk-0162.smt2                                                |    0.131s | 25.336MiB| sat | 0 |  |  |
|auk-0018.smt2                                                |    0.132s | 26.868MiB| unsat | 0 |  |  |
|auk-0058.smt2                                                |    0.133s | 25.468MiB| sat | 0 |  |  |
|auk-0008.smt2                                                |    0.134s | 28.9MiB| unsat | 0 |  |  |
|auk-0165.smt2                                                |    0.135s | 26.332MiB| sat | 0 |  |  |
|auk-0157.smt2                                                |    0.142s | 25.692MiB| sat | 0 |  |  |
|auk-0136.smt2                                                |    0.142s | 21.6MiB| sat | 0 |  |  |
|auk-0129.smt2                                                |    0.144s | 25.8MiB| sat | 0 |  |  |
|auk-0048.smt2                                                |    0.144s | 26.08MiB| unsat | 0 |  |  |
|auk-0175.smt2                                                |    0.146s | 27.064MiB| sat | 0 |  |  |
|auk-0016.smt2                                                |    0.149s | 26.076MiB| unsat | 0 |  |  |
|auk-0143.smt2                                                |    0.150s | 21.68MiB| unsat | 0 |  |  |
|auk-0174.smt2                                                |    0.151s | 26.44MiB| sat | 0 |  |  |
|auk-0172.smt2                                                |    0.155s | 26.452MiB| sat | 0 |  |  |
|auk-0171.smt2                                                |    0.156s | 26.432MiB| sat | 0 |  |  |
|auk-0176.smt2                                                |    0.159s | 27.016MiB| sat | 0 |  |  |
|auk-0163.smt2                                                |    0.160s | 26.56MiB| sat | 0 |  |  |
|auk-0062.smt2                                                |    0.161s | 25.528MiB| unsat | 0 |  |  |
|auk-0033.smt2                                                |    0.161s | 26.84MiB| sat | 0 |  |  |
|auk-0173.smt2                                                |    0.161s | 26.396MiB| sat | 0 |  |  |
|auk-0125.smt2                                                |    0.163s | 27.312MiB| sat | 0 |  |  |
|auk-0105.smt2                                                |    0.163s | 27.588MiB| sat | 0 |  |  |
|auk-0019.smt2                                                |    0.164s | 26.828MiB| unsat | 0 |  |  |
|auk-0102.smt2                                                |    0.166s | 27.776MiB| sat | 0 |  |  |
|auk-0110.smt2                                                |    0.168s | 25.596MiB| sat | 0 |  |  |
|auk-0127.smt2                                                |    0.169s | 27.344MiB| sat | 0 |  |  |
|auk-0013.smt2                                                |    0.169s | 28.896MiB| unsat | 0 |  |  |
|auk-0003.smt2                                                |    0.170s | 28.92MiB| unsat | 0 |  |  |
|auk-0114.smt2                                                |    0.170s | 26.44MiB| sat | 0 |  |  |
|auk-0155.smt2                                                |    0.171s | 26.108MiB| sat | 0 |  |  |
|auk-0029.smt2                                                |    0.171s | 27.108MiB| unsat | 0 |  |  |
|auk-0118.smt2                                                |    0.172s | 27.452MiB| sat | 0 |  |  |
|auk-0167.smt2                                                |    0.173s | 26.044MiB| sat | 0 |  |  |
|auk-0045.smt2                                                |    0.178s | 26.788MiB| unsat | 0 |  |  |
|auk-0107.smt2                                                |    0.180s | 27.512MiB| sat | 0 |  |  |
|auk-0152.smt2                                                |    0.180s | 25.832MiB| sat | 0 |  |  |
|auk-0169.smt2                                                |    0.180s | 27.884MiB| unsat | 0 |  |  |
|auk-0124.smt2                                                |    0.180s | 27.4MiB| sat | 0 |  |  |
|auk-0047.smt2                                                |    0.184s | 27.312MiB| unsat | 0 |  |  |
|auk-0021.smt2                                                |    0.185s | 24.272MiB| unsat | 0 |  |  |
|auk-0158.smt2                                                |    0.187s | 25.796MiB| sat | 0 |  |  |
|auk-0156.smt2                                                |    0.188s | 25.9MiB| sat | 0 |  |  |
|auk-0100.smt2                                                |    0.189s | 25.42MiB| sat | 0 |  |  |
|auk-0164.smt2                                                |    0.191s | 26.528MiB| sat | 0 |  |  |
|auk-0154.smt2                                                |    0.194s | 26.828MiB| sat | 0 |  |  |
|auk-0020.smt2                                                |    0.197s | 26.552MiB| unsat | 0 |  |  |
|auk-0037.smt2                                                |    0.199s | 26.78MiB| unsat | 0 |  |  |
|auk-0112.smt2                                                |    0.200s | 27.68MiB| sat | 0 |  |  |
|auk-0106.smt2                                                |    0.202s | 27.448MiB| sat | 0 |  |  |
|auk-0153.smt2                                                |    0.207s | 27.616MiB| sat | 0 |  |  |
|auk-0030.smt2                                                |    0.208s | 26.304MiB| unsat | 0 |  |  |
|auk-0177.smt2                                                |    0.208s | 27.648MiB| sat | 0 |  |  |
|auk-0111.smt2                                                |    0.210s | 28.86MiB| sat | 0 |  |  |
|auk-0014.smt2                                                |    0.211s | 28.812MiB| unsat | 0 |  |  |
|auk-0104.smt2                                                |    0.211s | 25.124MiB| sat | 0 |  |  |
|auk-0108.smt2                                                |    0.219s | 29.18MiB| sat | 0 |  |  |
|auk-0103.smt2                                                |    0.219s | 28.62MiB| sat | 0 |  |  |
|auk-0113.smt2                                                |    0.220s | 29.852MiB| sat | 0 |  |  |
|auk-0126.smt2                                                |    0.232s | 28.876MiB| sat | 0 |  |  |
|auk-0017.smt2                                                |    0.235s | 28.124MiB| unsat | 0 |  |  |
|auk-0139.smt2                                                |    0.237s | 22.668MiB| unsat | 0 |  |  |
|auk-0009.smt2                                                |    0.239s | 21.468MiB| sat | 0 |  |  |
|auk-0053.smt2                                                |    0.242s | 22.444MiB| sat | 0 |  |  |
|auk-0123.smt2                                                |    0.281s | 31.996MiB| sat | 0 |  |  |
|auk-0180.smt2                                                |    0.284s | 29.052MiB| unsat | 0 |  |  |
|auk-0117.smt2                                                |    0.289s | 32.332MiB| sat | 0 |  |  |
|auk-0122.smt2                                                |    0.296s | 32.128MiB| sat | 0 |  |  |
|auk-0022.smt2                                                |    0.300s | 29.128MiB| sat | 0 |  |  |
|auk-0010.smt2                                                |    0.300s | 35.336MiB| unsat | 0 |  |  |
|auk-0116.smt2                                                |    0.300s | 31.744MiB| sat | 0 |  |  |
|auk-0119.smt2                                                |    0.307s | 34.16MiB| sat | 0 |  |  |
|auk-0150.smt2                                                |    0.327s | 27.884MiB| unsat | 0 |  |  |
|auk-0096.smt2                                                |    0.357s | 29.692MiB| sat | 0 |  |  |
|auk-0070.smt2                                                |    0.364s | 31.824MiB| sat | 0 |  |  |
|auk-0128.smt2                                                |    0.372s | 33.868MiB| sat | 0 |  |  |
|auk-0056.smt2                                                |    0.372s | 27.924MiB| sat | 0 |  |  |
|auk-0115.smt2                                                |    0.377s | 36.152MiB| sat | 0 |  |  |
|auk-0036.smt2                                                |    0.399s | 22.732MiB| sat | 0 |  |  |
|auk-0130.smt2                                                |    0.420s | 34.216MiB| sat | 0 |  |  |
|auk-0023.smt2                                                |    0.472s | 29.736MiB| unsat | 0 |  |  |
|auk-0049.smt2                                                |    0.505s | 22.816MiB| unsat | 0 |  |  |
|auk-0006.smt2                                                |    0.510s | 36.608MiB| unsat | 0 |  |  |
|auk-0002.smt2                                                |    0.533s | 37.464MiB| unsat | 0 |  |  |
|auk-0078.smt2                                                |    0.647s | 33.016MiB| sat | 0 |  |  |
|auk-0120.smt2                                                |    0.649s | 32.744MiB| sat | 0 |  |  |
|auk-0072.smt2                                                |    0.661s | 32.876MiB| sat | 0 |  |  |
|auk-0135.smt2                                                |    0.777s | 28.9MiB| unsat | 0 |  |  |
|auk-0004.smt2                                                |    0.830s | 37.44MiB| unsat | 0 |  |  |
|auk-0094.smt2                                                |    0.870s | 33.016MiB| sat | 0 |  |  |
|auk-0190.smt2                                                |    0.872s | 32.604MiB| unsat | 0 |  |  |
|auk-0134.smt2                                                |    0.893s | 33.264MiB| sat | 0 |  |  |
|auk-0091.smt2                                                |    0.906s | 44.044MiB| sat | 0 |  |  |
|auk-0109.smt2                                                |    0.920s | 95.94MiB| sat | 0 |  |  |
|auk-0133.smt2                                                |    0.948s | 42.88MiB| sat | 0 |  |  |
|auk-0027.smt2                                                |    0.997s | 29.256MiB| unsat | 0 |  |  |
|auk-0080.smt2                                                |    1.072s | 34.78MiB| sat | 0 |  |  |
|auk-0101.smt2                                                |    1.132s | 32.932MiB| unsat | 0 |  |  |
|auk-0132.smt2                                                |    1.157s | 59.004MiB| sat | 0 |  |  |
|auk-0071.smt2                                                |    1.379s | 48.228MiB| sat | 0 |  |  |
|auk-0073.smt2                                                |    1.398s | 48.02MiB| sat | 0 |  |  |
|auk-0075.smt2                                                |    1.418s | 47.976MiB| sat | 0 |  |  |
|auk-0076.smt2                                                |    1.426s | 48.108MiB| sat | 0 |  |  |
|auk-0149.smt2                                                |    1.609s | 30.612MiB| unsat | 0 |  |  |
|auk-0151.smt2                                                |    1.695s | 23.512MiB| unsat | 0 |  |  |
|auk-0131.smt2                                                |    1.820s | 94.488MiB| sat | 0 |  |  |
|auk-0025.smt2                                                |    2.098s | 34.024MiB| unsat | 0 |  |  |
|auk-0142.smt2                                                |    2.152s | 35.56MiB| unsat | 0 |  |  |
|auk-0077.smt2                                                |    2.164s | 43.028MiB| sat | 0 |  |  |
|auk-0145.smt2                                                |    2.180s | 31.572MiB| unsat | 0 |  |  |
|auk-0146.smt2                                                |    2.408s | 30.532MiB| unsat | 0 |  |  |
|auk-0095.smt2                                                |    2.603s | 39.252MiB| unsat | 0 |  |  |
|auk-0083.smt2                                                |    3.466s | 100.0MiB| sat | 0 |  |  |
|auk-0082.smt2                                                |    3.511s | 100.0MiB| sat | 0 |  |  |
|auk-0148.smt2                                                |    3.610s | 35.712MiB| unsat | 0 |  |  |
|auk-0144.smt2                                                |    4.098s | 37.428MiB| unsat | 0 |  |  |
|auk-0183.smt2                                                |    4.471s | 60.48MiB| unsat | 0 |  |  |
|auk-0182.smt2                                                |    5.904s | 59.536MiB| unsat | 0 |  |  |
|auk-0005.smt2                                                |    6.233s | 39.54MiB| unsat | 0 |  |  |
|auk-0181.smt2                                                |   12.278s | 63.404MiB| unsat | 0 |  |  |
|auk-0186.smt2                                                |   12.348s | 63.724MiB| unsat | 0 |  |  |
|auk-0084.smt2                                                |   12.655s | 56.184MiB| unsat | 0 |  |  |
|auk-0137.smt2                                                |   13.289s | 31.984MiB| sat | 0 |  |  |
|auk-0007.smt2                                                |   15.209s | 42.636MiB| sat | 0 |  |  |
|auk-0098.smt2                                                |   20.157s | 189.0MiB| unsat | 0 |  |  |
|auk-0074.smt2                                                |   25.278s | 32.244MiB| unsat | 0 |  |  |
|auk-0140.smt2                                                |   26.149s | 41.264MiB| unsat | 0 |  |  |
|auk-0090.smt2                                                |   26.230s | 61.732MiB| unsat | 0 |  |  |
|auk-0099.smt2                                                |   27.656s | 224.0MiB| unsat | 0 |  |  |
|auk-0079.smt2                                                |   31.773s | 56.668MiB| sat | 0 |  |  |
|auk-0085.smt2                                                |   39.600s | 91.376MiB| unsat | 0 |  |  |
|auk-0185.smt2                                                |  100.015s | 55.152MiB| timeout | 0 |  |  |
|auk-0187.smt2                                                |  100.018s | 66.532MiB| timeout | 0 |  |  |
|auk-0089.smt2                                                |  100.041s | 75.888MiB| timeout | 0 |  |  |
|auk-0191.smt2                                                |  100.044s | 45.172MiB| timeout | 0 |  |  |
|auk-0188.smt2                                                |  100.048s | 56.176MiB| timeout | 0 |  |  |
|auk-0147.smt2                                                |  100.052s | 53.384MiB| timeout | 0 |  |  |
|auk-0086.smt2                                                |  100.054s | 118.0MiB| timeout | 0 |  |  |
|auk-0001.smt2                                                |  100.063s | 33.36MiB| timeout | 0 |  |  |
|auk-0011.smt2                                                |  100.065s | 52.076MiB| timeout | 0 |  |  |
|auk-0189.smt2                                                |  100.065s | 66.876MiB| timeout | 0 |  |  |
|auk-0092.smt2                                                |  100.066s | 125.0MiB| timeout | 0 |  |  |
|auk-0081.smt2                                                |  100.068s | 34.992MiB| timeout | 0 |  |  |
|auk-0015.smt2                                                |  100.069s | 49.308MiB| timeout | 0 |  |  |
|auk-0184.smt2                                                |  100.070s | 58.996MiB| timeout | 0 |  |  |
|auk-0141.smt2                                                |  100.073s | 77.184MiB| timeout | 0 |  |  |
|auk-0087.smt2                                                |  100.075s | 67.0MiB| timeout | 0 |  |  |
|auk-0088.smt2                                                |  100.086s | 173.0MiB| timeout | 0 |  |  |
