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
Experiment date and time: 2026-09-07 02:11:52 UTC
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://zenodo.org/records/16740866/files/QF_UFDTLIRA.tar.zst?download=1 | Source list: benchmarks-qf.txt
Job tag: coz3-https-zenodo.org-records-QF_UFDTLIRA
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 53621bed781b1f80d49d57b65e13b6ef814e563a
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/16740866/files/QF_UFDTLIRA.tar.zst?download=1
Z3 commit message: Add witness_instantiation simplifier and fix TPTP preprocessing pipeline fixpoint (#10679)

## Summary

Adds a new opt-in TPTP preprocessing pass, ptp.witness_instantiation,
that synthesizes a fresh witness constant for any uninterpreted sort
with no ground term anywhere in the problem, and uses it to instantiate
orall-quantified formulas over that sort. This is sound (all sorts are
non-empty in classical logic) and lets search-time E-matching proceed on
axioms that would otherwise never fire for lack of a ground term to
match against (e.g. ANA068^1.p's FINITE_REAL_INTERVAL_1 axiom,
quantified over eal, with no ground eal term anywhere in the problem).

Also fixes a real bug in the TPTP preprocessing pipeline: it was only
flushed once before check_sat, so simplifiers that add new formulas
(lambda_reify_simplifier, leibniz_simplifier,
witness_instantiation_simplifier) never got a second pass to fully
simplify/rewrite those newly-added formulas. Adds an extra push()/pop(1)
flush right before check_sat, gated on any of these three passes being
enabled, guarded by a ry/catch so a latent lambda_reify_simplifier
sort-mismatch bug (which check_sat's own exception handling already
downgrades gracefully to GaveUp) can't escape as a hard crash.

This fixes ANA068^1.p and 3 of its 4 sibling problems with
ptp.reify_lambda_literals=true (previously all timed out).

## Testing

- Full 5,279-file TPTP higher-order regression confirms both
eify_lambda_literals and witness_instantiation have an inherent
net-negative trade-off at full corpus scale (gains on some problems,
timeouts on many others due to extra instantiation noise), consistent
with eify_lambda_literals's pre-existing regression profile
(historically measured at 53 gained / 591 lost even without this fix).
Both remain opt-in, default off; default behavior is unaffected.
- 99/99 unit tests pass (
inja test-z3 + ./test-z3.exe /a).
- Merged latest origin/master into this branch; rebuilt cleanly and
re-verified ANA068^1.p still solves.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

---------

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | INFO |
|------------|----------:|---------:|-------------:| ----------:|------|
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_44_22_assert___00.smt2 |    0.019s | 20.104MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P511-009__test_attributes__component_size_attribute.adb_43_7_assert___00.smt2 |    0.020s | 19.476MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_66_22_assert___00.smt2 |    0.021s | 19.856MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC22-015__succ_overflow__p.adb_29_18_range_check___00.smt2 |    0.021s | 19.824MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P511-009__test_attributes__component_size_attribute.adb_37_7_assert___00.smt2 |    0.021s | 19.84MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/M604-044__overflow__misovattr.adb_6_22_assert___00.smt2 |    0.021s | 19.616MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/M604-044__overflow__misovattr.adb_8_22_assert___00.smt2 |    0.021s | 19.608MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_18_22_assert___00.smt2 |    0.022s | 19.9MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/S702-024__record_attributes_in_allocators__test_constr.adb_39_37_null_pointer_dereference___00.smt2 |    0.022s | 19.632MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__imported.ads_14_13_inconsistent_pre___00.smt2 |    0.022s | 20.36MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P511-009__test_attributes__component_size_attribute.adb_49_7_assert___00.smt2 |    0.023s | 19.644MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/S920-003__flow_implicit_noret__p.adb_5_10_raise___00.smt2 |    0.023s | 20.74MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/M604-044__overflow__misovattr.adb_7_35_range_check___00.smt2 |    0.023s | 20.408MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__real_world__real_world.adb_44_22_assert___00.smt2 |    0.024s | 20.616MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/O429-052__flow_tasking_review__unsync_access__bar.adb_16_17_range_check___00.smt2 |    0.024s | 19.816MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/stopwatch__user.ads_12_17_ceiling__priority_protocol___00.smt2 |    0.026s | 19.848MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__nested_packages.adb_27_16_dead_code___00.smt2 |    0.027s | 19.872MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_87_40_unreachable_branch___00.smt2 |    0.028s | 19.876MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_139_33_unreachable_branch___00.smt2 |    0.030s | 19.852MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC20-032__succ_pred__p.adb_41_19_range_check___00.smt2 |    0.031s | 19.592MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__call_proc_no_body.adb_4_7_precondition___00.smt2 |    0.032s | 20.044MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P712-004__prot_discr__main.adb_7_19_assert___00.smt2 |    0.032s | 19.728MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_30_22_assert___00.smt2 |    0.034s | 19.916MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/stopwatch__user.ads_26_17_ceiling__priority_protocol___00.smt2 |    0.035s | 19.856MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/OC09-042__po_in_spec_only_pkg__main.adb_3_11_ceiling__priority_protocol___00.smt2 |    0.036s | 19.6MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/N915-008__flow_acats_oo__cc1311a.adb_279_17_cc1311a.adb_441_6_range_check___00.smt2 |    0.036s | 19.86MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_139_44_unreachable_branch___00.smt2 |    0.036s | 19.872MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/R123-023__flow_unused_variable__q.adb_7_17_discriminant_check___00.smt2 |    0.036s | 20.4MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KB04-032__lib_level__p.adb_6_19_assert___00.smt2 |    0.036s | 20.316MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/NC11-033__variant__variants.ads_13_20_range_check___00.smt2 |    0.037s | 19.904MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/Q512-013__flow_type_aspects__a.adb_9_7_dead_code___00.smt2 |    0.037s | 19.864MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P602-007__protected_no_return__p.adb_28_8_precondition___00.smt2 |    0.038s | 20.692MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/Q130-019__missing_discr_check__p_init.ads_14_24_range_check___00.smt2 |    0.038s | 19.6MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC22-015__succ_overflow__p.adb_29_29_overflow_check___00.smt2 |    0.039s | 20.284MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_19_37_unreachable_branch___00.smt2 |    0.039s | 20.252MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/S920-003__flow_implicit_noret__p.adb_10_10_precondition___00.smt2 |    0.039s | 20.404MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC22-015__succ_overflow__p.adb_37_38_overflow_check___00.smt2 |    0.039s | 20.372MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/Q906-011__counterexample__copy_values.adb_6_22_assert___00.smt2 |    0.039s | 28.856MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/O429-052__flow_tasking_exclusivity_checks__task_type_array_single_so.ads_7_14_task_termination___00.smt2 |    0.039s | 20.364MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P725-016__recursive_spec__main.adb_5_19_assert___00.smt2 |    0.040s | 20.008MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_115_22_assert___00.smt2 |    0.040s | 19.852MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/O429-052__flow_tasking_exclusivity_checks__two_tasks_single_so.ads_9_9_task_termination___00.smt2 |    0.040s | 21.092MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P929-043__flow_pkg_elaboration_in_direct_calls__nest_in_proc.adb_9_7_precondition___00.smt2 |    0.040s | 19.596MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC20-032__succ_pred__p.adb_41_38_overflow_check___00.smt2 |    0.041s | 20.292MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC20-032__succ_pred__p.adb_32_29_overflow_check___00.smt2 |    0.042s | 20.36MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_19_22_assert___00.smt2 |    0.042s | 19.888MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/L323-005__recursive__p.adb_6_23_assert___00.smt2 |    0.042s | 20.16MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/OB17-029__ceiling_elaboration_task__t.ads_2_9_task_termination___00.smt2 |    0.042s | 19.496MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__functions.adb_22_10_precondition___00.smt2 |    0.043s | 19.6MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/L323-005__recursive__p.ads_7_36_postcondition___00.smt2 |    0.044s | 20.34MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__nested_blocks.adb_11_29_dead_code___00.smt2 |    0.044s | 19.612MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__imported.adb_10_7_precondition___00.smt2 |    0.045s | 20.084MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_24_22_assert___00.smt2 |    0.046s | 20.104MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__nest_in_proc.adb_21_13_dead_code___00.smt2 |    0.047s | 19.824MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/O402-017__tagged__no_primitive_use.adb_7_4_precondition___00.smt2 |    0.048s | 20.384MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/Q130-019__missing_discr_check__p_init.ads_15_24_range_check___00.smt2 |    0.049s | 19.588MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_51_22_assert___00.smt2 |    0.049s | 19.648MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/stopwatch__timer.adb_16_17_ceiling__priority_protocol___00.smt2 |    0.049s | 19.6MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__nonreturning_spec.ads_2_14_inconsistent_post___00.smt2 |    0.050s | 19.504MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/K512-016__task__t.ads_3_9_task_termination___00.smt2 |    0.050s | 19.84MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/S818-002__might_not_return__pack.adb_5_7_raise___00.smt2 |    0.050s | 20.136MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/L316-015__all__a2.adb_4_5_precondition___00.smt2 |    0.050s | 19.592MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC22-015__succ_overflow__p.adb_37_19_range_check___00.smt2 |    0.051s | 19.904MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/Q412-006__assert_true__p.ads_17_4_assert___00.smt2 |    0.052s | 19.656MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC20-032__succ_pred__p.adb_32_36_overflow_check___00.smt2 |    0.052s | 20.388MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__real_world__real_world.adb_45_21_range_check___00.smt2 |    0.052s | 20.456MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/O521-002__no_return__p.adb_23_6_precondition___00.smt2 |    0.052s | 19.82MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KB28-003__succ__p.adb_12_23_range_check___00.smt2 |    0.052s | 20.368MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/L531-021__discr__subty.ads_5_20_range_check___00.smt2 |    0.052s | 20.08MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KB28-003__succ__p.adb_6_23_range_check___00.smt2 |    0.053s | 20.28MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_27_22_assert___00.smt2 |    0.053s | 19.872MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/L316-015__all__a1.adb_4_5_precondition___00.smt2 |    0.053s | 19.48MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/M604-044__overflow__misovattr.adb_6_35_range_check___00.smt2 |    0.053s | 20.292MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__r.adb_46_10_dead_code___00.smt2 |    0.054s | 20.124MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/O429-052__flow_tasking_exclusivity_checks__two_tasks_single_so.ads_7_9_task_termination___00.smt2 |    0.054s | 20.372MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_14_18_inconsistent_pre___00.smt2 |    0.054s | 19.788MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/OB16-016__system_default_priority__main.adb_3_11_ceiling__priority_protocol___00.smt2 |    0.054s | 19.716MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC20-032__succ_pred__p.adb_32_18_range_check___00.smt2 |    0.055s | 19.6MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/M524-002__demorgan__test.adb_12_22_assert___00.smt2 |    0.055s | 19.9MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_24_37_unreachable_branch___00.smt2 |    0.055s | 19.852MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__functions.adb_16_7_functions.adb_29_4_precondition___00.smt2 |    0.055s | 19.648MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/stopwatch__user.ads_19_17_ceiling__priority_protocol___00.smt2 |    0.055s | 19.604MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/M809-022__scenario__q.adb_6_7_assert___00.smt2 |    0.055s | 19.836MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/Q508-012__flow_null_proc__expanded.ads_7_9_task_termination___00.smt2 |    0.055s | 19.612MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/PA11-007__depchain__a-reatim.ads_6_4_assert___00.smt2 |    0.055s | 19.68MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/O429-052__flow_tasking_exclusivity_checks__two_task_type_objects_single_so.ads_7_14_task_termination___00.smt2 |    0.056s | 20.352MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__nested_packages.adb_56_13_dead_code___00.smt2 |    0.056s | 19.484MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/NC11-033__variant__variants.ads_14_20_range_check___00.smt2 |    0.056s | 19.604MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__p.adb_22_10_dead_code___00.smt2 |    0.056s | 19.832MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/S702-024__record_attributes_in_allocators__test_constr.adb_24_30_null_pointer_dereference___00.smt2 |    0.057s | 19.856MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/S729-004__inline_dispatching__p.adb_6_22_assert___00.smt2 |    0.057s | 20.396MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__nested_packages.adb_62_10_dead_code___00.smt2 |    0.057s | 19.58MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_87_22_assert___00.smt2 |    0.058s | 19.892MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_112_22_assert___00.smt2 |    0.058s | 19.884MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/M604-044__overflow__misovattr.adb_8_35_range_check___00.smt2 |    0.059s | 20.632MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/M604-044__overflow__misovattr.adb_9_35_range_check___00.smt2 |    0.059s | 20.368MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/S702-024__record_attributes_in_allocators__test_constr.adb_39_22_assert___00.smt2 |    0.059s | 20.104MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_40_18_inconsistent_pre___00.smt2 |    0.059s | 19.916MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_77_22_assert___00.smt2 |    0.059s | 19.908MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_33_22_assert___00.smt2 |    0.059s | 19.948MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__enums__enums.adb_194_29_range_check___00.smt2 |    0.059s | 20.372MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/R220-054__fixed_point_small__fp_test.adb_6_17_overflow_check___00.smt2 |    0.059s | 21.284MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/Q818-007__projection__sc_status_type.adb_5_22_assert___00.smt2 |    0.060s | 20.364MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/N609-025__private_type_discr__record_discr.adb_24_28_discriminant_check___00.smt2 |    0.060s | 20.148MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_138_22_assert___00.smt2 |    0.060s | 20.172MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/R220-054__fixed_point_small__fp_test.ads_13_23_overflow_check___00.smt2 |    0.061s | 21.652MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/R123-023__flow_unused_variable__q.adb_2_21_range_check___00.smt2 |    0.062s | 19.812MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_33_37_unreachable_branch___00.smt2 |    0.062s | 19.78MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/S702-024__record_attributes_in_allocators__test_constr.adb_24_22_assert___00.smt2 |    0.063s | 20.108MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC20-032__succ_pred__p.adb_23_31_range_check___00.smt2 |    0.064s | 20.404MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC22-015__succ_overflow__p.adb_21_31_range_check___00.smt2 |    0.067s | 19.644MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/M524-002__demorgan__test.adb_8_22_assert___00.smt2 |    0.067s | 19.936MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_71_22_assert___00.smt2 |    0.067s | 19.86MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC22-015__succ_overflow__p.adb_29_36_overflow_check___00.smt2 |    0.068s | 20.368MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P511-009__test_attributes__storage_place_attributes.adb_73_22_assert___00.smt2 |    0.068s | 19.936MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/PA27-015__nested_no_return__nest_in_proc.adb_14_4_precondition___00.smt2 |    0.068s | 19.636MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/Q512-013__flow_type_aspects__a.adb_7_10_dead_code___00.smt2 |    0.068s | 19.636MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P511-009__test_attributes__storage_place_attributes.adb_51_22_assert___00.smt2 |    0.068s | 19.828MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P712-004__prot_discr__p.ads_7_13_range_check___00.smt2 |    0.068s | 19.78MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/S920-003__flow_noret__noret.adb_6_7_raise___00.smt2 |    0.068s | 19.896MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/L531-021__discr__special.ads_6_32_range_check___00.smt2 |    0.070s | 19.64MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KB10-018__cursor_location__cursor_location.ads_10_23_unreachable_branch___00.smt2 |    0.071s | 20.476MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P602-007__protected_no_return__p.adb_23_6_precondition___00.smt2 |    0.073s | 20.024MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/Q508-012__flow_null_proc__nullproc.ads_7_9_task_termination___00.smt2 |    0.073s | 20.084MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_20_33_unreachable_branch___00.smt2 |    0.073s | 19.884MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P126-025__generic_procedure_renaming__c.adb_8_4_precondition___00.smt2 |    0.073s | 19.704MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/Q512-018__null_task_id__main.adb_7_41_precondition___00.smt2 |    0.074s | 20.22MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P712-004__prot_discr__p.ads_6_13_range_check___00.smt2 |    0.074s | 19.9MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_80_22_assert___00.smt2 |    0.074s | 19.656MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/S711-051__constrained_attr__test_constrained.adb_34_28_null_pointer_dereference___00.smt2 |    0.074s | 19.624MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P725-016__recursive_spec__main.adb_5_19_precondition___00.smt2 |    0.075s | 19.872MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/S325-002__rec_pointers_bad__use_incomplete_type_auto.ads_4_19_assert___00.smt2 |    0.075s | 20.096MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/M524-002__demorgan__test.adb_10_22_assert___00.smt2 |    0.075s | 20.264MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P511-009__test_attributes__storage_place_attributes.adb_95_22_assert___00.smt2 |    0.075s | 19.832MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__decl_only.ads_8_18_inconsistent_pre___00.smt2 |    0.076s | 20.16MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/M524-002__demorgan__test.adb_6_22_assert___00.smt2 |    0.076s | 20.124MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KB10-018__cursor_location__cursor_location.ads_11_24_unreachable_branch___00.smt2 |    0.076s | 20.328MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P511-009__test_attributes__component_size_attribute.adb_55_7_assert___00.smt2 |    0.077s | 19.66MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_45_22_assert___00.smt2 |    0.077s | 20.12MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC22-015__succ_overflow__p.adb_21_38_range_check___00.smt2 |    0.077s | 20.428MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC20-032__succ_pred__p.adb_41_31_overflow_check___00.smt2 |    0.077s | 20.424MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/KC22-015__succ_overflow__p.adb_37_31_overflow_check___00.smt2 |    0.077s | 20.6MiB| unsat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__functions.adb_9_10_dead_code___00.smt2 |    0.077s | 19.648MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_54_22_assert___00.smt2 |    0.077s | 20.324MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/riposte__logic__logic.adb_74_22_assert___00.smt2 |    0.078s | 20.172MiB| sat | 0 |  |
|non-incremental/QF_UFDTLIRA/20200306-Kanig/spark2014bench/P715-019__flow_subprogram_termination__r.adb_53_7_precondition___00.smt2 |    0.078s | 19.716MiB| sat | 0 |  |
