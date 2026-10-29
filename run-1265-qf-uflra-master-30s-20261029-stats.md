# .

* SAT 759
* UNSAT 510
* TIMEOUT 0
* UNKNOWN 0

* ERRORS 15 (signal-15:15)

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-10-29 08:53:46 UTC
Job description: Compare Z3 5.3.1 against master pinned at 70f966a79844ea3b18fe35e5711d55754e80460e on full SMT-LIB QF_UFLRA; 30-second per-input timeout; identical settings and runner.
Job tag: qf-uflra-master-30s-20261029
Runner: rise-runner-1
Z3 repo: Z3Prover/z3
Z3 commit: 70f966a79844ea3b18fe35e5711d55754e80460e
Z3 branch: master
Z3 options: "-T:30 model_validate=true"
Ramon options: "--limit-time 30"
Input preset: raw
Z3 inputs: https://zenodo.org/records/16740866/files/QF_UFLRA.tar.zst?download=1
Z3 commit message: F*: check integer rounding cuts in th-lemma arith unsupported steps (QF_LIA rings) (#11430)

## Latest verified increment: symbolic BV-addition normalization

Commit `13f00d1ce` checks **105 more rewrite obligations across eight
ABV inputs on each backend**, with no added obligations. These files
remain PARTIAL; totals stay **25 ACCEPT / 18 PARTIAL / 1 timeout / 6
UNKNOWN** across50ABVinputs. Prior LIA and ABV completions are
preserved. Three independent reviewers approved. [Measured comparison,
tests and remaining
gaps](https://github.com/Z3Prover/z3/pull/11430#issuecomment-6487580106).

Current master `c95b61cb0` is included in the tested merged baseline.
**The multi-cohort campaign remains incomplete; QF_NIA stays excluded.**

---

## Latest verified increment: QF_ABV

Commit `28a828e0b` adds a proved bounded BV evaluator. On the 50 ABV
workflow inputs, **both backends improve from22ACCEPT/21PARTIAL
to25ACCEPT/18PARTIAL**, with timeout1/UNKNOWN6unchanged. Three new fully
checked inputs and13discharged steps per backend. The earlier LIA gains
are preserved. Three independent reviewers approved.

[Full measured progress, validation and
limitations](https://github.com/Z3Prover/z3/pull/11430#issuecomment-6486013436).
Tested baseline: `2bf154dd3`. Newer master `c95b61cb0` merges cleanly
but the combined tree is unbuilt. **The multi-cohort campaign remains
incomplete; QF_NIA stays excluded.**

---

## Check integer rounding cuts in `th-lemma arith unsupported` steps
(QF_LIA)

First slice of the Pulse checker PARTIAL-gap campaign (order:
QF_LIA/QF_IDL, then QF_ABV, then other non-QF_NIA cohorts). **The
campaign is not complete**; this PR covers one QF_LIA family only.

### Gap
Z3 labels a `false` theory lemma `th-lemma arith unsupported` when the
LP derivation behind a linear conflict has no recipe, and records no
certificate. The two QF_LIA ring benchmarks in the SMT-LIB 2025 sample
(`ring_2exp4_6vars_3ite_unsat`, `ring_2exp8_6vars_2ite_unsat`) left 18
and 19 such steps PARTIAL on both the OCaml and Cherry checkers. Each
step has a satisfiable rational relaxation, so no Farkas vector exists,
but it becomes refutable after one rounding: variables fixed to
constants are substituted, a row whose Int coefficients share a factor g
>= 2 that does not divide its constant is rounded (g*L + c <= 0 gives L
<= floor(-c/g)), and the rest has a Farkas refutation.

Source reports: Z3 run 40718873785 (producer 08228d998) and bench run
40723098000 (producer bfb47e299). Pending runs were not used as
evidence.

### Change
New `fstar/Z3LpIntegerCut.fst`:
- A certificate is a list of rounded bounds plus a final weight vector.
Each bound and the final refutation are replayed with the existing
`Z3LinearArithmeticAst.check` against the actual premise ASTs and
earlier bounds only. That check strengthens strict Int inequalities, so
the rounding is checked, not trusted, and is unavailable for Real terms.
- `lemma_check_sound` / `lemma_inferred_sound` use the same hypotheses
as `Z3LpGomory.lemma_check_sound`; the kernel call site discharges them
from the unchanged `model_sound`. No `assume`/`admit`/lax, no new model
axiom, no rlimit/fuel/ifuel change.
- The untrusted candidate search handles `false` conclusions only:
unit-coefficient equality substitution, rounding of non-strict all-Int
rows, then Fourier-Motzkin elimination. It is capped and fails closed to
a pending obligation (32 premises, 8 bounds of at most 64 terms, 128
rows, 64 eliminations, 4096 row pairs, magnitudes at most 2^128).
- `Z3UnifiedProof.checked_theory_lemma` gains one disjunct and
`lemma_check_node_sound` one matching branch. Only acceptance grows.

Still pending (not covered): genuine Gomory/branching cuts (the two
`cut_lemma_01_011` steps), more than one rounding step, non-`false`
conclusions, rows with Real atoms.

### Evidence (acceptance builds: fresh git-archive of tree c5b56cf9, vs
frozen baseline 45e285fb9)
Same-query before/after with workflow smtlib-sample options (10000 ms
solver timeout, 60 s case timeout, 4 GiB):

| benchmark | master 45e285fb9 (OCaml / Cherry) | this PR (OCaml /
Cherry) |
|---|---|---|
| ring_2exp4_6vars_3ite_unsat | PARTIAL 18 / PARTIAL 18 | ACCEPT /
ACCEPT |
| ring_2exp8_6vars_2ite_unsat | PARTIAL 19 / PARTIAL 19 | ACCEPT /
ACCEPT |
| cut_lemma_01_011 | PARTIAL 2 | PARTIAL 2 (unchanged) |

- Per-query recount over 225 + 225 sample inputs per backend: only the
two rings change; CRASH/TIMEOUT/UNKNOWN counts unchanged and none
counted as proof. QF_FF 25/25 ACCEPT unchanged; proofs corpus unchanged.
- Strict F* (`--warn_error '@1..999'`, F* ci 2026.09.27; Cherry nightly
2026.10.23) verified the new modules and `Z3UnifiedProof`; CLI suite
47/47 per backend; bridge group `checked-integer-rounding-cuts` (13
accepted, 13 rejected) passes. `lemma_check_node_sound` takes about 8.5
s to verify (known hotspot, no option raised).
- Negative tests: too strong/weak rounding, zero/negative divisor
weights, non-cancelling rational weights, wrong lengths, forward
references, swapped/dropped premises, non-false conclusion, Real sorts,
Int-SAT systems, Real-SAT countermodel.
- Independent review: fuzz with fresh seeds (mutated certificates 32181
cases, SAT variants 2749 cases) found no unsound accept; z3 cross-check
over a 3.78M-point rounding grid agrees. Three reviewers approved.

### Limitations and disclosures
- Tested pair is 45e285fb9 vs 536417efa. `master` has since advanced to
5520d2f14 (#11411, #11423). Those commits touch only `src/ast`
datatype/recfun code, `src/test` and `fstar/audits/pr-11411`, with no
file overlap with this PR, and a merge-tree check was clean. The
combined tree was **not revalidated**.
- Input row width is not capped before the search; a 2000-atom, 8-row
input costs about 2 s. It is bounded and fails closed.
- The commit's `Assistant-verification` trailers describe the
development build; the acceptance-build results are in this description.

### Next slices (not in this PR)
QF_LIA `cut_lemma_01_011`; QF_IDL `SelectionSort_safe_bgmc004` (PARTIAL
10 locally, TIMEOUT in the workflow); QF_ABV; other non-QF_NIA cohorts.

Assistant-model: Claude Opus 5.5
Assistant-workflow: goal orchestrator (run
91057a43-8a82-4aba-9174-894c7de3fe78)
Assistant-duration: 55m to the first frozen candidate, estimated 2-4h
Assistant-verification: strict F* (ci 2026.09.27) on fresh git-archive
build passed: 84 Verifying lines, 0 errors/warnings
Assistant-verification: OCaml and Cherry CLI suites passed: 47/47 each,
plus checked-integer-rounding-cuts bridge group
Assistant-verification: same-query before/after on 225+225 sample inputs
passed: only the two ring benchmarks changed (PARTIAL to ACCEPT)
Assistant-verification: combined tree with master 5520d2f14 unavailable:
not revalidated
User-preference: fill PARTIAL proof-checker gaps in QF_LIA and QF_IDL
first, then QF_ABV and the other cohorts, excluding QF_NIA
User-preference: do not reintroduce fstar/audits directories
Co-authored-by: Nikolaj Bjorner <nbjorner@microsoft.com>
</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | INFO |
|------------|----------:|---------:|-------------:| ----------:|------|
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.gcd_2_true-unreach-call.i.smt2 |    0.013s | 19.052MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.pointer_extension2_false-unreach-call.i.smt2 |    0.014s | 19.912MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--input--misc--pcap_keys.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.014s | 19.24MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--i2c--algos--i2c-algo-pca.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.014s | 19.024MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.while_infinite_loop_3_true-unreach-call_false-termination.i.smt2 |    0.014s | 19.072MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--rtc--rtc-ds1390.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.014s | 19.16MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--input--touchscreen--dynapro.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.014s | 19.168MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/FFT/smtlib.627677.smt2              |    0.015s | 19.668MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/FFT/smtlib.626139.smt2              |    0.015s | 19.584MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/FFT/smtlib.624882.smt2              |    0.015s | 19.516MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/FFT/smtlib.624916.smt2              |    0.015s | 19.64MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--input--touchscreen--touchright.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.015s | 19.26MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--input--misc--ab8500-ponkey.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.015s | 19.164MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.for_bounded_loop1_false-unreach-call_true-termination.i.smt2 |    0.015s | 19.972MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.gcd_1_true-unreach-call.i.smt2 |    0.015s | 19.048MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.fo_test.c_false-unreach-call.i.smt2 |    0.015s | 19.576MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.nec20_false-unreach-call.i.smt2 |    0.015s | 19.852MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.count_up_down_false-unreach-call_true-termination.i.smt2 |    0.015s | 19.78MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/FFT/smtlib.620524.smt2              |    0.016s | 19.724MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.parity_true-unreach-call.i.smt2 |    0.016s | 19.724MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.pointer_extension3_false-unreach-call.i.smt2 |    0.016s | 19.648MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--input--touchscreen--inexio.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.016s | 19.144MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.sum01_true-unreach-call_true-termination.i.smt2 |    0.016s | 19.624MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.terminator_02_false-unreach-call_true-termination.i.smt2 |    0.016s | 19.744MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.mutex_lock_struct.c_false-unreach-call.i.smt2 |    0.016s | 20.024MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.rule57_ebda_blast.c_false-unreach-call.i.smt2 |    0.016s | 20.012MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--input--touchscreen--mtouch.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.016s | 19.248MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.sum01_false-unreach-call_true-termination.i.smt2 |    0.016s | 19.74MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/FFT/smtlib.624898.smt2              |    0.017s | 19.604MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/FFT/smtlib.626159.smt2              |    0.017s | 19.564MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/FFT/smtlib.624914.smt2              |    0.017s | 19.524MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.gcd_4_true-unreach-call.i.smt2 |    0.017s | 19.164MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.jain_4_true-unreach-call.i.smt2 |    0.017s | 19.664MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.count_up_down_true-unreach-call_true-termination.i.smt2 |    0.017s | 19.78MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.1_3.c_false-unreach-call.i.smt2 |    0.017s | 19.812MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.m0_false-unreach-call_drivers-staging-comedi-drivers-ni_pcidio-ko--107_1a--adbbc36-1.c.smt2 |    0.017s | 19.944MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.jain_2_true-unreach-call.i.smt2 |    0.017s | 19.8MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.sum01_bug02_false-unreach-call_true-termination.i.smt2 |    0.017s | 19.632MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--atm--adummy.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.017s | 19.196MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--input--touchscreen--touchwin.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.017s | 19.54MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.sum03_true-unreach-call_false-termination.i.smt2 |    0.017s | 19.84MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.m0_false-unreach-call_drivers-staging-comedi-drivers-ni_670x-ko--107_1a--adbbc36-1.c.smt2 |    0.017s | 19.964MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.verisec_NetBSD-libc__loop_false-unreach-call.i.smt2 |    0.017s | 19.692MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.trex03_true-unreach-call.i.smt2 |    0.017s | 19.812MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.pointer_extension_false-unreach-call.i.smt2 |    0.017s | 19.804MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.jain_7_true-unreach-call.i.smt2 |    0.017s | 19.912MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/FFT/smtlib.623586.smt2              |    0.018s | 19.636MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/FFT/smtlib.626179.smt2              |    0.018s | 19.68MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.test_locks_7_true-unreach-call_false-termination.c.smt2 |    0.018s | 20.228MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.s3_srvr_6_false-unreach-call.cil.c.smt2 |    0.018s | 20.1MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.trex03_false-unreach-call_true-termination.i.smt2 |    0.018s | 20.016MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.modulus_true-unreach-call.i.smt2 |    0.018s | 19.896MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.m0_false-unreach-call_drivers-staging-comedi-drivers-ni_660x-ko--107_1a--adbbc36-1.c.smt2 |    0.018s | 20.048MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.mutex_lock_int.c_false-unreach-call.i.smt2 |    0.018s | 19.8MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.test_locks_6_true-unreach-call_false-termination.c.smt2 |    0.018s | 20.032MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.test_locks_14_false-unreach-call.c.smt2 |    0.018s | 20.308MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.jain_1_true-unreach-call.i.smt2 |    0.018s | 19.644MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.linear_search_false-unreach-call.i.smt2 |    0.018s | 20.308MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.trex01_false-unreach-call_true-termination.i.smt2 |    0.018s | 19.776MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.m0_false-unreach-call_drivers-staging-comedi-drivers-ni_65xx-ko--107_1a--adbbc36-1.c.smt2 |    0.018s | 19.912MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.pointer_extension_true-unreach-call.i.smt2 |    0.019s | 19.648MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--input--touchscreen--gunze.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.019s | 19.44MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.test_locks_5_true-unreach-call_false-termination.c.smt2 |    0.019s | 20.328MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--input--misc--mpu3050.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.019s | 19.148MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.alt_test.c_false-unreach-call.i.smt2 |    0.019s | 19.904MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.kbfiltr_simpl2_false-unreach-call_true-termination.cil.c.smt2 |    0.020s | 20.384MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.test_locks_11_true-unreach-call_false-termination.c.smt2 |    0.020s | 20.58MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--input--touchscreen--touchit213.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.020s | 19.244MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.rule60_list2.c_false-unreach-call_1.i.smt2 |    0.020s | 19.88MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.jain_6_true-unreach-call.i.smt2 |    0.020s | 19.68MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.test_locks_15_false-unreach-call.c.smt2 |    0.020s | 20.296MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0100_10_21.smt2 |    0.020s | 20.012MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0100_10_19.smt2 |    0.020s | 19.884MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-bmc-svcomp14/cpachecker-bmc.callfpointer.c_false-unreach-call.i.smt2 |    0.021s | 19.728MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.m0_false-unreach-call_drivers-staging-comedi-drivers-ni_6527-ko--107_1a--adbbc36-1.c.smt2 |    0.021s | 20.116MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.test_locks_12_true-unreach-call_false-termination.c.smt2 |    0.021s | 20.416MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.nec11_false-unreach-call.i.smt2 |    0.021s | 19.816MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.linear_sea.ch_true-unreach-call.i.smt2 |    0.021s | 20.244MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0100_10_17.smt2 |    0.021s | 19.988MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0150_10_20.smt2 |    0.021s | 20.012MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0150_10_16.smt2 |    0.021s | 20.132MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0100_10_28.smt2 |    0.021s | 19.924MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0200_10_29.smt2 |    0.021s | 19.984MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0150_10_17.smt2 |    0.021s | 20.18MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0150_10_10.smt2 |    0.021s | 19.912MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0100_10_24.smt2 |    0.021s | 20.056MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0100_10_15.smt2 |    0.021s | 20.088MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0250_10_19.smt2 |    0.021s | 20.028MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0100_10_10.smt2 |    0.021s | 20.008MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0150_10_29.smt2 |    0.021s | 20.2MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.trex02_false-unreach-call_true-termination.i.smt2 |    0.022s | 19.54MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.test_locks_14_true-unreach-call.c.smt2 |    0.022s | 20.552MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.floppy_simpl4_false-unreach-call_true-termination.cil.c.smt2 |    0.022s | 20.556MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.floppy_simpl3_false-unreach-call_true-termination.cil.c.smt2 |    0.022s | 20.344MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--w1--slaves--w1_ds2408.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.022s | 20.376MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0100_10_14.smt2 |    0.022s | 20.004MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0100_10_29.smt2 |    0.022s | 20.1MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0250_10_11.smt2 |    0.022s | 20.08MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0150_10_27.smt2 |    0.022s | 20.164MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0300_10_20.smt2 |    0.022s | 19.948MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0100_10_26.smt2 |    0.022s | 19.928MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0200_10_27.smt2 |    0.022s | 20.02MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0200_10_26.smt2 |    0.022s | 20.06MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0100_10_13.smt2 |    0.022s | 20.248MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0100_10_25.smt2 |    0.022s | 19.984MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0100_10_27.smt2 |    0.022s | 19.956MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0300_10_26.smt2 |    0.022s | 20.052MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.kbfiltr_false-unreach-call.i.cil.c.smt2 |    0.023s | 20.716MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.test_locks_15_true-unreach-call_false-termination.c.smt2 |    0.023s | 20.8MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.test_locks_10_true-unreach-call.c.smt2 |    0.023s | 20.352MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--power--test_power.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.023s | 20.536MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0250_10_16.smt2 |    0.023s | 20.076MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0200_10_17.smt2 |    0.023s | 20.072MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0250_10_26.smt2 |    0.023s | 20.06MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0150_10_23.smt2 |    0.023s | 20.028MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0250_10_10.smt2 |    0.023s | 20.068MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0300_10_17.smt2 |    0.023s | 20.036MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0200_10_14.smt2 |    0.023s | 19.924MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0200_10_13.smt2 |    0.023s | 20.316MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0150_10_21.smt2 |    0.023s | 20.008MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0250_10_12.smt2 |    0.023s | 19.964MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0200_10_22.smt2 |    0.023s | 20.104MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0250_10_27.smt2 |    0.023s | 20.112MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0250_10_18.smt2 |    0.023s | 19.996MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0100_10_11.smt2 |    0.023s | 20.076MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0250_10_25.smt2 |    0.023s | 20.112MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0150_10_11.smt2 |    0.023s | 20.012MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0150_10_28.smt2 |    0.023s | 20.068MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0100_10_23.smt2 |    0.023s | 20.004MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0150_10_18.smt2 |    0.023s | 20.052MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0200_10_15.smt2 |    0.023s | 19.964MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0250_10_15.smt2 |    0.023s | 20.092MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0100_10_16.smt2 |    0.023s | 20.052MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0250_10_28.smt2 |    0.023s | 20.124MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0200_10_19.smt2 |    0.023s | 19.976MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.string_true-unreach-call.i.smt2 |    0.024s | 20.032MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0150_10_15.smt2 |    0.024s | 20.076MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0250_10_17.smt2 |    0.024s | 20.14MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0200_10_23.smt2 |    0.024s | 20.164MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0100_10_18.smt2 |    0.024s | 19.98MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0100_10_12.smt2 |    0.024s | 20.164MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0150_10_24.smt2 |    0.024s | 20.04MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0200_10_16.smt2 |    0.024s | 19.992MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0300_10_14.smt2 |    0.024s | 20.416MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0300_10_25.smt2 |    0.024s | 20.04MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0300_10_29.smt2 |    0.024s | 20.156MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0300_10_22.smt2 |    0.024s | 20.032MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0250_10_24.smt2 |    0.024s | 20.128MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0200_10_21.smt2 |    0.024s | 20.072MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0300_10_11.smt2 |    0.024s | 20.092MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0300_10_24.smt2 |    0.024s | 19.976MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0300_10_10.smt2 |    0.024s | 20.128MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0200_10_24.smt2 |    0.024s | 20.104MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0300_10_19.smt2 |    0.024s | 20.02MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0250_10_22.smt2 |    0.024s | 20.032MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0300_10_15.smt2 |    0.024s | 19.984MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--edac--mce_amd_inj.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.025s | 20.5MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.s3_srvr_10_false-unreach-call.cil.c.smt2 |    0.025s | 20.304MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0200_10_10.smt2 |    0.025s | 19.94MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0300_10_27.smt2 |    0.025s | 20.076MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0250_10_23.smt2 |    0.025s | 20.144MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0250_10_20.smt2 |    0.025s | 20.072MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0200_10_25.smt2 |    0.025s | 20.056MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0200_10_18.smt2 |    0.025s | 20.432MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0150_10_22.smt2 |    0.025s | 20.04MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0300_10_18.smt2 |    0.025s | 20.12MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0300_10_28.smt2 |    0.025s | 20.144MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0250_10_14.smt2 |    0.025s | 20.172MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0300_10_16.smt2 |    0.025s | 19.916MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0300_10_21.smt2 |    0.025s | 20.036MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_23.smt2 |    0.025s | 20.392MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_10.smt2 |    0.025s | 20.52MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0200_10_26.smt2 |    0.026s | 20.288MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0300_10_23.smt2 |    0.026s | 20.024MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0200_10_28.smt2 |    0.026s | 20.024MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0300_10_11.smt2 |    0.026s | 20.384MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0200_10_18.smt2 |    0.026s | 20.304MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0300_10_13.smt2 |    0.026s | 20.116MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0150_10_19.smt2 |    0.026s | 19.972MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0250_10_21.smt2 |    0.026s | 20.144MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0150_10_13.smt2 |    0.026s | 20.096MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0200_10_10.smt2 |    0.026s | 20.42MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0200_10_12.smt2 |    0.026s | 20.088MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0300_10_12.smt2 |    0.026s | 20.024MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0200_10_15.smt2 |    0.026s | 20.336MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_73.smt2 |    0.026s | 20.324MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_48.smt2 |    0.026s | 20.24MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_28.smt2 |    0.026s | 20.136MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_18.smt2 |    0.026s | 20.38MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_92.smt2 |    0.026s | 20.36MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.email_spec3_product23_false-unreach-call.cil.c.smt2 |    0.027s | 21.876MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0200_10_17.smt2 |    0.027s | 20.352MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0200_10_12.smt2 |    0.027s | 20.288MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0200_10_11.smt2 |    0.027s | 20.404MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0300_10_15.smt2 |    0.027s | 20.472MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0200_10_27.smt2 |    0.027s | 20.288MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0150_10_26.smt2 |    0.027s | 19.968MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0200_10_24.smt2 |    0.027s | 20.428MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0300_10_26.smt2 |    0.027s | 20.48MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_78.smt2 |    0.027s | 20.472MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_17.smt2 |    0.027s | 20.388MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_86.smt2 |    0.027s | 20.508MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_25.smt2 |    0.027s | 20.44MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_95.smt2 |    0.027s | 20.516MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_08.smt2 |    0.027s | 20.428MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_57.smt2 |    0.027s | 20.192MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_03.smt2 |    0.027s | 20.472MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_04.smt2 |    0.027s | 20.368MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_64.smt2 |    0.027s | 20.432MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_09.smt2 |    0.027s | 20.28MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_12.smt2 |    0.027s | 20.28MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_90.smt2 |    0.027s | 20.404MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_79.smt2 |    0.027s | 20.36MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_86.smt2 |    0.027s | 20.22MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_81.smt2 |    0.027s | 20.312MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_63.smt2 |    0.027s | 20.352MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_28.smt2 |    0.027s | 20.352MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-bmc-svcomp14/cpachecker-bmc.integerpromotion_false-unreach-call.i.smt2 |    0.028s | 19.628MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0300_10_27.smt2 |    0.028s | 20.5MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0250_10_29.smt2 |    0.028s | 20.044MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0200_10_21.smt2 |    0.028s | 20.412MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0200_10_11.smt2 |    0.028s | 20.224MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0200_10_16.smt2 |    0.028s | 20.524MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0200_10_19.smt2 |    0.028s | 20.372MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0300_10_21.smt2 |    0.028s | 20.48MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0200_10_20.smt2 |    0.028s | 20.34MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0150_10_14.smt2 |    0.028s | 20.052MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_00.smt2 |    0.028s | 20.352MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_80.smt2 |    0.028s | 20.436MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_47.smt2 |    0.028s | 20.4MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_27.smt2 |    0.028s | 20.16MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_37.smt2 |    0.028s | 20.364MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_99.smt2 |    0.028s | 20.3MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_39.smt2 |    0.028s | 20.44MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_65.smt2 |    0.028s | 20.152MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_58.smt2 |    0.028s | 20.372MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_45.smt2 |    0.028s | 20.248MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_63.smt2 |    0.028s | 20.396MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_42.smt2 |    0.028s | 20.2MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_62.smt2 |    0.028s | 20.184MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_27.smt2 |    0.028s | 20.152MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_60.smt2 |    0.028s | 20.448MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_65.smt2 |    0.028s | 20.328MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_43.smt2 |    0.028s | 20.336MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_07.smt2 |    0.028s | 20.4MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_05.smt2 |    0.028s | 20.432MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_35.smt2 |    0.028s | 20.248MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_52.smt2 |    0.028s | 20.372MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_71.smt2 |    0.028s | 20.248MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_31.smt2 |    0.028s | 20.276MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_12.smt2 |    0.028s | 20.444MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_95.smt2 |    0.028s | 20.352MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_04.smt2 |    0.028s | 20.416MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_48.smt2 |    0.028s | 20.292MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_78.smt2 |    0.028s | 20.248MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_89.smt2 |    0.028s | 20.476MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_74.smt2 |    0.028s | 20.252MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0250_10_13.smt2 |    0.029s | 20.168MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0300_10_12.smt2 |    0.029s | 20.392MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0300_10_18.smt2 |    0.029s | 20.532MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0200_10_25.smt2 |    0.029s | 20.496MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0300_10_22.smt2 |    0.029s | 20.612MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0500_10_18.smt2 |    0.029s | 20.556MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0200_10_23.smt2 |    0.029s | 20.468MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0300_10_16.smt2 |    0.029s | 20.368MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0200_10_29.smt2 |    0.029s | 20.332MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_10.smt2 |    0.029s | 20.236MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_11.smt2 |    0.029s | 20.508MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_06.smt2 |    0.029s | 20.184MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_01.smt2 |    0.029s | 20.312MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_99.smt2 |    0.029s | 20.412MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_44.smt2 |    0.029s | 20.288MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_18.smt2 |    0.029s | 20.304MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_02.smt2 |    0.029s | 20.356MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_01.smt2 |    0.029s | 20.396MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_87.smt2 |    0.029s | 20.44MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_62.smt2 |    0.029s | 20.372MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_50.smt2 |    0.029s | 20.384MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_70.smt2 |    0.029s | 20.38MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_40.smt2 |    0.029s | 20.36MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_02.smt2 |    0.029s | 20.424MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_45.smt2 |    0.029s | 20.292MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_34.smt2 |    0.029s | 20.424MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_68.smt2 |    0.029s | 20.42MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_82.smt2 |    0.029s | 20.292MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_53.smt2 |    0.029s | 20.448MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_64.smt2 |    0.029s | 20.212MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_66.smt2 |    0.029s | 20.368MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_16.smt2 |    0.029s | 20.416MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_91.smt2 |    0.029s | 20.36MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_71.smt2 |    0.029s | 20.36MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_08.smt2 |    0.029s | 20.228MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_59.smt2 |    0.029s | 20.34MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_11.smt2 |    0.029s | 20.3MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_15.smt2 |    0.029s | 20.476MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_84.smt2 |    0.029s | 20.364MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_73.smt2 |    0.029s | 20.268MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_29.smt2 |    0.029s | 20.224MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_31.smt2 |    0.029s | 20.7MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_47.smt2 |    0.029s | 20.508MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_76.smt2 |    0.029s | 20.396MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_16.smt2 |    0.029s | 20.424MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_30.smt2 |    0.029s | 20.5MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_82.smt2 |    0.029s | 20.2MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_00.smt2 |    0.029s | 20.544MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_69.smt2 |    0.029s | 20.328MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_22.smt2 |    0.029s | 20.336MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--net--arcnet--rfc1051.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.030s | 19.048MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.email_spec3_product27_false-unreach-call.cil.c.smt2 |    0.030s | 21.856MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.array_false-unreach-call.i.smt2 |    0.030s | 19.668MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0200_10_22.smt2 |    0.030s | 20.428MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0300_10_29.smt2 |    0.030s | 20.588MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0300_10_25.smt2 |    0.030s | 20.604MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0300_10_10.smt2 |    0.030s | 20.472MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0300_10_23.smt2 |    0.030s | 20.432MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0300_10_27.smt2 |    0.030s | 20.944MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0200_10_13.smt2 |    0.030s | 20.432MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0300_10_20.smt2 |    0.030s | 20.64MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0300_10_19.smt2 |    0.030s | 20.508MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0300_10_28.smt2 |    0.030s | 20.46MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0500_10_16.smt2 |    0.030s | 20.488MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0400_10_15.smt2 |    0.030s | 20.412MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0300_10_14.smt2 |    0.030s | 20.424MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0500_10_21.smt2 |    0.030s | 20.632MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_67.smt2 |    0.030s | 20.388MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_25.smt2 |    0.030s | 20.216MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_38.smt2 |    0.030s | 20.204MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_51.smt2 |    0.030s | 20.332MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_19.smt2 |    0.030s | 20.38MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_81.smt2 |    0.030s | 20.428MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_03.smt2 |    0.030s | 20.264MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_87.smt2 |    0.030s | 20.352MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_44.smt2 |    0.030s | 20.356MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_32.smt2 |    0.030s | 20.46MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_88.smt2 |    0.030s | 20.368MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_36.smt2 |    0.030s | 20.38MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_97.smt2 |    0.030s | 20.144MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_57.smt2 |    0.030s | 20.348MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_31.smt2 |    0.030s | 20.468MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_33.smt2 |    0.030s | 20.3MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_33.smt2 |    0.030s | 20.376MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_83.smt2 |    0.030s | 20.152MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_23.smt2 |    0.030s | 20.392MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_26.smt2 |    0.030s | 20.296MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_54.smt2 |    0.030s | 20.272MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_85.smt2 |    0.030s | 20.384MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_21.smt2 |    0.030s | 20.3MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_59.smt2 |    0.030s | 20.2MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_56.smt2 |    0.030s | 20.46MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_72.smt2 |    0.030s | 20.36MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_96.smt2 |    0.030s | 20.516MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_54.smt2 |    0.030s | 20.368MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_46.smt2 |    0.030s | 20.464MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_37.smt2 |    0.030s | 20.204MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_93.smt2 |    0.030s | 20.356MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_41.smt2 |    0.030s | 20.372MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.email_spec3_product18_false-unreach-call.cil.c.smt2 |    0.031s | 21.664MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.test_locks_13_true-unreach-call.c.smt2 |    0.031s | 20.816MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.email_spec3_product24_false-unreach-call.cil.c.smt2 |    0.031s | 21.728MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.email_spec3_product31_false-unreach-call.cil.c.smt2 |    0.031s | 21.84MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0300_10_28.smt2 |    0.031s | 20.82MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0500_10_15.smt2 |    0.031s | 20.544MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0300_10_17.smt2 |    0.031s | 20.4MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0500_10_12.smt2 |    0.031s | 20.528MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0300_10_21.smt2 |    0.031s | 20.908MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0500_10_25.smt2 |    0.031s | 20.588MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0500_10_11.smt2 |    0.031s | 20.592MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0100_10_20.smt2 |    0.031s | 19.848MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0300_10_24.smt2 |    0.031s | 20.788MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0600_10_11.smt2 |    0.031s | 20.484MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0300_10_13.smt2 |    0.031s | 20.652MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0500_10_17.smt2 |    0.031s | 20.54MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0300_10_29.smt2 |    0.031s | 20.924MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0400_10_22.smt2 |    0.031s | 20.568MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0400_10_24.smt2 |    0.031s | 20.488MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0200_10_28.smt2 |    0.031s | 20.456MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_61.smt2 |    0.031s | 20.208MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_94.smt2 |    0.031s | 20.208MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_52.smt2 |    0.031s | 20.424MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_96.smt2 |    0.031s | 20.312MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_66.smt2 |    0.031s | 20.576MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_74.smt2 |    0.031s | 20.684MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_92.smt2 |    0.031s | 20.516MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_46.smt2 |    0.031s | 20.532MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_26.smt2 |    0.031s | 20.348MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_35.smt2 |    0.031s | 20.368MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_53.smt2 |    0.031s | 20.24MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_22.smt2 |    0.031s | 20.276MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_61.smt2 |    0.031s | 20.296MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_30.smt2 |    0.031s | 20.308MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_49.smt2 |    0.031s | 20.26MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_41.smt2 |    0.031s | 20.68MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_96.smt2 |    0.031s | 20.488MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_42.smt2 |    0.031s | 20.32MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_06.smt2 |    0.031s | 20.34MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_14.smt2 |    0.031s | 20.428MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_91.smt2 |    0.031s | 20.2MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_79.smt2 |    0.031s | 20.2MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_07.smt2 |    0.031s | 20.504MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_98.smt2 |    0.031s | 20.392MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_41.smt2 |    0.031s | 20.36MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_61.smt2 |    0.031s | 20.528MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_89.smt2 |    0.031s | 20.404MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_68.smt2 |    0.031s | 20.232MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_24.smt2 |    0.031s | 20.34MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_40.smt2 |    0.031s | 20.4MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_05.smt2 |    0.031s | 20.224MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_98.smt2 |    0.031s | 20.552MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_75.smt2 |    0.031s | 20.572MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.m0_true-unreach-call_drivers-hwmon-s3c-hwmon-ko--130_7a--af3071a.c.smt2 |    0.032s | 20.596MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.email_spec3_product28_false-unreach-call.cil.c.smt2 |    0.032s | 21.812MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.email_spec3_product34_false-unreach-call.cil.c.smt2 |    0.032s | 22.18MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0400_10_26.smt2 |    0.032s | 20.46MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0300_10_24.smt2 |    0.032s | 20.46MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0500_10_14.smt2 |    0.032s | 20.54MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0500_10_19.smt2 |    0.032s | 20.612MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0600_10_15.smt2 |    0.032s | 20.368MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0400_10_23.smt2 |    0.032s | 20.552MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0400_10_27.smt2 |    0.032s | 20.432MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0400_10_29.smt2 |    0.032s | 20.552MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0200_10_14.smt2 |    0.032s | 20.424MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0300_10_17.smt2 |    0.032s | 20.804MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0600_10_25.smt2 |    0.032s | 20.496MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0500_10_23.smt2 |    0.032s | 20.576MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0300_10_15.smt2 |    0.032s | 20.824MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0500_10_10.smt2 |    0.032s | 20.468MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0150_10_12.smt2 |    0.032s | 19.976MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0300_10_26.smt2 |    0.032s | 20.856MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0400_10_18.smt2 |    0.032s | 20.464MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0300_10_16.smt2 |    0.032s | 20.78MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0600_10_27.smt2 |    0.032s | 20.368MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_56.smt2 |    0.032s | 20.388MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_15.smt2 |    0.032s | 20.416MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_37.smt2 |    0.032s | 20.7MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_84.smt2 |    0.032s | 20.172MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_66.smt2 |    0.032s | 20.412MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_85.smt2 |    0.032s | 20.192MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_80.smt2 |    0.032s | 20.528MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_67.smt2 |    0.032s | 20.34MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_49.smt2 |    0.032s | 20.484MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_20.smt2 |    0.032s | 20.2MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_81.smt2 |    0.032s | 20.56MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_80.smt2 |    0.032s | 20.248MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_46.smt2 |    0.032s | 20.312MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_75.smt2 |    0.032s | 20.416MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_75.smt2 |    0.032s | 20.508MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_76.smt2 |    0.032s | 20.376MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_83.smt2 |    0.032s | 20.376MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_39.smt2 |    0.032s | 20.296MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_72.smt2 |    0.032s | 20.38MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_58.smt2 |    0.032s | 20.336MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--spi--spi-tle62x0.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.033s | 22.096MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.email_spec3_product32_false-unreach-call.cil.c.smt2 |    0.033s | 22.168MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.test_locks_8_true-unreach-call_false-termination.c.smt2 |    0.033s | 20.288MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0300_10_22.smt2 |    0.033s | 20.832MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0500_10_24.smt2 |    0.033s | 20.504MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0400_10_10.smt2 |    0.033s | 20.648MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0300_10_11.smt2 |    0.033s | 20.608MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0450_10_11.smt2 |    0.033s | 20.828MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0500_10_27.smt2 |    0.033s | 20.532MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0600_10_16.smt2 |    0.033s | 20.416MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0300_10_23.smt2 |    0.033s | 20.8MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0450_10_21.smt2 |    0.033s | 20.808MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0300_10_10.smt2 |    0.033s | 21.004MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0400_10_12.smt2 |    0.033s | 20.524MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0400_10_11.smt2 |    0.033s | 20.576MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0600_10_23.smt2 |    0.033s | 20.48MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0600_10_17.smt2 |    0.033s | 20.444MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0450_10_24.smt2 |    0.033s | 20.756MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_43.smt2 |    0.033s | 20.476MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_14.smt2 |    0.033s | 20.324MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_17.smt2 |    0.033s | 20.272MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_51.smt2 |    0.033s | 20.268MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_93.smt2 |    0.033s | 20.464MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_94.smt2 |    0.033s | 20.272MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_55.smt2 |    0.033s | 20.428MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_88.smt2 |    0.033s | 20.7MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_14.smt2 |    0.033s | 20.932MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_82.smt2 |    0.033s | 20.648MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_69.smt2 |    0.033s | 20.348MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_19.smt2 |    0.033s | 20.304MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_18.smt2 |    0.033s | 20.532MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_59.smt2 |    0.033s | 20.776MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_29.smt2 |    0.033s | 20.448MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_54.smt2 |    0.033s | 20.744MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_65.smt2 |    0.033s | 20.56MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_77.smt2 |    0.033s | 20.268MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_21.smt2 |    0.033s | 20.196MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_77.smt2 |    0.033s | 20.348MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.trex01_true-unreach-call.i.smt2 |    0.034s | 19.848MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.s3_srvr_14_false-unreach-call.cil.c.smt2 |    0.034s | 22.216MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--input--touchscreen--tsc40.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.034s | 19.072MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.email_spec3_product30_false-unreach-call.cil.c.smt2 |    0.034s | 21.976MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--net--arcnet--rfc1201.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.034s | 19.16MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0600_10_26.smt2 |    0.034s | 20.56MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0600_10_24.smt2 |    0.034s | 20.476MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0600_10_14.smt2 |    0.034s | 20.62MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0300_10_14.smt2 |    0.034s | 21.004MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0450_10_16.smt2 |    0.034s | 20.928MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0600_10_12.smt2 |    0.034s | 20.424MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0600_10_29.smt2 |    0.034s | 20.388MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0450_10_20.smt2 |    0.034s | 20.948MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0600_10_21.smt2 |    0.034s | 20.516MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0500_10_28.smt2 |    0.034s | 20.472MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0450_10_15.smt2 |    0.034s | 20.972MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0400_10_21.smt2 |    0.034s | 21.188MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0600_10_22.smt2 |    0.034s | 20.52MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0400_10_17.smt2 |    0.034s | 20.552MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0600_10_28.smt2 |    0.034s | 20.664MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0300_10_20.smt2 |    0.034s | 20.792MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0400_10_16.smt2 |    0.034s | 20.496MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0450_10_29.smt2 |    0.034s | 20.972MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_48.smt2 |    0.034s | 20.712MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_70.smt2 |    0.034s | 20.496MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_64.smt2 |    0.034s | 20.572MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_23.smt2 |    0.034s | 20.596MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_42.smt2 |    0.034s | 20.868MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_24.smt2 |    0.034s | 20.344MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_55.smt2 |    0.034s | 20.572MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_03.smt2 |    0.034s | 20.788MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_12.smt2 |    0.034s | 20.556MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_90.smt2 |    0.034s | 20.504MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_72.smt2 |    0.034s | 20.68MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_13.smt2 |    0.034s | 20.356MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_68.smt2 |    0.034s | 20.716MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_38.smt2 |    0.034s | 20.48MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_40.smt2 |    0.034s | 20.492MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_97.smt2 |    0.034s | 20.312MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_36.smt2 |    0.034s | 20.688MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_27.smt2 |    0.034s | 20.748MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_77.smt2 |    0.034s | 20.56MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_49.smt2 |    0.034s | 20.692MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_32.smt2 |    0.034s | 20.54MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_10.smt2 |    0.034s | 20.768MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.terminator_03_false-unreach-call_true-termination.i.smt2 |    0.035s | 19.604MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0600_10_18.smt2 |    0.035s | 20.556MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0400_10_16.smt2 |    0.035s | 21.056MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0500_10_20.smt2 |    0.035s | 20.456MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0600_10_20.smt2 |    0.035s | 20.672MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0300_10_19.smt2 |    0.035s | 20.864MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0500_10_22.smt2 |    0.035s | 20.652MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0450_10_25.smt2 |    0.035s | 20.972MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0400_10_15.smt2 |    0.035s | 21.12MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0400_10_20.smt2 |    0.035s | 20.72MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0500_10_29.smt2 |    0.035s | 20.448MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_90.smt2 |    0.035s | 20.596MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_22.smt2 |    0.035s | 20.7MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_20.smt2 |    0.035s | 20.408MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_32.smt2 |    0.035s | 20.404MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_26.smt2 |    0.035s | 20.544MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_94.smt2 |    0.035s | 20.552MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_74.smt2 |    0.035s | 20.4MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_50.smt2 |    0.035s | 20.708MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_13.smt2 |    0.035s | 20.192MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_42.smt2 |    0.035s | 20.676MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_69.smt2 |    0.035s | 20.52MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_60.smt2 |    0.035s | 20.328MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_40.smt2 |    0.035s | 20.812MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_47.smt2 |    0.035s | 20.732MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_25.smt2 |    0.035s | 20.608MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_34.smt2 |    0.035s | 20.6MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_34.smt2 |    0.035s | 20.292MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_15.smt2 |    0.035s | 20.672MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_66.smt2 |    0.035s | 20.84MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_36.smt2 |    0.035s | 20.404MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_99.smt2 |    0.035s | 20.62MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-bmc-svcomp14/cpachecker-bmc.while_infinite_loop_4_false-unreach-call_true-termination.i.smt2 |    0.036s | 19.536MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.email_spec3_product25_false-unreach-call.cil.c.smt2 |    0.036s | 21.704MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0300_10_18.smt2 |    0.036s | 21.048MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0400_10_26.smt2 |    0.036s | 21.032MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0500_10_13.smt2 |    0.036s | 20.544MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0450_10_26.smt2 |    0.036s | 20.824MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0450_10_14.smt2 |    0.036s | 20.92MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0400_10_25.smt2 |    0.036s | 21.176MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0400_10_21.smt2 |    0.036s | 20.516MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0450_10_23.smt2 |    0.036s | 20.98MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0100_10_22.smt2 |    0.036s | 19.92MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0400_10_10.smt2 |    0.036s | 21.252MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0400_10_29.smt2 |    0.036s | 21.132MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_22.smt2 |    0.036s | 20.744MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_30.smt2 |    0.036s | 20.752MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_52.smt2 |    0.036s | 20.724MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_53.smt2 |    0.036s | 20.816MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_76.smt2 |    0.036s | 20.58MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_83.smt2 |    0.036s | 20.488MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_17.smt2 |    0.036s | 20.608MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_78.smt2 |    0.036s | 20.724MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_63.smt2 |    0.036s | 20.644MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_97.smt2 |    0.036s | 20.556MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_10.smt2 |    0.036s | 20.68MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_88.smt2 |    0.036s | 20.548MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_55.smt2 |    0.036s | 20.272MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_87.smt2 |    0.036s | 20.7MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.m0_true-unreach-call_drivers-staging-comedi-drivers-ni_pcidio-ko--107_1a--adbbc36.c.smt2 |    0.037s | 19.9MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.terminator_01_false-unreach-call_false-termination.i.smt2 |    0.037s | 19.66MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--staging--iio--iio_dummy_evgen.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.037s | 19.104MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0400_10_28.smt2 |    0.037s | 20.8MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0300_10_25.smt2 |    0.037s | 20.964MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0400_10_22.smt2 |    0.037s | 21.108MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0400_10_27.smt2 |    0.037s | 21.376MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0600_10_11.smt2 |    0.037s | 20.92MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0150_10_25.smt2 |    0.037s | 19.928MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0400_10_20.smt2 |    0.037s | 21.296MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0600_10_13.smt2 |    0.037s | 20.652MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0600_10_16.smt2 |    0.037s | 20.788MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0400_10_14.smt2 |    0.037s | 20.708MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0400_10_13.smt2 |    0.037s | 21.092MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_64.smt2 |    0.037s | 20.628MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_62.smt2 |    0.037s | 20.876MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_20.smt2 |    0.037s | 20.916MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_25.smt2 |    0.037s | 20.892MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_79.smt2 |    0.037s | 20.572MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_77.smt2 |    0.037s | 20.768MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_95.smt2 |    0.037s | 20.74MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_90.smt2 |    0.037s | 21.0MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_90.smt2 |    0.037s | 20.776MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_51.smt2 |    0.037s | 20.62MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_73.smt2 |    0.037s | 20.82MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_74.smt2 |    0.037s | 20.928MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_87.smt2 |    0.037s | 20.888MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_44.smt2 |    0.037s | 20.824MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--input--touchscreen--hampshire.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.038s | 19.168MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.sum01_bug02_sum01_bug02_base.case_false-unreach-call_true-termination.i.smt2 |    0.038s | 19.68MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.email_spec3_product13_false-unreach-call.cil.c.smt2 |    0.038s | 21.58MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0400_10_11.smt2 |    0.038s | 21.276MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0600_10_26.smt2 |    0.038s | 21.02MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0400_10_24.smt2 |    0.038s | 21.384MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0600_10_27.smt2 |    0.038s | 20.98MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0450_10_13.smt2 |    0.038s | 20.952MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0400_10_13.smt2 |    0.038s | 20.644MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0400_10_28.smt2 |    0.038s | 21.308MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0450_10_17.smt2 |    0.038s | 21.028MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_20.smt2 |    0.038s | 20.668MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_45.smt2 |    0.038s | 20.72MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_85.smt2 |    0.038s | 20.576MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_21.smt2 |    0.038s | 20.712MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_43.smt2 |    0.038s | 20.956MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_09.smt2 |    0.038s | 20.732MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_73.smt2 |    0.038s | 20.46MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_71.smt2 |    0.038s | 20.604MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_87.smt2 |    0.038s | 20.68MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_86.smt2 |    0.038s | 20.724MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_50.smt2 |    0.038s | 20.416MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_28.smt2 |    0.038s | 20.8MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_67.smt2 |    0.038s | 20.376MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_33.smt2 |    0.038s | 20.52MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_88.smt2 |    0.038s | 20.396MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_02.smt2 |    0.038s | 20.6MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_06.smt2 |    0.038s | 20.688MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--input--joystick--stinger.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.039s | 19.252MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_50.smt2 |    0.039s | 20.88MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_27.smt2 |    0.039s | 20.78MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_92.smt2 |    0.039s | 20.74MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_28.smt2 |    0.039s | 20.82MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_21.smt2 |    0.039s | 20.788MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_04.smt2 |    0.039s | 20.948MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_29.smt2 |    0.039s | 20.876MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_29.smt2 |    0.039s | 20.824MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_97.smt2 |    0.039s | 20.812MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_18.smt2 |    0.039s | 20.816MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_14.smt2 |    0.039s | 20.772MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_47.smt2 |    0.039s | 20.716MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_40.smt2 |    0.039s | 20.78MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_60.smt2 |    0.039s | 20.704MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0450_10_10.smt2 |    0.040s | 20.9MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0600_10_28.smt2 |    0.040s | 21.384MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0900_10_22.smt2 |    0.040s | 20.932MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0600_10_10.smt2 |    0.040s | 21.176MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0450_10_19.smt2 |    0.040s | 21.112MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0600_10_29.smt2 |    0.040s | 21.12MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_31.smt2 |    0.040s | 20.78MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_05.smt2 |    0.040s | 20.772MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_39.smt2 |    0.040s | 20.832MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_71.smt2 |    0.040s | 20.696MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_84.smt2 |    0.040s | 20.668MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_38.smt2 |    0.040s | 20.976MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_26.smt2 |    0.040s | 20.872MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_28.smt2 |    0.040s | 20.808MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_42.smt2 |    0.040s | 20.648MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_49.smt2 |    0.040s | 20.8MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_62.smt2 |    0.040s | 20.556MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_70.smt2 |    0.040s | 20.704MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_72.smt2 |    0.040s | 20.844MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_33.smt2 |    0.040s | 20.916MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_60.smt2 |    0.040s | 20.744MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_00.smt2 |    0.040s | 20.596MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.recursive_list.c_false-unreach-call.i.smt2 |    0.041s | 19.968MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0600_10_12.smt2 |    0.041s | 21.332MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0450_10_27.smt2 |    0.041s | 20.944MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0600_10_11.smt2 |    0.041s | 21.424MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0750_10_16.smt2 |    0.041s | 20.972MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_10_0200_10_20.smt2 |    0.041s | 19.924MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0600_10_24.smt2 |    0.041s | 20.98MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0450_10_28.smt2 |    0.041s | 21.072MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0600_10_29.smt2 |    0.041s | 21.2MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0400_10_17.smt2 |    0.041s | 21.296MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0900_10_23.smt2 |    0.041s | 20.932MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0450_10_18.smt2 |    0.041s | 20.948MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0600_10_16.smt2 |    0.041s | 21.152MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0600_10_21.smt2 |    0.041s | 21.148MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0600_10_13.smt2 |    0.041s | 21.312MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0400_10_12.smt2 |    0.041s | 21.192MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0900_10_16.smt2 |    0.041s | 20.984MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0900_10_21.smt2 |    0.041s | 21.032MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0750_10_21.smt2 |    0.041s | 20.872MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_65.smt2 |    0.041s | 20.784MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_00.smt2 |    0.041s | 20.664MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_75.smt2 |    0.041s | 20.74MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_30.smt2 |    0.041s | 20.784MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_10.smt2 |    0.041s | 20.868MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_29.smt2 |    0.041s | 20.752MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_07.smt2 |    0.041s | 20.776MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_16.smt2 |    0.041s | 20.772MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_88.smt2 |    0.041s | 21.116MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_45.smt2 |    0.041s | 20.696MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_13.smt2 |    0.041s | 20.764MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_98.smt2 |    0.041s | 20.68MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_94.smt2 |    0.041s | 20.816MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_11.smt2 |    0.041s | 20.884MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_52.smt2 |    0.041s | 20.744MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_95.smt2 |    0.041s | 20.932MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_30.smt2 |    0.041s | 20.74MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_94.smt2 |    0.041s | 20.844MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_49.smt2 |    0.041s | 20.656MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_81.smt2 |    0.041s | 20.812MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_48.smt2 |    0.041s | 20.74MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_44.smt2 |    0.041s | 20.656MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.email_spec3_product29_false-unreach-call.cil.c.smt2 |    0.042s | 22.06MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0300_10_13.smt2 |    0.042s | 20.856MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0600_10_10.smt2 |    0.042s | 21.212MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0600_10_21.smt2 |    0.042s | 20.94MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0750_10_11.smt2 |    0.042s | 20.992MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0900_10_24.smt2 |    0.042s | 21.1MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0600_10_26.smt2 |    0.042s | 21.32MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0900_10_19.smt2 |    0.042s | 20.912MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0900_10_28.smt2 |    0.042s | 20.948MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0450_10_22.smt2 |    0.042s | 21.156MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0900_10_15.smt2 |    0.042s | 20.916MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_08.smt2 |    0.042s | 20.836MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_55.smt2 |    0.042s | 20.936MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_76.smt2 |    0.042s | 20.82MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_20.smt2 |    0.042s | 20.82MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_35.smt2 |    0.042s | 20.74MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_02.smt2 |    0.042s | 20.92MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_19.smt2 |    0.042s | 20.752MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_37.smt2 |    0.042s | 20.84MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_32.smt2 |    0.042s | 20.9MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_54.smt2 |    0.042s | 20.816MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_68.smt2 |    0.042s | 20.96MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_85.smt2 |    0.042s | 20.704MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_11.smt2 |    0.042s | 20.84MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_82.smt2 |    0.042s | 20.872MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_06.smt2 |    0.042s | 20.864MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_84.smt2 |    0.042s | 20.548MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_75_45_09.smt2 |    0.042s | 20.276MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_41.smt2 |    0.042s | 20.828MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_95.smt2 |    0.042s | 20.808MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_03.smt2 |    0.042s | 20.808MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_66.smt2 |    0.042s | 20.86MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_58.smt2 |    0.042s | 20.888MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_61.smt2 |    0.042s | 20.828MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.cdaudio_simpl1_false-unreach-call_true-termination.cil.c.smt2 |    0.043s | 20.544MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0900_10_11.smt2 |    0.043s | 20.972MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0900_10_20.smt2 |    0.043s | 21.128MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0750_10_13.smt2 |    0.043s | 21.056MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0600_10_10.smt2 |    0.043s | 20.532MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0900_10_26.smt2 |    0.043s | 21.024MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0900_10_25.smt2 |    0.043s | 20.972MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0600_10_23.smt2 |    0.043s | 21.16MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0750_10_17.smt2 |    0.043s | 20.984MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0750_10_28.smt2 |    0.043s | 20.832MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0600_10_28.smt2 |    0.043s | 21.016MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0750_10_15.smt2 |    0.043s | 20.86MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0750_10_24.smt2 |    0.043s | 20.98MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0750_10_22.smt2 |    0.043s | 20.848MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0600_10_27.smt2 |    0.043s | 21.316MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0750_10_26.smt2 |    0.043s | 20.84MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_12.smt2 |    0.043s | 20.856MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_85.smt2 |    0.043s | 20.74MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_65.smt2 |    0.043s | 20.74MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_51.smt2 |    0.043s | 20.824MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_56.smt2 |    0.043s | 20.812MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_43.smt2 |    0.043s | 20.968MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_16.smt2 |    0.043s | 20.916MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_76.smt2 |    0.043s | 20.908MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_46.smt2 |    0.043s | 20.956MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_39.smt2 |    0.043s | 20.876MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_24.smt2 |    0.043s | 20.776MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_35.smt2 |    0.043s | 20.892MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_58.smt2 |    0.043s | 20.58MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_67.smt2 |    0.043s | 20.716MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_32.smt2 |    0.043s | 20.832MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_70.smt2 |    0.043s | 20.6MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_19.smt2 |    0.043s | 20.736MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_51.smt2 |    0.043s | 20.932MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_81.smt2 |    0.043s | 20.9MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_05.smt2 |    0.043s | 20.828MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.m0_true-unreach-call_sound-oss-opl3-ko--111_1a--42f9f8d.c.smt2 |    0.044s | 19.98MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0900_10_10.smt2 |    0.044s | 20.932MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0750_10_18.smt2 |    0.044s | 20.912MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0900_10_18.smt2 |    0.044s | 21.108MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0900_10_17.smt2 |    0.044s | 21.032MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0900_10_12.smt2 |    0.044s | 21.188MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0600_10_25.smt2 |    0.044s | 21.312MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_86.smt2 |    0.044s | 20.812MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_22.smt2 |    0.044s | 20.728MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_63.smt2 |    0.044s | 20.828MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_53.smt2 |    0.044s | 20.684MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_05.smt2 |    0.044s | 20.852MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_91.smt2 |    0.044s | 20.736MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_91.smt2 |    0.044s | 20.8MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_17.smt2 |    0.044s | 20.804MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_83.smt2 |    0.044s | 20.784MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_62.smt2 |    0.044s | 20.924MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_08.smt2 |    0.044s | 20.652MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_99.smt2 |    0.044s | 20.66MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_77.smt2 |    0.044s | 20.908MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_84.smt2 |    0.044s | 20.984MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_54.smt2 |    0.044s | 20.872MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_01.smt2 |    0.044s | 20.788MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_50.smt2 |    0.044s | 20.796MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_06.smt2 |    0.044s | 20.62MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_15.smt2 |    0.044s | 20.728MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_82.smt2 |    0.044s | 20.768MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_18.smt2 |    0.044s | 20.796MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0900_10_29.smt2 |    0.045s | 21.064MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0750_10_19.smt2 |    0.045s | 21.052MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0600_10_22.smt2 |    0.045s | 21.488MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0900_10_14.smt2 |    0.045s | 20.912MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0450_10_12.smt2 |    0.045s | 21.292MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_52.smt2 |    0.045s | 20.892MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_89.smt2 |    0.045s | 20.88MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_70.smt2 |    0.045s | 20.844MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_23.smt2 |    0.045s | 20.736MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_33.smt2 |    0.045s | 20.772MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_39.smt2 |    0.045s | 20.668MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_91.smt2 |    0.045s | 20.824MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_57.smt2 |    0.045s | 20.648MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_07.smt2 |    0.045s | 20.712MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_31.smt2 |    0.045s | 20.928MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_19.smt2 |    0.045s | 20.6MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_04.smt2 |    0.045s | 20.912MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_78.smt2 |    0.045s | 20.916MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_63.smt2 |    0.045s | 20.932MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_47.smt2 |    0.045s | 20.744MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_45.smt2 |    0.045s | 20.668MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0600_10_17.smt2 |    0.046s | 20.992MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0900_10_13.smt2 |    0.046s | 21.06MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0750_10_10.smt2 |    0.046s | 21.024MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0600_10_20.smt2 |    0.046s | 21.484MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0300_10_12.smt2 |    0.046s | 21.14MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_25.smt2 |    0.046s | 20.732MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_86.smt2 |    0.046s | 20.768MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_74.smt2 |    0.046s | 20.852MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_00.smt2 |    0.046s | 20.84MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_24.smt2 |    0.046s | 20.528MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_09.smt2 |    0.046s | 20.712MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_38.smt2 |    0.046s | 20.772MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_16.smt2 |    0.046s | 20.584MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_92.smt2 |    0.046s | 20.904MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_12.smt2 |    0.046s | 20.792MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_79.smt2 |    0.046s | 20.924MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_67.smt2 |    0.046s | 20.768MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_60.smt2 |    0.046s | 20.82MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_96.smt2 |    0.046s | 20.76MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_69.smt2 |    0.046s | 20.824MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.email_spec3_product17_false-unreach-call.cil.c.smt2 |    0.047s | 21.836MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0600_10_20.smt2 |    0.047s | 20.964MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0750_10_20.smt2 |    0.047s | 21.068MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0600_10_15.smt2 |    0.047s | 21.36MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0750_10_27.smt2 |    0.047s | 21.044MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0600_10_19.smt2 |    0.047s | 21.548MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_92.smt2 |    0.047s | 20.872MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_02.smt2 |    0.047s | 20.896MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_57.smt2 |    0.047s | 20.516MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_08.smt2 |    0.047s | 20.9MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_64.smt2 |    0.047s | 20.924MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_21.smt2 |    0.047s | 20.996MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_59.smt2 |    0.047s | 20.92MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_93.smt2 |    0.047s | 20.876MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_97.smt2 |    0.047s | 21.024MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_93.smt2 |    0.047s | 20.676MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-bmc-svcomp14/cpachecker-bmc.implicitfloatconversion_false-unreach-call.i.smt2 |    0.048s | 19.496MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--mfd--tps6105x.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.048s | 20.736MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0600_10_22.smt2 |    0.048s | 21.252MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1200_10_10.smt2 |    0.048s | 21.388MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_03.smt2 |    0.048s | 20.908MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_98.smt2 |    0.048s | 20.624MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_44.smt2 |    0.048s | 20.772MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_26.smt2 |    0.048s | 20.692MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_14.smt2 |    0.048s | 20.884MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_53.smt2 |    0.048s | 20.992MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_89.smt2 |    0.048s | 20.648MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_98.smt2 |    0.048s | 20.86MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_36.smt2 |    0.048s | 20.68MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_41.smt2 |    0.048s | 20.956MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_56.smt2 |    0.048s | 20.636MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_43.smt2 |    0.048s | 20.6MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_55.smt2 |    0.048s | 20.968MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0800_10_11.smt2 |    0.049s | 21.364MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0900_10_27.smt2 |    0.049s | 20.868MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0750_10_23.smt2 |    0.049s | 21.02MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_93.smt2 |    0.049s | 20.864MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.list_search_false-unreach-call.i.smt2 |    0.050s | 20.284MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.array_true-unreach-call.i.smt2 |    0.050s | 19.788MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem01_00_true-unreach-call.c.smt2 |    0.050s | 24.492MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0800_10_15.smt2 |    0.050s | 21.492MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0750_10_25.smt2 |    0.050s | 20.852MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0800_10_25.smt2 |    0.050s | 21.408MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0750_10_29.smt2 |    0.050s | 21.048MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1000_10_23.smt2 |    0.050s | 21.288MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1000_10_10.smt2 |    0.050s | 21.332MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0400_10_25.smt2 |    0.050s | 20.656MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0600_10_19.smt2 |    0.050s | 20.576MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_89.smt2 |    0.050s | 20.932MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_34.smt2 |    0.050s | 20.924MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_150_30_80.smt2 |    0.050s | 20.972MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.test_locks_9_true-unreach-call.c.smt2 |    0.051s | 20.248MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0800_10_28.smt2 |    0.051s | 21.584MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_80.smt2 |    0.051s | 21.008MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_01.smt2 |    0.051s | 20.536MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.email_spec3_product19_false-unreach-call.cil.c.smt2 |    0.052s | 21.636MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0600_10_13.smt2 |    0.052s | 20.876MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0600_10_19.smt2 |    0.052s | 21.212MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1200_10_21.smt2 |    0.052s | 21.384MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0800_10_12.smt2 |    0.052s | 21.436MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0500_10_26.smt2 |    0.052s | 20.532MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_37.smt2 |    0.052s | 21.008MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_83.smt2 |    0.052s | 20.868MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_15.smt2 |    0.052s | 20.852MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_01.smt2 |    0.052s | 20.832MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1200_10_11.smt2 |    0.053s | 21.324MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1000_10_16.smt2 |    0.053s | 21.32MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1200_10_23.smt2 |    0.053s | 21.312MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_79.smt2 |    0.053s | 21.04MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_24.smt2 |    0.053s | 20.84MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1200_10_15.smt2 |    0.054s | 21.32MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1200_10_25.smt2 |    0.054s | 21.38MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1200_10_29.smt2 |    0.054s | 21.384MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0600_10_15.smt2 |    0.054s | 20.844MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1000_10_22.smt2 |    0.054s | 21.308MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_72.smt2 |    0.054s | 20.992MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_99.smt2 |    0.054s | 20.8MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1200_10_22.smt2 |    0.055s | 21.52MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1200_10_27.smt2 |    0.055s | 21.444MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_68.smt2 |    0.055s | 20.812MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_56.smt2 |    0.055s | 21.072MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_35.smt2 |    0.055s | 20.784MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_34.smt2 |    0.055s | 21.028MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.email_spec3_product35_false-unreach-call.cil.c.smt2 |    0.056s | 22.264MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1200_10_16.smt2 |    0.056s | 21.308MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0600_10_18.smt2 |    0.056s | 21.044MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1200_10_19.smt2 |    0.056s | 21.624MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_46.smt2 |    0.056s | 20.948MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_59.smt2 |    0.056s | 21.068MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_30_60_45_38.smt2 |    0.056s | 20.448MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1000_10_29.smt2 |    0.057s | 21.368MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1200_10_28.smt2 |    0.057s | 21.304MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_13.smt2 |    0.057s | 21.124MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1000_10_15.smt2 |    0.058s | 21.204MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_20_0400_10_19.smt2 |    0.058s | 20.596MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1200_10_13.smt2 |    0.059s | 21.412MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1000_10_13.smt2 |    0.059s | 21.392MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1000_10_25.smt2 |    0.059s | 21.236MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_09.smt2 |    0.059s | 20.944MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1200_10_24.smt2 |    0.060s | 21.44MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1000_10_11.smt2 |    0.060s | 21.268MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1200_10_20.smt2 |    0.060s | 21.488MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.email_spec3_product33_false-unreach-call.cil.c.smt2 |    0.062s | 22.096MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1000_10_19.smt2 |    0.062s | 21.384MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0400_10_19.smt2 |    0.062s | 21.56MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_48.smt2 |    0.062s | 20.764MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1000_10_21.smt2 |    0.063s | 21.384MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1200_10_26.smt2 |    0.063s | 21.384MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0400_10_18.smt2 |    0.063s | 21.592MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_96.smt2 |    0.063s | 20.948MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_11.smt2 |    0.063s | 20.644MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_36.smt2 |    0.063s | 20.944MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1200_10_17.smt2 |    0.064s | 21.468MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_07.smt2 |    0.064s | 20.848MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_73.smt2 |    0.064s | 21.056MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1200_10_18.smt2 |    0.065s | 21.756MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1000_10_26.smt2 |    0.065s | 21.212MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1000_10_24.smt2 |    0.069s | 21.512MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0800_10_27.smt2 |    0.069s | 21.748MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1000_10_20.smt2 |    0.069s | 21.356MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1000_10_18.smt2 |    0.070s | 21.644MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--input--joystick--turbografx.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.071s | 24.472MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0400_10_23.smt2 |    0.072s | 21.62MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0600_10_24.smt2 |    0.072s | 21.676MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0800_10_29.smt2 |    0.072s | 21.584MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0750_10_12.smt2 |    0.074s | 21.164MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_17.smt2 |    0.074s | 20.9MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0750_10_14.smt2 |    0.076s | 21.14MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1000_10_27.smt2 |    0.078s | 21.552MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1200_10_14.smt2 |    0.078s | 21.552MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1000_10_17.smt2 |    0.079s | 21.648MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1200_10_12.smt2 |    0.079s | 21.412MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0800_10_19.smt2 |    0.079s | 21.756MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_13.smt2 |    0.080s | 20.66MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_71.smt2 |    0.080s | 20.744MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0800_10_26.smt2 |    0.081s | 21.468MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0600_10_14.smt2 |    0.081s | 21.452MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--misc--sgi-xp--xp.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.082s | 21.024MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0800_10_22.smt2 |    0.082s | 21.732MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0600_10_18.smt2 |    0.082s | 21.76MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_78.smt2 |    0.082s | 21.152MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_27.smt2 |    0.082s | 21.12MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_61.smt2 |    0.083s | 20.896MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_58.smt2 |    0.083s | 20.844MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0800_10_10.smt2 |    0.085s | 21.356MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_23.smt2 |    0.087s | 20.708MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0600_10_25.smt2 |    0.092s | 21.232MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0800_10_20.smt2 |    0.093s | 21.804MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_75.smt2 |    0.094s | 21.308MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0800_10_17.smt2 |    0.098s | 21.796MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--usb--storage--ums-datafab.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.104s | 21.552MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_30_0600_10_12.smt2 |    0.108s | 21.468MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0800_10_21.smt2 |    0.109s | 21.728MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--usb--storage--ums-alauda.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.111s | 21.664MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--usb--storage--ums-freecom.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.114s | 21.428MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--usb--storage--ums-jumpshot.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.120s | 21.412MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0600_10_17.smt2 |    0.120s | 21.676MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1000_10_28.smt2 |    0.123s | 21.528MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--usb--storage--ums-sddr55.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.128s | 21.584MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0600_10_23.smt2 |    0.128s | 21.812MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0800_10_13.smt2 |    0.129s | 21.432MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--usb--storage--ums-usbat.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.130s | 21.424MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_57.smt2 |    0.146s | 21.324MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0400_10_14.smt2 |    0.152s | 21.644MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0800_10_16.smt2 |    0.154s | 21.984MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1000_10_12.smt2 |    0.158s | 21.792MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_50_100_30_69.smt2 |    0.159s | 21.38MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0800_10_23.smt2 |    0.163s | 21.472MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_1000_10_14.smt2 |    0.165s | 21.796MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomDecoupled/pb_real_40_80_60_04.smt2 |    0.174s | 20.796MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.s3_srvr.blast.10_false-unreach-call.i.cil.c.smt2 |    0.278s | 21.568MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product02_false-unreach-call.cil.c.smt2 |    0.279s | 21.308MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.s3_srvr.blast.08_false-unreach-call.i.cil.c.smt2 |    0.280s | 21.74MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.s3_srvr.blast.15_false-unreach-call.i.cil.c.smt2 |    0.282s | 21.612MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product09_false-unreach-call.cil.c.smt2 |    0.288s | 21.332MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product05_false-unreach-call.cil.c.smt2 |    0.289s | 21.676MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec2_product46_true-unreach-call.cil.c.smt2 |    0.290s | 22.228MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product14_false-unreach-call.cil.c.smt2 |    0.293s | 21.672MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product13_false-unreach-call.cil.c.smt2 |    0.294s | 21.716MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product29_false-unreach-call.cil.c.smt2 |    0.297s | 22.588MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec1_product46_true-unreach-call.cil.c.smt2 |    0.297s | 22.432MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product21_false-unreach-call.cil.c.smt2 |    0.302s | 22.372MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec2_product53_true-unreach-call.cil.c.smt2 |    0.304s | 22.388MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec2_product37_true-unreach-call.cil.c.smt2 |    0.304s | 21.84MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.module_get_put-drivers-block-drbd-drbd.ko_false-unreach-call.cil.out.i.pp.i.smt2 |    0.306s | 20.816MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--video--backlight--lms283gf05.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.310s | 22.308MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product01_false-unreach-call.cil.c.smt2 |    0.312s | 21.064MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product18_false-unreach-call.cil.c.smt2 |    0.313s | 21.576MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0800_10_24.smt2 |    0.314s | 22.328MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product10_false-unreach-call.cil.c.smt2 |    0.316s | 21.328MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product06_false-unreach-call.cil.c.smt2 |    0.316s | 21.688MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec1_product33_false-unreach-call.cil.c.smt2 |    0.316s | 21.504MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product52_false-unreach-call.cil.c.smt2 |    0.319s | 22.728MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec2_product38_true-unreach-call.cil.c.smt2 |    0.319s | 21.892MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product26_false-unreach-call.cil.c.smt2 |    0.320s | 21.588MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec4_product49_true-unreach-call.cil.c.smt2 |    0.322s | 22.168MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--media--common--tuners--tda18218.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.322s | 21.548MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product35_false-unreach-call.cil.c.smt2 |    0.323s | 21.892MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product51_false-unreach-call.cil.c.smt2 |    0.324s | 22.52MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product12_false-unreach-call.cil.c.smt2 |    0.324s | 21.32MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product37_true-unreach-call.cil.c.smt2 |    0.324s | 22.28MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product19_false-unreach-call.cil.c.smt2 |    0.326s | 21.592MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec4_product58_true-unreach-call.cil.c.smt2 |    0.327s | 22.368MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product08_false-unreach-call.cil.c.smt2 |    0.327s | 21.796MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec1_product38_false-unreach-call.cil.c.smt2 |    0.328s | 22.452MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec1_product35_false-unreach-call.cil.c.smt2 |    0.328s | 21.604MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec4_product53_true-unreach-call.cil.c.smt2 |    0.329s | 22.464MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec2_product45_true-unreach-call.cil.c.smt2 |    0.329s | 22.248MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec4_product54_true-unreach-call.cil.c.smt2 |    0.330s | 22.52MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product27_false-unreach-call.cil.c.smt2 |    0.330s | 21.588MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec1_product52_false-unreach-call.cil.c.smt2 |    0.330s | 22.48MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec1_product49_false-unreach-call.cil.c.smt2 |    0.331s | 22.424MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec1_product56_false-unreach-call.cil.c.smt2 |    0.331s | 23.44MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.sll_to_dll_rev_false-unreach-call.i.smt2 |    0.331s | 22.316MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec1_product34_false-unreach-call.cil.c.smt2 |    0.332s | 21.532MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product36_false-unreach-call.cil.c.smt2 |    0.333s | 21.908MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec1_product37_false-unreach-call.cil.c.smt2 |    0.334s | 22.452MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec4_product50_true-unreach-call.cil.c.smt2 |    0.335s | 22.16MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec1_product51_false-unreach-call.cil.c.smt2 |    0.336s | 22.496MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product16_false-unreach-call.cil.c.smt2 |    0.336s | 21.804MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec1_product39_false-unreach-call.cil.c.smt2 |    0.337s | 22.464MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec2_product61_true-unreach-call.cil.c.smt2 |    0.338s | 22.768MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--input--joystick--twidjoy.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.338s | 22.512MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec1_product40_false-unreach-call.cil.c.smt2 |    0.339s | 22.504MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec4_product57_true-unreach-call.cil.c.smt2 |    0.341s | 22.372MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product34_true-unreach-call.cil.c.smt2 |    0.341s | 21.612MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--media--video--gspca--gspca_finepix.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.342s | 23.92MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec4_product61_true-unreach-call.cil.c.smt2 |    0.343s | 22.856MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--media--common--tuners--mt2060.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.345s | 23.52MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product22_false-unreach-call.cil.c.smt2 |    0.345s | 22.444MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec2_product54_true-unreach-call.cil.c.smt2 |    0.345s | 22.5MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product33_true-unreach-call.cil.c.smt2 |    0.346s | 21.616MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec1_product50_false-unreach-call.cil.c.smt2 |    0.346s | 22.424MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product38_true-unreach-call.cil.c.smt2 |    0.348s | 22.288MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--input--misc--apanel.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.349s | 24.316MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product46_true-unreach-call.cil.c.smt2 |    0.350s | 22.456MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec1_product36_false-unreach-call.cil.c.smt2 |    0.351s | 21.564MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.s3_srvr.blast.12_false-unreach-call.i.cil.c.smt2 |    0.352s | 21.556MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec4_product62_true-unreach-call.cil.c.smt2 |    0.353s | 23.108MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec1_product54_false-unreach-call.cil.c.smt2 |    0.353s | 23.356MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--media--dvb--dvb-usb--dvb-usb-au6610.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.353s | 23.656MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product30_false-unreach-call.cil.c.smt2 |    0.354s | 22.38MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product42_true-unreach-call.cil.c.smt2 |    0.354s | 21.756MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.s3_srvr.blast.06_false-unreach-call.i.cil.c.smt2 |    0.355s | 21.692MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product11_false-unreach-call.cil.c.smt2 |    0.356s | 21.296MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.s3_srvr.blast.14_false-unreach-call.i.cil.c.smt2 |    0.358s | 21.628MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--input--keyboard--stowaway.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.358s | 23.216MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.s3_srvr.blast.16_false-unreach-call.i.cil.c.smt2 |    0.358s | 21.66MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec1_product64_true-unreach-call.cil.c.smt2 |    0.358s | 23.812MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product04_false-unreach-call.cil.c.smt2 |    0.358s | 21.328MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product17_false-unreach-call.cil.c.smt2 |    0.358s | 21.56MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product25_false-unreach-call.cil.c.smt2 |    0.360s | 21.544MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--media--dvb--dvb-usb--dvb-usb-gl861.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.362s | 23.644MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product41_true-unreach-call.cil.c.smt2 |    0.363s | 21.756MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.main1_true-unreach-call_drivers-media-video-tlg2300-poseidon-ko--32_7a--4a349aa-1.c.smt2 |    0.364s | 22.256MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product03_false-unreach-call.cil.c.smt2 |    0.365s | 21.296MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--media--dvb--dvb-usb--dvb-usb-nova-t-usb2.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.366s | 23.624MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product23_false-unreach-call.cil.c.smt2 |    0.368s | 22.46MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.main1_true-unreach-call_drivers-media-video-tlg2300-poseidon-ko--32_7a--4a349aa.c.smt2 |    0.370s | 21.992MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product32_false-unreach-call.cil.c.smt2 |    0.376s | 22.496MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--media--dvb--dvb-usb--dvb-usb-ce6230.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.376s | 23.556MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--media--dvb--dvb-usb--dvb-usb-vp7045.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.377s | 23.804MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product07_false-unreach-call.cil.c.smt2 |    0.378s | 21.796MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--input--keyboard--xtkbd.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.378s | 23.332MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product15_false-unreach-call.cil.c.smt2 |    0.379s | 21.844MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--xen--xenfs--xenfs.ko-ldv_main1_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.379s | 22.716MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec1_product63_true-unreach-call.cil.c.smt2 |    0.379s | 23.816MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec1_product55_false-unreach-call.cil.c.smt2 |    0.382s | 23.42MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem14_60_false-unreach-call.c.smt2 |    0.386s | 25.944MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product31_false-unreach-call.cil.c.smt2 |    0.388s | 22.48MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--input--keyboard--newtonkbd.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.391s | 23.68MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--video--backlight--vgg2432a4.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.391s | 23.568MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--mtd--ubi--gluebi.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.392s | 25.592MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--input--joystick--magellan.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.395s | 24.752MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--staging--iio--addac--adt7316-spi.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.401s | 24.236MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_7a_cilled_true-unreach-call_linux-3.8-rc1-drivers--regulator--isl6271a-regulator.ko-main.cil.out.c.smt2 |    0.402s | 23.096MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product24_false-unreach-call.cil.c.smt2 |    0.402s | 22.504MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--media--video--gspca--gspca_sq905c.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.404s | 24.612MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--staging--telephony--ixj_pcmcia.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.416s | 26.276MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec2_product63_true-unreach-call.cil.c.smt2 |    0.422s | 23.348MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec1_product53_false-unreach-call.cil.c.smt2 |    0.426s | 23.108MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--mtd--mtdoops.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.433s | 27.56MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--input--joystick--spaceorb.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.435s | 28.048MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--misc--vmw_balloon.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.440s | 24.344MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0800_10_18.smt2 |    0.443s | 22.212MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--input--touchscreen--egalax_ts.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.444s | 26.396MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--input--joystick--magellan.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.448s | 25.972MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product20_false-unreach-call.cil.c.smt2 |    0.452s | 21.576MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--usb--storage--ums-sddr09.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.462s | 22.672MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--media--video--gspca--gspca_stv0680.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.469s | 23.176MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec2_product62_true-unreach-call.cil.c.smt2 |    0.472s | 22.884MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product28_false-unreach-call.cil.c.smt2 |    0.472s | 21.652MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--media--video--gspca--gspca_pac207.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.477s | 27.388MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--input--touchscreen--cyttsp_spi.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.523s | 26.992MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--media--common--tuners--mt2131.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.556s | 28.708MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product43_false-unreach-call.cil.c.smt2 |    0.561s | 22.464MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec1_product45_true-unreach-call.cil.c.smt2 |    0.561s | 22.456MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product44_false-unreach-call.cil.c.smt2 |    0.567s | 22.472MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--watchdog--cpu5wdt.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.569s | 23.784MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_7_cilled_false-unreach-call_const_ok_linux-32_1-drivers--usb--image--microtek.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.571s | 31.396MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-bmc-svcomp14/cpachecker-bmc.diskperf_false-unreach-call.i.cil.c.smt2 |    0.581s | 28.616MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec1_product61_true-unreach-call.cil.c.smt2 |    0.592s | 23.236MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--regulator--max1586.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.600s | 32.584MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_7a_cilled_true-unreach-call_linux-3.8-rc1-drivers--misc--phantom.ko-main.cil.out.c.smt2 |    0.617s | 34.868MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--staging--speakup--speakup_soft.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.621s | 32.244MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--input--touchscreen--ad7879-spi.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.638s | 26.54MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--media--dvb--dvb-usb--dvb-usb-rtl28xxu.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.638s | 30.796MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec1_product62_true-unreach-call.cil.c.smt2 |    0.655s | 23.328MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--media--video--videobuf-vmalloc.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.675s | 29.932MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--scsi--dmx3191d.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.686s | 25.524MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product39_false-unreach-call.cil.c.smt2 |    0.693s | 22.872MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem02_00_true-unreach-call.c.smt2 |    0.694s | 24.992MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--mtd--ubi--gluebi.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.702s | 33.228MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product40_false-unreach-call.cil.c.smt2 |    0.721s | 22.888MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem01_30_true-unreach-call.c.smt2 |    0.737s | 25.196MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-bmc-svcomp14/cpachecker-bmc.parport_false-unreach-call.i.cil.c.smt2 |    0.744s | 35.776MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--w1--slaves--w1_ds2431.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.777s | 35.824MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product47_false-unreach-call.cil.c.smt2 |    0.786s | 23.056MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product48_false-unreach-call.cil.c.smt2 |    0.792s | 23.112MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product45_true-unreach-call.cil.c.smt2 |    0.814s | 22.64MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--tty--serial--altera_jtaguart.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.860s | 30.24MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.pc_sfifo_2_false-unreach-call_false-termination.cil.c.smt2 |    0.876s | 21.916MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.diskperf_true-unreach-call.i.cil.c.smt2 |    0.900s | 25.376MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product59_false-unreach-call.cil.c.smt2 |    0.915s | 23.352MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product56_false-unreach-call.cil.c.smt2 |    0.936s | 23.364MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--media--dvb--dvb-usb--dvb-usb-az6027.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.939s | 31.404MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.pc_sfifo_3_true-unreach-call_false-termination.cil.c.smt2 |    0.940s | 21.336MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product60_false-unreach-call.cil.c.smt2 |    0.958s | 23.312MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--media--common--tuners--mxl5007t.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    0.990s | 37.704MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product55_false-unreach-call.cil.c.smt2 |    0.993s | 23.392MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem14_00_true-unreach-call.c.smt2 |    1.013s | 24.624MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem18_60_false-unreach-call.c.smt2 |    1.017s | 48.064MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--leds--leds-lp5521.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    1.017s | 32.884MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product64_false-unreach-call.cil.c.smt2 |    1.030s | 23.96MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--rtc--rtc-r9701.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    1.032s | 36.384MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-bmc-svcomp14/cpachecker-bmc.floppy_false-unreach-call.i.cil.c.smt2 |    1.039s | 35.44MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.minepump_spec3_product63_false-unreach-call.cil.c.smt2 |    1.040s | 23.844MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.ddv_machzwd_inw_false-unreach-call.i.smt2 |    1.045s | 44.82MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.email_spec27_product13_true-unreach-call.cil.c.smt2 |    1.051s | 27.628MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem01_40_true-unreach-call.c.smt2 |    1.072s | 25.18MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--leds--leds-bd2802.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    1.084s | 37.076MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.main3_false-unreach-call_drivers-staging-usbip-vhci-hcd-ko--132_1a--927c3fa.c.smt2 |    1.132s | 41.376MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem02_30_true-unreach-call.c.smt2 |    1.140s | 25.06MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.main3_true-unreach-call_drivers-staging-usbip-vhci-hcd-ko--132_1a--927c3fa-1.c.smt2 |    1.162s | 38.056MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem03_60_false-unreach-call.c.smt2 |    1.163s | 46.304MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem10_20_true-unreach-call.c.smt2 |    1.173s | 23.452MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem14_20_true-unreach-call.c.smt2 |    1.173s | 24.704MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem10_00_true-unreach-call.c.smt2 |    1.173s | 23.484MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0800_10_14.smt2 |    1.192s | 21.984MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem14_30_true-unreach-call.c.smt2 |    1.199s | 24.724MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem02_40_true-unreach-call.c.smt2 |    1.227s | 25.036MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem10_10_true-unreach-call.c.smt2 |    1.237s | 23.408MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--media--dvb--frontends--tda18271c2dd.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    1.289s | 48.944MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.email_spec8_product14_true-unreach-call.cil.c.smt2 |    1.303s | 34.896MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.m0_false-unreach-call_sound-oss-opl3-ko--111_1a--42f9f8d-1.c.smt2 |    1.306s | 68.148MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem15_60_false-unreach-call.c.smt2 |    1.341s | 35.672MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem01_10_true-unreach-call.c.smt2 |    1.345s | 25.072MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--media--common--tuners--max2165.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    1.353s | 28.528MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/mathsat/RandomCoupled/pb_real_40_0600_10_14.smt2 |    1.439s | 21.892MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--rtc--rtc-m41t94.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    1.519s | 26.232MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem03_10_true-unreach-call.c.smt2 |    1.522s | 41.44MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem02_10_true-unreach-call.c.smt2 |    1.546s | 24.928MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem01_60_false-unreach-call.c.smt2 |    1.576s | 26.528MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.ddv_machzwd_outb_false-unreach-call.i.smt2 |    1.581s | 53.0MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem17_60_false-unreach-call.c.smt2 |    1.622s | 47.456MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem03_40_true-unreach-call.c.smt2 |    1.639s | 41.184MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem12_60_false-unreach-call.c.smt2 |    1.644s | 69.32MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem16_60_false-unreach-call.c.smt2 |    1.649s | 33.72MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem16_50_true-unreach-call.c.smt2 |    1.665s | 30.796MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem13_60_false-unreach-call.c.smt2 |    1.666s | 76.824MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.rekh_ctm_true-unreach-call.2.c.smt2 |    1.692s | 181.0MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--power--wm831x_power.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    1.786s | 66.112MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem03_30_true-unreach-call.c.smt2 |    1.863s | 41.428MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem04_30_true-unreach-call.c.smt2 |    2.068s | 115.0MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.linux-3.8-rc1-32_7a-drivers--hwmon--w83793.ko-ldv_main0_true-unreach-call.cil.out.c.smt2 |    2.112s | 51.656MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem04_20_true-unreach-call.c.smt2 |    2.262s | 115.0MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--watchdog--wm831x_wdt.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    2.263s | 31.996MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_7a_cilled_true-unreach-call_linux-3.8-rc1-32_7a-drivers--infiniband--hw--cxgb3--iw_cxgb3.ko-ldv_main6_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    2.418s | 72.376MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem06_60_false-unreach-call.c.smt2 |    2.703s | 241.0MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.rekh_ctm_true-unreach-call.1.c.smt2 |    2.733s | 228.0MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.rekh_ctm_false-unreach-call.2.c.smt2 |    2.763s | 222.0MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem05_60_false-unreach-call.c.smt2 |    2.880s | 242.0MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem04_50_true-unreach-call.c.smt2 |    2.905s | 115.0MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem04_10_true-unreach-call.c.smt2 |    2.917s | 115.0MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem04_60_false-unreach-call.c.smt2 |    3.094s | 126.0MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_7a_cilled_true-unreach-call_linux-3.8-rc1-drivers--infiniband--hw-cxgb3--iw_cxgb3.ko-main.cil.out.c.smt2 |    3.205s | 87.172MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_1_cilled_true-unreach-call_ok_nondet_linux-3.4-32_1-drivers--media--common--tuners--mt20xx.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    3.393s | 69.544MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.m0_true-unreach-call_drivers-net-slip-ko--108_1a--1b0b0ac.c.smt2 |    3.478s | 153.0MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_7a_cilled_true-unreach-call_linux-3.8-rc1-32_7a-drivers--platform--x86--samsung-laptop.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    3.480s | 121.0MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem19_60_false-unreach-call.c.smt2 |    3.502s | 125.0MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_7_cilled_false-unreach-call_const_ok_linux-32_1-drivers--media--video--mem2mem_testdev.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    3.549s | 67.188MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.module_get_put-drivers-bluetooth-btmrvl.ko_true-unreach-call.cil.out.i.pp.i.smt2 |    3.743s | 171.0MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem04_00_true-unreach-call.c.smt2 |    3.857s | 115.0MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.linux-3.8-rc1-32_7a-drivers--input--misc--uinput.ko-ldv_main0_true-unreach-call.cil.out.c.smt2 |    4.078s | 72.132MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_7a_cilled_true-unreach-call_linux-3.8-rc1-32_7a-drivers--net--usb--hso.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |    4.605s | 120.0MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.cs_stack_true-unreach-call.i.smt2 |    6.021s | 751.0MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.cs_stack_false-unreach-call.i.smt2 |    6.186s | 762.0MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.email_spec3_productSimulator_false-unreach-call.cil.c.smt2 |   14.110s | 318.0MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem07_50_true-unreach-call.c.smt2 |   17.252s | 1699.0MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem07_00_true-unreach-call.c.smt2 |   17.773s | 1699.0MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem07_10_true-unreach-call.c.smt2 |   19.422s | 1699.0MiB| unsat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem07_60_false-unreach-call.c.smt2 |   20.564s | 1865.0MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.cs_fib_true-unreach-call.i.smt2 |   21.789s | 2949.0MiB| sat | 0 |  |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.43_1a_cilled_true-unreach-call_ok_nondet_linux-43_1a-drivers--usb--serial--mos7840.ko-ldv_main0_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |   30.014s | 199.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40844261236-1-run-all-NnKHxR |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.32_7_cilled_false-unreach-call_const_ok_linux-32_1-drivers--vhost--vhost_net.ko-ldv_main1_sequence_infinite_withcheck_stateful.cil.out.c.smt2 |   30.023s | 500.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40844261236-1-run-all-NnKHxR |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.email_spec27_productSimulator_false-unreach-call.cil.c.smt2 |   30.028s | 575.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40844261236-1-run-all-NnKHxR |
|non-incremental/QF_UFLRA/cpachecker-bmc-svcomp14/cpachecker-bmc.rekh_ctm_true-unreach-call.3.c.smt2 |   30.067s | 1038.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40844261236-1-run-all-NnKHxR |
|non-incremental/QF_UFLRA/cpachecker-bmc-svcomp14/cpachecker-bmc.rekcba_ctm_true-unreach-call.1.c.smt2 |   30.112s | 1879.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40844261236-1-run-all-NnKHxR |
|non-incremental/QF_UFLRA/cpachecker-bmc-svcomp14/cpachecker-bmc.rekh_ctm_true-unreach-call.4.c.smt2 |   30.114s | 1742.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40844261236-1-run-all-NnKHxR |
|non-incremental/QF_UFLRA/cpachecker-bmc-svcomp14/cpachecker-bmc.rekcba_ctm_false-unreach-call.2.c.smt2 |   30.127s | 1869.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40844261236-1-run-all-NnKHxR |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.Problem08_60_false-unreach-call.c.smt2 |   30.131s | 3384.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40844261236-1-run-all-NnKHxR |
|non-incremental/QF_UFLRA/cpachecker-bmc-svcomp14/cpachecker-bmc.rekcba_ctm_true-unreach-call.2.c.smt2 |   30.132s | 1872.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40844261236-1-run-all-NnKHxR |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.cs_fib_longer_true-unreach-call.i.smt2 |   30.143s | 3508.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40844261236-1-run-all-NnKHxR |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.cs_fib_longer_false-unreach-call.i.smt2 |   30.146s | 3508.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40844261236-1-run-all-NnKHxR |
|non-incremental/QF_UFLRA/cpachecker-bmc-svcomp14/cpachecker-bmc.rekcba_ctm_false-unreach-call.3.c.smt2 |   30.188s | 3490.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40844261236-1-run-all-NnKHxR |
|non-incremental/QF_UFLRA/cpachecker-bmc-svcomp14/cpachecker-bmc.rekcba_ctm_true-unreach-call.3.c.smt2 |   30.188s | 3490.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40844261236-1-run-all-NnKHxR |
|non-incremental/QF_UFLRA/cpachecker-bmc-svcomp14/cpachecker-bmc.rekcba_ctm_true-unreach-call.4.c.smt2 |   30.218s | 3765.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40844261236-1-run-all-NnKHxR |
|non-incremental/QF_UFLRA/cpachecker-induction-svcomp14/cpachecker-induction.cs_fib_false-unreach-call.i.smt2 |   30.231s | 3980.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40844261236-1-run-all-NnKHxR |
