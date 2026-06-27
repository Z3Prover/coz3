# .

* SAT 80
* UNSAT 66
* TIMEOUT 0
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_UFDTLIRA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-16740866-files-QF_UFDTLIRA.tar.zs
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 6a62a531819d3c2aa21eaaffa8411aea4d9a2f40
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_UFDTLIRA.tar.zst?download=1
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
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC20-032__succ_pred__p.adb_41_19_range_check___00.smt2 |    0.019s | 19.736MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC20-032__succ_pred__p.adb_41_38_overflow_check___00.smt2 |    0.021s | 20.676MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_87_22_assert___00.smt2 |    0.021s | 20.104MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_14_18_inconsistent_pre___00.smt2 |    0.022s | 19.952MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__imported.ads_14_13_inconsistent_pre___00.smt2 |    0.022s | 20.04MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/S818-002__might_not_return__pack.adb_5_7_raise___00.smt2 |    0.024s | 20.56MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__r.adb_53_7_precondition___00.smt2 |    0.028s | 20.084MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/Q130-019__missing_discr_check__p_init.ads_15_24_range_check___00.smt2 |    0.030s | 19.632MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__nested_packages.adb_56_13_dead_code___00.smt2 |    0.031s | 19.88MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_27_22_assert___00.smt2 |    0.032s | 19.716MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_77_22_assert___00.smt2 |    0.032s | 19.708MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P511-009__test_attributes__component_size_attribute.adb_43_7_assert___00.smt2 |    0.032s | 19.496MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_139_33_unreachable_branch___00.smt2 |    0.032s | 19.784MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_87_40_unreachable_branch___00.smt2 |    0.033s | 20.016MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_51_22_assert___00.smt2 |    0.034s | 19.684MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/OB16-016__system_default_priority__main.adb_3_11_ceiling__priority_protocol___00.smt2 |    0.034s | 19.968MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_44_22_assert___00.smt2 |    0.034s | 19.912MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/OC09-042__po_in_spec_only_pkg__main.adb_3_11_ceiling__priority_protocol___00.smt2 |    0.034s | 19.812MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/K512-016__task__t.ads_3_9_task_termination___00.smt2 |    0.034s | 19.6MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_138_22_assert___00.smt2 |    0.035s | 19.86MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/PA11-007__depchain__a-reatim.ads_6_4_assert___00.smt2 |    0.035s | 19.592MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/Q508-012__flow_null_proc__nullproc.ads_7_9_task_termination___00.smt2 |    0.035s | 19.86MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/M604-044__overflow__misovattr.adb_9_35_range_check___00.smt2 |    0.037s | 20.324MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__real_world__real_world.adb_44_22_assert___00.smt2 |    0.038s | 20.356MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KB04-032__lib_level__p.adb_6_19_assert___00.smt2 |    0.038s | 20.348MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC20-032__succ_pred__p.adb_32_36_overflow_check___00.smt2 |    0.039s | 20.364MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/M524-002__demorgan__test.adb_12_22_assert___00.smt2 |    0.039s | 20.012MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__nested_packages.adb_62_10_dead_code___00.smt2 |    0.040s | 19.504MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/Q818-007__projection__sc_status_type.adb_5_22_assert___00.smt2 |    0.040s | 20.236MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/stopwatch__timer.adb_16_17_ceiling__priority_protocol___00.smt2 |    0.041s | 19.704MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/Q512-013__flow_type_aspects__a.adb_7_10_dead_code___00.smt2 |    0.041s | 19.596MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/OB17-029__ceiling_elaboration_task__t.ads_2_9_task_termination___00.smt2 |    0.042s | 19.468MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__nested_packages.adb_27_16_dead_code___00.smt2 |    0.042s | 19.62MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/stopwatch__user.ads_19_17_ceiling__priority_protocol___00.smt2 |    0.042s | 19.78MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC22-015__succ_overflow__p.adb_37_19_range_check___00.smt2 |    0.042s | 19.844MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_66_22_assert___00.smt2 |    0.042s | 20.052MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/M524-002__demorgan__test.adb_6_22_assert___00.smt2 |    0.042s | 19.628MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__functions.adb_9_10_dead_code___00.smt2 |    0.042s | 19.828MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/O429-052__flow_tasking_exclusivity_checks__two_task_type_objects_single_so.ads_7_14_task_termination___00.smt2 |    0.042s | 20.264MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC22-015__succ_overflow__p.adb_29_18_range_check___00.smt2 |    0.043s | 20.116MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_18_22_assert___00.smt2 |    0.043s | 19.596MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/L316-015__all__a2.adb_4_5_precondition___00.smt2 |    0.043s | 20.14MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/S920-003__flow_noret__noret.adb_6_7_raise___00.smt2 |    0.043s | 19.624MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_24_37_unreachable_branch___00.smt2 |    0.043s | 19.644MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P511-009__test_attributes__storage_place_attributes.adb_73_22_assert___00.smt2 |    0.044s | 19.876MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/S702-024__record_attributes_in_allocators__test_constr.adb_39_22_assert___00.smt2 |    0.044s | 20.172MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC22-015__succ_overflow__p.adb_29_36_overflow_check___00.smt2 |    0.044s | 20.364MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__call_proc_no_body.adb_4_7_precondition___00.smt2 |    0.044s | 20.356MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/M604-044__overflow__misovattr.adb_6_22_assert___00.smt2 |    0.044s | 19.596MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KB10-018__cursor_location__cursor_location.ads_10_23_unreachable_branch___00.smt2 |    0.044s | 20.364MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__enums__enums.adb_194_29_range_check___00.smt2 |    0.044s | 20.424MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_24_22_assert___00.smt2 |    0.044s | 19.876MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_74_22_assert___00.smt2 |    0.044s | 19.86MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC22-015__succ_overflow__p.adb_37_31_overflow_check___00.smt2 |    0.045s | 20.36MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_40_18_inconsistent_pre___00.smt2 |    0.045s | 19.848MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__imported.adb_10_7_precondition___00.smt2 |    0.045s | 20.136MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/R123-023__flow_unused_variable__q.adb_7_17_discriminant_check___00.smt2 |    0.045s | 20.108MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P511-009__test_attributes__component_size_attribute.adb_49_7_assert___00.smt2 |    0.045s | 19.904MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/N609-025__private_type_discr__record_discr.adb_24_28_discriminant_check___00.smt2 |    0.045s | 20.1MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/L323-005__recursive__p.ads_7_36_postcondition___00.smt2 |    0.046s | 20.36MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/M604-044__overflow__misovattr.adb_8_35_range_check___00.smt2 |    0.046s | 20.356MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_30_22_assert___00.smt2 |    0.046s | 19.832MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__decl_only.ads_8_18_inconsistent_pre___00.smt2 |    0.046s | 20.384MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KB10-018__cursor_location__cursor_location.ads_11_24_unreachable_branch___00.smt2 |    0.046s | 20.388MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/S325-002__rec_pointers_bad__use_incomplete_type_auto.ads_4_19_assert___00.smt2 |    0.046s | 20.112MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/S729-004__inline_dispatching__p.adb_6_22_assert___00.smt2 |    0.049s | 20.372MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/R220-054__fixed_point_small__fp_test.adb_6_17_overflow_check___00.smt2 |    0.052s | 20.916MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P511-009__test_attributes__component_size_attribute.adb_37_7_assert___00.smt2 |    0.054s | 19.68MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/M604-044__overflow__misovattr.adb_8_22_assert___00.smt2 |    0.055s | 19.608MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/S920-003__flow_implicit_noret__p.adb_5_10_raise___00.smt2 |    0.057s | 20.456MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/M604-044__overflow__misovattr.adb_6_35_range_check___00.smt2 |    0.057s | 20.396MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KB28-003__succ__p.adb_6_23_range_check___00.smt2 |    0.057s | 20.396MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_54_22_assert___00.smt2 |    0.061s | 20.24MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KB28-003__succ__p.adb_12_23_range_check___00.smt2 |    0.063s | 20.856MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/L316-015__all__a1.adb_4_5_precondition___00.smt2 |    0.064s | 19.848MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_20_33_unreachable_branch___00.smt2 |    0.064s | 20.416MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P725-016__recursive_spec__main.adb_5_19_assert___00.smt2 |    0.064s | 19.936MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC22-015__succ_overflow__p.adb_21_31_range_check___00.smt2 |    0.065s | 19.6MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/M524-002__demorgan__test.adb_10_22_assert___00.smt2 |    0.068s | 20.368MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/Q906-011__counterexample__copy_values.adb_6_22_assert___00.smt2 |    0.070s | 29.308MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_45_22_assert___00.smt2 |    0.070s | 19.856MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/S920-003__flow_implicit_noret__p.adb_10_10_precondition___00.smt2 |    0.070s | 20.604MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_71_22_assert___00.smt2 |    0.071s | 20.068MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/PA27-015__nested_no_return__nest_in_proc.adb_14_4_precondition___00.smt2 |    0.072s | 20.088MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC20-032__succ_pred__p.adb_32_29_overflow_check___00.smt2 |    0.072s | 20.608MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/Q412-006__assert_true__p.ads_17_4_assert___00.smt2 |    0.073s | 19.596MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/NC11-033__variant__variants.ads_13_20_range_check___00.smt2 |    0.074s | 19.824MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/L323-005__recursive__p.adb_6_23_assert___00.smt2 |    0.074s | 20.364MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_115_22_assert___00.smt2 |    0.075s | 20.324MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/M809-022__scenario__q.adb_6_7_assert___00.smt2 |    0.076s | 19.848MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/M604-044__overflow__misovattr.adb_7_35_range_check___00.smt2 |    0.076s | 20.616MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__r.adb_46_10_dead_code___00.smt2 |    0.077s | 19.596MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_33_22_assert___00.smt2 |    0.078s | 20.352MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_19_22_assert___00.smt2 |    0.079s | 19.896MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P126-025__generic_procedure_renaming__c.adb_8_4_precondition___00.smt2 |    0.079s | 19.596MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P929-043__flow_pkg_elaboration_in_direct_calls__nest_in_proc.adb_9_7_precondition___00.smt2 |    0.080s | 19.856MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC22-015__succ_overflow__p.adb_37_38_overflow_check___00.smt2 |    0.082s | 20.412MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC22-015__succ_overflow__p.adb_21_38_range_check___00.smt2 |    0.084s | 20.364MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__p.adb_22_10_dead_code___00.smt2 |    0.084s | 19.904MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC22-015__succ_overflow__p.adb_29_29_overflow_check___00.smt2 |    0.085s | 20.276MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC20-032__succ_pred__p.adb_41_31_overflow_check___00.smt2 |    0.086s | 20.364MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P511-009__test_attributes__component_size_attribute.adb_55_7_assert___00.smt2 |    0.086s | 19.84MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/S702-024__record_attributes_in_allocators__test_constr.adb_24_22_assert___00.smt2 |    0.087s | 20.112MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_19_37_unreachable_branch___00.smt2 |    0.087s | 20.196MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/L531-021__discr__subty.ads_5_20_range_check___00.smt2 |    0.089s | 19.636MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_33_37_unreachable_branch___00.smt2 |    0.090s | 20.164MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P511-009__test_attributes__storage_place_attributes.adb_51_22_assert___00.smt2 |    0.092s | 19.64MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/Q512-013__flow_type_aspects__a.adb_9_7_dead_code___00.smt2 |    0.092s | 20.364MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/N915-008__flow_acats_oo__cc1311a.adb_279_17_cc1311a.adb_441_6_range_check___00.smt2 |    0.093s | 19.892MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/S702-024__record_attributes_in_allocators__test_constr.adb_39_37_null_pointer_dereference___00.smt2 |    0.093s | 19.644MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_112_22_assert___00.smt2 |    0.093s | 19.892MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/Q130-019__missing_discr_check__p_init.ads_14_24_range_check___00.smt2 |    0.094s | 19.72MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_139_44_unreachable_branch___00.smt2 |    0.094s | 20.368MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__nested_blocks.adb_11_29_dead_code___00.smt2 |    0.094s | 19.82MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__functions.adb_16_7_functions.adb_29_4_precondition___00.smt2 |    0.094s | 19.892MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC20-032__succ_pred__p.adb_32_18_range_check___00.smt2 |    0.095s | 19.844MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/R123-023__flow_unused_variable__q.adb_2_21_range_check___00.smt2 |    0.095s | 19.696MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/O521-002__no_return__p.adb_23_6_precondition___00.smt2 |    0.095s | 19.712MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P712-004__prot_discr__p.ads_6_13_range_check___00.smt2 |    0.095s | 19.592MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/M524-002__demorgan__test.adb_8_22_assert___00.smt2 |    0.095s | 20.012MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P712-004__prot_discr__main.adb_7_19_assert___00.smt2 |    0.095s | 19.632MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/stopwatch__user.ads_26_17_ceiling__priority_protocol___00.smt2 |    0.095s | 19.6MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/O429-052__flow_tasking_review__unsync_access__bar.adb_16_17_range_check___00.smt2 |    0.095s | 19.876MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/NC11-033__variant__variants.ads_14_20_range_check___00.smt2 |    0.095s | 19.872MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P712-004__prot_discr__p.ads_7_13_range_check___00.smt2 |    0.095s | 20.116MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P511-009__test_attributes__storage_place_attributes.adb_95_22_assert___00.smt2 |    0.095s | 19.628MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__nonreturning_spec.ads_2_14_inconsistent_post___00.smt2 |    0.095s | 19.88MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_80_22_assert___00.smt2 |    0.095s | 19.604MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/L531-021__discr__special.ads_6_32_range_check___00.smt2 |    0.096s | 19.592MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/Q512-018__null_task_id__main.adb_7_41_precondition___00.smt2 |    0.096s | 20.588MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P725-016__recursive_spec__main.adb_5_19_precondition___00.smt2 |    0.096s | 19.592MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/O429-052__flow_tasking_exclusivity_checks__two_tasks_single_so.ads_7_9_task_termination___00.smt2 |    0.096s | 20.272MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/stopwatch__user.ads_12_17_ceiling__priority_protocol___00.smt2 |    0.096s | 19.6MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__functions.adb_22_10_precondition___00.smt2 |    0.096s | 19.66MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__nest_in_proc.adb_21_13_dead_code___00.smt2 |    0.096s | 19.596MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/S702-024__record_attributes_in_allocators__test_constr.adb_24_30_null_pointer_dereference___00.smt2 |    0.096s | 19.636MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/S711-051__constrained_attr__test_constrained.adb_34_28_null_pointer_dereference___00.smt2 |    0.096s | 20.004MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P602-007__protected_no_return__p.adb_23_6_precondition___00.smt2 |    0.096s | 19.872MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/O429-052__flow_tasking_exclusivity_checks__two_tasks_single_so.ads_9_9_task_termination___00.smt2 |    0.096s | 20.392MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/Q508-012__flow_null_proc__expanded.ads_7_9_task_termination___00.smt2 |    0.096s | 19.86MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/O402-017__tagged__no_primitive_use.adb_7_4_precondition___00.smt2 |    0.096s | 20.396MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/O429-052__flow_tasking_exclusivity_checks__task_type_array_single_so.ads_7_14_task_termination___00.smt2 |    0.097s | 20.372MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC20-032__succ_pred__p.adb_23_31_range_check___00.smt2 |    0.097s | 20.372MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__real_world__real_world.adb_45_21_range_check___00.smt2 |    0.097s | 20.864MiB| unsat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P602-007__protected_no_return__p.adb_28_8_precondition___00.smt2 |    0.098s | 20.412MiB| sat | 0 |  |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/R220-054__fixed_point_small__fp_test.ads_13_23_overflow_check___00.smt2 |    0.099s | 21.376MiB| sat | 0 |  |  |
