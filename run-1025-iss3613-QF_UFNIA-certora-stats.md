# .

* SAT 24
* UNSAT 5
* TIMEOUT 177
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-08-21 00:56:00 UTC
Job description: Z3Prover/bench#3613 A/B, QF_UFNIA restricted to 20240410-certora (the 201903-Zohar-alive family has colon filenames that break upload-artifact). ref=iss3613-bool_var2bound-dense
Job tag: iss3613-QF_UFNIA-certora
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 7ba26830413225af507adb14cb99c16e129dbc5b
Z3 branch: iss3613-bool_var2bound-dense
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_UFNIA.tar.zst?download=1 20240410-certora
Z3 commit message: theory_lra: index m_bool_var2bound by bool_var instead of hashing it

m_bool_var2bound was a u_map<api_bound*>. u_hash is the identity, so
every key lands at slot (bv & mask), and internalize_atom does
erase(bv) immediately before insert(bv, b). core_hashtable::insert
only stops its linear probe at a FREE slot -- a deleted slot is
remembered as a reuse candidate but does not terminate the scan.

bool_vars are recycled on backtracking, so on quantifier- and
theory-driven workloads the same small id range is internalized over
and over. The live keys then occupy a contiguous prefix of the table
that is permanently used-or-deleted, and each insert walks that whole
prefix looking for a free slot. The operation becomes
O(#distinct bool_vars) instead of O(1).

On inputs/issues/iss-2178/bug-1.smt2 (AUFLIRA, Z3Prover/bench#3613)
this made the insert alone 18.8% of all instructions: 147,668 inserts
costing 11,279 instructions each. Replaying the real key trace through
the probing logic gives 207,448,738 probes, 1,404.8 per insert, for a
table holding only 8,699 live keys.

bool_var ids are dense small unsigned integers, and smt already keeps
several bool_var-indexed vectors, so index a ptr_vector directly.
api_bound* is never null when stored, which makes the
find/contains -> get(bv, nullptr) rewrite exact.

Measured on bug-1.smt2, same tree and flags, identical search
(conflicts, decisions, mk-bool-var, quant-instantiations, rlimit-count
and memory all unchanged):

  instructions, rlimit=3000000   8,872,944,804 -> 7,202,192,073  -18.8%
  wall, rlimit= 3,000,000              1.55s -> 1.40s   -10%
  wall, rlimit=20,000,000             17.48s -> 12.84s  -27%
  wall, rlimit=60,000,000             71.41s -> 43.44s  -39%

No memory regression: max-memory 187.47 -> 186.29 MB.

A differential run over 600 benchmarks from Z3Prover/bench at
rlimit=2000000, comparing full stdout and stderr, shows no differences.

src/sat/smt/arith_solver.h has the same u_map and the same
erase-then-insert pattern; it is left alone here.
</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | INFO |
|------------|----------:|---------:|-------------:| ----------:|------|
|lia-splits/0022.smt2                                         |    2.590s | 47.8MiB| sat | 0 |  |
|nia-splits/0002.smt2                                         |    3.129s | 29.868MiB| sat | 0 |  |
|lia/0022.smt2                                                |    4.037s | 38.564MiB| sat | 0 |  |
|lia/0026.smt2                                                |    4.127s | 68.38MiB| sat | 0 |  |
|nia-splits/0003.smt2                                         |    5.151s | 39.4MiB| sat | 0 |  |
|lia/0056.smt2                                                |    5.706s | 86.776MiB| sat | 0 |  |
|lia-splits/0037.smt2                                         |    6.098s | 78.012MiB| sat | 0 |  |
|nia-splits/0009.smt2                                         |    6.207s | 34.332MiB| sat | 0 |  |
|nia/0027.smt2                                                |    6.219s | 39.596MiB| unsat | 0 |  |
|nia-splits/0032.smt2                                         |    6.443s | 74.664MiB| sat | 0 |  |
|nia-splits/0033.smt2                                         |    6.817s | 46.616MiB| sat | 0 |  |
|lia/0013.smt2                                                |    7.418s | 33.62MiB| sat | 0 |  |
|lia/0051.smt2                                                |    8.099s | 69.224MiB| sat | 0 |  |
|lia-splits/0034.smt2                                         |    8.668s | 236.0MiB| unsat | 0 |  |
|nia-splits/0041.smt2                                         |    9.392s | 73.08MiB| sat | 0 |  |
|lia-splits/0009.smt2                                         |    9.884s | 77.344MiB| sat | 0 |  |
|lia-splits/0041.smt2                                         |   10.441s | 78.276MiB| sat | 0 |  |
|nia-splits/0014.smt2                                         |   10.471s | 35.912MiB| sat | 0 |  |
|nia-splits/0023.smt2                                         |   10.767s | 62.012MiB| sat | 0 |  |
|nia-splits/0031.smt2                                         |   12.181s | 44.62MiB| sat | 0 |  |
|lia-splits/0013.smt2                                         |   13.344s | 77.048MiB| sat | 0 |  |
|lia-splits/0021.smt2                                         |   13.870s | 118.0MiB| unsat | 0 |  |
|lia-splits/0048.smt2                                         |   15.214s | 73.7MiB| unsat | 0 |  |
|lia-splits/0005.smt2                                         |   15.808s | 47.084MiB| sat | 0 |  |
|lia/0008.smt2                                                |   17.460s | 84.872MiB| sat | 0 |  |
|lia/0049.smt2                                                |   18.051s | 86.956MiB| sat | 0 |  |
|lia-splits/0001.smt2                                         |   18.072s | 656.0MiB| unsat | 0 |  |
|lia-splits/0010.smt2                                         |   18.603s | 122.0MiB| sat | 0 |  |
|lia/0011.smt2                                                |   18.681s | 47.144MiB| sat | 0 |  |
|nia/0006.smt2                                                |   20.015s | 32.824MiB| timeout | 0 |  |
|nia/0011.smt2                                                |   20.020s | 43.72MiB| timeout | 0 |  |
|nia-splits/0026.smt2                                         |   20.028s | 95.68MiB| timeout | 0 |  |
|nia-splits/0011.smt2                                         |   20.039s | 68.22MiB| timeout | 0 |  |
|lia-splits/0003.smt2                                         |   20.040s | 67.252MiB| timeout | 0 |  |
|nia-splits/0035.smt2                                         |   20.041s | 30.692MiB| timeout | 0 |  |
|nia-splits/0029.smt2                                         |   20.046s | 46.12MiB| timeout | 0 |  |
|lia-splits/0049.smt2                                         |   20.048s | 106.0MiB| timeout | 0 |  |
|nia-splits/0001.smt2                                         |   20.049s | 58.872MiB| timeout | 0 |  |
|nia/0020.smt2                                                |   20.052s | 133.0MiB| timeout | 0 |  |
|nia-splits/0027.smt2                                         |   20.054s | 41.284MiB| timeout | 0 |  |
|nia-splits/0036.smt2                                         |   20.054s | 28.668MiB| timeout | 0 |  |
|lia-splits/0006.smt2                                         |   20.054s | 32.824MiB| timeout | 0 |  |
|nia-splits/0024.smt2                                         |   20.056s | 50.532MiB| timeout | 0 |  |
|nia-splits/0039.smt2                                         |   20.058s | 48.476MiB| timeout | 0 |  |
|nia/0003.smt2                                                |   20.058s | 34.832MiB| timeout | 0 |  |
|nia-splits/0037.smt2                                         |   20.059s | 26.1MiB| timeout | 0 |  |
|nia/0028.smt2                                                |   20.060s | 67.772MiB| timeout | 0 |  |
|nia-splits/0006.smt2                                         |   20.061s | 31.748MiB| timeout | 0 |  |
|nia/0033.smt2                                                |   20.062s | 147.0MiB| timeout | 0 |  |
|nia/0048.smt2                                                |   20.064s | 59.532MiB| timeout | 0 |  |
|nia-splits/0028.smt2                                         |   20.065s | 44.02MiB| timeout | 0 |  |
|nia-splits/0038.smt2                                         |   20.066s | 39.752MiB| timeout | 0 |  |
|nia/0031.smt2                                                |   20.066s | 216.0MiB| timeout | 0 |  |
|nia-splits/0012.smt2                                         |   20.068s | 99.276MiB| timeout | 0 |  |
|nia-splits/0000.smt2                                         |   20.069s | 52.944MiB| timeout | 0 |  |
|nia/0012.smt2                                                |   20.069s | 77.128MiB| timeout | 0 |  |
|nia-splits/0004.smt2                                         |   20.070s | 45.608MiB| timeout | 0 |  |
|lia/0065.smt2                                                |   20.073s | 108.0MiB| timeout | 0 |  |
|nia-splits/0005.smt2                                         |   20.074s | 33.548MiB| timeout | 0 |  |
|lia-splits/0038.smt2                                         |   20.074s | 39.62MiB| timeout | 0 |  |
|nia-splits/0008.smt2                                         |   20.075s | 92.2MiB| timeout | 0 |  |
|nia/0005.smt2                                                |   20.075s | 33.812MiB| timeout | 0 |  |
|nia-splits/0018.smt2                                         |   20.078s | 90.828MiB| timeout | 0 |  |
|nia-splits/0030.smt2                                         |   20.079s | 51.184MiB| timeout | 0 |  |
|nia-splits/0034.smt2                                         |   20.080s | 99.064MiB| timeout | 0 |  |
|lia-splits/0014.smt2                                         |   20.081s | 122.0MiB| timeout | 0 |  |
|nia/0001.smt2                                                |   20.081s | 43.94MiB| timeout | 0 |  |
|nia/0022.smt2                                                |   20.081s | 140.0MiB| timeout | 0 |  |
|nia-splits/0040.smt2                                         |   20.083s | 227.0MiB| timeout | 0 |  |
|nia-splits/0019.smt2                                         |   20.084s | 52.304MiB| timeout | 0 |  |
|nia/0010.smt2                                                |   20.085s | 68.14MiB| timeout | 0 |  |
|nia/0019.smt2                                                |   20.085s | 40.884MiB| timeout | 0 |  |
|lia-splits/0042.smt2                                         |   20.086s | 124.0MiB| timeout | 0 |  |
|lia-splits/0019.smt2                                         |   20.086s | 92.368MiB| timeout | 0 |  |
|nia/0034.smt2                                                |   20.086s | 40.612MiB| timeout | 0 |  |
|lia/0019.smt2                                                |   20.087s | 39.964MiB| timeout | 0 |  |
|lia-splits/0029.smt2                                         |   20.088s | 125.0MiB| timeout | 0 |  |
|nia/0000.smt2                                                |   20.088s | 42.444MiB| timeout | 0 |  |
|lia/0045.smt2                                                |   20.089s | 129.0MiB| timeout | 0 |  |
|lia-splits/0012.smt2                                         |   20.089s | 49.324MiB| timeout | 0 |  |
|lia-splits/0024.smt2                                         |   20.090s | 128.0MiB| timeout | 0 |  |
|nia-splits/0017.smt2                                         |   20.091s | 103.0MiB| timeout | 0 |  |
|nia/0008.smt2                                                |   20.092s | 58.24MiB| timeout | 0 |  |
|lia-splits/0051.smt2                                         |   20.093s | 241.0MiB| timeout | 0 |  |
|nia/0029.smt2                                                |   20.093s | 43.564MiB| timeout | 0 |  |
|lia/0060.smt2                                                |   20.094s | 83.128MiB| timeout | 0 |  |
|lia-splits/0015.smt2                                         |   20.094s | 154.0MiB| timeout | 0 |  |
|lia-splits/0004.smt2                                         |   20.094s | 61.952MiB| timeout | 0 |  |
|nia/0046.smt2                                                |   20.094s | 98.28MiB| timeout | 0 |  |
|nia/0018.smt2                                                |   20.095s | 105.0MiB| timeout | 0 |  |
|lia-splits/0025.smt2                                         |   20.096s | 98.68MiB| timeout | 0 |  |
|lia/0009.smt2                                                |   20.099s | 79.388MiB| timeout | 0 |  |
|lia-splits/0035.smt2                                         |   20.099s | 133.0MiB| timeout | 0 |  |
|lia-splits/0023.smt2                                         |   20.099s | 176.0MiB| timeout | 0 |  |
|lia/0061.smt2                                                |   20.101s | 116.0MiB| timeout | 0 |  |
|nia/0040.smt2                                                |   20.101s | 48.74MiB| timeout | 0 |  |
|nia/0036.smt2                                                |   20.103s | 82.516MiB| timeout | 0 |  |
|lia/0017.smt2                                                |   20.104s | 123.0MiB| timeout | 0 |  |
|nia-splits/0021.smt2                                         |   20.104s | 163.0MiB| timeout | 0 |  |
|nia/0041.smt2                                                |   20.104s | 60.32MiB| timeout | 0 |  |
|nia/0004.smt2                                                |   20.104s | 34.832MiB| timeout | 0 |  |
|nia-splits/0022.smt2                                         |   20.105s | 190.0MiB| timeout | 0 |  |
|nia/0009.smt2                                                |   20.106s | 80.58MiB| timeout | 0 |  |
|lia-splits/0031.smt2                                         |   20.107s | 172.0MiB| timeout | 0 |  |
|lia-splits/0040.smt2                                         |   20.107s | 267.0MiB| timeout | 0 |  |
|nia-splits/0013.smt2                                         |   20.108s | 120.0MiB| timeout | 0 |  |
|nia/0045.smt2                                                |   20.109s | 49.612MiB| timeout | 0 |  |
|lia-splits/0045.smt2                                         |   20.110s | 95.728MiB| timeout | 0 |  |
|nia-splits/0016.smt2                                         |   20.111s | 406.0MiB| timeout | 0 |  |
|nia-splits/0007.smt2                                         |   20.112s | 121.0MiB| timeout | 0 |  |
|lia-splits/0053.smt2                                         |   20.112s | 103.0MiB| timeout | 0 |  |
|nia/0007.smt2                                                |   20.112s | 110.0MiB| timeout | 0 |  |
|nia-splits/0025.smt2                                         |   20.113s | 213.0MiB| timeout | 0 |  |
|lia-splits/0008.smt2                                         |   20.114s | 111.0MiB| timeout | 0 |  |
|lia/0050.smt2                                                |   20.115s | 86.168MiB| timeout | 0 |  |
|lia/0012.smt2                                                |   20.115s | 54.976MiB| timeout | 0 |  |
|lia/0057.smt2                                                |   20.116s | 100.0MiB| timeout | 0 |  |
|lia-splits/0000.smt2                                         |   20.117s | 108.0MiB| timeout | 0 |  |
|nia/0044.smt2                                                |   20.117s | 58.12MiB| timeout | 0 |  |
|nia/0014.smt2                                                |   20.117s | 157.0MiB| timeout | 0 |  |
|lia/0032.smt2                                                |   20.118s | 143.0MiB| timeout | 0 |  |
|nia/0047.smt2                                                |   20.118s | 117.0MiB| timeout | 0 |  |
|nia/0042.smt2                                                |   20.121s | 66.564MiB| timeout | 0 |  |
|lia/0010.smt2                                                |   20.122s | 84.432MiB| timeout | 0 |  |
|lia/0007.smt2                                                |   20.122s | 56.124MiB| timeout | 0 |  |
|lia-splits/0007.smt2                                         |   20.122s | 143.0MiB| timeout | 0 |  |
|lia/0021.smt2                                                |   20.123s | 114.0MiB| timeout | 0 |  |
|nia-splits/0020.smt2                                         |   20.124s | 356.0MiB| timeout | 0 |  |
|nia/0023.smt2                                                |   20.124s | 65.5MiB| timeout | 0 |  |
|lia-splits/0002.smt2                                         |   20.125s | 224.0MiB| timeout | 0 |  |
|lia/0047.smt2                                                |   20.126s | 56.856MiB| timeout | 0 |  |
|lia-splits/0044.smt2                                         |   20.126s | 88.164MiB| timeout | 0 |  |
|lia/0006.smt2                                                |   20.131s | 122.0MiB| timeout | 0 |  |
|nia-splits/0015.smt2                                         |   20.131s | 467.0MiB| timeout | 0 |  |
|lia/0016.smt2                                                |   20.134s | 103.0MiB| timeout | 0 |  |
|lia/0039.smt2                                                |   20.135s | 124.0MiB| timeout | 0 |  |
|lia-splits/0046.smt2                                         |   20.136s | 706.0MiB| timeout | 0 |  |
|lia/0038.smt2                                                |   20.137s | 166.0MiB| timeout | 0 |  |
|lia-splits/0027.smt2                                         |   20.138s | 120.0MiB| timeout | 0 |  |
|lia/0031.smt2                                                |   20.139s | 156.0MiB| timeout | 0 |  |
|nia/0002.smt2                                                |   20.139s | 46.432MiB| timeout | 0 |  |
|lia/0033.smt2                                                |   20.140s | 111.0MiB| timeout | 0 |  |
|lia-splits/0032.smt2                                         |   20.140s | 152.0MiB| timeout | 0 |  |
|nia/0049.smt2                                                |   20.140s | 61.692MiB| timeout | 0 |  |
|lia-splits/0043.smt2                                         |   20.141s | 277.0MiB| timeout | 0 |  |
|lia-splits/0030.smt2                                         |   20.141s | 142.0MiB| timeout | 0 |  |
|nia/0017.smt2                                                |   20.142s | 100.0MiB| timeout | 0 |  |
|lia-splits/0033.smt2                                         |   20.143s | 251.0MiB| timeout | 0 |  |
|lia/0005.smt2                                                |   20.144s | 158.0MiB| timeout | 0 |  |
|nia/0016.smt2                                                |   20.145s | 133.0MiB| timeout | 0 |  |
|lia/0042.smt2                                                |   20.147s | 244.0MiB| timeout | 0 |  |
|nia/0037.smt2                                                |   20.149s | 208.0MiB| timeout | 0 |  |
|nia/0030.smt2                                                |   20.149s | 248.0MiB| timeout | 0 |  |
|lia/0027.smt2                                                |   20.153s | 192.0MiB| timeout | 0 |  |
|nia/0038.smt2                                                |   20.153s | 205.0MiB| timeout | 0 |  |
|nia/0026.smt2                                                |   20.153s | 198.0MiB| timeout | 0 |  |
|nia/0051.smt2                                                |   20.157s | 120.0MiB| timeout | 0 |  |
|lia/0015.smt2                                                |   20.159s | 146.0MiB| timeout | 0 |  |
|lia-splits/0016.smt2                                         |   20.161s | 932.0MiB| timeout | 0 |  |
|nia/0039.smt2                                                |   20.161s | 309.0MiB| timeout | 0 |  |
|nia/0035.smt2                                                |   20.162s | 161.0MiB| timeout | 0 |  |
|lia/0040.smt2                                                |   20.165s | 172.0MiB| timeout | 0 |  |
|nia/0015.smt2                                                |   20.166s | 97.0MiB| timeout | 0 |  |
|lia/0036.smt2                                                |   20.170s | 247.0MiB| timeout | 0 |  |
|lia-splits/0047.smt2                                         |   20.171s | 214.0MiB| timeout | 0 |  |
|lia-splits/0017.smt2                                         |   20.172s | 933.0MiB| timeout | 0 |  |
|lia/0035.smt2                                                |   20.173s | 248.0MiB| timeout | 0 |  |
|lia/0064.smt2                                                |   20.174s | 227.0MiB| timeout | 0 |  |
|lia/0052.smt2                                                |   20.176s | 241.0MiB| timeout | 0 |  |
|nia/0021.smt2                                                |   20.177s | 303.0MiB| timeout | 0 |  |
|lia/0030.smt2                                                |   20.178s | 248.0MiB| timeout | 0 |  |
|lia-splits/0039.smt2                                         |   20.178s | 240.0MiB| timeout | 0 |  |
|lia/0067.smt2                                                |   20.182s | 233.0MiB| timeout | 0 |  |
|lia/0004.smt2                                                |   20.190s | 328.0MiB| timeout | 0 |  |
|lia/0024.smt2                                                |   20.192s | 350.0MiB| timeout | 0 |  |
|nia/0032.smt2                                                |   20.193s | 234.0MiB| timeout | 0 |  |
|lia/0044.smt2                                                |   20.194s | 324.0MiB| timeout | 0 |  |
|lia-splits/0020.smt2                                         |   20.194s | 441.0MiB| timeout | 0 |  |
|nia/0024.smt2                                                |   20.194s | 248.0MiB| timeout | 0 |  |
|lia/0043.smt2                                                |   20.196s | 245.0MiB| timeout | 0 |  |
|lia/0029.smt2                                                |   20.198s | 248.0MiB| timeout | 0 |  |
|lia-splits/0018.smt2                                         |   20.201s | 933.0MiB| timeout | 0 |  |
|lia-splits/0050.smt2                                         |   20.203s | 485.0MiB| timeout | 0 |  |
|lia-splits/0026.smt2                                         |   20.204s | 401.0MiB| timeout | 0 |  |
|nia/0050.smt2                                                |   20.204s | 272.0MiB| timeout | 0 |  |
|lia/0001.smt2                                                |   20.212s | 503.0MiB| timeout | 0 |  |
|lia/0066.smt2                                                |   20.216s | 482.0MiB| timeout | 0 |  |
|lia-splits/0028.smt2                                         |   20.220s | 450.0MiB| timeout | 0 |  |
|lia/0000.smt2                                                |   20.223s | 479.0MiB| timeout | 0 |  |
|lia-splits/0052.smt2                                         |   20.231s | 628.0MiB| timeout | 0 |  |
|nia/0025.smt2                                                |   20.232s | 248.0MiB| timeout | 0 |  |
|lia/0037.smt2                                                |   20.251s | 261.0MiB| timeout | 0 |  |
|lia/0003.smt2                                                |   20.268s | 326.0MiB| timeout | 0 |  |
|lia/0053.smt2                                                |   20.279s | 282.0MiB| timeout | 0 |  |
|lia-splits/0036.smt2                                         |   20.290s | 932.0MiB| timeout | 0 |  |
|lia/0054.smt2                                                |   20.292s | 939.0MiB| timeout | 0 |  |
|lia/0055.smt2                                                |   20.304s | 939.0MiB| timeout | 0 |  |
|lia/0046.smt2                                                |   20.306s | 478.0MiB| timeout | 0 |  |
|lia/0002.smt2                                                |   20.315s | 476.0MiB| timeout | 0 |  |
|lia/0058.smt2                                                |   20.321s | 494.0MiB| timeout | 0 |  |
|lia/0059.smt2                                                |   20.323s | 500.0MiB| timeout | 0 |  |
|lia/0034.smt2                                                |   20.331s | 621.0MiB| timeout | 0 |  |
|lia/0063.smt2                                                |   20.363s | 708.0MiB| timeout | 0 |  |
|lia/0062.smt2                                                |   20.415s | 932.0MiB| timeout | 0 |  |
|lia/0048.smt2                                                |   20.429s | 935.0MiB| timeout | 0 |  |
|lia/0041.smt2                                                |   20.482s | 1854.0MiB| timeout | 0 |  |
