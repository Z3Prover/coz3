# .

* SAT 23
* UNSAT 6
* TIMEOUT 177
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-08-21 00:58:51 UTC
Job description: Z3Prover/bench#3613 A/B, QF_UFNIA restricted to 20240410-certora (the 201903-Zohar-alive family has colon filenames that break upload-artifact). ref=master
Job tag: iss3613-base-QF_UFNIA-certora
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 414189804519311450363b3858ed4a15501a4afe
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_UFNIA.tar.zst?download=1 20240410-certora
Z3 commit message: Fix intblast translation of BV logical shift right (#10596)

Z3 could report `sat` for an `unsat` quantified bit-vector formula when
using `smt.bv.solver=2`. The issue came from translating symbolic
`bvlshr` into an arithmetic `lshr` term in plugin mode, where the
arithmetic axioms were insufficient under quantifiers.

- **BV-to-Int translation**
- Use the existing finite ITE expansion for `bvlshr` while running as
the intblast plugin.
  - Keep the symbolic arithmetic `lshr` path for non-plugin translation.

```cpp
case OP_BLSHR:
    if (!m_is_plugin && !a.is_numeral(arg(0)) && !a.is_numeral(arg(1)))
        r = a.mk_lshr(bv.get_bv_size(e), arg(0), arg(1));
    else {
        expr* x = arg(0), * y = umod(e, 1);
        r = a.mk_int(0);
        for (unsigned i = 0; i < bv.get_bv_size(e); ++i)
            r = if_eq(y, i, a.mk_idiv(x, a.mk_int(rational::power_of_two(i))), r);
    }
```

- **Regression coverage**
- Added a focused `smt_context` regression for the reported quantified
BV shift formula with `smt.bv.solver=2`.

<!-- START COPILOT CODING AGENT SUFFIX -->

- Fixes #10595

---------

Co-authored-by: copilot-swe-agent[bot] <198982749+Copilot@users.noreply.github.com>
Co-authored-by: NikolajBjorner <3085284+NikolajBjorner@users.noreply.github.com>
</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | INFO |
|------------|----------:|---------:|-------------:| ----------:|------|
|lia-splits/0022.smt2                                         |    2.736s | 47.932MiB| sat | 0 |  |
|lia/0026.smt2                                                |    3.983s | 68.568MiB| sat | 0 |  |
|nia-splits/0003.smt2                                         |    4.324s | 39.616MiB| sat | 0 |  |
|lia/0022.smt2                                                |    4.346s | 38.72MiB| sat | 0 |  |
|nia-splits/0002.smt2                                         |    5.030s | 30.04MiB| sat | 0 |  |
|nia/0027.smt2                                                |    6.407s | 39.704MiB| unsat | 0 |  |
|lia/0013.smt2                                                |    6.768s | 33.96MiB| sat | 0 |  |
|lia-splits/0037.smt2                                         |    7.130s | 78.384MiB| sat | 0 |  |
|nia-splits/0033.smt2                                         |    7.148s | 46.672MiB| sat | 0 |  |
|nia-splits/0009.smt2                                         |    7.450s | 34.396MiB| sat | 0 |  |
|nia-splits/0032.smt2                                         |    7.642s | 74.984MiB| sat | 0 |  |
|nia-splits/0014.smt2                                         |    7.883s | 35.892MiB| sat | 0 |  |
|lia/0056.smt2                                                |    8.884s | 87.336MiB| sat | 0 |  |
|lia-splits/0041.smt2                                         |    9.428s | 78.852MiB| sat | 0 |  |
|lia-splits/0034.smt2                                         |   10.018s | 237.0MiB| unsat | 0 |  |
|nia-splits/0041.smt2                                         |   10.245s | 73.088MiB| sat | 0 |  |
|lia-splits/0009.smt2                                         |   10.569s | 77.692MiB| sat | 0 |  |
|nia-splits/0023.smt2                                         |   11.875s | 62.256MiB| sat | 0 |  |
|lia/0051.smt2                                                |   11.981s | 69.656MiB| sat | 0 |  |
|lia-splits/0010.smt2                                         |   13.397s | 122.0MiB| sat | 0 |  |
|lia-splits/0013.smt2                                         |   13.565s | 77.316MiB| sat | 0 |  |
|lia-splits/0048.smt2                                         |   14.229s | 73.976MiB| unsat | 0 |  |
|lia-splits/0005.smt2                                         |   16.566s | 47.5MiB| sat | 0 |  |
|lia-splits/0021.smt2                                         |   16.803s | 119.0MiB| unsat | 0 |  |
|nia-splits/0031.smt2                                         |   16.922s | 44.74MiB| sat | 0 |  |
|lia/0008.smt2                                                |   16.944s | 85.296MiB| sat | 0 |  |
|lia-splits/0001.smt2                                         |   17.711s | 656.0MiB| unsat | 0 |  |
|lia-splits/0033.smt2                                         |   17.822s | 252.0MiB| unsat | 0 |  |
|lia/0049.smt2                                                |   19.409s | 87.672MiB| sat | 0 |  |
|nia/0008.smt2                                                |   20.022s | 59.428MiB| timeout | 0 |  |
|nia/0000.smt2                                                |   20.023s | 40.364MiB| timeout | 0 |  |
|lia/0011.smt2                                                |   20.024s | 41.824MiB| timeout | 0 |  |
|nia/0028.smt2                                                |   20.024s | 71.556MiB| timeout | 0 |  |
|nia-splits/0038.smt2                                         |   20.030s | 39.952MiB| timeout | 0 |  |
|nia-splits/0029.smt2                                         |   20.030s | 44.8MiB| timeout | 0 |  |
|nia/0022.smt2                                                |   20.031s | 144.0MiB| timeout | 0 |  |
|lia-splits/0044.smt2                                         |   20.034s | 82.416MiB| timeout | 0 |  |
|nia-splits/0035.smt2                                         |   20.035s | 29.952MiB| timeout | 0 |  |
|nia-splits/0006.smt2                                         |   20.035s | 32.004MiB| timeout | 0 |  |
|lia/0060.smt2                                                |   20.039s | 83.896MiB| timeout | 0 |  |
|nia-splits/0001.smt2                                         |   20.040s | 58.464MiB| timeout | 0 |  |
|nia-splits/0036.smt2                                         |   20.042s | 27.796MiB| timeout | 0 |  |
|nia/0030.smt2                                                |   20.042s | 248.0MiB| timeout | 0 |  |
|lia-splits/0042.smt2                                         |   20.043s | 124.0MiB| timeout | 0 |  |
|nia-splits/0028.smt2                                         |   20.045s | 42.464MiB| timeout | 0 |  |
|lia/0010.smt2                                                |   20.047s | 85.0MiB| timeout | 0 |  |
|nia/0011.smt2                                                |   20.048s | 42.692MiB| timeout | 0 |  |
|nia/0048.smt2                                                |   20.048s | 65.696MiB| timeout | 0 |  |
|nia/0012.smt2                                                |   20.048s | 75.344MiB| timeout | 0 |  |
|nia-splits/0027.smt2                                         |   20.052s | 42.392MiB| timeout | 0 |  |
|nia-splits/0019.smt2                                         |   20.053s | 52.608MiB| timeout | 0 |  |
|lia-splits/0012.smt2                                         |   20.054s | 49.832MiB| timeout | 0 |  |
|lia/0012.smt2                                                |   20.057s | 56.956MiB| timeout | 0 |  |
|nia-splits/0004.smt2                                         |   20.057s | 44.812MiB| timeout | 0 |  |
|nia-splits/0037.smt2                                         |   20.058s | 26.828MiB| timeout | 0 |  |
|nia-splits/0005.smt2                                         |   20.058s | 33.64MiB| timeout | 0 |  |
|nia-splits/0017.smt2                                         |   20.058s | 103.0MiB| timeout | 0 |  |
|nia/0019.smt2                                                |   20.058s | 40.9MiB| timeout | 0 |  |
|nia-splits/0011.smt2                                         |   20.059s | 67.272MiB| timeout | 0 |  |
|nia-splits/0039.smt2                                         |   20.059s | 48.36MiB| timeout | 0 |  |
|nia/0020.smt2                                                |   20.060s | 133.0MiB| timeout | 0 |  |
|lia/0047.smt2                                                |   20.061s | 60.58MiB| timeout | 0 |  |
|nia-splits/0024.smt2                                         |   20.061s | 50.192MiB| timeout | 0 |  |
|lia/0040.smt2                                                |   20.062s | 157.0MiB| timeout | 0 |  |
|nia-splits/0034.smt2                                         |   20.062s | 99.772MiB| timeout | 0 |  |
|nia-splits/0026.smt2                                         |   20.062s | 135.0MiB| timeout | 0 |  |
|nia/0033.smt2                                                |   20.063s | 149.0MiB| timeout | 0 |  |
|lia-splits/0006.smt2                                         |   20.064s | 33.052MiB| timeout | 0 |  |
|nia/0047.smt2                                                |   20.064s | 117.0MiB| timeout | 0 |  |
|nia-splits/0007.smt2                                         |   20.066s | 128.0MiB| timeout | 0 |  |
|nia/0010.smt2                                                |   20.066s | 68.324MiB| timeout | 0 |  |
|nia-splits/0022.smt2                                         |   20.069s | 190.0MiB| timeout | 0 |  |
|nia-splits/0008.smt2                                         |   20.070s | 88.124MiB| timeout | 0 |  |
|nia/0031.smt2                                                |   20.070s | 213.0MiB| timeout | 0 |  |
|nia/0036.smt2                                                |   20.070s | 77.04MiB| timeout | 0 |  |
|nia-splits/0040.smt2                                         |   20.072s | 228.0MiB| timeout | 0 |  |
|nia/0004.smt2                                                |   20.072s | 34.476MiB| timeout | 0 |  |
|nia-splits/0000.smt2                                         |   20.073s | 51.864MiB| timeout | 0 |  |
|nia/0007.smt2                                                |   20.073s | 109.0MiB| timeout | 0 |  |
|nia/0009.smt2                                                |   20.074s | 80.328MiB| timeout | 0 |  |
|nia-splits/0018.smt2                                         |   20.077s | 90.664MiB| timeout | 0 |  |
|nia-splits/0013.smt2                                         |   20.078s | 117.0MiB| timeout | 0 |  |
|nia/0024.smt2                                                |   20.080s | 248.0MiB| timeout | 0 |  |
|nia-splits/0021.smt2                                         |   20.083s | 158.0MiB| timeout | 0 |  |
|lia-splits/0053.smt2                                         |   20.085s | 103.0MiB| timeout | 0 |  |
|lia/0057.smt2                                                |   20.086s | 88.584MiB| timeout | 0 |  |
|lia/0007.smt2                                                |   20.086s | 54.368MiB| timeout | 0 |  |
|nia/0035.smt2                                                |   20.090s | 163.0MiB| timeout | 0 |  |
|lia/0065.smt2                                                |   20.091s | 108.0MiB| timeout | 0 |  |
|lia-splits/0008.smt2                                         |   20.092s | 121.0MiB| timeout | 0 |  |
|lia-splits/0045.smt2                                         |   20.095s | 95.812MiB| timeout | 0 |  |
|nia/0014.smt2                                                |   20.096s | 143.0MiB| timeout | 0 |  |
|lia/0032.smt2                                                |   20.097s | 143.0MiB| timeout | 0 |  |
|lia-splits/0049.smt2                                         |   20.099s | 107.0MiB| timeout | 0 |  |
|lia/0044.smt2                                                |   20.100s | 349.0MiB| timeout | 0 |  |
|lia-splits/0038.smt2                                         |   20.100s | 37.472MiB| timeout | 0 |  |
|lia-splits/0003.smt2                                         |   20.100s | 57.388MiB| timeout | 0 |  |
|nia/0005.smt2                                                |   20.100s | 33.668MiB| timeout | 0 |  |
|lia/0050.smt2                                                |   20.101s | 87.92MiB| timeout | 0 |  |
|nia-splits/0030.smt2                                         |   20.101s | 51.032MiB| timeout | 0 |  |
|lia-splits/0025.smt2                                         |   20.102s | 99.952MiB| timeout | 0 |  |
|lia/0021.smt2                                                |   20.104s | 120.0MiB| timeout | 0 |  |
|nia-splits/0012.smt2                                         |   20.104s | 98.0MiB| timeout | 0 |  |
|nia/0045.smt2                                                |   20.106s | 47.48MiB| timeout | 0 |  |
|nia/0015.smt2                                                |   20.106s | 97.0MiB| timeout | 0 |  |
|nia-splits/0025.smt2                                         |   20.109s | 212.0MiB| timeout | 0 |  |
|lia-splits/0014.smt2                                         |   20.109s | 122.0MiB| timeout | 0 |  |
|lia/0015.smt2                                                |   20.112s | 149.0MiB| timeout | 0 |  |
|lia-splits/0024.smt2                                         |   20.112s | 123.0MiB| timeout | 0 |  |
|nia/0003.smt2                                                |   20.112s | 35.356MiB| timeout | 0 |  |
|lia/0039.smt2                                                |   20.114s | 134.0MiB| timeout | 0 |  |
|lia/0005.smt2                                                |   20.114s | 156.0MiB| timeout | 0 |  |
|lia/0027.smt2                                                |   20.115s | 199.0MiB| timeout | 0 |  |
|lia-splits/0000.smt2                                         |   20.115s | 104.0MiB| timeout | 0 |  |
|lia-splits/0032.smt2                                         |   20.115s | 154.0MiB| timeout | 0 |  |
|lia-splits/0019.smt2                                         |   20.115s | 92.624MiB| timeout | 0 |  |
|nia/0002.smt2                                                |   20.115s | 43.748MiB| timeout | 0 |  |
|lia/0061.smt2                                                |   20.116s | 101.0MiB| timeout | 0 |  |
|lia/0016.smt2                                                |   20.117s | 96.8MiB| timeout | 0 |  |
|lia/0009.smt2                                                |   20.118s | 72.348MiB| timeout | 0 |  |
|lia-splits/0015.smt2                                         |   20.118s | 153.0MiB| timeout | 0 |  |
|nia/0042.smt2                                                |   20.118s | 64.82MiB| timeout | 0 |  |
|nia/0023.smt2                                                |   20.119s | 65.516MiB| timeout | 0 |  |
|nia/0006.smt2                                                |   20.120s | 32.844MiB| timeout | 0 |  |
|nia/0040.smt2                                                |   20.120s | 47.272MiB| timeout | 0 |  |
|nia/0029.smt2                                                |   20.122s | 42.672MiB| timeout | 0 |  |
|nia/0044.smt2                                                |   20.124s | 67.332MiB| timeout | 0 |  |
|nia/0034.smt2                                                |   20.124s | 39.636MiB| timeout | 0 |  |
|nia-splits/0015.smt2                                         |   20.125s | 438.0MiB| timeout | 0 |  |
|lia-splits/0023.smt2                                         |   20.126s | 184.0MiB| timeout | 0 |  |
|nia/0041.smt2                                                |   20.126s | 60.812MiB| timeout | 0 |  |
|nia/0001.smt2                                                |   20.126s | 44.312MiB| timeout | 0 |  |
|lia/0043.smt2                                                |   20.128s | 236.0MiB| timeout | 0 |  |
|lia-splits/0035.smt2                                         |   20.128s | 134.0MiB| timeout | 0 |  |
|lia-splits/0029.smt2                                         |   20.128s | 127.0MiB| timeout | 0 |  |
|nia/0049.smt2                                                |   20.128s | 61.46MiB| timeout | 0 |  |
|lia-splits/0004.smt2                                         |   20.132s | 65.164MiB| timeout | 0 |  |
|lia/0006.smt2                                                |   20.134s | 118.0MiB| timeout | 0 |  |
|lia/0019.smt2                                                |   20.137s | 38.356MiB| timeout | 0 |  |
|nia-splits/0016.smt2                                         |   20.137s | 425.0MiB| timeout | 0 |  |
|lia/0004.smt2                                                |   20.139s | 321.0MiB| timeout | 0 |  |
|lia-splits/0040.smt2                                         |   20.145s | 255.0MiB| timeout | 0 |  |
|nia/0046.smt2                                                |   20.146s | 99.0MiB| timeout | 0 |  |
|lia-splits/0043.smt2                                         |   20.148s | 276.0MiB| timeout | 0 |  |
|lia-splits/0007.smt2                                         |   20.149s | 150.0MiB| timeout | 0 |  |
|lia-splits/0051.smt2                                         |   20.150s | 237.0MiB| timeout | 0 |  |
|lia/0037.smt2                                                |   20.151s | 266.0MiB| timeout | 0 |  |
|nia/0018.smt2                                                |   20.153s | 99.612MiB| timeout | 0 |  |
|lia/0003.smt2                                                |   20.157s | 328.0MiB| timeout | 0 |  |
|nia/0017.smt2                                                |   20.157s | 98.0MiB| timeout | 0 |  |
|lia/0036.smt2                                                |   20.159s | 238.0MiB| timeout | 0 |  |
|nia/0051.smt2                                                |   20.160s | 121.0MiB| timeout | 0 |  |
|nia/0016.smt2                                                |   20.162s | 133.0MiB| timeout | 0 |  |
|lia/0033.smt2                                                |   20.165s | 103.0MiB| timeout | 0 |  |
|lia-splits/0047.smt2                                         |   20.165s | 209.0MiB| timeout | 0 |  |
|lia/0017.smt2                                                |   20.168s | 124.0MiB| timeout | 0 |  |
|nia/0038.smt2                                                |   20.168s | 200.0MiB| timeout | 0 |  |
|lia/0045.smt2                                                |   20.169s | 130.0MiB| timeout | 0 |  |
|lia/0031.smt2                                                |   20.170s | 154.0MiB| timeout | 0 |  |
|lia-splits/0027.smt2                                         |   20.170s | 126.0MiB| timeout | 0 |  |
|lia/0067.smt2                                                |   20.172s | 228.0MiB| timeout | 0 |  |
|lia/0052.smt2                                                |   20.174s | 237.0MiB| timeout | 0 |  |
|lia-splits/0028.smt2                                         |   20.175s | 456.0MiB| timeout | 0 |  |
|lia-splits/0002.smt2                                         |   20.176s | 212.0MiB| timeout | 0 |  |
|lia/0059.smt2                                                |   20.177s | 487.0MiB| timeout | 0 |  |
|lia-splits/0030.smt2                                         |   20.177s | 152.0MiB| timeout | 0 |  |
|lia-splits/0039.smt2                                         |   20.178s | 241.0MiB| timeout | 0 |  |
|nia/0021.smt2                                                |   20.178s | 305.0MiB| timeout | 0 |  |
|lia/0038.smt2                                                |   20.179s | 167.0MiB| timeout | 0 |  |
|lia-splits/0031.smt2                                         |   20.181s | 173.0MiB| timeout | 0 |  |
|lia/0030.smt2                                                |   20.182s | 248.0MiB| timeout | 0 |  |
|nia/0026.smt2                                                |   20.186s | 203.0MiB| timeout | 0 |  |
|lia/0064.smt2                                                |   20.192s | 230.0MiB| timeout | 0 |  |
|lia-splits/0046.smt2                                         |   20.193s | 774.0MiB| timeout | 0 |  |
|nia/0032.smt2                                                |   20.198s | 236.0MiB| timeout | 0 |  |
|nia/0025.smt2                                                |   20.199s | 248.0MiB| timeout | 0 |  |
|lia/0053.smt2                                                |   20.200s | 266.0MiB| timeout | 0 |  |
|nia/0037.smt2                                                |   20.200s | 208.0MiB| timeout | 0 |  |
|lia-splits/0016.smt2                                         |   20.201s | 932.0MiB| timeout | 0 |  |
|lia/0042.smt2                                                |   20.203s | 246.0MiB| timeout | 0 |  |
|nia-splits/0020.smt2                                         |   20.210s | 396.0MiB| timeout | 0 |  |
|lia/0035.smt2                                                |   20.211s | 248.0MiB| timeout | 0 |  |
|lia/0029.smt2                                                |   20.214s | 248.0MiB| timeout | 0 |  |
|nia/0050.smt2                                                |   20.215s | 273.0MiB| timeout | 0 |  |
|lia/0063.smt2                                                |   20.217s | 914.0MiB| timeout | 0 |  |
|lia/0024.smt2                                                |   20.217s | 355.0MiB| timeout | 0 |  |
|lia-splits/0020.smt2                                         |   20.218s | 483.0MiB| timeout | 0 |  |
|nia/0039.smt2                                                |   20.220s | 357.0MiB| timeout | 0 |  |
|lia/0000.smt2                                                |   20.222s | 479.0MiB| timeout | 0 |  |
|lia/0001.smt2                                                |   20.228s | 503.0MiB| timeout | 0 |  |
|lia-splits/0050.smt2                                         |   20.228s | 530.0MiB| timeout | 0 |  |
|lia/0058.smt2                                                |   20.230s | 467.0MiB| timeout | 0 |  |
|lia-splits/0052.smt2                                         |   20.232s | 686.0MiB| timeout | 0 |  |
|lia/0034.smt2                                                |   20.238s | 651.0MiB| timeout | 0 |  |
|lia-splits/0026.smt2                                         |   20.239s | 424.0MiB| timeout | 0 |  |
|lia/0002.smt2                                                |   20.241s | 1036.0MiB| timeout | 0 |  |
|lia-splits/0018.smt2                                         |   20.250s | 933.0MiB| timeout | 0 |  |
|lia-splits/0017.smt2                                         |   20.252s | 933.0MiB| timeout | 0 |  |
|lia/0046.smt2                                                |   20.265s | 905.0MiB| timeout | 0 |  |
|lia/0066.smt2                                                |   20.305s | 1025.0MiB| timeout | 0 |  |
|lia-splits/0036.smt2                                         |   20.321s | 932.0MiB| timeout | 0 |  |
|lia/0062.smt2                                                |   20.341s | 932.0MiB| timeout | 0 |  |
|lia/0048.smt2                                                |   20.381s | 935.0MiB| timeout | 0 |  |
|lia/0054.smt2                                                |   20.391s | 939.0MiB| timeout | 0 |  |
|lia/0055.smt2                                                |   20.397s | 939.0MiB| timeout | 0 |  |
|lia/0041.smt2                                                |   20.461s | 1854.0MiB| timeout | 0 |  |
