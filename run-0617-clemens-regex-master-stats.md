# .

* SAT 964
* UNSAT 68
* TIMEOUT 215
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: 
Job tag: clemens-regex-master
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: eccdffa78168015d5d21564c310c66ca2db0dd8e
Z3 branch: master
Z3 options: "-T:5 model_validate=true smt.random_seed=1"
Z3 inputs: inputs/ClemensRegexAll
Z3 commit message: [snapshot-regression-fix] opt: preserve strict supremum/infimum optima with infinitesimal component (#10052)

## Summary

Fixes a Z3 optimization regression where maximizing a real objective
under a
**strict** inequality returned a plain feasible model value instead of
the
strict supremum.

Originating discussion:
https://github.com/Z3Prover/bench/discussions/3053
Benchmark: `iss-5720/bug-1.smt2` (in `Z3Prover/bench`)

```smt2
(declare-const r Real)
(assert (< r 1))
(maximize r)
(check-sat)
(get-objectives)
```

The optimum of `r` subject to `r < 1` is the strict supremum `1 -
epsilon`,
which the recorded oracle expects. Current z3 instead reports the
feasible
point `0`.

## Divergence

```diff
--- bug-1.expected.out (expected)
+++ produced (current z3)
@@ -1,4 +1,4 @@
 sat
 (objectives
- (r (+ 1.0 (* (- 1.0) epsilon)))
+ (r 0)
 )
```

## Root cause

Regression from commit `fdc32d0e6` ("Fix inconsistent optimization
result with
unvalidated LP bound", #10028). That change stopped committing the LP
optimization hint `val` to `m_objective_values` up front in
`opt_solver::maximize_objective`, deferring the commit until
`check_bound()`
validates it. Its goal is to reject **plain-rational** over-estimates
produced
when the objective shares symbols with other theories (e.g. the
auxiliary
uninterpreted function used to encode large `distinct` constraints);
those
over-estimates have a **zero** infinitesimal.

For a strict real supremum/infimum the hint has a **non-zero**
infinitesimal
(here `val = 1 - epsilon`). `check_bound()` can never validate it,
because
`opt_solver::mk_ge` drops the negative infinitesimal (mathematically,
`r >= 1 - epsilon` is equivalent over the reals to `r >= 1`), turning
the
validation bound into the unsatisfiable `r >= 1` given `r < 1`.
Validation
therefore fails, `maximize_objective` returns `false`, and
`m_objective_values`
is left holding the strictly smaller current **model** value (`0`),
which
`optsmt::geometric_lex` then reports as the optimum.

## Fix

`src/opt/opt_solver.cpp`, `maximize_objective`: restore the pre-#10028
eager
commit of the hint, **scoped to finite values with a non-zero
infinitesimal**:

```cpp
if (val.is_finite() && !val.get_infinitesimal().is_zero() && val > m_objective_values[i])
    m_objective_values[i] = val;
```

A finite value with a non-zero infinitesimal is a strict optimum that no
concrete model can attain and that `check_bound()` cannot validate, so
the
arithmetic hint is authoritative and must be preserved. Plain-rational
(zero-infinitesimal) values — including **all integer objectives** and
the
`#10028` shared-symbol over-estimates — do not enter this branch and
continue
through the deferred-commit validation path unchanged, so `#10028` is
structurally preserved. The change does not alter control flow or the
return
value, so the lex/box drivers behave as before.

## Validation

Built the patched `./z3` checkout (`./configure && make -C build
-j$(nproc)`)
and re-ran the benchmark with the same options the snapshot capture
uses:

```
$ ./build/z3 -T:20 inputs/issues/iss-5720/bug-1.smt2
sat
(objectives
 (r (+ 1.0 (* (- 1.0) epsilon)))
)
```

This is a **byte-exact match** with the recorded `bug-1.expected.out`
oracle.

Additional before/after checks on the rebuilt binary (baseline = current
nightly, unpatched):

| case | baseline | patched |
| --- | --- | --- |
| single strict-real max `r<1` | `r=0` ❌ | `r=1-eps` ✅ (target) |
| single strict-real min `r>1` | `r=0` ❌ | `r=1+eps` ✅ |
| non-strict real max `r<=1` | `r=1` ✅ | `r=1` ✅ |
| integer max `x<10` | `x=9` ✅ | `x=9` ✅ |
| bounded strict `2<r<5` | — | `r=5-eps` ✅ |
| box: two strict reals | — | both `-eps` ✅ |
| lex: nonstrict-real then int | `r=1,x=9` ✅ | `r=1,x=9` ✅ |
| lex: int then nonstrict-real | `x=9,r=5` ✅ | `x=9,r=5` ✅ |
| lex: all integer | `x=9,y=9` ✅ | `x=9,y=9` ✅ |
| lex: int then strict-real (final) | `x=9,r=0` ❌ | `x=9,r=1-eps` ✅ |

All well-defined lex/box cases are preserved, and a strict-real
objective as
the **final** lex objective is now also correct.

## Known limitation (pre-existing, honest disclosure)

Lexicographic optimization where a **non-final** objective is a strict
real
supremum (e.g. `maximize r` with `r<1`, *then* `maximize x`) remains
ill-defined: the supremum `1-epsilon` is not attained by any model, so
`optsmt::commit_assignment` asserting `r >= 1-epsilon` (which `mk_ge`
reduces to
`r >= 1`) contradicts `r < 1`. This corner is already mishandled by the
current
nightly (it returns `r=0`) and is not what discussion #3053 reports;
this patch
does not attempt to redefine that semantics. It changes only how that
corner
manifests, while fixing the reported single-objective divergence and all
well-defined cases above.

Draft for human review.




> Generated by [Fix a Z3 snapshot-regression
divergence](https://github.com/Z3Prover/bench/actions/runs/28733642068)
· 691.9 AIC · ⌖ 44.2 AIC · ⊞ 8.9K ·
[◷](https://github.com/search?q=repo%3AZ3Prover%2Fz3+%22gh-aw-workflow-id%3A+snapshot-regression-fixer%22&type=pullrequests)

<!-- gh-aw-agentic-workflow: Fix a Z3 snapshot-regression divergence,
engine: copilot, version: 1.0.63, model: claude-opus-4.8, id:
28733642068, workflow_id: snapshot-regression-fixer, run:
https://github.com/Z3Prover/bench/actions/runs/28733642068 -->

<!-- gh-aw-workflow-id: snapshot-regression-fixer -->
<!-- gh-aw-workflow-call-id: Z3Prover/bench/snapshot-regression-fixer
-->

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|QF_extracted/no_eqs/20240318-omark_cloud-service-query-2_harvest_000000_no_eq.smt2 |    0.026s | 20.224MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_cloud-service-query-2_harvest_000005_no_eq.smt2 |    0.027s | 20.38MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4560_sink_harvest_000000.smt2 |    0.028s | 20.816MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_lyndon-schuetzenberg-2_harvest_000007_no_eq.smt2 |    0.028s | 20.14MiB| unsat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_lyndon-schuetzenberg-1_harvest_000007_no_eq.smt2 |    0.029s | 20.528MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_noodles-unsat-6_harvest_000002_no_eq.smt2 |    0.030s | 20.516MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_lyndon-schuetzenberg-1_harvest_000010_no_eq.smt2 |    0.030s | 20.372MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2461_sink_harvest_000008.smt2 |    0.034s | 20.236MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_lyndon-schuetzenberg-1_harvest_000000.smt2 |    0.034s | 20.42MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000035.smt2 |    0.037s | 20.392MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_lyndon-schuetzenberg-1_harvest_000012_no_eq.smt2 |    0.037s | 20.368MiB| sat | 0 |  |  |
|hard_ext_12.smt2                                             |    0.038s | 20.356MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4827_sink_harvest_000000.smt2 |    0.039s | 20.256MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4891_sink_harvest_000000.smt2 |    0.041s | 20.368MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1612_sink_harvest_000014.smt2 |    0.041s | 20.396MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_noodles-unsat-9_harvest_000003.smt2 |    0.041s | 20.112MiB| unsat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_cloud-service-query-2_harvest_000002_no_eq.smt2 |    0.041s | 20.372MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2461_sink_harvest_000016.smt2 |    0.042s | 20.376MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000036.smt2 |    0.042s | 20.296MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000031.smt2 |    0.042s | 20.432MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_765_sink_harvest_000000.smt2 |    0.044s | 21.368MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000002.smt2 |    0.044s | 21.468MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000004.smt2 |    0.044s | 21.008MiB| sat | 0 |  |  |
|deep_nest_membership_t26.smt2                                |    0.045s | 20.744MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1612_sink_harvest_000019.smt2 |    0.045s | 20.36MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_noodles-unsat-8_harvest_000003.smt2 |    0.045s | 20.328MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3434_sink_harvest_000002.smt2 |    0.045s | 21.196MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1612_sink_harvest_000023.smt2 |    0.045s | 20.4MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_lyndon-schuetzenberg-1_harvest_000006_no_eq.smt2 |    0.045s | 20.42MiB| unsat | 0 |  |  |
|QF_extracted/20240318-omark_lyndon-schuetzenberg-2_harvest_000001.smt2 |    0.046s | 20.176MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000048.smt2 |    0.046s | 21.088MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_cloud-service-query-2_harvest_000000.smt2 |    0.046s | 19.904MiB| unsat | 0 |  |  |
|QF_extracted/20240318-omark_lyndon-schuetzenberg-1_harvest_000006.smt2 |    0.046s | 20.448MiB| unsat | 0 |  |  |
|QF_extracted/20240318-omark_noodles-unsat_harvest_000000.smt2 |    0.047s | 20.528MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1612_sink_harvest_000003.smt2 |    0.047s | 21.268MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000003.smt2 |    0.047s | 21.284MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000050.smt2 |    0.048s | 21.132MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000027.smt2 |    0.048s | 21.212MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3074_sink_harvest_000000.smt2 |    0.049s | 20.208MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2461_sink_harvest_000020.smt2 |    0.049s | 20.432MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000009.smt2 |    0.049s | 21.136MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1612_sink_harvest_000018.smt2 |    0.049s | 20.444MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_noodles-unsat-5_1_harvest_000003_no_eq.smt2 |    0.049s | 20.264MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3837_sink_harvest_000000.smt2 |    0.050s | 21.832MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2461_sink_harvest_000012.smt2 |    0.050s | 20.376MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000051.smt2 |    0.050s | 21.04MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4135_sink_harvest_000000.smt2 |    0.050s | 22.208MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1612_sink_harvest_000016.smt2 |    0.051s | 20.392MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000001.smt2 |    0.051s | 20.676MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4597_sink_harvest_000000.smt2 |    0.051s | 21.284MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_lyndon-schuetzenberg-2_harvest_000000.smt2 |    0.051s | 20.224MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000035.smt2 |    0.051s | 21.232MiB| sat | 0 |  |  |
|hard_cyclic_9_subsumption.smt2                               |    0.052s | 20.54MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000014.smt2 |    0.052s | 21.188MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1612_sink_harvest_000006.smt2 |    0.052s | 20.216MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_lyndon-schuetzenberg-2_harvest_000007.smt2 |    0.053s | 20.188MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1612_sink_harvest_000005.smt2 |    0.053s | 20.408MiB| sat | 0 |  |  |
|noodler_killer_1_nth_from_end.smt2                           |    0.054s | 21.028MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3452_sink_harvest_000000.smt2 |    0.054s | 23.836MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2461_sink_harvest_000002.smt2 |    0.054s | 20.368MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4099_sink_harvest_000000.smt2 |    0.055s | 21.948MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_lyndon-schuetzenberg-2_harvest_000005.smt2 |    0.055s | 20.308MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2461_sink_harvest_000006.smt2 |    0.055s | 20.232MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3641_sink_harvest_000000.smt2 |    0.055s | 23.18MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000000.smt2 |    0.055s | 20.432MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000054.smt2 |    0.056s | 21.048MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3440_sink_harvest_000000.smt2 |    0.056s | 22.42MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1612_sink_harvest_000002.smt2 |    0.056s | 21.184MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3434_sink_harvest_000007.smt2 |    0.056s | 21.096MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4811_sink_harvest_000000.smt2 |    0.057s | 20.692MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2461_sink_harvest_000023.smt2 |    0.057s | 20.284MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2461_sink_harvest_000000.smt2 |    0.057s | 20.384MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000056.smt2 |    0.057s | 21.272MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000040.smt2 |    0.058s | 21.148MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1612_sink_harvest_000011.smt2 |    0.058s | 21.192MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_lyndon-schuetzenberg-2_harvest_000005_no_eq.smt2 |    0.058s | 20.112MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3434_sink_harvest_000005.smt2 |    0.059s | 21.3MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000009.smt2 |    0.059s | 21.26MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4256_sink_harvest_000000.smt2 |    0.059s | 21.84MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4889_sink_harvest_000000.smt2 |    0.059s | 20.396MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000023.smt2 |    0.059s | 21.5MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2461_sink_harvest_000022.smt2 |    0.059s | 20.452MiB| sat | 0 |  |  |
|noodler_killer_16_non_commutative_periods.smt2               |    0.060s | 20.212MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2461_sink_harvest_000019.smt2 |    0.060s | 20.276MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2964_sink_harvest_000000.smt2 |    0.060s | 20.348MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3434_sink_harvest_000000.smt2 |    0.060s | 20.248MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_noodles-unsat-5_harvest_000001.smt2 |    0.060s | 20.384MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000043.smt2 |    0.060s | 25.0MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000000.smt2 |    0.060s | 20.352MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_lyndon-schuetzenberg-1_harvest_000013_no_eq.smt2 |    0.060s | 20.484MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_noodles-unsat-5_1_harvest_000001_no_eq.smt2 |    0.060s | 20.624MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_980_sink_harvest_000000.smt2 |    0.061s | 20.72MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000033.smt2 |    0.061s | 21.432MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2461_sink_harvest_000003.smt2 |    0.062s | 20.368MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000058.smt2 |    0.062s | 21.24MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3075_sink_harvest_000000.smt2 |    0.062s | 20.256MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000015.smt2 |    0.062s | 21.14MiB| sat | 0 |  |  |
|hard_cyclic_11_deep_unrolling.smt2                           |    0.063s | 20.888MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000011.smt2 |    0.063s | 21.42MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000036.smt2 |    0.063s | 21.496MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000008.smt2 |    0.063s | 21.268MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000023.smt2 |    0.063s | 21.052MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4832_sink_harvest_000000.smt2 |    0.063s | 20.292MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2461_sink_harvest_000021.smt2 |    0.063s | 20.516MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_cloud-service-query-2_harvest_000001_no_eq.smt2 |    0.063s | 20.372MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_noodles-unsat-2_harvest_000003_no_eq.smt2 |    0.063s | 20.64MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000040.smt2 |    0.064s | 21.132MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5045_sink_harvest_000000.smt2 |    0.064s | 25.74MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000032.smt2 |    0.064s | 21.064MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5507_sink_harvest_000000.smt2 |    0.064s | 21.164MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_lyndon-schuetzenberg-1_harvest_000003.smt2 |    0.064s | 20.412MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3303_sink_harvest_000000.smt2 |    0.064s | 21.884MiB| sat | 0 |  |  |
|hard_cyclic_10_linear_form.smt2                              |    0.065s | 20.188MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000045.smt2 |    0.065s | 21.036MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3434_sink_harvest_000009.smt2 |    0.065s | 21.236MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000006.smt2 |    0.065s | 21.128MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4144_sink_harvest_000000.smt2 |    0.065s | 22.096MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1612_sink_harvest_000020.smt2 |    0.065s | 20.352MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_lyndon-schuetzenberg-2_harvest_000001_no_eq.smt2 |    0.065s | 20.368MiB| unsat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_noodles-unsat-8_harvest_000001_no_eq.smt2 |    0.065s | 20.776MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000060.smt2 |    0.066s | 21.008MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000005.smt2 |    0.066s | 21.196MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_cyclic-xy_harvest_000000.smt2    |    0.067s | 20.4MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000006.smt2 |    0.067s | 21.4MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_lyndon-schuetzenberg-1_harvest_000002.smt2 |    0.067s | 20.54MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000057.smt2 |    0.067s | 23.408MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5505_sink_harvest_000000.smt2 |    0.067s | 20.968MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3434_sink_harvest_000001.smt2 |    0.067s | 21.248MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_lyndon-schuetzenberg-1_harvest_000008_no_eq.smt2 |    0.067s | 20.628MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1612_sink_harvest_000004.smt2 |    0.068s | 20.316MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000059.smt2 |    0.068s | 21.232MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4050_sink_harvest_000000.smt2 |    0.068s | 21.588MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2461_sink_harvest_000011.smt2 |    0.068s | 20.4MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3434_sink_harvest_000010.smt2 |    0.068s | 21.112MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_noodles-unsat-5_1_harvest_000002.smt2 |    0.068s | 20.428MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000025.smt2 |    0.068s | 21.408MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000020.smt2 |    0.068s | 21.08MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3434_sink_harvest_000011.smt2 |    0.069s | 21.076MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1612_sink_harvest_000015.smt2 |    0.069s | 20.228MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000008.smt2 |    0.069s | 21.908MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000041.smt2 |    0.069s | 20.42MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1612_sink_harvest_000008.smt2 |    0.069s | 20.456MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000019.smt2 |    0.069s | 20.972MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5336_sink_harvest_000000.smt2 |    0.069s | 22.472MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000039.smt2 |    0.069s | 21.088MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_noodles-unsat-2_harvest_000001_no_eq.smt2 |    0.069s | 20.628MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000028.smt2 |    0.070s | 21.24MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000044.smt2 |    0.070s | 21.184MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4902_sink_harvest_000000.smt2 |    0.070s | 20.4MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2835_sink_harvest_000000.smt2 |    0.070s | 21.296MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_noodles-unsat-5_1_harvest_000001.smt2 |    0.070s | 20.516MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1612_sink_harvest_000021.smt2 |    0.070s | 20.288MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000024.smt2 |    0.070s | 21.268MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3436_sink_harvest_000000.smt2 |    0.071s | 22.384MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2461_sink_harvest_000026.smt2 |    0.071s | 20.372MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4677_sink_harvest_000000.smt2 |    0.071s | 21.328MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_noodles-unsat_harvest_000001_no_eq.smt2 |    0.071s | 20.876MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_noodles-unsat-5_harvest_000002_no_eq.smt2 |    0.071s | 20.392MiB| unsat | 0 |  |  |
|hard_ext_11.smt2                                             |    0.072s | 20.276MiB| unsat | 0 |  |  |
|generated_tiny/split_membership_tiny_sat_0025.smt2           |    0.072s | 20.876MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_noodles-unsat-6_harvest_000002.smt2 |    0.072s | 20.64MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000032.smt2 |    0.072s | 23.48MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2461_sink_harvest_000004.smt2 |    0.072s | 20.428MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000029.smt2 |    0.072s | 21.448MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3434_sink_harvest_000004.smt2 |    0.072s | 21.264MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4374_sink_harvest_000000.smt2 |    0.072s | 21.764MiB| sat | 0 |  |  |
|hard_ext_4.smt2                                              |    0.073s | 20.864MiB| sat | 0 |  |  |
|hard_ext_10.smt2                                             |    0.073s | 20.368MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1612_sink_harvest_000022.smt2 |    0.073s | 20.456MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000019.smt2 |    0.073s | 21.3MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000041.smt2 |    0.073s | 21.044MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000066.smt2 |    0.073s | 21.264MiB| sat | 0 |  |  |
|noodler_killer_3_nested_complements.smt2                     |    0.074s | 19.884MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3518_sink_harvest_000000.smt2 |    0.074s | 21.432MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000030.smt2 |    0.074s | 21.36MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1828_sink_harvest_000001.smt2 |    0.074s | 22.2MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000044.smt2 |    0.074s | 21.228MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000070.smt2 |    0.074s | 21.32MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000002.smt2 |    0.074s | 21.336MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4318_sink_harvest_000000.smt2 |    0.074s | 21.816MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000018.smt2 |    0.074s | 21.392MiB| sat | 0 |  |  |
|deep_nest_membership_t19.smt2                                |    0.075s | 21.584MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4234_sink_harvest_000000.smt2 |    0.075s | 21.74MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2461_sink_harvest_000025.smt2 |    0.075s | 20.392MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3697_sink_harvest_000000.smt2 |    0.075s | 20.844MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4656_sink_harvest_000000.smt2 |    0.075s | 22.064MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5233_sink_harvest_000000.smt2 |    0.075s | 22.34MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4261_sink_harvest_000000.smt2 |    0.075s | 21.388MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2461_sink_harvest_000024.smt2 |    0.075s | 20.24MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3311_sink_harvest_000000.smt2 |    0.075s | 22.184MiB| sat | 0 |  |  |
|noodler_killer_18_var_complement.smt2                        |    0.076s | 21.016MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2857_sink_harvest_000001.smt2 |    0.076s | 21.256MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3434_sink_harvest_000008.smt2 |    0.076s | 21.376MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1856_sink_harvest_000002.smt2 |    0.076s | 22.18MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1828_sink_harvest_000003.smt2 |    0.076s | 22.032MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000010.smt2 |    0.076s | 21.208MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4006_sink_harvest_000000.smt2 |    0.077s | 21.948MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000043.smt2 |    0.077s | 21.292MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2461_sink_harvest_000010.smt2 |    0.077s | 20.244MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4616_sink_harvest_000000.smt2 |    0.077s | 21.132MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000049.smt2 |    0.077s | 23.488MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_noodles-unsat_harvest_000001.smt2 |    0.078s | 20.772MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3523_sink_harvest_000000.smt2 |    0.078s | 22.284MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5437_sink_harvest_000000.smt2 |    0.078s | 20.376MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000017.smt2 |    0.078s | 21.48MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000061.smt2 |    0.078s | 23.652MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000052.smt2 |    0.078s | 20.376MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5438_sink_harvest_000000.smt2 |    0.078s | 20.284MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000012.smt2 |    0.078s | 21.352MiB| sat | 0 |  |  |
|hard_ext_7.smt2                                              |    0.079s | 20.388MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3982_sink_harvest_000000.smt2 |    0.079s | 21.96MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000004.smt2 |    0.079s | 21.812MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_cloud-service-query-2_harvest_000001.smt2 |    0.079s | 20.164MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000050.smt2 |    0.079s | 21.648MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3434_sink_harvest_000003.smt2 |    0.079s | 21.448MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000024.smt2 |    0.079s | 21.052MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000007.smt2 |    0.079s | 21.156MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4394_sink_harvest_000000.smt2 |    0.079s | 22.304MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_lyndon-schuetzenberg-2_harvest_000006_no_eq.smt2 |    0.079s | 20.16MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1612_sink_harvest_000017.smt2 |    0.080s | 20.412MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4900_sink_harvest_000000.smt2 |    0.080s | 20.248MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000049.smt2 |    0.080s | 21.228MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_lyndon-schuetzenberg-2_harvest_000006.smt2 |    0.080s | 20.364MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3833_sink_harvest_000000.smt2 |    0.080s | 21.948MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2461_sink_harvest_000014.smt2 |    0.080s | 20.296MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000067.smt2 |    0.080s | 23.672MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000046.smt2 |    0.080s | 20.464MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4596_sink_harvest_000000.smt2 |    0.081s | 21.896MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3289_sink_harvest_000000.smt2 |    0.081s | 21.892MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4310_sink_harvest_000000.smt2 |    0.081s | 21.772MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4161_sink_harvest_000000.smt2 |    0.081s | 21.828MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2688_sink_harvest_000000.smt2 |    0.081s | 22.064MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_noodles-unsat-5_harvest_000000.smt2 |    0.081s | 20.412MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000025.smt2 |    0.081s | 24.816MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5444_sink_harvest_000000.smt2 |    0.081s | 21.68MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_noodles-unsat-6_harvest_000001_no_eq.smt2 |    0.081s | 20.628MiB| sat | 0 |  |  |
|generated_tiny/split_membership_tiny_sat_0007.smt2           |    0.082s | 21.144MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3095_sink_harvest_000000.smt2 |    0.082s | 21.936MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000006.smt2 |    0.082s | 21.388MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4291_sink_harvest_000000.smt2 |    0.082s | 22.464MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000015.smt2 |    0.082s | 21.416MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2461_sink_harvest_000018.smt2 |    0.082s | 20.38MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1612_sink_harvest_000012.smt2 |    0.082s | 21.32MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000047.smt2 |    0.082s | 21.248MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4003_sink_harvest_000000.smt2 |    0.082s | 22.252MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1612_sink_harvest_000000.smt2 |    0.082s | 20.628MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_noodles-unsat-3_harvest_000000_no_eq.smt2 |    0.082s | 20.604MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_noodles-unsat_harvest_000000_no_eq.smt2 |    0.082s | 20.644MiB| sat | 0 |  |  |
|noodler_killer_20_alphabet_transducer.smt2                   |    0.083s | 20.628MiB| unsat | 0 |  |  |
|noodler_killer_6_ambiguous_nfa_comp.smt2                     |    0.083s | 20.588MiB| sat | 0 |  |  |
|generated_tiny/split_membership_tiny_unsat_0010.smt2         |    0.083s | 21.136MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3948_sink_harvest_000000.smt2 |    0.083s | 23.068MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3912_sink_harvest_000000.smt2 |    0.083s | 21.824MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1069_sink_harvest_000000.smt2 |    0.083s | 21.324MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_839_sink_harvest_000000.smt2 |    0.083s | 21.148MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000022.smt2 |    0.083s | 24.912MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000000.smt2 |    0.083s | 21.336MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000025.smt2 |    0.083s | 21.252MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000034.smt2 |    0.083s | 21.116MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_noodles-unsat-2_harvest_000000.smt2 |    0.083s | 20.576MiB| unsat | 0 |  |  |
|noodler_killer_9_comp_inter_comp.smt2                        |    0.084s | 21.12MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3308_sink_harvest_000000.smt2 |    0.084s | 22.176MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4258_sink_harvest_000000.smt2 |    0.084s | 23.056MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000031.smt2 |    0.084s | 21.124MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3038_sink_harvest_000000.smt2 |    0.084s | 22.668MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_250_sink_harvest_000000.smt2 |    0.084s | 22.04MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_lyndon-schuetzenberg-2_harvest_000002.smt2 |    0.084s | 20.356MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000021.smt2 |    0.084s | 20.988MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_lyndon-schuetzenberg-1_harvest_000005_no_eq.smt2 |    0.084s | 20.44MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_cloud-service-query-2_harvest_000004_no_eq.smt2 |    0.084s | 20.264MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4702_sink_harvest_000000.smt2 |    0.085s | 21.0MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000037.smt2 |    0.085s | 20.996MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2461_sink_harvest_000005.smt2 |    0.085s | 20.264MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3727_sink_harvest_000000.smt2 |    0.085s | 21.868MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3748_sink_harvest_000000.smt2 |    0.085s | 22.076MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000105.smt2 |    0.085s | 25.436MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000004.smt2 |    0.085s | 21.256MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_noodles-unsat-2_harvest_000000_no_eq.smt2 |    0.085s | 20.776MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_noodles-unsat-8_harvest_000000_no_eq.smt2 |    0.085s | 20.9MiB| sat | 0 |  |  |
|hard_ext_1.smt2                                              |    0.086s | 20.728MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2991_sink_harvest_000000.smt2 |    0.086s | 20.228MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3605_sink_harvest_000000.smt2 |    0.086s | 21.844MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000005.smt2 |    0.086s | 21.112MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3310_sink_harvest_000000.smt2 |    0.086s | 22.248MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000060.smt2 |    0.086s | 23.6MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000006.smt2 |    0.086s | 24.972MiB| sat | 0 |  |  |
|deep_nest_membership_t7.smt2                                 |    0.087s | 20.084MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3978_sink_harvest_000000.smt2 |    0.087s | 23.32MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2461_sink_harvest_000017.smt2 |    0.087s | 20.324MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000051.smt2 |    0.087s | 21.332MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000005.smt2 |    0.087s | 21.104MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000013.smt2 |    0.087s | 23.556MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000049.smt2 |    0.087s | 21.236MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000003.smt2 |    0.087s | 21.42MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3402_sink_harvest_000000.smt2 |    0.087s | 21.988MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4700_sink_harvest_000000.smt2 |    0.087s | 21.1MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_lyndon-schuetzenberg-2_harvest_000004_no_eq.smt2 |    0.087s | 20.224MiB| unsat | 0 |  |  |
|noodler_killer_19_double_star_comp.smt2                      |    0.088s | 20.232MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2962_sink_harvest_000000.smt2 |    0.088s | 21.296MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4598_sink_harvest_000000.smt2 |    0.088s | 22.132MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4297_sink_harvest_000000.smt2 |    0.088s | 21.704MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1828_sink_harvest_000002.smt2 |    0.088s | 22.22MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000014.smt2 |    0.088s | 25.736MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4201_sink_harvest_000000.smt2 |    0.088s | 24.324MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_lyndon-schuetzenberg-2_harvest_000003.smt2 |    0.088s | 20.256MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_633_sink_harvest_000000.smt2 |    0.088s | 21.892MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3640_sink_harvest_000000.smt2 |    0.088s | 23.216MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2174_sink_harvest_000000.smt2 |    0.088s | 21.224MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_690_sink_harvest_000000.smt2 |    0.088s | 24.036MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000067.smt2 |    0.088s | 21.072MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_lyndon-schuetzenberg-1_harvest_000002_no_eq.smt2 |    0.088s | 21.004MiB| unsat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_cloud-service-query-2_harvest_000003_no_eq.smt2 |    0.088s | 20.364MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_lyndon-schuetzenberg-1_harvest_000009_no_eq.smt2 |    0.088s | 20.328MiB| unsat | 0 |  |  |
|noodler_killer_4_nfa_poly_comp.smt2                          |    0.089s | 20.668MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4306_sink_harvest_000000.smt2 |    0.089s | 21.944MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4199_sink_harvest_000000.smt2 |    0.089s | 21.916MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2461_sink_harvest_000007.smt2 |    0.089s | 20.308MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000002.smt2 |    0.089s | 21.048MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2461_sink_harvest_000009.smt2 |    0.089s | 20.34MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000052.smt2 |    0.089s | 25.144MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5144_sink_harvest_000000.smt2 |    0.089s | 25.592MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4304_sink_harvest_000000.smt2 |    0.089s | 21.904MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000054.smt2 |    0.089s | 25.116MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_noodles-unsat_harvest_000002_no_eq.smt2 |    0.089s | 20.576MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_lyndon-schuetzenberg-2_harvest_000000_no_eq.smt2 |    0.089s | 20.108MiB| unsat | 0 |  |  |
|generated_tiny/split_membership_tiny_sat_0027.smt2           |    0.090s | 21.924MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000063.smt2 |    0.090s | 21.212MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4354_sink_harvest_000003.smt2 |    0.090s | 25.868MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3290_sink_harvest_000000.smt2 |    0.090s | 21.9MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2461_sink_harvest_000013.smt2 |    0.090s | 20.368MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000073.smt2 |    0.090s | 25.352MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3304_sink_harvest_000000.smt2 |    0.090s | 21.952MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3720_sink_harvest_000000.smt2 |    0.090s | 21.784MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4296_sink_harvest_000000.smt2 |    0.090s | 21.496MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000048.smt2 |    0.091s | 21.236MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000013.smt2 |    0.091s | 25.396MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1612_sink_harvest_000009.smt2 |    0.091s | 21.156MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3434_sink_harvest_000006.smt2 |    0.091s | 21.38MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000077.smt2 |    0.091s | 24.888MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_noodles-unsat-5_harvest_000002.smt2 |    0.091s | 20.408MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000059.smt2 |    0.091s | 25.012MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000046.smt2 |    0.091s | 21.42MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4157_sink_harvest_000000.smt2 |    0.091s | 21.86MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000038.smt2 |    0.091s | 21.412MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000001.smt2 |    0.091s | 20.44MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000074.smt2 |    0.091s | 25.028MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000045.smt2 |    0.091s | 21.42MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000023.smt2 |    0.091s | 25.056MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000060.smt2 |    0.091s | 25.088MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2857_sink_harvest_000000.smt2 |    0.091s | 21.232MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000053.smt2 |    0.091s | 21.308MiB| sat | 0 |  |  |
|hard_cyclic_5_iso_sat.smt2                                   |    0.092s | 20.928MiB| sat | 0 |  |  |
|hard_cyclic_7_overlapping_cycles.smt2                        |    0.092s | 20.664MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000012.smt2 |    0.092s | 21.128MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000063.smt2 |    0.092s | 24.828MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3207_sink_harvest_000000.smt2 |    0.092s | 21.924MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000093.smt2 |    0.092s | 25.22MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000052.smt2 |    0.092s | 24.82MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3601_sink_harvest_000000.smt2 |    0.092s | 22.012MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000016.smt2 |    0.092s | 21.444MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3434_sink_harvest_000012.smt2 |    0.092s | 21.096MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_33_sink_harvest_000000.smt2 |    0.092s | 21.96MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3209_sink_harvest_000000.smt2 |    0.092s | 21.872MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000027.smt2 |    0.092s | 21.032MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3400_sink_harvest_000000.smt2 |    0.092s | 21.828MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_lyndon-schuetzenberg-1_harvest_000001.smt2 |    0.092s | 20.34MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000021.smt2 |    0.092s | 21.384MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3089_sink_harvest_000000.smt2 |    0.093s | 21.876MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3203_sink_harvest_000000.smt2 |    0.093s | 21.836MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3517_sink_harvest_000000.smt2 |    0.093s | 21.488MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_noodles-unsat-9_harvest_000002_no_eq.smt2 |    0.093s | 20.748MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000026.smt2 |    0.094s | 21.412MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000022.smt2 |    0.094s | 21.232MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000069.smt2 |    0.094s | 23.468MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000026.smt2 |    0.094s | 25.632MiB| sat | 0 |  |  |
|hard_ext_5.smt2                                              |    0.095s | 20.332MiB| unsat | 0 |  |  |
|deep_nest_membership_t3.smt2                                 |    0.095s | 21.412MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4374_sink_harvest_000001.smt2 |    0.095s | 21.76MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000071.smt2 |    0.095s | 25.528MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000101.smt2 |    0.095s | 24.86MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000013.smt2 |    0.095s | 21.256MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4198_sink_harvest_000000.smt2 |    0.095s | 21.872MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000042.smt2 |    0.095s | 21.616MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000090.smt2 |    0.095s | 25.032MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4596_sink_harvest_000001.smt2 |    0.095s | 22.364MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000062.smt2 |    0.095s | 20.232MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000031.smt2 |    0.095s | 25.164MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_noodles-unsat-5_harvest_000000_no_eq.smt2 |    0.095s | 20.252MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4442_sink_harvest_000000.smt2 |    0.096s | 25.076MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000013.smt2 |    0.096s | 21.112MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000003.smt2 |    0.096s | 25.1MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5165_sink_harvest_000000.smt2 |    0.096s | 25.5MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4262_sink_harvest_000000.smt2 |    0.096s | 22.008MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4845_sink_harvest_000000.smt2 |    0.096s | 23.156MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000055.smt2 |    0.096s | 21.336MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000034.smt2 |    0.096s | 21.18MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_632_sink_harvest_000000.smt2 |    0.096s | 22.172MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5298_sink_harvest_000000.smt2 |    0.097s | 25.472MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_57_sink_harvest_000000.smt2 |    0.097s | 21.916MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4037_sink_harvest_000000.smt2 |    0.097s | 23.052MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2461_sink_harvest_000015.smt2 |    0.097s | 20.692MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000007.smt2 |    0.097s | 21.56MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_692_sink_harvest_000000.smt2 |    0.097s | 24.252MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000045.smt2 |    0.097s | 21.18MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3636_sink_harvest_000000.smt2 |    0.097s | 22.692MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5026_sink_harvest_000000.smt2 |    0.097s | 25.484MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000063.smt2 |    0.098s | 23.488MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4628_sink_harvest_000000.smt2 |    0.098s | 25.008MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1856_sink_harvest_000003.smt2 |    0.098s | 22.26MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_lyndon-schuetzenberg-2_harvest_000004.smt2 |    0.098s | 20.248MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000052.smt2 |    0.098s | 21.216MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000047.smt2 |    0.098s | 23.424MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000016.smt2 |    0.098s | 21.24MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000059.smt2 |    0.098s | 24.972MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_lyndon-schuetzenberg-2_harvest_000002_no_eq.smt2 |    0.098s | 20.168MiB| unsat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_lyndon-schuetzenberg-2_harvest_000003_no_eq.smt2 |    0.098s | 20.152MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5043_sink_harvest_000000.smt2 |    0.099s | 25.76MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4299_sink_harvest_000000.smt2 |    0.099s | 22.128MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3259_sink_harvest_000000.smt2 |    0.099s | 25.26MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3282_sink_harvest_000000.smt2 |    0.099s | 23.396MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000014.smt2 |    0.099s | 24.864MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1828_sink_harvest_000000.smt2 |    0.099s | 22.092MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000061.smt2 |    0.099s | 24.784MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000113.smt2 |    0.099s | 24.832MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_noodles-unsat-5_harvest_000001_no_eq.smt2 |    0.099s | 20.36MiB| unsat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_lyndon-schuetzenberg-1_harvest_000011_no_eq.smt2 |    0.099s | 20.536MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_lyndon-schuetzenberg-1_harvest_000001_no_eq.smt2 |    0.099s | 20.46MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000033.smt2 |    0.100s | 21.584MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4134_sink_harvest_000000.smt2 |    0.100s | 21.9MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2461_sink_harvest_000027.smt2 |    0.100s | 20.384MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000030.smt2 |    0.100s | 21.296MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000028.smt2 |    0.100s | 21.468MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3830_sink_harvest_000000.smt2 |    0.100s | 21.792MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000037.smt2 |    0.100s | 21.308MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4309_sink_harvest_000000.smt2 |    0.100s | 24.788MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_164_sink_harvest_000000.smt2 |    0.100s | 25.164MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_noodles-unsat-5_1_harvest_000002_no_eq.smt2 |    0.100s | 20.58MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3911_sink_harvest_000000.smt2 |    0.101s | 21.896MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000064.smt2 |    0.101s | 21.188MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4229_sink_harvest_000000.smt2 |    0.101s | 22.28MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4146_sink_harvest_000000.smt2 |    0.101s | 22.132MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_lyndon-schuetzenberg-1_harvest_000009.smt2 |    0.101s | 20.512MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000018.smt2 |    0.101s | 21.132MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000042.smt2 |    0.101s | 21.216MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4638_sink_harvest_000000.smt2 |    0.101s | 24.772MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000102.smt2 |    0.101s | 25.736MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5139_sink_harvest_000000.smt2 |    0.101s | 25.58MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000011.smt2 |    0.101s | 23.476MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5044_sink_harvest_000000.smt2 |    0.101s | 25.48MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000024.smt2 |    0.101s | 23.496MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000093.smt2 |    0.101s | 25.048MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_noodles-unsat-5_1_harvest_000000_no_eq.smt2 |    0.101s | 20.36MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000065.smt2 |    0.102s | 21.128MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5344_sink_harvest_000000.smt2 |    0.102s | 25.496MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000042.smt2 |    0.102s | 23.552MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000037.smt2 |    0.102s | 23.456MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000095.smt2 |    0.102s | 25.128MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4316_sink_harvest_000000.smt2 |    0.102s | 24.448MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000123.smt2 |    0.102s | 25.772MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3159_sink_harvest_000000.smt2 |    0.102s | 22.288MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000122.smt2 |    0.102s | 24.856MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_noodles-unsat-8_harvest_000002_no_eq.smt2 |    0.102s | 20.44MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000057.smt2 |    0.103s | 21.224MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1396_sink_harvest_000000.smt2 |    0.103s | 22.036MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1612_sink_harvest_000013.smt2 |    0.103s | 20.26MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3943_sink_harvest_000000.smt2 |    0.103s | 22.464MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000007.smt2 |    0.103s | 21.764MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_167_sink_harvest_000000.smt2 |    0.103s | 22.564MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000029.smt2 |    0.103s | 25.348MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_lyndon-schuetzenberg-1_harvest_000004_no_eq.smt2 |    0.103s | 20.288MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1612_sink_harvest_000010.smt2 |    0.104s | 20.196MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000015.smt2 |    0.104s | 25.108MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3726_sink_harvest_000000.smt2 |    0.104s | 21.776MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_noodles-unsat-6_harvest_000001.smt2 |    0.104s | 20.86MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3309_sink_harvest_000000.smt2 |    0.104s | 22.06MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000032.smt2 |    0.104s | 21.324MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_noodles-unsat-8_harvest_000003_no_eq.smt2 |    0.104s | 20.628MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2461_sink_harvest_000001.smt2 |    0.105s | 20.204MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000043.smt2 |    0.105s | 23.444MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000045.smt2 |    0.105s | 25.072MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000022.smt2 |    0.105s | 21.692MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3094_sink_harvest_000000.smt2 |    0.105s | 21.84MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000083.smt2 |    0.105s | 25.0MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3300_sink_harvest_000000.smt2 |    0.105s | 21.924MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000017.smt2 |    0.105s | 25.02MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3306_sink_harvest_000000.smt2 |    0.105s | 21.944MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000067.smt2 |    0.105s | 24.808MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4269_sink_harvest_000000.smt2 |    0.105s | 22.136MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1856_sink_harvest_000001.smt2 |    0.105s | 22.18MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3907_sink_harvest_000000.smt2 |    0.106s | 21.84MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4155_sink_harvest_000000.smt2 |    0.106s | 21.796MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5300_sink_harvest_000000.smt2 |    0.106s | 25.576MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4429_sink_harvest_000000.smt2 |    0.106s | 26.004MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_noodles-unsat_harvest_000004_no_eq.smt2 |    0.106s | 20.8MiB| unsat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_cyclic-xy_harvest_000000_no_eq.smt2 |    0.106s | 20.396MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4305_sink_harvest_000000.smt2 |    0.107s | 21.944MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_noodles-unsat_harvest_000002.smt2 |    0.107s | 21.148MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000004.smt2 |    0.107s | 24.956MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2970_sink_harvest_000001.smt2 |    0.107s | 25.956MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000066.smt2 |    0.107s | 23.648MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000010.smt2 |    0.108s | 21.48MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3728_sink_harvest_000000.smt2 |    0.108s | 21.848MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3606_sink_harvest_000000.smt2 |    0.108s | 22.0MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5027_sink_harvest_000000.smt2 |    0.108s | 25.492MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4154_sink_harvest_000000.smt2 |    0.108s | 21.944MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4430_sink_harvest_000000.smt2 |    0.108s | 25.996MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000008.smt2 |    0.108s | 25.716MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000005.smt2 |    0.108s | 25.104MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4008_sink_harvest_000000.smt2 |    0.108s | 25.424MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000020.smt2 |    0.108s | 24.836MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_651_sink_harvest_000000.smt2 |    0.108s | 21.896MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000108.smt2 |    0.109s | 25.488MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2894_sink_harvest_000000.smt2 |    0.109s | 29.476MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000078.smt2 |    0.109s | 25.024MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3913_sink_harvest_000000.smt2 |    0.109s | 21.924MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000000.smt2 |    0.109s | 24.948MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5036_sink_harvest_000000.smt2 |    0.109s | 25.432MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3408_sink_harvest_000000.smt2 |    0.109s | 21.976MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000003.smt2 |    0.109s | 24.94MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1828_sink_harvest_000004.smt2 |    0.109s | 22.26MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3908_sink_harvest_000000.smt2 |    0.109s | 21.924MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000050.smt2 |    0.109s | 23.68MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000002.smt2 |    0.109s | 25.316MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2914_sink_harvest_000000.smt2 |    0.109s | 22.164MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_noodles-unsat-9_harvest_000000_no_eq.smt2 |    0.109s | 20.88MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000085.smt2 |    0.110s | 24.976MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000035.smt2 |    0.110s | 25.12MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000020.smt2 |    0.110s | 21.664MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000026.smt2 |    0.110s | 24.98MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4102_sink_harvest_000000.smt2 |    0.110s | 22.676MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000116.smt2 |    0.110s | 24.788MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4138_sink_harvest_000000.smt2 |    0.110s | 22.544MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3210_sink_harvest_000000.smt2 |    0.110s | 21.94MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000014.smt2 |    0.110s | 21.388MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000063.smt2 |    0.110s | 25.136MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000038.smt2 |    0.110s | 20.288MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3301_sink_harvest_000000.smt2 |    0.110s | 21.82MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000026.smt2 |    0.111s | 20.448MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000075.smt2 |    0.111s | 24.872MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4268_sink_harvest_000000.smt2 |    0.111s | 24.588MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000044.smt2 |    0.111s | 24.84MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000082.smt2 |    0.111s | 25.136MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000030.smt2 |    0.111s | 25.004MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000062.smt2 |    0.111s | 24.968MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3291_sink_harvest_000000.smt2 |    0.111s | 26.048MiB| sat | 0 |  |  |
|generated_tiny/split_membership_tiny_sat_0022.smt2           |    0.112s | 21.728MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3724_sink_harvest_000000.smt2 |    0.112s | 21.984MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000001.smt2 |    0.112s | 25.052MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000038.smt2 |    0.112s | 25.336MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000058.smt2 |    0.112s | 24.956MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4039_sink_harvest_000000.smt2 |    0.112s | 21.868MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000065.smt2 |    0.112s | 23.492MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000069.smt2 |    0.113s | 24.976MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3407_sink_harvest_000000.smt2 |    0.113s | 21.988MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3722_sink_harvest_000000.smt2 |    0.113s | 21.8MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000050.smt2 |    0.113s | 24.948MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_noodles-unsat-2_harvest_000002.smt2 |    0.113s | 20.968MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000011.smt2 |    0.114s | 25.088MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000009.smt2 |    0.114s | 25.148MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000071.smt2 |    0.114s | 25.036MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000001.smt2 |    0.114s | 25.104MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000103.smt2 |    0.114s | 24.96MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000001.smt2 |    0.114s | 21.156MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000022.smt2 |    0.114s | 25.556MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3302_sink_harvest_000000.smt2 |    0.114s | 22.02MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000062.smt2 |    0.114s | 23.424MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000120.smt2 |    0.114s | 24.976MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3792_sink_harvest_000000.smt2 |    0.114s | 24.672MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3516_sink_harvest_000000.smt2 |    0.114s | 21.612MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4101_sink_harvest_000000.smt2 |    0.114s | 25.384MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000051.smt2 |    0.114s | 23.432MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4846_sink_harvest_000000.smt2 |    0.114s | 24.748MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3544_sink_harvest_000000.smt2 |    0.115s | 24.112MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000009.smt2 |    0.115s | 25.844MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000046.smt2 |    0.115s | 23.656MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4067_sink_harvest_000000.smt2 |    0.115s | 25.128MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000003.smt2 |    0.115s | 21.104MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3604_sink_harvest_000000.smt2 |    0.115s | 21.912MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000034.smt2 |    0.115s | 24.964MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000002.smt2 |    0.115s | 25.124MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000007.smt2 |    0.115s | 25.044MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5163_sink_harvest_000000.smt2 |    0.115s | 25.816MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000044.smt2 |    0.115s | 25.684MiB| sat | 0 |  |  |
|generated_tiny/split_membership_tiny_sat_0017.smt2           |    0.116s | 21.672MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4592_sink_harvest_000000.smt2 |    0.116s | 23.112MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4475_sink_harvest_000000.smt2 |    0.116s | 22.288MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4236_sink_harvest_000000.smt2 |    0.116s | 21.868MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000109.smt2 |    0.116s | 24.96MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000047.smt2 |    0.116s | 24.988MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000076.smt2 |    0.116s | 24.956MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_noodles-unsat-9_harvest_000001_no_eq.smt2 |    0.116s | 20.74MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000018.smt2 |    0.117s | 24.952MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3598_sink_harvest_000000.smt2 |    0.117s | 21.968MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000075.smt2 |    0.117s | 25.132MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000029.smt2 |    0.117s | 24.936MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000080.smt2 |    0.117s | 25.384MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5308_sink_harvest_000000.smt2 |    0.117s | 25.74MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000043.smt2 |    0.117s | 21.204MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1612_sink_harvest_000001.smt2 |    0.117s | 21.268MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_lyndon-schuetzenberg-1_harvest_000000_no_eq.smt2 |    0.117s | 20.412MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3723_sink_harvest_000000.smt2 |    0.118s | 21.892MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000097.smt2 |    0.118s | 24.936MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5299_sink_harvest_000000.smt2 |    0.118s | 25.548MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_980_sink_harvest_000001.smt2 |    0.118s | 22.164MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3091_sink_harvest_000000.smt2 |    0.118s | 21.804MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3838_sink_harvest_000000.smt2 |    0.118s | 21.904MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5033_sink_harvest_000000.smt2 |    0.118s | 25.508MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000011.smt2 |    0.118s | 21.24MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000028.smt2 |    0.119s | 25.06MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000034.smt2 |    0.119s | 24.952MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000110.smt2 |    0.119s | 24.924MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000059.smt2 |    0.119s | 23.852MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000006.smt2 |    0.119s | 25.004MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000035.smt2 |    0.119s | 25.02MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000097.smt2 |    0.119s | 25.404MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4265_sink_harvest_000000.smt2 |    0.119s | 21.832MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5293_sink_harvest_000000.smt2 |    0.119s | 25.612MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000082.smt2 |    0.119s | 25.496MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000054.smt2 |    0.119s | 25.74MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5312_sink_harvest_000000.smt2 |    0.119s | 25.868MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4392_sink_harvest_000000.smt2 |    0.119s | 25.388MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000048.smt2 |    0.119s | 25.152MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000012.smt2 |    0.120s | 24.972MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000105.smt2 |    0.120s | 25.572MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5406_sink_harvest_000000.smt2 |    0.120s | 25.436MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000042.smt2 |    0.120s | 24.828MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2969_sink_harvest_000001.smt2 |    0.120s | 25.904MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3789_sink_harvest_000000.smt2 |    0.120s | 22.152MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000041.smt2 |    0.120s | 25.176MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000118.smt2 |    0.120s | 24.92MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000056.smt2 |    0.120s | 24.808MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000039.smt2 |    0.120s | 21.396MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000000.smt2 |    0.120s | 24.964MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000036.smt2 |    0.120s | 24.832MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000052.smt2 |    0.121s | 25.752MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000026.smt2 |    0.121s | 24.964MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000021.smt2 |    0.121s | 25.432MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000058.smt2 |    0.121s | 24.956MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000070.smt2 |    0.121s | 25.096MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3305_sink_harvest_000000.smt2 |    0.121s | 21.828MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000057.smt2 |    0.121s | 25.092MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000087.smt2 |    0.121s | 24.94MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5323_sink_harvest_000000.smt2 |    0.121s | 25.752MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000099.smt2 |    0.121s | 24.98MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000028.smt2 |    0.121s | 25.004MiB| sat | 0 |  |  |
|noodler_killer_14_alignment_hell.smt2                        |    0.122s | 21.692MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5329_sink_harvest_000000.smt2 |    0.122s | 25.8MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5309_sink_harvest_000000.smt2 |    0.122s | 25.848MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000035.smt2 |    0.122s | 23.472MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000009.smt2 |    0.122s | 25.048MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5322_sink_harvest_000000.smt2 |    0.122s | 29.392MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000046.smt2 |    0.122s | 25.584MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4446_sink_harvest_000000.smt2 |    0.122s | 25.068MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3834_sink_harvest_000000.smt2 |    0.122s | 21.94MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000029.smt2 |    0.122s | 21.088MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000039.smt2 |    0.122s | 25.112MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000077.smt2 |    0.122s | 25.172MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000017.smt2 |    0.123s | 21.424MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000017.smt2 |    0.123s | 24.876MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000099.smt2 |    0.123s | 24.972MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3438_sink_harvest_000061.smt2 |    0.123s | 21.476MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000010.smt2 |    0.123s | 25.116MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000055.smt2 |    0.123s | 25.588MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000032.smt2 |    0.123s | 24.932MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000094.smt2 |    0.123s | 24.836MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000067.smt2 |    0.123s | 25.048MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000047.smt2 |    0.123s | 24.96MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4427_sink_harvest_000000.smt2 |    0.123s | 26.068MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_lyndon-schuetzenberg-1_harvest_000003_no_eq.smt2 |    0.123s | 20.368MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000037.smt2 |    0.124s | 24.976MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000039.smt2 |    0.124s | 23.6MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000040.smt2 |    0.124s | 25.172MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3783_sink_harvest_000000.smt2 |    0.124s | 23.096MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4434_sink_harvest_000000.smt2 |    0.124s | 24.98MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3406_sink_harvest_000000.smt2 |    0.124s | 21.792MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_770_sink_harvest_000000.smt2 |    0.124s | 22.536MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000019.smt2 |    0.124s | 24.812MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4145_sink_harvest_000000.smt2 |    0.125s | 22.048MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000096.smt2 |    0.125s | 25.452MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1856_sink_harvest_000000.smt2 |    0.125s | 22.056MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000032.smt2 |    0.125s | 24.92MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000083.smt2 |    0.126s | 24.992MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3090_sink_harvest_000000.smt2 |    0.126s | 21.868MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3701_sink_harvest_000000.smt2 |    0.126s | 23.436MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000034.smt2 |    0.126s | 23.452MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000101.smt2 |    0.126s | 24.824MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000040.smt2 |    0.126s | 23.432MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000064.smt2 |    0.127s | 23.628MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000091.smt2 |    0.127s | 25.304MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4808_sink_harvest_000000.smt2 |    0.127s | 25.852MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000080.smt2 |    0.127s | 25.128MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000087.smt2 |    0.127s | 24.98MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3519_sink_harvest_000000.smt2 |    0.127s | 21.44MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000058.smt2 |    0.127s | 23.5MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000065.smt2 |    0.127s | 24.932MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_noodles-unsat-9_harvest_000003_no_eq.smt2 |    0.127s | 20.956MiB| unsat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_noodles-unsat-2_harvest_000002_no_eq.smt2 |    0.127s | 20.608MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000027.smt2 |    0.128s | 25.12MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000117.smt2 |    0.128s | 25.092MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000056.smt2 |    0.128s | 23.528MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000081.smt2 |    0.128s | 25.1MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000015.smt2 |    0.128s | 24.996MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000010.smt2 |    0.128s | 25.16MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000100.smt2 |    0.129s | 25.912MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000055.smt2 |    0.129s | 25.332MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000079.smt2 |    0.129s | 24.968MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000111.smt2 |    0.129s | 25.0MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3947_sink_harvest_000000.smt2 |    0.129s | 23.26MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4041_sink_harvest_000000.smt2 |    0.129s | 25.68MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3434_sink_harvest_000013.smt2 |    0.129s | 21.204MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000024.smt2 |    0.129s | 24.976MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000104.smt2 |    0.129s | 25.332MiB| sat | 0 |  |  |
|hard_len_nonprim_1_alternating_parity.smt2                   |    0.130s | 20.388MiB| sat | 0 |  |  |
|deep_nest_membership_t17.smt2                                |    0.130s | 21.46MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000018.smt2 |    0.130s | 26.0MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000068.smt2 |    0.130s | 23.612MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000100.smt2 |    0.130s | 25.216MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4160_sink_harvest_000000.smt2 |    0.130s | 21.9MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4141_sink_harvest_000000.smt2 |    0.130s | 22.344MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000086.smt2 |    0.130s | 25.648MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000050.smt2 |    0.130s | 25.384MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000095.smt2 |    0.130s | 25.704MiB| sat | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_noodles-unsat-6_harvest_000000_no_eq.smt2 |    0.130s | 20.584MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000060.smt2 |    0.131s | 25.508MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000102.smt2 |    0.131s | 24.944MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000094.smt2 |    0.131s | 24.98MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000114.smt2 |    0.131s | 25.748MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000031.smt2 |    0.132s | 25.644MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4387_sink_harvest_000000.smt2 |    0.132s | 25.328MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4311_sink_harvest_000000.smt2 |    0.132s | 21.728MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000033.smt2 |    0.132s | 24.996MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4354_sink_harvest_000000.smt2 |    0.132s | 27.3MiB| sat | 0 |  |  |
|deep_nest_membership_t29.smt2                                |    0.133s | 21.168MiB| sat | 0 |  |  |
|deep_nest_membership_t6.smt2                                 |    0.133s | 20.996MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000044.smt2 |    0.133s | 24.948MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000014.smt2 |    0.133s | 25.112MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000115.smt2 |    0.134s | 25.016MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1612_sink_harvest_000007.smt2 |    0.134s | 20.396MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000126.smt2 |    0.134s | 25.716MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000020.smt2 |    0.134s | 24.956MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4426_sink_harvest_000000.smt2 |    0.134s | 26.072MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000088.smt2 |    0.134s | 25.508MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000066.smt2 |    0.134s | 25.488MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000066.smt2 |    0.134s | 25.092MiB| sat | 0 |  |  |
|hard_length_1_semilinear_complement.smt2                     |    0.135s | 23.712MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000085.smt2 |    0.135s | 24.968MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3211_sink_harvest_000000.smt2 |    0.135s | 21.8MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_noodles-unsat-5_1_harvest_000003.smt2 |    0.135s | 20.404MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000048.smt2 |    0.135s | 25.58MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000018.smt2 |    0.136s | 24.984MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000061.smt2 |    0.136s | 25.164MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3404_sink_harvest_000000.smt2 |    0.136s | 22.012MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000098.smt2 |    0.136s | 25.624MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000038.smt2 |    0.136s | 25.804MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000008.smt2 |    0.136s | 24.984MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5031_sink_harvest_000000.smt2 |    0.136s | 25.38MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000125.smt2 |    0.137s | 24.976MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000022.smt2 |    0.137s | 25.028MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000086.smt2 |    0.137s | 25.02MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000042.smt2 |    0.137s | 25.116MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000074.smt2 |    0.137s | 25.728MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5305_sink_harvest_000000.smt2 |    0.137s | 25.72MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_1578_sink_harvest_000000.smt2 |    0.137s | 22.344MiB| sat | 0 |  |  |
|noodler_killer_17_alternating_automata.smt2                  |    0.138s | 22.68MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000043.smt2 |    0.138s | 25.284MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000079.smt2 |    0.138s | 24.976MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4391_sink_harvest_000000.smt2 |    0.138s | 25.364MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000104.smt2 |    0.138s | 24.864MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000055.smt2 |    0.138s | 24.996MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000033.smt2 |    0.138s | 25.772MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5355_sink_harvest_000000.smt2 |    0.138s | 25.908MiB| sat | 0 |  |  |
|deep_nest_membership_t20.smt2                                |    0.139s | 21.524MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000027.smt2 |    0.139s | 24.988MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000036.smt2 |    0.139s | 25.02MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000056.smt2 |    0.139s | 25.044MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000053.smt2 |    0.139s | 25.524MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_lyndon-schuetzenberg-1_harvest_000010.smt2 |    0.139s | 20.376MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000016.smt2 |    0.140s | 24.832MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000107.smt2 |    0.140s | 24.88MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3205_sink_harvest_000000.smt2 |    0.140s | 21.812MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000072.smt2 |    0.140s | 24.968MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2781_sink_harvest_000000.smt2 |    0.141s | 22.728MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4354_sink_harvest_000002.smt2 |    0.141s | 27.364MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4312_sink_harvest_000000.smt2 |    0.142s | 32.856MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000076.smt2 |    0.142s | 25.632MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000025.smt2 |    0.142s | 25.552MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000064.smt2 |    0.142s | 25.456MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5310_sink_harvest_000000.smt2 |    0.143s | 29.548MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000053.smt2 |    0.143s | 25.588MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000069.smt2 |    0.143s | 25.744MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5039_sink_harvest_000000.smt2 |    0.143s | 29.34MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3286_sink_harvest_000000.smt2 |    0.144s | 23.476MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000062.smt2 |    0.144s | 25.456MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000010.smt2 |    0.144s | 25.664MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000030.smt2 |    0.144s | 25.8MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000098.smt2 |    0.144s | 25.02MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000092.smt2 |    0.145s | 25.02MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000112.smt2 |    0.145s | 25.696MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000089.smt2 |    0.145s | 24.924MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000078.smt2 |    0.145s | 24.892MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4238_sink_harvest_000000.smt2 |    0.145s | 24.72MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4393_sink_harvest_000000.smt2 |    0.145s | 25.348MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000091.smt2 |    0.145s | 25.092MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000046.smt2 |    0.146s | 25.0MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5149_sink_harvest_000000.smt2 |    0.146s | 25.48MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2469_sink_harvest_000000.smt2 |    0.146s | 22.44MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000013.smt2 |    0.146s | 25.096MiB| sat | 0 |  |  |
|noodler_killer_11_symmetric_nth.smt2                         |    0.147s | 22.972MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2969_sink_harvest_000000.smt2 |    0.147s | 28.332MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4118_sink_harvest_000000.smt2 |    0.147s | 25.596MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000049.smt2 |    0.147s | 24.82MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5400_sink_harvest_000000.smt2 |    0.147s | 29.312MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5040_sink_harvest_000000.smt2 |    0.147s | 25.86MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000023.smt2 |    0.147s | 25.468MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4227_sink_harvest_000000.smt2 |    0.147s | 28.592MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000012.smt2 |    0.147s | 25.08MiB| sat | 0 |  |  |
|deep_nest_membership_t11.smt2                                |    0.148s | 21.904MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000005.smt2 |    0.148s | 25.064MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4439_sink_harvest_000000.smt2 |    0.148s | 25.38MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000041.smt2 |    0.148s | 28.412MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000090.smt2 |    0.149s | 25.716MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000021.smt2 |    0.149s | 28.344MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000007.smt2 |    0.149s | 25.064MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000039.smt2 |    0.149s | 25.02MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000070.smt2 |    0.149s | 24.976MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000038.smt2 |    0.150s | 24.964MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000068.smt2 |    0.150s | 24.824MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000021.smt2 |    0.150s | 25.068MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4644_sink_harvest_000047.smt2 |    0.151s | 21.42MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2979_sink_harvest_000000.smt2 |    0.151s | 27.62MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3382_sink_harvest_000000.smt2 |    0.152s | 27.052MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000016.smt2 |    0.152s | 28.324MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000084.smt2 |    0.152s | 25.084MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5292_sink_harvest_000000.smt2 |    0.152s | 25.468MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000041.smt2 |    0.153s | 25.36MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000033.smt2 |    0.153s | 24.928MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000051.smt2 |    0.153s | 25.156MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000084.smt2 |    0.153s | 25.684MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000017.smt2 |    0.154s | 25.768MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2970_sink_harvest_000000.smt2 |    0.154s | 28.296MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000073.smt2 |    0.154s | 25.46MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5042_sink_harvest_000000.smt2 |    0.154s | 29.384MiB| sat | 0 |  |  |
|generated_tiny/split_membership_tiny_sat_0013.smt2           |    0.155s | 22.392MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4149_sink_harvest_000000.smt2 |    0.155s | 27.416MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5369_sink_harvest_000000.smt2 |    0.155s | 29.176MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4354_sink_harvest_000001.smt2 |    0.155s | 25.872MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000051.smt2 |    0.155s | 25.456MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2972_sink_harvest_000000.smt2 |    0.156s | 28.944MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000088.smt2 |    0.156s | 25.012MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5234_sink_harvest_000000.smt2 |    0.156s | 29.592MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000015.smt2 |    0.157s | 25.632MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000092.smt2 |    0.157s | 25.016MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000011.smt2 |    0.157s | 25.048MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5231_sink_harvest_000000.smt2 |    0.158s | 25.868MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5046_sink_harvest_000000.smt2 |    0.158s | 25.432MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000031.smt2 |    0.158s | 25.388MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000036.smt2 |    0.158s | 25.552MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3909_sink_harvest_000000.smt2 |    0.159s | 21.86MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5098_sink_harvest_000000.smt2 |    0.159s | 29.256MiB| sat | 0 |  |  |
|QF_extracted/20240318-omark_noodles-unsat-2_harvest_000001.smt2 |    0.159s | 21.536MiB| unsat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000057.smt2 |    0.159s | 24.968MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5038_sink_harvest_000000.smt2 |    0.159s | 29.224MiB| sat | 0 |  |  |
|hard_ext_3.smt2                                              |    0.160s | 21.784MiB| sat | 0 |  |  |
|generated_tiny/split_membership_tiny_sat_0021.smt2           |    0.160s | 22.412MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5135_sink_harvest_000000.smt2 |    0.160s | 29.308MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000096.smt2 |    0.160s | 25.172MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2092_sink_harvest_000000.smt2 |    0.160s | 30.596MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5032_sink_harvest_000000.smt2 |    0.161s | 25.484MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000064.smt2 |    0.162s | 25.076MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4389_sink_harvest_000000.smt2 |    0.162s | 25.544MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000045.smt2 |    0.163s | 24.96MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000019.smt2 |    0.164s | 25.148MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000072.smt2 |    0.164s | 25.024MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000065.smt2 |    0.164s | 24.912MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5319_sink_harvest_000000.smt2 |    0.164s | 29.236MiB| sat | 0 |  |  |
|generated_tiny/split_membership_tiny_sat_0024.smt2           |    0.165s | 22.04MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5392_sink_harvest_000000.smt2 |    0.165s | 29.256MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5291_sink_harvest_000000.smt2 |    0.165s | 29.316MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4719_sink_harvest_000000.smt2 |    0.165s | 30.072MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5262_sink_harvest_000000.smt2 |    0.165s | 29.22MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000025.smt2 |    0.165s | 24.924MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000053.smt2 |    0.165s | 25.112MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000030.smt2 |    0.166s | 24.86MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2973_sink_harvest_000000.smt2 |    0.166s | 29.132MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000008.smt2 |    0.166s | 24.924MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000004.smt2 |    0.166s | 25.004MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000037.smt2 |    0.166s | 24.964MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000106.smt2 |    0.166s | 24.92MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5326_sink_harvest_000000.smt2 |    0.166s | 25.596MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000121.smt2 |    0.167s | 24.848MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5401_sink_harvest_000000.smt2 |    0.167s | 29.292MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5340_sink_harvest_000000.smt2 |    0.168s | 29.304MiB| sat | 0 |  |  |
|generated_tiny/split_membership_tiny_sat_0008.smt2           |    0.169s | 22.644MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4513_sink_harvest_000000.smt2 |    0.169s | 23.592MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000049.smt2 |    0.169s | 25.0MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4192_sink_harvest_000000.smt2 |    0.170s | 29.056MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000048.smt2 |    0.170s | 25.32MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5041_sink_harvest_000000.smt2 |    0.170s | 29.116MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5337_sink_harvest_000000.smt2 |    0.170s | 29.46MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_38_sink_harvest_000000.smt2 |    0.170s | 29.08MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000029.smt2 |    0.171s | 28.444MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4308_sink_harvest_000000.smt2 |    0.171s | 32.884MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000024.smt2 |    0.171s | 24.82MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000089.smt2 |    0.173s | 25.044MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5025_sink_harvest_000000.smt2 |    0.173s | 29.34MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5086_sink_harvest_000000.smt2 |    0.174s | 29.372MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_62_sink_harvest_000000.smt2 |    0.174s | 29.084MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000016.smt2 |    0.174s | 24.964MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5356_sink_harvest_000000.smt2 |    0.175s | 29.26MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4209_sink_harvest_000000.smt2 |    0.175s | 33.464MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_30_sink_harvest_000000.smt2 |    0.175s | 29.1MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3084_sink_harvest_000000.smt2 |    0.176s | 27.44MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000119.smt2 |    0.177s | 25.288MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000040.smt2 |    0.177s | 25.432MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3793_sink_harvest_000068.smt2 |    0.178s | 24.972MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5332_sink_harvest_000000.smt2 |    0.179s | 31.888MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5403_sink_harvest_000000.smt2 |    0.180s | 29.156MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5048_sink_harvest_000000.smt2 |    0.180s | 29.372MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000103.smt2 |    0.180s | 25.704MiB| sat | 0 |  |  |
|noodler_killer_10_100th_from_end.smt2                        |    0.181s | 21.84MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5172_sink_harvest_000000.smt2 |    0.181s | 29.132MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4192_sink_harvest_000001.smt2 |    0.181s | 29.072MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000012.smt2 |    0.182s | 28.392MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000124.smt2 |    0.183s | 24.856MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5232_sink_harvest_000000.smt2 |    0.183s | 29.276MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5304_sink_harvest_000000.smt2 |    0.184s | 31.936MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5306_sink_harvest_000000.smt2 |    0.184s | 31.972MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4367_sink_harvest_000000.smt2 |    0.185s | 31.784MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5252_sink_harvest_000000.smt2 |    0.185s | 29.148MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5103_sink_harvest_000000.smt2 |    0.186s | 29.216MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5357_sink_harvest_000000.smt2 |    0.190s | 31.044MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000081.smt2 |    0.191s | 25.732MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000028.smt2 |    0.192s | 28.468MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000019.smt2 |    0.192s | 28.76MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3778_sink_harvest_000000.smt2 |    0.192s | 24.472MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_46_sink_harvest_000000.smt2 |    0.193s | 29.048MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3779_sink_harvest_000054.smt2 |    0.195s | 24.936MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000023.smt2 |    0.195s | 31.884MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_54_sink_harvest_000000.smt2 |    0.197s | 29.152MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4243_sink_harvest_000000.smt2 |    0.198s | 30.088MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5180_sink_harvest_000000.smt2 |    0.200s | 29.24MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2135_sink_harvest_000002.smt2 |    0.200s | 35.452MiB| sat | 0 |  |  |
|generated_tiny/split_membership_tiny_sat_0001.smt2           |    0.201s | 22.656MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3662_sink_harvest_000000.smt2 |    0.201s | 31.168MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3689_sink_harvest_000001.smt2 |    0.201s | 29.456MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5060_sink_harvest_000000.smt2 |    0.203s | 29.244MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4716_sink_harvest_000000.smt2 |    0.204s | 32.772MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2685_sink_harvest_000000.smt2 |    0.205s | 22.184MiB| sat | 0 |  |  |
|deep_nest_membership_t9.smt2                                 |    0.206s | 21.332MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3022_sink_harvest_000000.smt2 |    0.206s | 23.22MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000027.smt2 |    0.206s | 28.52MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2928_sink_harvest_000000.smt2 |    0.208s | 28.3MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5235_sink_harvest_000000.smt2 |    0.211s | 31.116MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5302_sink_harvest_000000.smt2 |    0.212s | 31.812MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5242_sink_harvest_000000.smt2 |    0.212s | 29.292MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5176_sink_harvest_000000.smt2 |    0.214s | 35.352MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5230_sink_harvest_000000.smt2 |    0.216s | 31.988MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3191_sink_harvest_000000.smt2 |    0.220s | 31.104MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3051_sink_harvest_000000.smt2 |    0.220s | 31.732MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2940_sink_harvest_000000.smt2 |    0.223s | 33.524MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4158_sink_harvest_000000.smt2 |    0.223s | 32.78MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5354_sink_harvest_000000.smt2 |    0.225s | 32.012MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2135_sink_harvest_000000.smt2 |    0.225s | 30.34MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4602_sink_harvest_000000.smt2 |    0.225s | 29.124MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4061_sink_harvest_000000.smt2 |    0.228s | 31.164MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4200_sink_harvest_000000.smt2 |    0.235s | 32.904MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4237_sink_harvest_000000.smt2 |    0.238s | 32.772MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2135_sink_harvest_000003.smt2 |    0.238s | 32.572MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5147_sink_harvest_000000.smt2 |    0.239s | 31.796MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4617_sink_harvest_000020.smt2 |    0.241s | 28.512MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5063_sink_harvest_000000.smt2 |    0.241s | 34.612MiB| sat | 0 |  |  |
|generated_tiny/split_membership_tiny_sat_0028.smt2           |    0.243s | 23.088MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4255_sink_harvest_000000.smt2 |    0.249s | 32.464MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5294_sink_harvest_000000.smt2 |    0.255s | 38.66MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4510_sink_harvest_000000.smt2 |    0.255s | 35.676MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5358_sink_harvest_000000.smt2 |    0.255s | 34.684MiB| sat | 0 |  |  |
|generated_tiny/split_membership_tiny_sat_0029.smt2           |    0.257s | 22.928MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5101_sink_harvest_000000.smt2 |    0.258s | 35.86MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4594_sink_harvest_000000.smt2 |    0.258s | 40.532MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3073_sink_harvest_000000.smt2 |    0.259s | 33.428MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5359_sink_harvest_000000.smt2 |    0.261s | 35.444MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3524_sink_harvest_000000.smt2 |    0.261s | 33.352MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5290_sink_harvest_000000.smt2 |    0.262s | 31.336MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5121_sink_harvest_000000.smt2 |    0.262s | 35.5MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4350_sink_harvest_000000.smt2 |    0.263s | 37.384MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5138_sink_harvest_000000.smt2 |    0.267s | 35.484MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3689_sink_harvest_000000.smt2 |    0.268s | 30.568MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5227_sink_harvest_000000.smt2 |    0.269s | 31.264MiB| sat | 0 |  |  |
|deep_nest_membership_t12.smt2                                |    0.270s | 21.896MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3796_sink_harvest_000000.smt2 |    0.278s | 35.48MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5136_sink_harvest_000000.smt2 |    0.279s | 34.688MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2961_sink_harvest_000000.smt2 |    0.280s | 33.716MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5137_sink_harvest_000000.smt2 |    0.294s | 36.168MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5134_sink_harvest_000000.smt2 |    0.296s | 35.812MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3549_sink_harvest_000000.smt2 |    0.301s | 39.364MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4040_sink_harvest_000000.smt2 |    0.302s | 40.472MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5049_sink_harvest_000000.smt2 |    0.306s | 38.536MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5236_sink_harvest_000000.smt2 |    0.312s | 34.52MiB| sat | 0 |  |  |
|generated_tiny/split_membership_tiny_sat_0006.smt2           |    0.313s | 24.096MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5028_sink_harvest_000000.smt2 |    0.316s | 38.548MiB| sat | 0 |  |  |
|hard_regex_membership_4.smt2                                 |    0.317s | 22.672MiB| sat | 0 |  |  |
|deep_nest_membership_t15.smt2                                |    0.321s | 21.82MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_5084_sink_harvest_000000.smt2 |    0.331s | 34.54MiB| sat | 0 |  |  |
|generated_tiny/split_membership_tiny_sat_0009.smt2           |    0.334s | 23.692MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3785_sink_harvest_000000.smt2 |    0.344s | 40.588MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4007_sink_harvest_000000.smt2 |    0.352s | 40.792MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4049_sink_harvest_000000.smt2 |    0.352s | 22.828MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4100_sink_harvest_000000.smt2 |    0.358s | 41.144MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_4071_sink_harvest_000000.smt2 |    0.368s | 40.784MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2924_sink_harvest_000000.smt2 |    0.370s | 42.164MiB| sat | 0 |  |  |
|generated_tiny/split_membership_tiny_sat_0004.smt2           |    0.416s | 23.972MiB| sat | 0 |  |  |
|hard_ext_9.smt2                                              |    0.444s | 27.988MiB| sat | 0 |  |  |
|generated_easy/split_membership_easy_sat_0003.smt2           |    0.709s | 26.416MiB| sat | 0 |  |  |
|generated_easy/split_membership_easy_sat_0016.smt2           |    0.803s | 25.88MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_182_sink_harvest_000000.smt2 |    0.900s | 75.632MiB| sat | 0 |  |  |
|deep_nest_membership_t1.smt2                                 |    0.955s | 24.52MiB| sat | 0 |  |  |
|generated/split_membership_medium_sat_0035.smt2              |    1.075s | 24.288MiB| sat | 0 |  |  |
|generated_easy/split_membership_easy_sat_0014.smt2           |    1.425s | 29.076MiB| sat | 0 |  |  |
|generated_easy/split_membership_easy_sat_0004.smt2           |    1.500s | 29.808MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3892_sink_harvest_000000.smt2 |    1.784s | 88.876MiB| sat | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3384_sink_harvest_000000.smt2 |    1.921s | 250.0MiB| sat | 0 |  |  |
|generated_easy/split_membership_easy_sat_0019.smt2           |    2.138s | 30.576MiB| sat | 0 |  |  |
|generated/split_membership_medium_sat_0000.smt2              |    2.179s | 30.232MiB| sat | 0 |  |  |
|generated/split_membership_medium_sat_0042.smt2              |    2.192s | 26.82MiB| sat | 0 |  |  |
|generated/split_membership_medium_sat_0059.smt2              |    2.195s | 30.176MiB| sat | 0 |  |  |
|generated/split_membership_medium_sat_0033.smt2              |    2.377s | 29.072MiB| sat | 0 |  |  |
|generated/split_membership_medium_sat_0053.smt2              |    2.604s | 32.632MiB| sat | 0 |  |  |
|generated/split_membership_medium_sat_0049.smt2              |    2.665s | 30.7MiB| sat | 0 |  |  |
|generated/split_membership_hard_sat_0018.smt2                |    2.864s | 30.492MiB| sat | 0 |  |  |
|generated/split_membership_medium_sat_0026.smt2              |    3.063s | 31.968MiB| sat | 0 |  |  |
|hard_ext_6.smt2                                              |    3.935s | 33.072MiB| sat | 0 |  |  |
|generated_easy/split_membership_easy_sat_0008.smt2           |    4.594s | 34.388MiB| sat | 0 |  |  |
|deep_nest_membership_batch2_5.smt2                           |    5.014s | 31.2MiB| timeout | 0 |  |  |
|QF_extracted/20240318-omark_lyndon-schuetzenberg-1_harvest_000004.smt2 |    5.014s | 26.44MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0027.smt2            |    5.014s | 29.112MiB| timeout | 0 |  |  |
|hard_cyclic_12_cascade.smt2                                  |    5.015s | 21.376MiB| timeout | 0 |  |  |
|hard_ext_2.smt2                                              |    5.015s | 30.368MiB| timeout | 0 |  |  |
|QF_extracted/20240318-omark_lyndon-schuetzenberg-1_harvest_000011.smt2 |    5.015s | 24.62MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0041.smt2            |    5.015s | 35.144MiB| timeout | 0 |  |  |
|generated/split_membership_medium_sat_0005.smt2              |    5.015s | 30.348MiB| timeout | 0 |  |  |
|generated/split_membership_medium_sat_0003.smt2              |    5.015s | 31.168MiB| timeout | 0 |  |  |
|QF_extracted/20240318-omark_lyndon-schuetzenberg-1_harvest_000007.smt2 |    5.016s | 35.64MiB| timeout | 0 |  |  |
|QF_extracted/20240318-omark_noodles-unsat-9_harvest_000001.smt2 |    5.016s | 45.368MiB| timeout | 0 |  |  |
|generated/split_membership_hard_sat_0015.smt2                |    5.016s | 33.408MiB| timeout | 0 |  |  |
|hard_regex_membership_9.smt2                                 |    5.017s | 29.36MiB| timeout | 0 |  |  |
|QF_extracted/20240318-omark_lyndon-schuetzenberg-1_harvest_000012.smt2 |    5.017s | 28.46MiB| timeout | 0 |  |  |
|generated/split_membership_hard_sat_0009.smt2                |    5.017s | 50.504MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0019.smt2            |    5.017s | 32.764MiB| timeout | 0 |  |  |
|deep_nest_membership_batch2_2.smt2                           |    5.018s | 49.316MiB| timeout | 0 |  |  |
|hard_cyclic_8_overapprox_conflict.smt2                       |    5.018s | 79.732MiB| timeout | 0 |  |  |
|generated_easy/split_membership_easy_sat_0001.smt2           |    5.018s | 31.06MiB| timeout | 0 |  |  |
|generated_tiny/split_membership_tiny_unsat_0023.smt2         |    5.018s | 31.268MiB| timeout | 0 |  |  |
|hard_cyclic_2.smt2                                           |    5.019s | 60.68MiB| timeout | 0 |  |  |
|hard_gen_10.smt2                                             |    5.020s | 50.556MiB| timeout | 0 |  |  |
|hard_gen_13.smt2                                             |    5.020s | 51.192MiB| timeout | 0 |  |  |
|generated_easy/split_membership_easy_unsat_0000.smt2         |    5.020s | 39.468MiB| timeout | 0 |  |  |
|hard_regex_membership_7.smt2                                 |    5.021s | 94.788MiB| timeout | 0 |  |  |
|QF_extracted/20240318-omark_noodles-unsat-2_harvest_000003.smt2 |    5.021s | 50.232MiB| timeout | 0 |  |  |
|generated_easy/split_membership_easy_unsat_0007.smt2         |    5.022s | 33.916MiB| timeout | 0 |  |  |
|xxRaaSa.smt2                                                 |    5.023s | 75.932MiB| timeout | 0 |  |  |
|hard_cyclic_1.smt2                                           |    5.024s | 64.548MiB| timeout | 0 |  |  |
|xbxRaaObaaaS.smt2                                            |    5.025s | 79.108MiB| timeout | 0 |  |  |
|hard_regex_membership_3.smt2                                 |    5.025s | 114.0MiB| timeout | 0 |  |  |
|generated/split_membership_hard_unsat_0017.smt2              |    5.026s | 81.024MiB| timeout | 0 |  |  |
|hard_regex_membership_6.smt2                                 |    5.028s | 50.224MiB| timeout | 0 |  |  |
|generated/split_membership_hard_unsat_0002.smt2              |    5.028s | 37.276MiB| timeout | 0 |  |  |
|deep_nest_membership_t4.smt2                                 |    5.030s | 58.464MiB| timeout | 0 |  |  |
|QF_extracted/20240318-omark_noodles-unsat-3_harvest_000000.smt2 |    5.030s | 66.88MiB| timeout | 0 |  |  |
|hard_regex_membership_5.smt2                                 |    5.031s | 45.84MiB| timeout | 0 |  |  |
|generated_easy/split_membership_easy_unsat_0010.smt2         |    5.031s | 30.512MiB| timeout | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2135_sink_harvest_000001.smt2 |    5.032s | 132.0MiB| timeout | 0 |  |  |
|hard_length_2_cegar_gradient.smt2                            |    5.033s | 122.0MiB| timeout | 0 |  |  |
|deep_nest_membership_t25.smt2                                |    5.033s | 29.324MiB| timeout | 0 |  |  |
|hard_gen_15.smt2                                             |    5.034s | 50.428MiB| timeout | 0 |  |  |
|hard_regex_membership_8.smt2                                 |    5.035s | 32.644MiB| timeout | 0 |  |  |
|test_hard_1.smt2                                             |    5.036s | 79.084MiB| timeout | 0 |  |  |
|deep_nest_membership_t23.smt2                                |    5.036s | 40.004MiB| timeout | 0 |  |  |
|generated_easy/split_membership_easy_sat_0018.smt2           |    5.036s | 36.932MiB| timeout | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2954_sink_harvest_000000.smt2 |    5.036s | 62.664MiB| timeout | 0 |  |  |
|QF_extracted/20240318-omark_noodles-unsat-8_harvest_000002.smt2 |    5.036s | 42.896MiB| timeout | 0 |  |  |
|generated_easy/split_membership_easy_unsat_0013.smt2         |    5.038s | 38.352MiB| timeout | 0 |  |  |
|generated_easy/split_membership_easy_unsat_0006.smt2         |    5.039s | 30.38MiB| timeout | 0 |  |  |
|noodler_killer_5_lcm_1_to_20.smt2                            |    5.040s | 232.0MiB| timeout | 0 |  |  |
|xbxRaaObbS.smt2                                              |    5.041s | 77.528MiB| timeout | 0 |  |  |
|noodler_killer_8_missing_all_length_8.smt2                   |    5.041s | 66.908MiB| timeout | 0 |  |  |
|QF_extracted/20240318-omark_noodles-unsat-8_harvest_000001.smt2 |    5.041s | 36.408MiB| timeout | 0 |  |  |
|hard_gen_26.smt2                                             |    5.042s | 34.512MiB| timeout | 0 |  |  |
|generated/split_membership_hard_sat_0011.smt2                |    5.045s | 50.768MiB| timeout | 0 |  |  |
|xbxRabcObcS.smt2                                             |    5.046s | 115.0MiB| timeout | 0 |  |  |
|generated_easy/split_membership_easy_sat_0017.smt2           |    5.046s | 30.348MiB| timeout | 0 |  |  |
|generated/split_membership_medium_sat_0018.smt2              |    5.046s | 52.104MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0039.smt2            |    5.046s | 30.128MiB| timeout | 0 |  |  |
|generated_easy/split_membership_easy_unsat_0002.smt2         |    5.047s | 39.176MiB| timeout | 0 |  |  |
|QF_extracted/20240318-omark_noodles-unsat-6_harvest_000000.smt2 |    5.047s | 60.936MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0024.smt2            |    5.047s | 29.34MiB| timeout | 0 |  |  |
|QF_extracted/20240318-omark_cloud-service-query-2_harvest_000002.smt2 |    5.048s | 28.132MiB| timeout | 0 |  |  |
|QF_extracted/20240318-omark_noodles-unsat-9_harvest_000002.smt2 |    5.049s | 43.256MiB| timeout | 0 |  |  |
|generated/split_membership_medium_sat_0054.smt2              |    5.049s | 31.888MiB| timeout | 0 |  |  |
|generated/split_membership_medium_sat_0052.smt2              |    5.049s | 31.596MiB| timeout | 0 |  |  |
|hard_len_nonprim_4_subsume_len_clash.smt2                    |    5.050s | 41.052MiB| timeout | 0 |  |  |
|generated_tiny/split_membership_tiny_unsat_0000.smt2         |    5.050s | 37.1MiB| timeout | 0 |  |  |
|generated/split_membership_hard_sat_0008.smt2                |    5.050s | 36.956MiB| timeout | 0 |  |  |
|generated/split_membership_medium_sat_0046.smt2              |    5.050s | 32.62MiB| timeout | 0 |  |  |
|hard_regex_membership_14.smt2                                |    5.051s | 30.316MiB| timeout | 0 |  |  |
|hard_regex_membership_2.smt2                                 |    5.051s | 79.296MiB| timeout | 0 |  |  |
|generated_easy/split_membership_easy_unsat_0011.smt2         |    5.051s | 38.624MiB| timeout | 0 |  |  |
|generated_easy/split_membership_easy_unsat_0015.smt2         |    5.051s | 35.032MiB| timeout | 0 |  |  |
|generated_easy/split_membership_easy_unsat_0009.smt2         |    5.051s | 30.148MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0051.smt2            |    5.051s | 29.284MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0056.smt2            |    5.051s | 28.936MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0050.smt2            |    5.051s | 34.98MiB| timeout | 0 |  |  |
|generated/split_membership_hard_unsat_0010.smt2              |    5.051s | 33.224MiB| timeout | 0 |  |  |
|QF_extracted/20240318-omark_cloud-service-query-2_harvest_000004.smt2 |    5.052s | 31.284MiB| timeout | 0 |  |  |
|QF_extracted/20240318-omark_noodles-unsat-8_harvest_000000.smt2 |    5.052s | 51.592MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0037.smt2            |    5.052s | 32.724MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0021.smt2            |    5.052s | 38.024MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0058.smt2            |    5.052s | 39.48MiB| timeout | 0 |  |  |
|hard_len_nonprim_5_odd_even_boundary_clash.smt2              |    5.053s | 53.048MiB| timeout | 0 |  |  |
|deep_nest_membership_t24.smt2                                |    5.053s | 37.768MiB| timeout | 0 |  |  |
|generated_easy/split_membership_easy_unsat_0005.smt2         |    5.053s | 37.476MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0001.smt2            |    5.053s | 31.156MiB| timeout | 0 |  |  |
|hard_gen_25.smt2                                             |    5.054s | 29.244MiB| timeout | 0 |  |  |
|hard_cyclic_13_parallel_graphs.smt2                          |    5.054s | 61.28MiB| timeout | 0 |  |  |
|deep_nest_membership_batch2_3.smt2                           |    5.054s | 48.556MiB| timeout | 0 |  |  |
|deep_nest_membership_batch2_4.smt2                           |    5.054s | 42.468MiB| timeout | 0 |  |  |
|generated_easy/split_membership_easy_sat_0012.smt2           |    5.054s | 31.304MiB| timeout | 0 |  |  |
|QF_extracted/20240318-omark_lyndon-schuetzenberg-1_harvest_000005.smt2 |    5.054s | 50.048MiB| timeout | 0 |  |  |
|hard_gen_23.smt2                                             |    5.055s | 33.668MiB| timeout | 0 |  |  |
|QF_extracted/20240318-omark_noodles-unsat-9_harvest_000000.smt2 |    5.055s | 64.324MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0023.smt2            |    5.055s | 29.584MiB| timeout | 0 |  |  |
|noodler_killer_13_permutation_grid.smt2                      |    5.056s | 47.26MiB| timeout | 0 |  |  |
|deep_nest_membership_t16.smt2                                |    5.056s | 80.568MiB| timeout | 0 |  |  |
|generated_tiny/split_membership_tiny_unsat_0011.smt2         |    5.056s | 35.176MiB| timeout | 0 |  |  |
|test_hard_0.smt2                                             |    5.057s | 76.676MiB| timeout | 0 |  |  |
|generated_tiny/split_membership_tiny_unsat_0026.smt2         |    5.057s | 31.616MiB| timeout | 0 |  |  |
|generated_tiny/split_membership_tiny_unsat_0020.smt2         |    5.057s | 34.252MiB| timeout | 0 |  |  |
|QF_extracted/20240318-omark_noodles-unsat_harvest_000004.smt2 |    5.057s | 248.0MiB| timeout | 0 |  |  |
|QF_extracted/20240318-omark_lyndon-schuetzenberg-1_harvest_000008.smt2 |    5.057s | 37.056MiB| timeout | 0 |  |  |
|hard_cyclic_14_ring_buffer.smt2                              |    5.058s | 80.468MiB| timeout | 0 |  |  |
|xxRabObaPaaaa.smt2                                           |    5.058s | 84.464MiB| timeout | 0 |  |  |
|hard_len_nonprim_8_dual_diophantine.smt2                     |    5.059s | 287.0MiB| timeout | 0 |  |  |
|hard_gen_24.smt2                                             |    5.059s | 29.468MiB| timeout | 0 |  |  |
|generated_tiny/split_membership_tiny_unsat_0002.smt2         |    5.059s | 37.132MiB| timeout | 0 |  |  |
|generated/split_membership_medium_sat_0006.smt2              |    5.059s | 33.344MiB| timeout | 0 |  |  |
|QF_extracted/20240318-omark_cloud-service-query-2_harvest_000005.smt2 |    5.060s | 30.844MiB| timeout | 0 |  |  |
|generated/split_membership_medium_sat_0025.smt2              |    5.060s | 32.92MiB| timeout | 0 |  |  |
|hard_len_nonprim_2_cyclic_phase_shift.smt2                   |    5.061s | 57.448MiB| timeout | 0 |  |  |
|hard_cyclic_3.smt2                                           |    5.061s | 40.916MiB| timeout | 0 |  |  |
|generated/split_membership_medium_sat_0045.smt2              |    5.061s | 35.356MiB| timeout | 0 |  |  |
|QF_extracted/20240318-omark_lyndon-schuetzenberg-1_harvest_000013.smt2 |    5.063s | 32.948MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0047.smt2            |    5.063s | 32.916MiB| timeout | 0 |  |  |
|deep_nest_membership_t30.smt2                                |    5.064s | 30.804MiB| timeout | 0 |  |  |
|QF_extracted/20240318-omark_cloud-service-query-2_harvest_000003.smt2 |    5.064s | 32.592MiB| timeout | 0 |  |  |
|deep_nest_membership_t21.smt2                                |    5.065s | 50.184MiB| timeout | 0 |  |  |
|hard_cyclic_15_multivar_subsume.smt2                         |    5.067s | 63.82MiB| timeout | 0 |  |  |
|noodler_killer_7_alignment_explosion.smt2                    |    5.067s | 28.744MiB| timeout | 0 |  |  |
|hard_gen_22.smt2                                             |    5.067s | 32.124MiB| timeout | 0 |  |  |
|deep_nest_membership_batch3_24.smt2                          |    5.067s | 98.0MiB| timeout | 0 |  |  |
|generated/split_membership_hard_sat_0003.smt2                |    5.067s | 36.6MiB| timeout | 0 |  |  |
|xabxRabSAxbabxRabaS.smt2                                     |    5.068s | 81.348MiB| timeout | 0 |  |  |
|deep_nest_membership_t28.smt2                                |    5.068s | 60.98MiB| timeout | 0 |  |  |
|deep_nest_membership_t5.smt2                                 |    5.068s | 77.372MiB| timeout | 0 |  |  |
|deep_nest_membership_t13.smt2                                |    5.068s | 42.816MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0017.smt2            |    5.068s | 29.592MiB| timeout | 0 |  |  |
|deep_nest_membership_batch2_1.smt2                           |    5.069s | 48.848MiB| timeout | 0 |  |  |
|generated/split_membership_medium_sat_0044.smt2              |    5.069s | 30.824MiB| timeout | 0 |  |  |
|generated/split_membership_hard_unsat_0014.smt2              |    5.069s | 34.876MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0034.smt2            |    5.069s | 29.352MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0048.smt2            |    5.069s | 35.14MiB| timeout | 0 |  |  |
|hard_gen_21.smt2                                             |    5.070s | 29.12MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0031.smt2            |    5.070s | 29.92MiB| timeout | 0 |  |  |
|generated/split_membership_hard_sat_0019.smt2                |    5.070s | 32.464MiB| timeout | 0 |  |  |
|deep_nest_membership_t27.smt2                                |    5.071s | 31.668MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0028.smt2            |    5.072s | 30.748MiB| timeout | 0 |  |  |
|generated/split_membership_medium_sat_0011.smt2              |    5.073s | 31.84MiB| timeout | 0 |  |  |
|generated/split_membership_hard_unsat_0004.smt2              |    5.073s | 31.364MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0030.smt2            |    5.073s | 31.948MiB| timeout | 0 |  |  |
|hard_gen_16.smt2                                             |    5.074s | 35.828MiB| timeout | 0 |  |  |
|generated_tiny/split_membership_tiny_unsat_0005.smt2         |    5.074s | 38.168MiB| timeout | 0 |  |  |
|generated_tiny/split_membership_tiny_unsat_0014.smt2         |    5.075s | 34.172MiB| timeout | 0 |  |  |
|generated_tiny/split_membership_tiny_unsat_0015.smt2         |    5.075s | 38.532MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0022.smt2            |    5.075s | 30.468MiB| timeout | 0 |  |  |
|generated/split_membership_hard_sat_0000.smt2                |    5.075s | 42.884MiB| timeout | 0 |  |  |
|generated/split_membership_medium_sat_0004.smt2              |    5.075s | 32.352MiB| timeout | 0 |  |  |
|QF_extracted/20240318-omark_noodles-unsat-5_1_harvest_000000.smt2 |    5.076s | 45.7MiB| timeout | 0 |  |  |
|generated/split_membership_hard_unsat_0013.smt2              |    5.076s | 52.568MiB| timeout | 0 |  |  |
|hard_gen_12.smt2                                             |    5.077s | 30.332MiB| timeout | 0 |  |  |
|hard_regex_membership_10.smt2                                |    5.077s | 50.548MiB| timeout | 0 |  |  |
|deep_nest_membership_t10.smt2                                |    5.077s | 32.632MiB| timeout | 0 |  |  |
|deep_nest_membership_t2.smt2                                 |    5.077s | 28.836MiB| timeout | 0 |  |  |
|hard_gen_11.smt2                                             |    5.077s | 30.384MiB| timeout | 0 |  |  |
|generated_tiny/split_membership_tiny_unsat_0003.smt2         |    5.077s | 40.28MiB| timeout | 0 |  |  |
|generated_tiny/split_membership_tiny_unsat_0019.smt2         |    5.078s | 35.688MiB| timeout | 0 |  |  |
|deep_nest_membership_t18.smt2                                |    5.079s | 49.676MiB| timeout | 0 |  |  |
|hard_regex_membership_1.smt2                                 |    5.080s | 76.684MiB| timeout | 0 |  |  |
|hard_regex_membership_17.smt2                                |    5.080s | 32.756MiB| timeout | 0 |  |  |
|hard_cyclic_4_iso_unsat.smt2                                 |    5.080s | 53.704MiB| timeout | 0 |  |  |
|deep_nest_membership_t8.smt2                                 |    5.080s | 50.116MiB| timeout | 0 |  |  |
|QF_extracted/20240318-omark_noodles-unsat_harvest_000003.smt2 |    5.081s | 251.0MiB| timeout | 0 |  |  |
|hard_cyclic_6_cross_boundary.smt2                            |    5.082s | 115.0MiB| timeout | 0 |  |  |
|noodler_killer_2_primorial_length.smt2                       |    5.083s | 127.0MiB| timeout | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_2133_sink_harvest_000000.smt2 |    5.083s | 50.448MiB| timeout | 0 |  |  |
|noodler_killer_15_debruijn_trap.smt2                         |    5.084s | 820.0MiB| timeout | 0 |  |  |
|xabxRabaS.smt2                                               |    5.084s | 71.424MiB| timeout | 0 |  |  |
|QF_extracted/no_eqs/20240318-omark_noodles-unsat_harvest_000003_no_eq.smt2 |    5.085s | 94.78MiB| timeout | 0 |  |  |
|generated/split_membership_medium_sat_0009.smt2              |    5.085s | 29.596MiB| timeout | 0 |  |  |
|hard_ext_8.smt2                                              |    5.086s | 30.54MiB| timeout | 0 |  |  |
|generated_tiny/split_membership_tiny_unsat_0018.smt2         |    5.086s | 38.064MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0010.smt2            |    5.086s | 28.548MiB| timeout | 0 |  |  |
|generated/split_membership_medium_sat_0013.smt2              |    5.086s | 31.192MiB| timeout | 0 |  |  |
|generated/split_membership_medium_sat_0036.smt2              |    5.086s | 29.012MiB| timeout | 0 |  |  |
|QF_extracted/2019-Jiang_slog_slog_stranger_3353_sink_harvest_000000.smt2 |    5.087s | 744.0MiB| timeout | 0 |  |  |
|generated/split_membership_medium_sat_0016.smt2              |    5.087s | 30.844MiB| timeout | 0 |  |  |
|xbxRabS.smt2                                                 |    5.088s | 248.0MiB| timeout | 0 |  |  |
|generated/split_membership_medium_sat_0040.smt2              |    5.089s | 30.944MiB| timeout | 0 |  |  |
|generated/split_membership_hard_unsat_0016.smt2              |    5.089s | 31.364MiB| timeout | 0 |  |  |
|generated/split_membership_medium_sat_0002.smt2              |    5.089s | 32.12MiB| timeout | 0 |  |  |
|generated_tiny/split_membership_tiny_unsat_0016.smt2         |    5.090s | 31.04MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0008.smt2            |    5.090s | 33.768MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0055.smt2            |    5.091s | 30.692MiB| timeout | 0 |  |  |
|hard_len_nonprim_6_cegar_interleaved.smt2                    |    5.092s | 110.0MiB| timeout | 0 |  |  |
|deep_nest_membership_t14.smt2                                |    5.093s | 49.452MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0057.smt2            |    5.093s | 29.88MiB| timeout | 0 |  |  |
|hard_len_nonprim_3_cegar_diophantine.smt2                    |    5.094s | 135.0MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0007.smt2            |    5.094s | 31.092MiB| timeout | 0 |  |  |
|generated/split_membership_medium_sat_0012.smt2              |    5.094s | 31.12MiB| timeout | 0 |  |  |
|generated/split_membership_hard_sat_0007.smt2                |    5.095s | 34.728MiB| timeout | 0 |  |  |
|xaxRabS.smt2                                                 |    5.096s | 481.0MiB| timeout | 0 |  |  |
|deep_nest_membership_t22.smt2                                |    5.098s | 32.516MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0043.smt2            |    5.098s | 30.564MiB| timeout | 0 |  |  |
|hard_gen_20.smt2                                             |    5.100s | 31.864MiB| timeout | 0 |  |  |
|hard_gen_14.smt2                                             |    5.103s | 50.34MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0029.smt2            |    5.103s | 29.208MiB| timeout | 0 |  |  |
|generated/split_membership_hard_unsat_0012.smt2              |    5.103s | 37.196MiB| timeout | 0 |  |  |
|generated/split_membership_hard_unsat_0006.smt2              |    5.105s | 31.78MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0032.smt2            |    5.106s | 35.468MiB| timeout | 0 |  |  |
|generated/split_membership_medium_sat_0014.smt2              |    5.106s | 30.272MiB| timeout | 0 |  |  |
|generated/split_membership_medium_sat_0015.smt2              |    5.106s | 30.252MiB| timeout | 0 |  |  |
|generated/split_membership_medium_sat_0038.smt2              |    5.106s | 28.304MiB| timeout | 0 |  |  |
|generated/split_membership_medium_unsat_0020.smt2            |    5.107s | 34.848MiB| timeout | 0 |  |  |
|generated/split_membership_hard_unsat_0005.smt2              |    5.107s | 32.588MiB| timeout | 0 |  |  |
|hard_length_4_cross_variable_parity.smt2                     |    5.110s | 204.0MiB| timeout | 0 |  |  |
|generated/split_membership_hard_sat_0001.smt2                |    5.110s | 50.844MiB| timeout | 0 |  |  |
|noodler_killer_12_block_unary_lcm.smt2                       |    5.112s | 437.0MiB| timeout | 0 |  |  |
|generated_tiny/split_membership_tiny_unsat_0012.smt2         |    5.116s | 252.0MiB| timeout | 0 |  |  |
|hard_len_nonprim_7_difference_parity.smt2                    |    5.143s | 383.0MiB| timeout | 0 |  |  |
|hard_length_3_delayed_conflict.smt2                          |    5.149s | 634.0MiB| timeout | 0 |  |  |
