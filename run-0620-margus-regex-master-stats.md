# .

* SAT 184
* UNSAT 40
* TIMEOUT 101
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: 
Job tag: margus-regex-master
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: eccdffa78168015d5d21564c310c66ca2db0dd8e
Z3 branch: master
Z3 options: "-T:5 model_validate=true smt.random_seed=1"
Z3 inputs: inputs/MargusRegex
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
|lookaround/gen/t10-disjoint-latinext-cyr-m1-unsat.smt2       |    0.034s | 20.988MiB| unsat | 0 |  |  |
|lookaround/gen/t04-mod-lower-2-sat.smt2                      |    0.037s | 21.28MiB| sat | 0 |  |  |
|lookaround/gen/t04-mod-lower-3-sat.smt2                      |    0.037s | 22.46MiB| sat | 0 |  |  |
|levels/L2-05-compl-before-concat-sat.smt2                    |    0.040s | 20.768MiB| sat | 0 |  |  |
|lookaround/gen-lb/l12-flank-in-lower-ing-xyx-sat.smt2        |    0.040s | 21.148MiB| sat | 0 |  |  |
|lookaround/gen-lb/l11-nlb-in-digit-xyx-sat.smt2              |    0.042s | 21.432MiB| sat | 0 |  |  |
|lookaround/gen-lb/l11-nlb-sub-lower-xyx-sat.smt2             |    0.047s | 21.28MiB| sat | 0 |  |  |
|lookaround/gen/t01-border-fromofin-unsat.smt2                |    0.048s | 20.872MiB| unsat | 0 |  |  |
|hard_cyclic_9_subsumption.smt2                               |    0.049s | 20.324MiB| sat | 0 |  |  |
|levels/L3-03-nested-compl-d3-sat.smt2                        |    0.051s | 20.396MiB| sat | 0 |  |  |
|lookaround/gen/t02-contains-cyr-xyxy-sat.smt2                |    0.051s | 22.152MiB| sat | 0 |  |  |
|lookaround/pin-five-digits-parity-unsat.smt2                 |    0.053s | 21.012MiB| unsat | 0 |  |  |
|lookaround/gen/t02-contains-digit-xyxy-sat.smt2              |    0.054s | 21.644MiB| sat | 0 |  |  |
|lookaround/gen/t08-periodic-xy-xyxy-sat.smt2                 |    0.054s | 20.896MiB| sat | 0 |  |  |
|levels/L1-03-compl-sat.smt2                                  |    0.055s | 20.46MiB| sat | 0 |  |  |
|levels/L0-01-prim-star-sat.smt2                              |    0.055s | 20.42MiB| sat | 0 |  |  |
|lookaround/gen/t09-conflict-lower-digit-unsat.smt2           |    0.055s | 20.748MiB| unsat | 0 |  |  |
|lookaround/gen/t04-exact-digit-2-sat.smt2                    |    0.055s | 21.164MiB| sat | 0 |  |  |
|lookaround/gen/t09-conflict-cjk-lower-unsat.smt2             |    0.057s | 20.88MiB| unsat | 0 |  |  |
|lookaround/gen/t10-disjoint-emoji-m1-unsat.smt2              |    0.057s | 21.404MiB| unsat | 0 |  |  |
|lookaround/gen/t09-conflict-digit-lower-unsat.smt2           |    0.057s | 20.908MiB| unsat | 0 |  |  |
|lookaround/gen/t09-conflict-latinext-digit-unsat.smt2        |    0.057s | 20.924MiB| unsat | 0 |  |  |
|lookaround/gen/t08-periodic-de-xyxy-sat.smt2                 |    0.057s | 20.884MiB| sat | 0 |  |  |
|hard_cyclic_11_deep_unrolling.smt2                           |    0.058s | 20.872MiB| sat | 0 |  |  |
|levels/L3-04-multivar-inter-sat.smt2                         |    0.058s | 20.996MiB| sat | 0 |  |  |
|lookaround/gen/t08-periodic-ab-xyxy-sat.smt2                 |    0.058s | 20.624MiB| sat | 0 |  |  |
|lookaround/gen/t08-periodic-ab-xyzxyz-sat.smt2               |    0.058s | 21.612MiB| sat | 0 |  |  |
|lookaround/gen/t08-periodic-abc-xyzxyz-sat.smt2              |    0.059s | 21.176MiB| sat | 0 |  |  |
|lookaround/gen-lb/l12-flank-cu-lower-ed-xyyx-sat.smt2        |    0.060s | 21.076MiB| sat | 0 |  |  |
|lookaround/gen-lb/l12-flank-cu-lower-ing-xyyx-sat.smt2       |    0.061s | 21.144MiB| sat | 0 |  |  |
|lookaround/gen/t04-exact-lower-4-sat.smt2                    |    0.061s | 22.264MiB| sat | 0 |  |  |
|lookaround/gen/t04-mod-digit-3-sat.smt2                      |    0.062s | 22.012MiB| sat | 0 |  |  |
|levels/L2-02-compl-in-concat-sat.smt2                        |    0.063s | 20.512MiB| sat | 0 |  |  |
|levels/L2-07-inter-in-concat-unsat.smt2                      |    0.063s | 20.568MiB| unsat | 0 |  |  |
|lookaround/gen/t04-mod-hex-3-sat.smt2                        |    0.064s | 22.652MiB| sat | 0 |  |  |
|levels/L0-05-two-var-sat.smt2                                |    0.065s | 20.592MiB| sat | 0 |  |  |
|levels/L1-02-inter-unsat.smt2                                |    0.065s | 20.216MiB| unsat | 0 |  |  |
|lookaround/gen/t01-border-dow-unsat.smt2                     |    0.066s | 20.652MiB| unsat | 0 |  |  |
|lookaround/gen/t04-exact-lower-2-sat.smt2                    |    0.067s | 21.12MiB| sat | 0 |  |  |
|lookaround/gen/t10-disjoint-digit-upper-m1-unsat.smt2        |    0.068s | 20.648MiB| unsat | 0 |  |  |
|lookaround/gen/t04-mod-hex-2-sat.smt2                        |    0.069s | 22.036MiB| sat | 0 |  |  |
|lookaround/gen/t10-disjoint-surrogate-m2-unsat.smt2          |    0.069s | 21.376MiB| unsat | 0 |  |  |
|lookaround/gen/t08-periodic-abc-xyxy-sat.smt2                |    0.070s | 21.144MiB| sat | 0 |  |  |
|levels/L1-04-compl-unsat.smt2                                |    0.071s | 19.544MiB| unsat | 0 |  |  |
|lookaround/gen/t01-border-boolnull-unsat.smt2                |    0.071s | 21.008MiB| unsat | 0 |  |  |
|levels/L0-02-concat-word-sat.smt2                            |    0.072s | 20.728MiB| sat | 0 |  |  |
|lookaround/gen/t09-conflict-digit-upper-unsat.smt2           |    0.072s | 20.808MiB| unsat | 0 |  |  |
|lookaround/gen/t01-border-method-unsat.smt2                  |    0.072s | 21.116MiB| unsat | 0 |  |  |
|lookaround/gen/t04-exact-digit-6-sat.smt2                    |    0.072s | 22.416MiB| sat | 0 |  |  |
|lookaround/gen/t02-contains-lower-xyx-sat.smt2               |    0.072s | 21.136MiB| sat | 0 |  |  |
|lookaround/gen/t09-conflict-cyr-lower-unsat.smt2             |    0.073s | 20.764MiB| unsat | 0 |  |  |
|lookaround/gen/t04-exact-digit-4-sat.smt2                    |    0.073s | 22.432MiB| sat | 0 |  |  |
|lookaround/gen-lb/l12-flank-cu-digit-ed-xyyx-sat.smt2        |    0.074s | 21.132MiB| sat | 0 |  |  |
|lookaround/gen/t04-mod-lower-5-sat.smt2                      |    0.074s | 22.128MiB| sat | 0 |  |  |
|lookaround/gen/t10-disjoint-surrogate-m1-unsat.smt2          |    0.074s | 21.024MiB| unsat | 0 |  |  |
|hard_cyclic_5_iso_sat.smt2                                   |    0.075s | 20.972MiB| sat | 0 |  |  |
|levels/L1-01-inter-sat.smt2                                  |    0.075s | 20.7MiB| sat | 0 |  |  |
|lookaround/gen-lb/l12-flank-cu-digit-ing-xyyx-sat.smt2       |    0.075s | 20.872MiB| sat | 0 |  |  |
|lookaround/gen/t10-disjoint-latinext-cyr-m2-unsat.smt2       |    0.075s | 21.264MiB| unsat | 0 |  |  |
|lookaround/gen/t08-pairs-upper-digit-xyxy-sat.smt2           |    0.075s | 21.168MiB| sat | 0 |  |  |
|lookaround/gen-lb/l15-negcount-lower-m3-dash-sat.smt2        |    0.076s | 22.456MiB| sat | 0 |  |  |
|lookaround/gen-lb/l15-negcount-lower-m3-under-sat.smt2       |    0.077s | 22.468MiB| sat | 0 |  |  |
|lookaround/gen/t02-contains-hex-xyx-sat.smt2                 |    0.077s | 21.616MiB| sat | 0 |  |  |
|lookaround/gen/t10-disjoint-emoji-m2-unsat.smt2              |    0.077s | 21.692MiB| unsat | 0 |  |  |
|lookaround/gen/t01-border-spectest-sat.smt2                  |    0.078s | 21.084MiB| sat | 0 |  |  |
|hard_cyclic_7_overlapping_cycles.smt2                        |    0.079s | 20.524MiB| sat | 0 |  |  |
|levels/L2-03-alt-sat.smt2                                    |    0.079s | 20.476MiB| sat | 0 |  |  |
|lookaround/gen/t08-periodic-xy-xyzxyz-sat.smt2               |    0.079s | 21.536MiB| sat | 0 |  |  |
|lookaround/gen/t09-conflict-upper-digit-unsat.smt2           |    0.079s | 20.9MiB| unsat | 0 |  |  |
|lookaround/gen/t01-border-unit-unsat.smt2                    |    0.079s | 20.744MiB| unsat | 0 |  |  |
|lookaround/gen/t01-border-scheme-unsat.smt2                  |    0.079s | 20.94MiB| unsat | 0 |  |  |
|lookaround/gen/t02-contains-hex-xyxy-sat.smt2                |    0.080s | 22.192MiB| sat | 0 |  |  |
|lookaround/gen/t02-contains-alnum-xyxy-sat.smt2              |    0.081s | 22.648MiB| sat | 0 |  |  |
|lookaround/gen/t02-contains-alnum-xxyy-sat.smt2              |    0.081s | 22.644MiB| sat | 0 |  |  |
|lookaround/gen-lb/l11-nlb-cu-digit-xyx-sat.smt2              |    0.082s | 21.408MiB| sat | 0 |  |  |
|lookaround/gen-lb/l15-negcount-hex-m2-colon-sat.smt2         |    0.083s | 22.708MiB| sat | 0 |  |  |
|lookaround/gen/t04-exact-lower-6-sat.smt2                    |    0.083s | 22.532MiB| sat | 0 |  |  |
|levels/L4-01-loop-sat.smt2                                   |    0.084s | 20.808MiB| sat | 0 |  |  |
|lookaround/gen-lb/l12-flank-in-digit-ed-xyx-sat.smt2         |    0.084s | 21.188MiB| sat | 0 |  |  |
|lookaround/gen/t01-border-boollit-unsat.smt2                 |    0.084s | 21.108MiB| unsat | 0 |  |  |
|lookaround/gen/t02-contains-hex-xxyy-sat.smt2                |    0.084s | 22.152MiB| sat | 0 |  |  |
|lookaround/gen/t04-exact-hex-2-sat.smt2                      |    0.084s | 21.58MiB| sat | 0 |  |  |
|lookaround/gen-lb/l15-negcount-digit-m3-colon-sat.smt2       |    0.086s | 22.4MiB| sat | 0 |  |  |
|lookaround/gen-lb/l12-flank-cu-lower-ing-xyx-sat.smt2        |    0.086s | 21.096MiB| sat | 0 |  |  |
|lookaround/gen/t04-mod-digit-2-sat.smt2                      |    0.087s | 21.408MiB| sat | 0 |  |  |
|lookaround/gen-lb/l12-flank-in-lower-ed-xyyx-sat.smt2        |    0.088s | 20.912MiB| sat | 0 |  |  |
|lookaround/gen-lb/l16-nest-ingg-lower-xyx-sat.smt2           |    0.090s | 21.46MiB| sat | 0 |  |  |
|lookaround/gen-lb/l16-nest-ingg-digit-xyx-sat.smt2           |    0.090s | 21.764MiB| sat | 0 |  |  |
|lookaround/gen-lb/l11-nlb-in-lower-xyx-sat.smt2              |    0.091s | 21.1MiB| sat | 0 |  |  |
|lookaround/gen-lb/l12-flank-cu-digit-ed-xyx-sat.smt2         |    0.091s | 20.792MiB| sat | 0 |  |  |
|lookaround/gen-lb/l15-negcount-digit-m3-under-sat.smt2       |    0.091s | 22.02MiB| sat | 0 |  |  |
|lookaround/gen-lb/l16-nest-xyzz-lower-xyx-sat.smt2           |    0.092s | 21.796MiB| sat | 0 |  |  |
|lookaround/gen/t02-contains-cyr-xyx-sat.smt2                 |    0.092s | 21.128MiB| sat | 0 |  |  |
|lookaround/gen-lb/l12-flank-in-lower-ing-xyyx-sat.smt2       |    0.093s | 21.068MiB| sat | 0 |  |  |
|lookaround/gen/t02-contains-lower-xyxy-sat.smt2              |    0.093s | 21.448MiB| sat | 0 |  |  |
|lookaround/gen/t01-border-loglevel-unsat.smt2                |    0.093s | 21.38MiB| unsat | 0 |  |  |
|lookaround/gen-lb/l15-negcount-digit-m2-colon-sat.smt2       |    0.094s | 21.612MiB| sat | 0 |  |  |
|lookaround/gen-lb/l11-nlb-in-lower-xyyx-sat.smt2             |    0.094s | 21.8MiB| sat | 0 |  |  |
|lookaround/gen/t10-disjoint-digit-upper-m2-unsat.smt2        |    0.094s | 21.216MiB| unsat | 0 |  |  |
|lookaround/gen/t02-contains-lower-xxyy-sat.smt2              |    0.094s | 21.392MiB| sat | 0 |  |  |
|lookaround/gen/t03-twosided-upper-digit-xyx-sat.smt2         |    0.096s | 22.896MiB| sat | 0 |  |  |
|lookaround/gen/t03-twosided-hex-upper-xyx-sat.smt2           |    0.096s | 22.96MiB| sat | 0 |  |  |
|levels/L3-01-double-compl-sat.smt2                           |    0.097s | 20.56MiB| sat | 0 |  |  |
|levels/L2-01-inter-in-concat-sat.smt2                        |    0.098s | 20.664MiB| sat | 0 |  |  |
|lookaround/gen-lb/l12-flank-cu-digit-ing-xyx-sat.smt2        |    0.098s | 20.872MiB| sat | 0 |  |  |
|lookaround/gen-lb/l12-flank-in-lower-ed-xyx-sat.smt2         |    0.098s | 20.876MiB| sat | 0 |  |  |
|lookaround/gen/t02-contains-cyr-xxyy-sat.smt2                |    0.098s | 21.588MiB| sat | 0 |  |  |
|hard_cyclic_10_linear_form.smt2                              |    0.099s | 20.192MiB| unsat | 0 |  |  |
|levels/L1-05-multi-inter-sat.smt2                            |    0.099s | 20.368MiB| sat | 0 |  |  |
|lookaround/gen/t08-periodic-de-xyzyxz-sat.smt2               |    0.099s | 23.212MiB| sat | 0 |  |  |
|lookaround/gen/t03-twosided-lower-digit-xyx-sat.smt2         |    0.100s | 22.916MiB| sat | 0 |  |  |
|lookaround/hex-color-3-or-6-sat.smt2                         |    0.101s | 22.152MiB| sat | 0 |  |  |
|lookaround/gen-lb/l15-negcount-digit-m3-dash-sat.smt2        |    0.101s | 22.22MiB| sat | 0 |  |  |
|lookaround/gen-lb/l12-flank-in-digit-ed-xyyx-sat.smt2        |    0.101s | 21.132MiB| sat | 0 |  |  |
|lookaround/gen/t03-twosided-digit-lower-xyyx-sat.smt2        |    0.101s | 23.696MiB| sat | 0 |  |  |
|lookaround/gen-lb/l15-negcount-digit-m2-under-sat.smt2       |    0.102s | 21.724MiB| sat | 0 |  |  |
|lookaround/gen-lb/l12-flank-in-digit-ing-xyx-sat.smt2        |    0.102s | 20.916MiB| sat | 0 |  |  |
|lookaround/gen/t08-periodic-ab-xyzyxz-sat.smt2               |    0.102s | 22.96MiB| sat | 0 |  |  |
|lookaround/gen/t03-twosided-upper-cyr-xyx-sat.smt2           |    0.103s | 22.856MiB| sat | 0 |  |  |
|lookaround/thousands-group-comma-sat.smt2                    |    0.104s | 22.192MiB| sat | 0 |  |  |
|lookaround/gen/t02-contains-digit-xxyy-sat.smt2              |    0.104s | 21.384MiB| sat | 0 |  |  |
|lookaround/gen-lb/l15-negcount-hex-m3-under-sat.smt2         |    0.105s | 23.696MiB| sat | 0 |  |  |
|lookaround/gen/t02-contains-latinext-xyxy-sat.smt2           |    0.105s | 21.42MiB| sat | 0 |  |  |
|lookaround/gen/t03-twosided-latinext-digit-xyx-sat.smt2      |    0.105s | 22.396MiB| sat | 0 |  |  |
|lookaround/gen-lb/l15-negcount-lower-m4-under-sat.smt2       |    0.106s | 22.476MiB| sat | 0 |  |  |
|lookaround/ipv6-exactly-one-cc-sat.smt2                      |    0.107s | 22.288MiB| sat | 0 |  |  |
|lookaround/gen-lb/l15-negcount-lower-m2-under-sat.smt2       |    0.107s | 21.732MiB| sat | 0 |  |  |
|levels/L1-06-multi-compl-unsat.smt2                          |    0.109s | 20.112MiB| unsat | 0 |  |  |
|lookaround/escaped-string-sat.smt2                           |    0.110s | 21.62MiB| sat | 0 |  |  |
|lookaround/gen-lb/l12-flank-cu-lower-ed-xyx-sat.smt2         |    0.110s | 20.932MiB| sat | 0 |  |  |
|lookaround/gen/t08-periodic-de-xyxzyz-sat.smt2               |    0.111s | 22.952MiB| sat | 0 |  |  |
|lookaround/cyrillic-prepositions-border-unsat.smt2           |    0.112s | 21.376MiB| unsat | 0 |  |  |
|lookaround/gen-lb/l16-nest-xyzz-lower-xyyx-sat.smt2          |    0.112s | 22.436MiB| sat | 0 |  |  |
|lookaround/gen/t04-mod-digit-5-sat.smt2                      |    0.113s | 22.156MiB| sat | 0 |  |  |
|lookaround/gen/t08-periodic-ab-xyxzyz-sat.smt2               |    0.113s | 22.948MiB| sat | 0 |  |  |
|lookaround/gen-lb/l15-negcount-lower-m2-dash-sat.smt2        |    0.114s | 21.68MiB| sat | 0 |  |  |
|levels/L2-06-compl-interior-sat.smt2                         |    0.115s | 20.708MiB| sat | 0 |  |  |
|lookaround/gen-lb/l15-negcount-digit-m4-under-sat.smt2       |    0.116s | 22.412MiB| sat | 0 |  |  |
|lookaround/gen-lb/l15-negcount-digit-m4-dash-sat.smt2        |    0.116s | 22.412MiB| sat | 0 |  |  |
|lookaround/gen-lb/l11-nlb-cu-lower-xyx-sat.smt2              |    0.116s | 21.388MiB| sat | 0 |  |  |
|lookaround/gen-lb/l16-nest-abcd-digit-xyx-sat.smt2           |    0.116s | 21.136MiB| sat | 0 |  |  |
|lookaround/gen-lb/l15-negcount-digit-m4-colon-sat.smt2       |    0.118s | 22.416MiB| sat | 0 |  |  |
|lookaround/gen/t08-pairs-hex-hex-xyxzyz-sat.smt2             |    0.118s | 23.4MiB| sat | 0 |  |  |
|lookaround/gen/t06-adjacency-xxyy-cyr-unsat.smt2             |    0.118s | 22.92MiB| unsat | 0 |  |  |
|lookaround/gen-lb/l11-nlb-left-digit-xyx-sat.smt2            |    0.119s | 21.288MiB| sat | 0 |  |  |
|lookaround/gen-lb/l16-nest-xyzz-digit-xyyx-sat.smt2          |    0.119s | 21.74MiB| sat | 0 |  |  |
|lookaround/gen-lb/l11-nlb-sub-digit-xyyx-sat.smt2            |    0.119s | 21.544MiB| sat | 0 |  |  |
|lookaround/gen/t04-exact-hex-6-sat.smt2                      |    0.119s | 23.912MiB| sat | 0 |  |  |
|lookaround/gen/t08-periodic-de-xyzxyz-sat.smt2               |    0.119s | 21.596MiB| sat | 0 |  |  |
|lookaround/two-sided-anchored-token-sat.smt2                 |    0.120s | 22.812MiB| sat | 0 |  |  |
|lookaround/gen-lb/l11-nlb-sub-digit-xyx-sat.smt2             |    0.120s | 21.864MiB| sat | 0 |  |  |
|lookaround/gen/t01-border-cssfunc-sat.smt2                   |    0.121s | 21.132MiB| sat | 0 |  |  |
|lookaround/gen-lb/l16-nest-abcd-lower-xyx-sat.smt2           |    0.122s | 21.396MiB| sat | 0 |  |  |
|lookaround/gen-lb/l11-nlb-cu-lower-xyyx-sat.smt2             |    0.122s | 21.564MiB| sat | 0 |  |  |
|lookaround/gen/t08-pairs-hex-hex-xyxy-sat.smt2               |    0.122s | 21.9MiB| sat | 0 |  |  |
|lookaround/gen/t01-border-tskw-unsat.smt2                    |    0.124s | 21.54MiB| unsat | 0 |  |  |
|lookaround/gen-lb/l13-inter-lower-xyx-k2-sat.smt2            |    0.125s | 22.188MiB| sat | 0 |  |  |
|lookaround/gen-lb/l12-flank-in-digit-ing-xyyx-sat.smt2       |    0.126s | 21.28MiB| sat | 0 |  |  |
|lookaround/gen/t08-pairs-digit-lower-xyxy-sat.smt2           |    0.126s | 21.388MiB| sat | 0 |  |  |
|lookaround/gen/t08-periodic-xy-xyxzyz-sat.smt2               |    0.127s | 22.96MiB| sat | 0 |  |  |
|lookaround/gen/t04-mod-hex-5-sat.smt2                        |    0.127s | 23.824MiB| sat | 0 |  |  |
|lookaround/gen/t03-twosided-lower-digit-xyyx-sat.smt2        |    0.128s | 23.008MiB| sat | 0 |  |  |
|lookaround/gen-lb/l15-negcount-hex-m4-under-sat.smt2         |    0.130s | 23.94MiB| sat | 0 |  |  |
|lookaround/isolated-year-flanked-sat.smt2                    |    0.131s | 24.228MiB| sat | 0 |  |  |
|lookaround/emoji-astral-ranges-border-unsat.smt2             |    0.132s | 21.192MiB| unsat | 0 |  |  |
|lookaround/gen-lb/l11-nlb-sub-lower-xyyx-sat.smt2            |    0.132s | 21.64MiB| sat | 0 |  |  |
|lookaround/gen-lb/l11-nlb-left-lower-xyyx-sat.smt2           |    0.132s | 21.644MiB| sat | 0 |  |  |
|lookaround/gen-lb/l16-nest-ingg-lower-xyyx-sat.smt2          |    0.132s | 21.584MiB| sat | 0 |  |  |
|lookaround/gen/t03-twosided-upper-digit-xyyx-sat.smt2        |    0.134s | 23.34MiB| sat | 0 |  |  |
|lookaround/gen/t02-contains-digit-xyx-sat.smt2               |    0.135s | 21.292MiB| sat | 0 |  |  |
|hard_length_1_semilinear_complement.smt2                     |    0.136s | 23.936MiB| unsat | 0 |  |  |
|lookaround/gen-lb/l15-negcount-digit-m2-dash-sat.smt2        |    0.136s | 21.648MiB| sat | 0 |  |  |
|lookaround/gen/t01-border-ordinal-unsat.smt2                 |    0.136s | 20.552MiB| unsat | 0 |  |  |
|lookaround/gen/t03-twosided-latinext-digit-xyyx-sat.smt2     |    0.136s | 23.04MiB| sat | 0 |  |  |
|lookaround/gen-lb/l16-nest-abcd-digit-xyyx-sat.smt2          |    0.137s | 21.84MiB| sat | 0 |  |  |
|lookaround/token-upper-start-digit-end-sat.smt2              |    0.138s | 23.276MiB| sat | 0 |  |  |
|lookaround/gen/t03-twosided-upper-cyr-xyyx-sat.smt2          |    0.140s | 23.792MiB| sat | 0 |  |  |
|lookaround/http-method-border-unsat.smt2                     |    0.141s | 22.084MiB| unsat | 0 |  |  |
|lookaround/gen-lb/l15-negcount-lower-m2-colon-sat.smt2       |    0.141s | 21.724MiB| sat | 0 |  |  |
|lookaround/gen-lb/l15-negcount-lower-m4-dash-sat.smt2        |    0.141s | 22.456MiB| sat | 0 |  |  |
|lookaround/gen-lb/l16-nest-abcd-lower-xyyx-sat.smt2          |    0.142s | 22.16MiB| sat | 0 |  |  |
|lookaround/gen/t06-adjacency-xxyy-digit-unsat.smt2           |    0.143s | 23.144MiB| unsat | 0 |  |  |
|levels/L4-02-semilinear-unsat.smt2                           |    0.145s | 23.852MiB| unsat | 0 |  |  |
|lookaround/password-classes-sat.smt2                         |    0.145s | 23.576MiB| sat | 0 |  |  |
|lookaround/gen/t08-periodic-xy-xyzyxz-sat.smt2               |    0.147s | 23.22MiB| sat | 0 |  |  |
|lookaround/gen-lb/l11-nlb-cu-digit-xyyx-sat.smt2             |    0.148s | 21.672MiB| sat | 0 |  |  |
|lookaround/gen/t03-twosided-digit-lower-xyx-sat.smt2         |    0.148s | 22.712MiB| sat | 0 |  |  |
|lookaround/gen-lb/l11-nlb-in-digit-xyyx-sat.smt2             |    0.149s | 21.82MiB| sat | 0 |  |  |
|lookaround/gen-lb/l15-negcount-lower-m4-colon-sat.smt2       |    0.150s | 22.42MiB| sat | 0 |  |  |
|lookaround/gen/t02-contains-latinext-xyx-sat.smt2            |    0.150s | 21.132MiB| sat | 0 |  |  |
|lookaround/gen-lb/l15-negcount-hex-m3-colon-sat.smt2         |    0.153s | 23.648MiB| sat | 0 |  |  |
|lookaround/gen-lb/l15-negcount-lower-m3-colon-sat.smt2       |    0.155s | 22.516MiB| sat | 0 |  |  |
|lookaround/gen/t02-contains-latinext-xxyy-sat.smt2           |    0.155s | 22.044MiB| sat | 0 |  |  |
|lookaround/gen/t08-periodic-abc-xyxzyz-sat.smt2              |    0.155s | 25.512MiB| sat | 0 |  |  |
|lookaround/gen-lb/l11-nlb-left-lower-xyx-sat.smt2            |    0.156s | 21.392MiB| sat | 0 |  |  |
|lookaround/gen-lb/l11-nlb-left-digit-xyyx-sat.smt2           |    0.156s | 21.62MiB| sat | 0 |  |  |
|lookaround/gen/t06-adjacency-xxyy-lower-unsat.smt2           |    0.156s | 22.976MiB| unsat | 0 |  |  |
|lookaround/gen-lb/l15-negcount-hex-m4-colon-sat.smt2         |    0.159s | 23.916MiB| sat | 0 |  |  |
|lookaround/gen-lb/l16-nest-xyzz-digit-xyx-sat.smt2           |    0.162s | 21.248MiB| sat | 0 |  |  |
|lookaround/gen/t02-contains-alnum-xyx-sat.smt2               |    0.163s | 21.952MiB| sat | 0 |  |  |
|lookaround/triple-doubled-hex-shorthand-sat.smt2             |    0.170s | 24.204MiB| sat | 0 |  |  |
|lookaround/gen/t04-exact-hex-4-sat.smt2                      |    0.171s | 22.968MiB| sat | 0 |  |  |
|lookaround/gen/t08-pairs-digit-lower-xyxzyz-sat.smt2         |    0.176s | 24.92MiB| sat | 0 |  |  |
|lookaround/gen-lb/l15-negcount-hex-m2-under-sat.smt2         |    0.177s | 22.668MiB| sat | 0 |  |  |
|lookaround/gen-lb/l15-negcount-hex-m2-dash-sat.smt2          |    0.177s | 22.708MiB| sat | 0 |  |  |
|lookaround/gen/t03-twosided-hex-upper-xyyx-sat.smt2          |    0.178s | 24.16MiB| sat | 0 |  |  |
|lookaround/identifier-doubled-latinext-sat.smt2              |    0.180s | 24.748MiB| sat | 0 |  |  |
|lookaround/gen-lb/l16-nest-ingg-digit-xyyx-sat.smt2          |    0.180s | 21.724MiB| sat | 0 |  |  |
|lookaround/gen-lb/l15-negcount-hex-m3-dash-sat.smt2          |    0.182s | 23.728MiB| sat | 0 |  |  |
|lookaround/triple-interleaved-pairs-sat.smt2                 |    0.184s | 24.768MiB| sat | 0 |  |  |
|lookaround/gen/t08-periodic-abc-xyzyxz-sat.smt2              |    0.186s | 25.74MiB| sat | 0 |  |  |
|lookaround/gen-lb/l15-negcount-hex-m4-dash-sat.smt2          |    0.208s | 24.172MiB| sat | 0 |  |  |
|lookaround/gen-lb/l13-inter-digit-xyxy-k2-sat.smt2           |    0.209s | 22.304MiB| sat | 0 |  |  |
|lookaround/gen-lb/l13-inter-lower-xyxy-k2-sat.smt2           |    0.212s | 21.892MiB| sat | 0 |  |  |
|lookaround/gen-lb/l13-inter-digit-xyxy-k3-sat.smt2           |    0.213s | 22.232MiB| sat | 0 |  |  |
|lookaround/gen/t08-pairs-upper-digit-xyxzyz-sat.smt2         |    0.240s | 25.7MiB| sat | 0 |  |  |
|lookaround/gen-lb/l13-inter-lower-xyx-k3-sat.smt2            |    0.287s | 22.16MiB| sat | 0 |  |  |
|lookaround/gen/t06-adjacency-xxyy-hex-unsat.smt2             |    0.439s | 25.724MiB| unsat | 0 |  |  |
|lookaround/gen-lb/l13-inter-lower-xyxy-k3-sat.smt2           |    0.514s | 23.308MiB| sat | 0 |  |  |
|lookaround/gen-lb/l13-inter-digit-xyxy-k4-sat.smt2           |    0.527s | 24.448MiB| sat | 0 |  |  |
|lookaround/gen-lb/l13-inter-digit-xyxy-k5-sat.smt2           |    0.593s | 24.432MiB| sat | 0 |  |  |
|lookaround/gen-lb/l13-inter-lower-xyx-k4-sat.smt2            |    0.662s | 23.94MiB| sat | 0 |  |  |
|lookaround/gen-lb/l13-inter-lower-xyx-k5-sat.smt2            |    1.104s | 26.388MiB| sat | 0 |  |  |
|lookaround/gen-lb/l13-inter-lower-xyxy-k4-sat.smt2           |    1.685s | 25.596MiB| sat | 0 |  |  |
|hard_cyclic_12_cascade.smt2                                  |    5.011s | 21.444MiB| timeout | 0 |  |  |
|lookaround/gen/t06-adjacency-xyzzyx-digit-unsat.smt2         |    5.015s | 44.156MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-upper-digit-in-xyx-m3-unsat.smt2 |    5.016s | 50.716MiB| timeout | 0 |  |  |
|lookaround/gen/t05-parity-xyxy-cyr-c430-unsat.smt2           |    5.016s | 40.116MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-digit-lower-left-xyx-m3-unsat.smt2 |    5.017s | 48.7MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-digit-upper-cu-xyx-m2-unsat.smt2 |    5.018s | 51.8MiB| timeout | 0 |  |  |
|lookaround/gen/t06-adjacency-xyyx-lower-unsat.smt2           |    5.020s | 45.4MiB| timeout | 0 |  |  |
|hard_len_nonprim_2_cyclic_phase_shift.smt2                   |    5.021s | 66.108MiB| timeout | 0 |  |  |
|hard_len_nonprim_3_cegar_diophantine.smt2                    |    5.022s | 135.0MiB| timeout | 0 |  |  |
|hard_cyclic_4_iso_unsat.smt2                                 |    5.022s | 63.98MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-lower-digit-left-xyyx-m2-unsat.smt2 |    5.023s | 37.392MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-digit-lower-left-xyx-m2-unsat.smt2 |    5.025s | 45.16MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l13-inter-lower-xyxy-k5-sat.smt2           |    5.025s | 31.788MiB| timeout | 0 |  |  |
|hard_len_nonprim_4_subsume_len_clash.smt2                    |    5.032s | 46.492MiB| timeout | 0 |  |  |
|hard_len_nonprim_6_cegar_interleaved.smt2                    |    5.032s | 130.0MiB| timeout | 0 |  |  |
|hard_cyclic_8_overapprox_conflict.smt2                       |    5.034s | 79.088MiB| timeout | 0 |  |  |
|lookaround/delim-block-nested-compl-unsat.smt2               |    5.034s | 32.224MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-lower-digit-left-xyx-m2-unsat.smt2 |    5.037s | 44.58MiB| timeout | 0 |  |  |
|lookaround/gen/t05-parity-xxyy-lower-c61-unsat.smt2          |    5.038s | 44.744MiB| timeout | 0 |  |  |
|lookaround/gen/t05-parity-xxyy-digit-c30-unsat.smt2          |    5.039s | 43.072MiB| timeout | 0 |  |  |
|lookaround/triple-mirror-adjacent-digit-unsat.smt2           |    5.040s | 46.98MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-lower-digit-left-xyx-m3-unsat.smt2 |    5.040s | 47.912MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-digit-lower-cu-xyyx-m2-unsat.smt2 |    5.040s | 38.748MiB| timeout | 0 |  |  |
|lookaround/gen/t05-parity-xyyx-lower-c61-unsat.smt2          |    5.040s | 42.352MiB| timeout | 0 |  |  |
|lookaround/records-nested-complement-unsat.smt2              |    5.041s | 47.524MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-digit-upper-left-xyyx-m2-unsat.smt2 |    5.041s | 47.064MiB| timeout | 0 |  |  |
|lookaround/gen/t05-parity-xxyy-cyr-c430-unsat.smt2           |    5.041s | 39.512MiB| timeout | 0 |  |  |
|lookaround/gen/t05-parity-xyxy-hex-c66-unsat.smt2            |    5.041s | 42.584MiB| timeout | 0 |  |  |
|lookaround/entity-bare-amp-unsat.smt2                        |    5.042s | 92.884MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-digit-lower-in-xyyx-m2-unsat.smt2 |    5.043s | 41.912MiB| timeout | 0 |  |  |
|lookaround/gen/t05-parity-xyyx-cyr-c430-unsat.smt2           |    5.043s | 39.516MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-upper-digit-left-xyx-m3-unsat.smt2 |    5.044s | 46.74MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-digit-upper-in-xyyx-m2-unsat.smt2 |    5.044s | 43.664MiB| timeout | 0 |  |  |
|lookaround/gen/t06-adjacency-xyyx-digit-unsat.smt2           |    5.044s | 36.6MiB| timeout | 0 |  |  |
|hard_cyclic_2.smt2                                           |    5.045s | 60.148MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-digit-lower-in-xyx-m2-unsat.smt2 |    5.045s | 53.312MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-lower-digit-in-xyx-m2-unsat.smt2 |    5.045s | 47.676MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-lower-digit-cu-xyx-m2-unsat.smt2 |    5.045s | 47.2MiB| timeout | 0 |  |  |
|hard_len_nonprim_1_alternating_parity.smt2                   |    5.046s | 76.676MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-digit-lower-in-xyx-m3-unsat.smt2 |    5.046s | 46.588MiB| timeout | 0 |  |  |
|hard_cyclic_14_ring_buffer.smt2                              |    5.047s | 139.0MiB| timeout | 0 |  |  |
|hard_len_nonprim_5_odd_even_boundary_clash.smt2              |    5.052s | 53.212MiB| timeout | 0 |  |  |
|hard_cyclic_1.smt2                                           |    5.052s | 59.92MiB| timeout | 0 |  |  |
|hard_length_4_cross_variable_parity.smt2                     |    5.053s | 289.0MiB| timeout | 0 |  |  |
|lookaround/gen/t07-nestedcompl-under-lower-unsat.smt2        |    5.053s | 528.0MiB| timeout | 0 |  |  |
|lookaround/unescaped-semicolon-lookbehind-unsat.smt2         |    5.054s | 57.06MiB| timeout | 0 |  |  |
|lookaround/cyrillic-block-novowel-unsat.smt2                 |    5.055s | 107.0MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-digit-upper-in-xyx-m3-unsat.smt2 |    5.056s | 55.672MiB| timeout | 0 |  |  |
|lookaround/gen/t05-parity-xxyy-hex-c66-unsat.smt2            |    5.058s | 41.78MiB| timeout | 0 |  |  |
|lookaround/gen/t05-parity-xyyx-digit-c30-unsat.smt2          |    5.059s | 50.768MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-upper-digit-left-xyyx-m2-unsat.smt2 |    5.060s | 37.544MiB| timeout | 0 |  |  |
|lookaround/gen/t06-adjacency-xyyx-cyr-unsat.smt2             |    5.060s | 35.972MiB| timeout | 0 |  |  |
|lookaround/gen/t06-adjacency-xyzzyx-lower-unsat.smt2         |    5.062s | 46.724MiB| timeout | 0 |  |  |
|lookaround/gen/t05-parity-xyyx-hex-c66-unsat.smt2            |    5.065s | 37.388MiB| timeout | 0 |  |  |
|levels/L3-05-multivar-compl-unsat.smt2                       |    5.066s | 59.204MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-lower-digit-cu-xyyx-m2-unsat.smt2 |    5.066s | 41.544MiB| timeout | 0 |  |  |
|lookaround/gen/t05-parity-xyxy-digit-c30-unsat.smt2          |    5.066s | 42.088MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-upper-digit-in-xyyx-m2-unsat.smt2 |    5.067s | 41.84MiB| timeout | 0 |  |  |
|levels/L0-03-parity-unsat.smt2                               |    5.068s | 481.0MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-digit-upper-left-xyx-m3-unsat.smt2 |    5.068s | 48.888MiB| timeout | 0 |  |  |
|levels/L2-04-alt-even-unsat.smt2                             |    5.069s | 76.472MiB| timeout | 0 |  |  |
|hard_length_2_cegar_gradient.smt2                            |    5.071s | 200.0MiB| timeout | 0 |  |  |
|hard_len_nonprim_7_difference_parity.smt2                    |    5.071s | 414.0MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-digit-lower-left-xyyx-m2-unsat.smt2 |    5.071s | 40.484MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-lower-digit-in-xyyx-m2-unsat.smt2 |    5.071s | 40.396MiB| timeout | 0 |  |  |
|levels/L0-04-cyclic-unsat.smt2                               |    5.072s | 59.876MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-digit-upper-in-xyx-m2-unsat.smt2 |    5.072s | 46.024MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-digit-upper-left-xyx-m2-unsat.smt2 |    5.073s | 48.664MiB| timeout | 0 |  |  |
|hard_len_nonprim_8_dual_diophantine.smt2                     |    5.074s | 487.0MiB| timeout | 0 |  |  |
|lookaround/quote-parity-unsat.smt2                           |    5.074s | 50.284MiB| timeout | 0 |  |  |
|hard_cyclic_13_parallel_graphs.smt2                          |    5.075s | 60.808MiB| timeout | 0 |  |  |
|lookaround/parens-wrong-order-unsat.smt2                     |    5.076s | 53.248MiB| timeout | 0 |  |  |
|hard_cyclic_15_multivar_subsume.smt2                         |    5.079s | 59.208MiB| timeout | 0 |  |  |
|lookaround/gen/t07-nestedcompl-comma-lower-unsat.smt2        |    5.079s | 528.0MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-upper-digit-left-xyx-m2-unsat.smt2 |    5.081s | 40.948MiB| timeout | 0 |  |  |
|hard_cyclic_3.smt2                                           |    5.084s | 54.168MiB| timeout | 0 |  |  |
|hard_cyclic_6_cross_boundary.smt2                            |    5.085s | 111.0MiB| timeout | 0 |  |  |
|lookaround/gen/t07-nestedcompl-comma-digit-unsat.smt2        |    5.086s | 525.0MiB| timeout | 0 |  |  |
|lookaround/gen/t06-adjacency-xyyx-hex-unsat.smt2             |    5.086s | 40.056MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-upper-digit-in-xyx-m2-unsat.smt2 |    5.087s | 42.568MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-digit-upper-cu-xyx-m3-unsat.smt2 |    5.092s | 45.876MiB| timeout | 0 |  |  |
|lookaround/gen/t07-nestedcompl-dash-lower-unsat.smt2         |    5.092s | 528.0MiB| timeout | 0 |  |  |
|lookaround/gen/t07-nestedcompl-amp-lower-unsat.smt2          |    5.092s | 528.0MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-upper-digit-cu-xyyx-m2-unsat.smt2 |    5.093s | 41.116MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-digit-lower-cu-xyx-m2-unsat.smt2 |    5.096s | 41.696MiB| timeout | 0 |  |  |
|lookaround/gen/t07-nestedcompl-dash-digit-unsat.smt2         |    5.099s | 525.0MiB| timeout | 0 |  |  |
|levels/L3-02-nested-compl-d2-unsat.smt2                      |    5.103s | 481.0MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-lower-digit-cu-xyx-m3-unsat.smt2 |    5.103s | 50.5MiB| timeout | 0 |  |  |
|lookaround/gen/t07-nestedcompl-slash-digit-unsat.smt2        |    5.103s | 525.0MiB| timeout | 0 |  |  |
|lookaround/gen/t05-parity-xyxy-lower-c61-unsat.smt2          |    5.104s | 35.264MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-upper-digit-cu-xyx-m3-unsat.smt2 |    5.105s | 46.816MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-lower-digit-in-xyx-m3-unsat.smt2 |    5.105s | 53.768MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-digit-upper-cu-xyyx-m2-unsat.smt2 |    5.105s | 44.312MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-digit-lower-cu-xyx-m3-unsat.smt2 |    5.106s | 51.948MiB| timeout | 0 |  |  |
|lookaround/gen-lb/l14-conflict-upper-digit-cu-xyx-m2-unsat.smt2 |    5.106s | 52.64MiB| timeout | 0 |  |  |
|lookaround/gen/t07-nestedcompl-slash-lower-unsat.smt2        |    5.112s | 528.0MiB| timeout | 0 |  |  |
|lookaround/gen/t07-nestedcompl-semi-lower-unsat.smt2         |    5.114s | 528.0MiB| timeout | 0 |  |  |
|lookaround/gen/t07-nestedcompl-amp-digit-unsat.smt2          |    5.120s | 525.0MiB| timeout | 0 |  |  |
|lookaround/gen/t07-nestedcompl-under-digit-unsat.smt2        |    5.125s | 525.0MiB| timeout | 0 |  |  |
|lookaround/gen/t07-nestedcompl-semi-digit-unsat.smt2         |    5.138s | 525.0MiB| timeout | 0 |  |  |
|hard_length_3_delayed_conflict.smt2                          |    5.157s | 878.0MiB| timeout | 0 |  |  |
