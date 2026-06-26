# .

* SAT 33
* UNSAT 11
* TIMEOUT 88
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
Z3 commit: 6a62a531819d3c2aa21eaaffa8411aea4d9a2f40
Z3 branch: master
Z3 options: "-T:5 model_validate=true smt.random_seed=1"
Z3 inputs: inputs/ClemensRegexAll
Z3 commit message: qsat: decide quantifier-free goals so qe2 returns sat instead of unknown (fixes iss-7027/small-30) (#9970)

## Summary

Fixes the `iss-7027/small-30` snapshot regression (`Z3Prover/bench`
discussion #2705) **at its root**, instead of working around it by
retuning the LP heuristics.

- **Benchmark:** `inputs/issues/iss-7027/small-30.smt2` —
`(check-sat-using qe2)` over a single `(distinct ...)` of 33 mixed
Int/Real terms.
- The recorded oracle was `unknown`; current `master` produces
`timeout`.

## Root cause

`unknown`/`timeout` are both wrong here: the formula is a `distinct`
over 33 terms (free Int/Real constants plus the literals `0`/`1`), which
is **trivially `sat`** — there are infinitely many distinct reals.

The real bug is in the `qsat` tactic that backs `qe2`. Running
quantifier elimination on a **quantifier-free** formula has nothing to
eliminate, so `qsat` left an undecided residual goal and
`check-sat-using` reported `unknown`. This reproduces on any ground
formula with free variables, e.g.:

```
(declare-fun a () Int)(assert (> a 0))(check-sat-using qe2)   ; -> unknown (should be sat)
```

For `small-30` the QE alternation additionally drove `theory_lra`
integer branch-and-bound down a non-terminating path, surfacing as a
`timeout` under the capture budget (the symptom the `random_hammers`
schedule change happened to expose).

## Fix

Under `check-sat` semantics, top-level free variables are implicitly
existentially quantified. So when the `qsat` input has no quantifiers,
decide satisfiability directly (route through the existing `qsat_sat`
path) instead of producing a residual goal. `qe2`/`qe` now return
`sat`/`unsat` for ground formulas.

QE of genuinely-quantified formulas is unchanged: `apply qe2` on a
quantified goal produces the same projected formula as before (verified
identical to `master`). Only the degenerate quantifier-free case is
affected.

This supersedes the previous approach in this PR (reverting the
`lp.random_hammers` default). That default is left **unchanged**
(`true`), preserving #9958's aggregate QF_LIA benefit. `small-30` now
returns `sat` in ~0.01s regardless of the heuristic schedule, because
the QE machinery no longer runs on this ground instance.

Two changes:
- `src/qe/qsat.cpp`: short-circuit quantifier-free input to the
satisfiability decision path.
- `Z3Prover/bench` `inputs/issues/iss-7027/small-30.expected.out`:
oracle updated `unknown` -> `sat` (to be committed alongside this fix).

## Validation

```
$ z3 small-30.smt2
sat            # ~0.01s

$ echo '(declare-fun a () Int)(assert (> a 0))(check-sat-using qe2)' | z3 -in
sat
$ echo '(declare-fun a () Int)(assert (and (> a 0)(< a 0)))(check-sat-using qe2)' | z3 -in
unsat
```

Full unit-test suite (`test-z3 /a`) passes (92/92). Quantified `qe2`
round-trips (`apply qe2`) match `master` byte-for-byte.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|deep_nest_membership_t7.smt2                                 |    0.025s | 20.352MiB| unsat | 0 |  |  |
|noodler_killer_16_non_commutative_periods.smt2               |    0.033s | 20.276MiB| unsat | 0 |  |  |
|hard_ext_12.smt2                                             |    0.033s | 20.328MiB| unsat | 0 |  |  |
|hard_cyclic_9_subsumption.smt2                               |    0.036s | 20.508MiB| sat | 0 |  |  |
|hard_ext_4.smt2                                              |    0.040s | 20.988MiB| sat | 0 |  |  |
|hard_ext_11.smt2                                             |    0.046s | 20.352MiB| unsat | 0 |  |  |
|noodler_killer_6_ambiguous_nfa_comp.smt2                     |    0.052s | 20.492MiB| sat | 0 |  |  |
|hard_ext_7.smt2                                              |    0.056s | 20.364MiB| unsat | 0 |  |  |
|hard_cyclic_10_linear_form.smt2                              |    0.058s | 20.108MiB| unsat | 0 |  |  |
|noodler_killer_19_double_star_comp.smt2                      |    0.058s | 20.172MiB| unsat | 0 |  |  |
|hard_ext_5.smt2                                              |    0.061s | 20.324MiB| unsat | 0 |  |  |
|hard_ext_1.smt2                                              |    0.065s | 20.916MiB| sat | 0 |  |  |
|noodler_killer_3_nested_complements.smt2                     |    0.065s | 19.636MiB| sat | 0 |  |  |
|deep_nest_membership_t26.smt2                                |    0.068s | 20.712MiB| sat | 0 |  |  |
|noodler_killer_20_alphabet_transducer.smt2                   |    0.075s | 20.764MiB| unsat | 0 |  |  |
|noodler_killer_18_var_complement.smt2                        |    0.076s | 21.14MiB| sat | 0 |  |  |
|hard_len_nonprim_1_alternating_parity.smt2                   |    0.076s | 20.624MiB| sat | 0 |  |  |
|deep_nest_membership_t19.smt2                                |    0.078s | 21.644MiB| sat | 0 |  |  |
|noodler_killer_11_symmetric_nth.smt2                         |    0.079s | 23.06MiB| sat | 0 |  |  |
|noodler_killer_4_nfa_poly_comp.smt2                          |    0.080s | 20.616MiB| sat | 0 |  |  |
|hard_cyclic_7_overlapping_cycles.smt2                        |    0.081s | 20.724MiB| sat | 0 |  |  |
|deep_nest_membership_t3.smt2                                 |    0.083s | 21.608MiB| sat | 0 |  |  |
|noodler_killer_9_comp_inter_comp.smt2                        |    0.088s | 21.132MiB| sat | 0 |  |  |
|noodler_killer_14_alignment_hell.smt2                        |    0.091s | 21.772MiB| sat | 0 |  |  |
|deep_nest_membership_t29.smt2                                |    0.092s | 21.024MiB| sat | 0 |  |  |
|hard_ext_10.smt2                                             |    0.096s | 20.62MiB| unsat | 0 |  |  |
|noodler_killer_10_100th_from_end.smt2                        |    0.102s | 21.932MiB| sat | 0 |  |  |
|deep_nest_membership_t6.smt2                                 |    0.104s | 21.104MiB| sat | 0 |  |  |
|hard_cyclic_11_deep_unrolling.smt2                           |    0.104s | 20.876MiB| sat | 0 |  |  |
|noodler_killer_17_alternating_automata.smt2                  |    0.112s | 22.868MiB| sat | 0 |  |  |
|hard_ext_3.smt2                                              |    0.114s | 21.932MiB| sat | 0 |  |  |
|hard_cyclic_5_iso_sat.smt2                                   |    0.117s | 21.148MiB| sat | 0 |  |  |
|deep_nest_membership_t9.smt2                                 |    0.122s | 21.436MiB| sat | 0 |  |  |
|noodler_killer_1_nth_from_end.smt2                           |    0.123s | 21.632MiB| sat | 0 |  |  |
|deep_nest_membership_t11.smt2                                |    0.132s | 22.152MiB| sat | 0 |  |  |
|hard_length_1_semilinear_complement.smt2                     |    0.136s | 24.18MiB| unsat | 0 |  |  |
|deep_nest_membership_t20.smt2                                |    0.142s | 21.392MiB| sat | 0 |  |  |
|deep_nest_membership_t17.smt2                                |    0.168s | 21.396MiB| sat | 0 |  |  |
|hard_regex_membership_4.smt2                                 |    0.179s | 22.668MiB| sat | 0 |  |  |
|deep_nest_membership_t12.smt2                                |    0.198s | 21.9MiB| sat | 0 |  |  |
|deep_nest_membership_t15.smt2                                |    0.205s | 21.892MiB| sat | 0 |  |  |
|hard_ext_9.smt2                                              |    0.242s | 28.036MiB| sat | 0 |  |  |
|deep_nest_membership_t1.smt2                                 |    0.536s | 24.328MiB| sat | 0 |  |  |
|hard_ext_6.smt2                                              |    2.634s | 33.256MiB| sat | 0 |  |  |
|hard_gen_20.smt2                                             |    5.014s | 32.796MiB| timeout | 0 |  |  |
|deep_nest_membership_t25.smt2                                |    5.015s | 29.152MiB| timeout | 0 |  |  |
|hard_gen_15.smt2                                             |    5.016s | 50.396MiB| timeout | 0 |  |  |
|hard_len_nonprim_5_odd_even_boundary_clash.smt2              |    5.017s | 80.98MiB| timeout | 0 |  |  |
|xxRaaSa.smt2                                                 |    5.018s | 75.94MiB| timeout | 0 |  |  |
|hard_gen_16.smt2                                             |    5.018s | 56.888MiB| timeout | 0 |  |  |
|xxRabObaPaaaa.smt2                                           |    5.026s | 80.34MiB| timeout | 0 |  |  |
|noodler_killer_5_lcm_1_to_20.smt2                            |    5.027s | 232.0MiB| timeout | 0 |  |  |
|hard_regex_membership_6.smt2                                 |    5.030s | 50.076MiB| timeout | 0 |  |  |
|hard_gen_14.smt2                                             |    5.031s | 50.464MiB| timeout | 0 |  |  |
|hard_gen_24.smt2                                             |    5.031s | 31.984MiB| timeout | 0 |  |  |
|hard_ext_2.smt2                                              |    5.032s | 50.832MiB| timeout | 0 |  |  |
|test_hard_1.smt2                                             |    5.034s | 47.012MiB| timeout | 0 |  |  |
|hard_regex_membership_17.smt2                                |    5.034s | 33.936MiB| timeout | 0 |  |  |
|hard_cyclic_6_cross_boundary.smt2                            |    5.035s | 127.0MiB| timeout | 0 |  |  |
|hard_cyclic_2.smt2                                           |    5.037s | 78.028MiB| timeout | 0 |  |  |
|hard_regex_membership_8.smt2                                 |    5.038s | 33.656MiB| timeout | 0 |  |  |
|hard_gen_10.smt2                                             |    5.039s | 50.38MiB| timeout | 0 |  |  |
|hard_ext_8.smt2                                              |    5.040s | 31.912MiB| timeout | 0 |  |  |
|xbxRaaObaaaS.smt2                                            |    5.040s | 78.852MiB| timeout | 0 |  |  |
|deep_nest_membership_t22.smt2                                |    5.040s | 39.428MiB| timeout | 0 |  |  |
|test_hard_0.smt2                                             |    5.041s | 135.0MiB| timeout | 0 |  |  |
|deep_nest_membership_t24.smt2                                |    5.041s | 36.908MiB| timeout | 0 |  |  |
|hard_cyclic_3.smt2                                           |    5.041s | 72.848MiB| timeout | 0 |  |  |
|xbxRaaObbS.smt2                                              |    5.042s | 77.708MiB| timeout | 0 |  |  |
|noodler_killer_8_missing_all_length_8.smt2                   |    5.044s | 66.896MiB| timeout | 0 |  |  |
|noodler_killer_13_permutation_grid.smt2                      |    5.044s | 47.28MiB| timeout | 0 |  |  |
|hard_regex_membership_14.smt2                                |    5.044s | 50.588MiB| timeout | 0 |  |  |
|hard_gen_26.smt2                                             |    5.045s | 35.66MiB| timeout | 0 |  |  |
|deep_nest_membership_t28.smt2                                |    5.046s | 70.764MiB| timeout | 0 |  |  |
|hard_regex_membership_3.smt2                                 |    5.046s | 102.0MiB| timeout | 0 |  |  |
|deep_nest_membership_t14.smt2                                |    5.049s | 67.076MiB| timeout | 0 |  |  |
|hard_cyclic_1.smt2                                           |    5.049s | 68.276MiB| timeout | 0 |  |  |
|hard_len_nonprim_6_cegar_interleaved.smt2                    |    5.049s | 150.0MiB| timeout | 0 |  |  |
|deep_nest_membership_batch2_4.smt2                           |    5.049s | 52.68MiB| timeout | 0 |  |  |
|hard_gen_25.smt2                                             |    5.052s | 31.872MiB| timeout | 0 |  |  |
|deep_nest_membership_batch2_2.smt2                           |    5.054s | 49.008MiB| timeout | 0 |  |  |
|deep_nest_membership_batch2_1.smt2                           |    5.054s | 61.26MiB| timeout | 0 |  |  |
|deep_nest_membership_t18.smt2                                |    5.057s | 81.02MiB| timeout | 0 |  |  |
|hard_cyclic_15_multivar_subsume.smt2                         |    5.057s | 68.436MiB| timeout | 0 |  |  |
|deep_nest_membership_t16.smt2                                |    5.057s | 80.528MiB| timeout | 0 |  |  |
|hard_regex_membership_2.smt2                                 |    5.057s | 79.244MiB| timeout | 0 |  |  |
|hard_gen_23.smt2                                             |    5.058s | 35.98MiB| timeout | 0 |  |  |
|hard_gen_12.smt2                                             |    5.060s | 50.752MiB| timeout | 0 |  |  |
|hard_gen_22.smt2                                             |    5.060s | 32.044MiB| timeout | 0 |  |  |
|hard_cyclic_4_iso_unsat.smt2                                 |    5.060s | 79.264MiB| timeout | 0 |  |  |
|hard_len_nonprim_4_subsume_len_clash.smt2                    |    5.063s | 56.248MiB| timeout | 0 |  |  |
|hard_len_nonprim_3_cegar_diophantine.smt2                    |    5.063s | 255.0MiB| timeout | 0 |  |  |
|deep_nest_membership_batch3_24.smt2                          |    5.063s | 91.984MiB| timeout | 0 |  |  |
|deep_nest_membership_t13.smt2                                |    5.063s | 60.364MiB| timeout | 0 |  |  |
|deep_nest_membership_t21.smt2                                |    5.064s | 50.072MiB| timeout | 0 |  |  |
|deep_nest_membership_t2.smt2                                 |    5.065s | 33.556MiB| timeout | 0 |  |  |
|deep_nest_membership_t8.smt2                                 |    5.065s | 50.08MiB| timeout | 0 |  |  |
|deep_nest_membership_t27.smt2                                |    5.066s | 31.392MiB| timeout | 0 |  |  |
|hard_length_2_cegar_gradient.smt2                            |    5.067s | 301.0MiB| timeout | 0 |  |  |
|deep_nest_membership_t30.smt2                                |    5.067s | 42.384MiB| timeout | 0 |  |  |
|xaxRabS.smt2                                                 |    5.068s | 481.0MiB| timeout | 0 |  |  |
|xbxRabcObcS.smt2                                             |    5.069s | 152.0MiB| timeout | 0 |  |  |
|hard_gen_13.smt2                                             |    5.069s | 51.188MiB| timeout | 0 |  |  |
|deep_nest_membership_t4.smt2                                 |    5.070s | 57.216MiB| timeout | 0 |  |  |
|hard_regex_membership_7.smt2                                 |    5.070s | 102.0MiB| timeout | 0 |  |  |
|hard_regex_membership_10.smt2                                |    5.071s | 50.716MiB| timeout | 0 |  |  |
|hard_regex_membership_9.smt2                                 |    5.072s | 29.968MiB| timeout | 0 |  |  |
|noodler_killer_7_alignment_explosion.smt2                    |    5.072s | 29.372MiB| timeout | 0 |  |  |
|hard_cyclic_14_ring_buffer.smt2                              |    5.072s | 139.0MiB| timeout | 0 |  |  |
|hard_cyclic_13_parallel_graphs.smt2                          |    5.072s | 91.192MiB| timeout | 0 |  |  |
|deep_nest_membership_t10.smt2                                |    5.073s | 61.252MiB| timeout | 0 |  |  |
|xabxRabaS.smt2                                               |    5.074s | 119.0MiB| timeout | 0 |  |  |
|deep_nest_membership_t5.smt2                                 |    5.074s | 77.4MiB| timeout | 0 |  |  |
|deep_nest_membership_batch2_3.smt2                           |    5.074s | 64.936MiB| timeout | 0 |  |  |
|hard_len_nonprim_2_cyclic_phase_shift.smt2                   |    5.075s | 85.724MiB| timeout | 0 |  |  |
|noodler_killer_2_primorial_length.smt2                       |    5.076s | 233.0MiB| timeout | 0 |  |  |
|hard_regex_membership_1.smt2                                 |    5.076s | 135.0MiB| timeout | 0 |  |  |
|hard_len_nonprim_7_difference_parity.smt2                    |    5.078s | 478.0MiB| timeout | 0 |  |  |
|deep_nest_membership_t23.smt2                                |    5.082s | 40.724MiB| timeout | 0 |  |  |
|hard_cyclic_8_overapprox_conflict.smt2                       |    5.086s | 79.796MiB| timeout | 0 |  |  |
|deep_nest_membership_batch2_5.smt2                           |    5.096s | 38.476MiB| timeout | 0 |  |  |
|hard_cyclic_12_cascade.smt2                                  |    5.098s | 21.64MiB| timeout | 0 |  |  |
|hard_gen_11.smt2                                             |    5.098s | 50.648MiB| timeout | 0 |  |  |
|hard_gen_21.smt2                                             |    5.099s | 31.784MiB| timeout | 0 |  |  |
|hard_regex_membership_5.smt2                                 |    5.103s | 54.284MiB| timeout | 0 |  |  |
|xabxRabSAxbabxRabaS.smt2                                     |    5.104s | 100.0MiB| timeout | 0 |  |  |
|xbxRabS.smt2                                                 |    5.106s | 481.0MiB| timeout | 0 |  |  |
|hard_len_nonprim_8_dual_diophantine.smt2                     |    5.109s | 487.0MiB| timeout | 0 |  |  |
|noodler_killer_15_debruijn_trap.smt2                         |    5.117s | 820.0MiB| timeout | 0 |  |  |
|noodler_killer_12_block_unary_lcm.smt2                       |    5.129s | 437.0MiB| timeout | 0 |  |  |
|hard_length_4_cross_variable_parity.smt2                     |    5.142s | 1000.0MiB| timeout | 0 |  |  |
|hard_length_3_delayed_conflict.smt2                          |    5.150s | 556.0MiB| timeout | 0 |  |  |
