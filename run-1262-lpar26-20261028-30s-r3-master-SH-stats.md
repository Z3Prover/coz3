# .

* SAT 0
* UNSAT 220
* TIMEOUT 0
* UNKNOWN 0

* ERRORS 294 (signal-15:294)

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-10-28 22:27:03 UTC
Job description: Pinned full public LPAR cohort; SH; master; 30s solver and Ramon limits; four independent solver workers; not the paper's filtered cohort or four-core portfolio
Job tag: lpar26-20261028-30s-r3-master-SH
Runner: rise-runner-1
Z3 repo: Z3Prover/z3
Z3 commit: 2b25abc65ca59569450f56ea1c988638083cb800
Z3 branch: master
Z3 options: "-T:30 -st"
Ramon options: "--limit-time 30"
Input preset: alasca-lpar-2026
Z3 inputs: https://github.com/vprover/vampire_benchmarks/archive/6563baa75aea885cee14db5d6dfcff1cf3b50a1c.tar.gz alasca-lpar-2026/SH
Z3 commit message: Prove propagation lemmas as tactic scripts, trim clause logs from 4 KB, and fix native proof trimmer conflicts (#11424)

## Summary

Submit the five unmerged native-certificate/Lean commits from fork
iterations
37 to 41, preserved as separate commits for review. Four are Python,
Lean, and documentation only; the fifth fixes three bugs in the native
proof trimmer (`src/sat/sat_proof_trim.cpp` and the trimmer command in
`proof_cmds.cpp`). Ordinary solver search code is not modified.

Together they close the Lean completeness gaps left by the z3test
`smtlib-sample` scans (a QF_LRA clock synchronization file, a QF_IDL
bounded
model checking circuit, the QF_UF `NEQ004_size4` and `cambridge` files),
make
every clause log of 4 KB and up go through the native trimmer before
replay,
and make the trimmer succeed on the logs that used to make it fall back:
on a scan of 133 random logs above 4 KB, trimmer fallbacks went from 43
to 0.

This is a snapshot branch of `levnach/z3`
(`lean-certificates-20261028`):
the fork's `master` at `213bdc320` (upstream `02f6a7e14`, #11370, plus
fork
PRs 37 to 40) plus fork PR 41 squashed to one commit. The tip's tree is
identical to the head of fork PR 41 at `d12b33842`, the commit its CI
ran
on; the fork bot's later F* audit commit, which only adds files under
`fstar/audits/pr-41/`, is left out. A read-only merge preview against
the
current upstream `master` (`668d59795`) is conflict-free, and no
upstream
commit since the base touches any of the 24 files changed here.

## Included changes

| Fork PR | Change | Report |
| --- | --- | --- |
| levnach/z3#37 | Normalized rewrites proved by propagation over
let-named structures |
[Report](https://levnach.github.io/z3/reports/pr-37.html) |
| levnach/z3#38 | Propagation proofs emitted as tactic scripts over
named gates; interned replay keys |
[Report](https://levnach.github.io/z3/reports/pr-38.html) |
| levnach/z3#39 | Trimmed QF_UF clause logs accepted; assumption search
limited to conjuncts sharing a symbol |
[Report](https://levnach.github.io/z3/reports/pr-39.html) |
| levnach/z3#40 | Clause logs trimmed from 4 KB instead of 1 MB; Lean
memory capped at 8 GB |
[Report](https://levnach.github.io/z3/reports/pr-40.html) |
| levnach/z3#41 | Three native proof trimmer bugs fixed (missed
conflicts, empty assumptions, hints without a checker) |
[Report](https://levnach.github.io/z3/reports/pr-41.html) |

24 files, 1,878 insertions and 313 deletions. The C++ part is 160
insertions
and 11 deletions across six files, 101 of them a new native regression
test in `src/test/proof_replay.cpp`. The rest is Python reconstruction
code, its
tests, two SMT-LIB test inputs, and the plan document.

## The Lean reconstruction changes (fork PRs #37 and #38)

Both PRs change how `examples/python/proof_to_lean.py` proves the
propositional lemmas of a certificate: the `cnf` bridges, the rewrite
lemmas, and the propositional part of a normalized theory rewrite.

**#37.** A wide normalized rewrite (a 74-way left-nested disjunction
over
85 atoms against Z3's flattened 48-way form, from the clock
synchronization
file) had no Lean proof: the projection-only search knew nothing of the
constant atoms, and `grind (splits := 64)` failed on the whole
equivalence
after 97 s. `_normalized_rewrite` now builds both sides as Boolean
structures
with every normalized atom an opaque leaf and constant atoms as `True`
and
`False`, and proves the lemma with the existing gate-propagation prover
(`_PropositionalProver`), keeping `grind` as the fallback. The prover
names
every connective once (`let _sN : Prop := ...`) instead of restating it
in
each `have`, which made the terms quadratic, and does case analysis on
an
assumed disjunction before splitting on an atom. The clock
synchronization
file now verifies in 23 s of Lean (a 240 KB direct term); its 29 `cnf`
lemmas shrink from 4.8 MB to 2.2 MB.

**#38.** The QF_IDL bounded model checking circuit (one 13,000-node
assertion, a certificate of 890 steps resting on a single `cnf` lemma)
failed three ways: the lemma's head unfolded all 13,091 formula
definitions
(`transform` timeout), the propagation proof as one nested term of 9,499
`have` steps timed out at `whnf` after 400 s (Lean elaborates nested
binders
in quadratic time: 1,000 type-free `have`s took 547 s as a term and 4.9
s as
tactic lines), and the export spent 134 s re-sorting canonical keys. The
prover now emits a tactic script (`have` lines, `refine` with one hole
per
case, bullets), states every proposition over the reader's own formula
definitions so nothing is unfolded before the attempt, and leaves the
other
operands of a gate to unification. `proof_clause_log._Normalizer`
interns
canonical forms as integers. The circuit verifies in 58 s of Lean (a 1.1
MB
lemma of 9,841 `have` lines) and is now test data.

## The exporter changes (fork PRs #39 and #40)

**#39.** The native trimmer prints the input's `(declare-sort ...)`
ahead of
the declarations and `_dependency_core` accepted only `declare-fun`, so
every
trimmed QF_UF log was rejected and the export fell back, with a warning,
to
the untrimmed log (`NEQ004_size4`: 47,038 resolution steps instead of
16,643).
It keeps sort declarations now. The replay's assumption search, which
ran one
solver check per conjunct of every assertion for a clause with no exact
match (274,092 checks and 90 s on the trimmed `cambridge` log), is
limited to
the conjuncts sharing an uninterpreted symbol with the clause, plus the
symbol-free ones; inputs of at most 64 conjuncts and symbol-free clauses
still search everything (941 checks, 1.2 s, same certificate).

**#40.** `_TRIM_THRESHOLD` goes from 1 MB to 4,096 characters: below the
old
threshold the replay certified Z3's whole search, not the part the final
conflict needs (`cambridge`, a 695 KB log: 20,656 steps and 17.4 MB of
Lean
before, 458 steps and 1.9 MB after; export plus Lean 68 s to 6 s).
Trimming
shrank every one of the 15 logs measured. The fallback warning now says
why
the trimmer failed. `scripts/check_lean.sh` passes `--memory=8192` to
every
`lake env lean` call and `lean/lakefile.toml` adds it to `moreLeanArgs`,
after four uncapped `lean` processes at 24 to 41 GB each froze the
fork's
development machine; the largest proof in the suite peaks near 4.5 GB.

## The C++ change (fork PR #41)

With logs of 4 KB and up trimmed, the trimmer's failures became visible:
on
40 seeds of each of the 12 random generators of the Lean suites, 43 of
133
logs above 4 KB fell back to the untrimmed replay, from three bugs.

1. **Missed conflicts.** `proof_trim::assume` handed the solver each
clause
   in sorted order, and at base level the SAT solver watches a clause's
   first two literals as given. A clause whose first two literals were
   already false never propagated, the trimmer missed the conflict, the
   empty clause went in as a fresh conflict, and its core was itself:
`(deps 172 172)`, rejected as a cyclic dependency. `assume` now passes
   the non-false literals first (`std::stable_partition`); the key in
   `m_clauses` stays the sorted clause.
2. **Empty assumptions.** An assertion that preprocessing reduces to
false
   is logged as `(assume)`. "Conflict found" was encoded as a non-empty
   conflict clause, so this empty conflict looked like no conflict; the
   trimmer kept adding clauses to an inconsistent solver and wrote a
self-dependent core again. The state is now a flag, `m_has_conflict`; an
assumed empty clause sets the solver conflict directly, only the first
   empty clause is registered, and an inferred empty clause that arrives
without a conflict triggers a full re-propagation. If that finds nothing
   either, `trim()` throws "the empty clause is not derivable by unit
   propagation" instead of writing a cycle.
3. **Crashes on hints without a checker.** Z3 logs div/mod axioms under
a
   bare `arith` hint, for which `euf::theory_checker` has no plugin. The
trimmer called `m_checker.clause(hint)` anyway, read through a missing
map
   entry guarded only by `SASSERT`, and died with SIGBUS/SIGSEGV and no
output. `theory_checker::has_rule(expr*)` is new, and the trimmer
assumes
the logged clause for a hint without a plugin, as for any theory axiom.
   The native checker (`smt_proof_checker`) is unchanged.

`tst_trim_watches_literals_that_are_not_false` in
`src/test/proof_replay.cpp`
covers the missed conflict, the explicit rejection, and the assumed
empty
clause; it fails on the old trimmer with "Failed to verify: core.size()
== 6".
Three Python tests run the `z3` executable's trimmer with warnings as
errors
on each reproducer (one embeds Z3's real 60-line division log); all
three
fail on the old build. After the fix the scan's fallbacks are 0 of 133.
Four Lean-suite tests that had silently been checking untrimmed
certificates
through these fallbacks now export the untrimmed log explicitly, so they
keep
covering the wide Farkas lemmas they were written for; the wide
conjunction
test checks both forms.

## Trust boundary and review focus

These changes were produced and reviewed through the automated loop on
the
fork. Human review matters most at these boundaries:

- **Trimmer output stays untrusted.** The trimmer only selects steps.
The
Python replay re-derives every retained step from its listed
dependencies
by unit propagation, and Lean checks the result; a wrong core is
rejected,
never accepted. Treating a hint without a plugin as an assumption
applies
to trimming only; what an `arith` hint proves is decided by the replay.
- **Structural proofs are all Lean-checked.** The tactic scripts, the
  `let`-named terms, and every `grind` fallback are checked by Lean. The
  `cnf` lemmas and normalized rewrites of theory certificates use
`Classical.byContradiction` and `Classical.em`; those certificates
already
  depended on the standard axioms through `grind`, so the axiom report
  (`propext, Classical.choice, Quot.sound`) is unchanged. Boolean
  certificates keep their non-classical prover path and stay axiom-free.
- **Which conjunct ties an assumed clause** (#39) can change on inputs
with
  more than 64 conjuncts, only when an earlier, symbol-disjoint conjunct
would have matched (a valid clause or an unsatisfiable conjunct). A
later
  candidate or the unsat-core fallback ties it instead; soundness is
  unaffected.
- **Different certificates.** Every log of 4 KB and up may now trim to a
different, usually smaller, core than before, which changed the Lean
text
  of many tests. No test was relaxed; the full suite passes.
- **`--memory=8192`** could make a very large proof fail where it passed
  before. No test in the suite comes close.

There are no new axioms, `sorry` placeholders, higher trust settings, or
solver-oracle substitutes for Lean proofs. Parsing and statement
translation
remain trusted frontend components, as before.

## Verification

On the fork, PR #41 (whose tree is this branch's tip, at `d12b33842`)
passed every GitHub check (CI on Ubuntu, macOS, Windows, OCaml, mingw,
WebAssembly, solver determinism, finite-field regressions, trace
comparisons,
Python bindings) except `Ubuntu build - cmake - releaseClang`, which was
cancelled when the fork bot pushed its audit commit; the reruns on that
bot
commit await approval. PR #40 passed 26 GitHub checks; PRs #37 to #39
(Python and documentation only) merged on the fork's local gate, and the
fork's `master` CI ran after each merge.

On a local merge of this branch with upstream `668d59795` (Release build
with Python bindings, macOS arm64, every Lean-using command run one at a
time):

| Check | Result |
| --- | --- |
| Release build and `test-z3` target | Pass |
| `test-z3 proof_replay` (includes the new trimmer regression) | Pass |
| `test-z3 -a` | 121 passed, 0 failed |
| `test_proof_certificate.py` | 96 passed |
| z3test `scripts/proofs/test_proof*.py` | 46 passed |
| `scripts/tests` | 39 of 40 passed; see below |
| `scripts/check_lean.sh` | Pass |
| Lean-certified Boolean matrix, legacy proof object | 6/6 verified |
| Lean-certified QF_LRA matrix, clause log | 8/8 verified |
| `test_proof_to_lean*.py` | 293 passed (883 s) |
| SMT2 regression corpus (`z3test/regressions/smt2`) | 951 benchmarks,
pass |

The one `scripts/tests` failure,
`test_shell.test_log_extension_diagnostic`
(from #11281), fails the same way on upstream `master` alone: it expects
a
".log extension" hint that `src/shell/main.cpp` does not print. This
branch
touches neither the test nor `src/shell`.

## Scope limits

- Mixed Int/Real arithmetic, nonlinear arithmetic, arrays, bit-vectors,
  strings, and quantifiers are not added to the Lean-certified slice.
- Two trimmer defects remain and surface as explicit errors with the
untrimmed fallback: the final empty clause of two `random_bool_argument`
seeds is not RUP over the clauses the trimmer assumes (the native
checker
reports "not congruent" on `euf` lemmas over `(pb (not q))` in the same
logs), and one Boolean-predicates log has an inferred clause that is not
  derivable.
- Revived clauses in the trimmer's backward pass may still watch false
  literals; the probes' full re-propagation covers this, at some cost.
- Large external benchmark suites beyond the recorded regressions and
the
  `smtlib-sample` scans have not been rerun for this submission.

🤖 Generated with [Claude Code](https://claude.com/claude-code)
</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | INFO |
|------------|----------:|---------:|-------------:| ----------:|------|
|Prime_Distribution_Elementary/0007_Elementary_Prime_Bounds/prob_00574_023347__19010816.smt2 |    0.037s | 20.576MiB| unsat | 0 |  |
|Prime_Distribution_Elementary/0011_Shapiro_Tauberian/prob_00194_007832__20809176.smt2 |    0.044s | 22.052MiB| unsat | 0 |  |
|Multi_Party_Computation/0021_Semi_Honest_Def/prob_00086_004410__24397714.smt2 |    0.046s | 22.092MiB| unsat | 0 |  |
|Prime_Distribution_Elementary/0015_PNT_Consequences/prob_00189_007362__22397640.smt2 |    0.049s | 21.32MiB| unsat | 0 |  |
|Prime_Harmonic_Series/0030_Prime_Harmonic_Misc/prob_00076_002524__16265542.smt2 |    0.050s | 21.572MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00224_008065__17682490.smt2        |    0.050s | 21.324MiB| unsat | 0 |  |
|Poincare_Disc/0003_Poincare_Distance/prob_00931_034586__9856730.smt2 |    0.051s | 21.576MiB| unsat | 0 |  |
|Prime_Distribution_Elementary/0015_PNT_Consequences/prob_00243_009243__22482686.smt2 |    0.053s | 21.824MiB| unsat | 0 |  |
|Cauchy/0001_CauchysMeanTheorem/prob_00790_025444__14983980.smt2 |    0.053s | 21.324MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00214_007471__17247474.smt2        |    0.054s | 23.116MiB| unsat | 0 |  |
|Ergodic_Theory/0008_Ergodicity/prob_00178_007847__21363944.smt2 |    0.054s | 23.524MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00240_008712__18263406.smt2        |    0.055s | 23.62MiB| unsat | 0 |  |
|Irrationality_J_Hancl/0013_Irrationality_J_Hancl/prob_00133_005231__14474630.smt2 |    0.055s | 21.324MiB| unsat | 0 |  |
|Poincare_Disc/0003_Poincare_Distance/prob_01534_057589__10340112.smt2 |    0.056s | 21.828MiB| unsat | 0 |  |
|Poincare_Disc/0003_Poincare_Distance/prob_00642_024652__9617008.smt2 |    0.056s | 23.624MiB| unsat | 0 |  |
|Prime_Distribution_Elementary/0008_Partial_Zeta_Bounds/prob_00133_005878__19231164.smt2 |    0.056s | 21.064MiB| unsat | 0 |  |
|Ergodic_Theory/0006_Recurrence/prob_01677_084747__18985256.smt2 |    0.056s | 21.832MiB| unsat | 0 |  |
|Ergodic_Theory/0010_Gouezel_Karlsson/prob_00577_031761__23793580.smt2 |    0.056s | 21.832MiB| unsat | 0 |  |
|Poincare_Disc/0010_Poincare_Tarski/prob_00965_035421__15193680.smt2 |    0.057s | 22.856MiB| unsat | 0 |  |
|Irrationality_J_Hancl/0013_Irrationality_J_Hancl/prob_00367_015065__17336978.smt2 |    0.057s | 22.088MiB| unsat | 0 |  |
|Cauchy/0001_CauchysMeanTheorem/prob_00383_012005__9930914.smt2 |    0.057s | 23.464MiB| unsat | 0 |  |
|Cauchy/0001_CauchysMeanTheorem/prob_00844_027047__15711538.smt2 |    0.057s | 22.868MiB| unsat | 0 |  |
|Poincare_Disc/0006_Poincare_Perpendicular/prob_00119_004553__13002594.smt2 |    0.058s | 22.344MiB| unsat | 0 |  |
|Poincare_Disc/0004_Poincare_Between/prob_00781_027941__11024872.smt2 |    0.058s | 22.348MiB| unsat | 0 |  |
|Poincare_Disc/0007_Poincare_Circles/prob_00557_021796__14053094.smt2 |    0.058s | 23.372MiB| unsat | 0 |  |
|Prime_Harmonic_Series/0030_Prime_Harmonic_Misc/prob_00072_002270__15995960.smt2 |    0.059s | 21.572MiB| unsat | 0 |  |
|Bernoulli/0045_Bernoulli_FPS/prob_01202_053187__22222020.smt2 |    0.059s | 23.6MiB| unsat | 0 |  |
|List_Update/0020_Phase_Partitioning/prob_00434_017129__25327032.smt2 |    0.059s | 22.12MiB| unsat | 0 |  |
|Cauchy/0000_CauchySchwarz/prob_00089_002341__5333318.smt2    |    0.059s | 22.088MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00244_008921__18437530.smt2        |    0.060s | 23.576MiB| unsat | 0 |  |
|Density_Compiler/0004_PDF_Density_Contexts/prob_00925_048546__21538302.smt2 |    0.060s | 23.104MiB| unsat | 0 |  |
|Multi_Party_Computation/0024_Noar_Pinkas_OT/prob_00210_008905__28024846.smt2 |    0.060s | 22.344MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00236_008474__18063190.smt2        |    0.061s | 23.616MiB| unsat | 0 |  |
|Poincare_Disc/0010_Poincare_Tarski/prob_00336_011701__14513634.smt2 |    0.061s | 23.364MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00235_008411__17952468.smt2        |    0.062s | 23.228MiB| unsat | 0 |  |
|Poincare_Disc/0005_Poincare_Lines_Axis_Intersections/prob_00236_009174__11552610.smt2 |    0.062s | 23.888MiB| unsat | 0 |  |
|Probabilistic_Noninterference/0009_Language_Semantics/prob_00687_021796__20693468.smt2 |    0.062s | 23.62MiB| unsat | 0 |  |
|Ergodic_Theory/0005_Measure_Preserving_Transformations/prob_01121_043538__17334752.smt2 |    0.062s | 21.84MiB| unsat | 0 |  |
|Prime_Distribution_Elementary/0015_PNT_Consequences/prob_00145_005774__22311272.smt2 |    0.063s | 22.34MiB| unsat | 0 |  |
|Irrationality_J_Hancl/0013_Irrationality_J_Hancl/prob_00042_001947__13459462.smt2 |    0.063s | 21.828MiB| unsat | 0 |  |
|Poincare_Disc/0004_Poincare_Between/prob_00358_012267__10621106.smt2 |    0.064s | 21.324MiB| unsat | 0 |  |
|First_Welfare_Theorem/0006_Arrow_Debreu_Model/prob_00454_018098__24430292.smt2 |    0.064s | 24.644MiB| unsat | 0 |  |
|Density_Compiler/0003_PDF_Semantics/prob_00560_023297__18415420.smt2 |    0.064s | 23.516MiB| unsat | 0 |  |
|List_Update/0019_OPT2/prob_00474_023595__24309606.smt2       |    0.064s | 24.136MiB| unsat | 0 |  |
|Polynomial_Interpolation/0000_Missing_Unsorted/prob_00383_014421__13364278.smt2 |    0.065s | 22.864MiB| unsat | 0 |  |
|Poincare_Disc/0010_Poincare_Tarski/prob_00836_030789__15107176.smt2 |    0.065s | 23.872MiB| unsat | 0 |  |
|Ergodic_Theory/0005_Measure_Preserving_Transformations/prob_01316_051877__17431944.smt2 |    0.065s | 22.34MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00104_003436__14278900.smt2        |    0.066s | 23.616MiB| unsat | 0 |  |
|Probabilistic_Noninterference/0009_Language_Semantics/prob_01591_053343__22252904.smt2 |    0.066s | 24.392MiB| unsat | 0 |  |
|Density_Compiler/0007_PDF_Target_Density_Contexts/prob_00258_013106__27169804.smt2 |    0.066s | 23.116MiB| unsat | 0 |  |
|Ergodic_Theory/0012_Transfer_Operator/prob_00267_014010__25018368.smt2 |    0.066s | 23.116MiB| unsat | 0 |  |
|List_Update/0010_BIT/prob_01139_050109__18547876.smt2        |    0.066s | 23.624MiB| unsat | 0 |  |
|List_Update/0013_BIT_pairwise/prob_00274_013697__22309286.smt2 |    0.066s | 24.392MiB| unsat | 0 |  |
|Cauchy/0000_CauchySchwarz/prob_00166_004789__5983274.smt2    |    0.066s | 23.624MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00068_002171__13492240.smt2        |    0.067s | 23.108MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00250_009257__18684974.smt2        |    0.067s | 23.496MiB| unsat | 0 |  |
|First_Welfare_Theorem/0007_Exchange_Economy/prob_00289_010327__26916166.smt2 |    0.067s | 21.836MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00212_007394__17138014.smt2        |    0.068s | 23.112MiB| unsat | 0 |  |
|Poincare_Disc/0004_Poincare_Between/prob_01074_038995__11229322.smt2 |    0.068s | 23.364MiB| unsat | 0 |  |
|First_Welfare_Theorem/0003_Preferences/prob_00527_017268__15936226.smt2 |    0.068s | 24.132MiB| unsat | 0 |  |
|Ergodic_Theory/0007_Invariants/prob_00095_003862__19661432.smt2 |    0.068s | 24.4MiB| unsat | 0 |  |
|Ergodic_Theory/0007_Invariants/prob_00532_026257__20185236.smt2 |    0.068s | 24.912MiB| unsat | 0 |  |
|Ergodic_Theory/0008_Ergodicity/prob_00323_014780__21466020.smt2 |    0.068s | 22.348MiB| unsat | 0 |  |
|List_Update/0010_BIT/prob_00877_036064__18311046.smt2        |    0.068s | 24.644MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00071_002263__13579552.smt2        |    0.069s | 22.856MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00063_002042__13388584.smt2        |    0.069s | 23.368MiB| unsat | 0 |  |
|Probabilistic_Noninterference/0009_Language_Semantics/prob_01180_038561__21378968.smt2 |    0.069s | 23.716MiB| unsat | 0 |  |
|Ergodic_Theory/0001_Kohlberg_Neyman_Karlsson/prob_00126_004637__15113802.smt2 |    0.069s | 23.876MiB| unsat | 0 |  |
|Ergodic_Theory/0011_Shift_Operator/prob_00146_005550__24782042.smt2 |    0.069s | 24.144MiB| unsat | 0 |  |
|Transcendence_Series_Hancl_Rucki/0084_Transcendence_Series/prob_00057_002036__16069370.smt2 |    0.070s | 23.512MiB| unsat | 0 |  |
|Polynomial_Interpolation/0003_Missing_Polynomial/prob_00882_030962__17050520.smt2 |    0.070s | 22.868MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00054_001645__13214870.smt2        |    0.070s | 23.884MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00142_004948__15085060.smt2        |    0.070s | 24.172MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00091_002960__14087272.smt2        |    0.070s | 23.636MiB| unsat | 0 |  |
|Poincare_Disc/0006_Poincare_Perpendicular/prob_00219_008378__13083836.smt2 |    0.070s | 22.084MiB| unsat | 0 |  |
|Prime_Distribution_Elementary/0015_PNT_Consequences/prob_00424_016650__22776576.smt2 |    0.070s | 24.904MiB| unsat | 0 |  |
|Polynomial_Interpolation/0003_Missing_Polynomial/prob_01215_042600__17824644.smt2 |    0.071s | 24.128MiB| unsat | 0 |  |
|First_Welfare_Theorem/0004_Utility_Functions/prob_00216_006310__18719900.smt2 |    0.071s | 22.0MiB| unsat | 0 |  |
|Density_Compiler/0005_PDF_Compiler_Pred/prob_00114_005772__21975248.smt2 |    0.071s | 25.176MiB| unsat | 0 |  |
|Polynomial_Interpolation/0003_Missing_Polynomial/prob_01308_046278__18083364.smt2 |    0.072s | 24.396MiB| unsat | 0 |  |
|Polynomial_Interpolation/0006_Is_Rat_To_Rat/prob_00070_002409__18734810.smt2 |    0.073s | 24.064MiB| unsat | 0 |  |
|Triangle/0001_Triangle/prob_00092_003390__19850452.smt2      |    0.073s | 23.368MiB| unsat | 0 |  |
|Probabilistic_Noninterference/0011_Compositionality/prob_00660_021413__27709912.smt2 |    0.073s | 26.44MiB| unsat | 0 |  |
|Probabilistic_Noninterference/0010_Resumption_Based/prob_00874_026641__24321230.smt2 |    0.073s | 24.132MiB| unsat | 0 |  |
|Irrationality_J_Hancl/0013_Irrationality_J_Hancl/prob_00334_013504__16846998.smt2 |    0.073s | 24.664MiB| unsat | 0 |  |
|Ergodic_Theory/0003_SG_Library_Complement/prob_00808_029610__15695294.smt2 |    0.073s | 23.88MiB| unsat | 0 |  |
|Ergodic_Theory/0010_Gouezel_Karlsson/prob_01265_070532__24296054.smt2 |    0.073s | 25.664MiB| unsat | 0 |  |
|Cauchy/0000_CauchySchwarz/prob_00177_005242__6069218.smt2    |    0.073s | 23.88MiB| unsat | 0 |  |
|Transcendence_Series_Hancl_Rucki/0084_Transcendence_Series/prob_00200_008182__17228090.smt2 |    0.074s | 23.88MiB| unsat | 0 |  |
|Transcendence_Series_Hancl_Rucki/0084_Transcendence_Series/prob_00750_032670__24128266.smt2 |    0.074s | 23.368MiB| unsat | 0 |  |
|Bertrands_Postulate/0004_Bertrand/prob_00863_034779__17985614.smt2 |    0.074s | 23.636MiB| unsat | 0 |  |
|Poincare_Disc/0005_Poincare_Lines_Axis_Intersections/prob_00583_022333__12076448.smt2 |    0.074s | 26.44MiB| unsat | 0 |  |
|Probabilistic_Noninterference/0009_Language_Semantics/prob_01441_047773__21924864.smt2 |    0.074s | 24.128MiB| unsat | 0 |  |
|First_Welfare_Theorem/0004_Utility_Functions/prob_00062_001621__17378230.smt2 |    0.074s | 24.9MiB| unsat | 0 |  |
|Density_Compiler/0006_PDF_Target_Semantics/prob_00576_024149__25880332.smt2 |    0.074s | 23.112MiB| unsat | 0 |  |
|Cauchy/0000_CauchySchwarz/prob_00194_005814__6153412.smt2    |    0.074s | 24.148MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00234_008317__17845680.smt2        |    0.075s | 24.92MiB| unsat | 0 |  |
|First_Welfare_Theorem/0003_Preferences/prob_00584_019730__16528392.smt2 |    0.075s | 24.508MiB| unsat | 0 |  |
|First_Welfare_Theorem/0004_Utility_Functions/prob_00073_002018__17525490.smt2 |    0.075s | 22.592MiB| unsat | 0 |  |
|CryptHOL/0032_Generative_Probabilistic_Value/prob_03445_144572__24875318.smt2 |    0.075s | 24.908MiB| unsat | 0 |  |
|Berlekamp_Zassenhaus/0074_Factor_Bound/prob_00043_001219__33850784.smt2 |    0.075s | 24.812MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00211_007300__17031166.smt2        |    0.076s | 25.16MiB| unsat | 0 |  |
|Triangle/0001_Triangle/prob_00096_003554__19941394.smt2      |    0.076s | 23.632MiB| unsat | 0 |  |
|Bertrands_Postulate/0004_Bertrand/prob_00842_033821__17890998.smt2 |    0.076s | 23.364MiB| unsat | 0 |  |
|Poincare_Disc/0000_Poincare_Lines/prob_00933_036823__6819570.smt2 |    0.076s | 23.888MiB| unsat | 0 |  |
|First_Welfare_Theorem/0003_Preferences/prob_00541_017951__16082512.smt2 |    0.076s | 24.388MiB| unsat | 0 |  |
|Multi_Party_Computation/0026_GMW/prob_00463_023300__33678162.smt2 |    0.076s | 23.876MiB| unsat | 0 |  |
|First_Welfare_Theorem/0005_Common/prob_00070_001804__19958848.smt2 |    0.077s | 23.884MiB| unsat | 0 |  |
|Irrationality_J_Hancl/0013_Irrationality_J_Hancl/prob_00297_011718__16517644.smt2 |    0.077s | 24.132MiB| unsat | 0 |  |
|Ergodic_Theory/0005_Measure_Preserving_Transformations/prob_00171_007190__16688682.smt2 |    0.077s | 26.952MiB| unsat | 0 |  |
|Grothendieck_Schemes/0007_Comm_Ring/prob_04764_206298__17833444.smt2 |    0.078s | 22.596MiB| unsat | 0 |  |
|Density_Compiler/0000_Density_Predicates/prob_00196_007812__15150262.smt2 |    0.078s | 25.668MiB| unsat | 0 |  |
|Prime_Distribution_Elementary/0007_Elementary_Prime_Bounds/prob_00423_017359__18789584.smt2 |    0.078s | 25.156MiB| unsat | 0 |  |
|Bernoulli/0034_Periodic_Bernpoly/prob_00099_004084__16318318.smt2 |    0.078s | 24.136MiB| unsat | 0 |  |
|Prime_Harmonic_Series/0031_Squarefree_Nat/prob_00164_006481__20784122.smt2 |    0.079s | 25.932MiB| unsat | 0 |  |
|Poincare_Disc/0004_Poincare_Between/prob_00675_024166__10955118.smt2 |    0.079s | 24.772MiB| unsat | 0 |  |
|Probabilistic_Noninterference/0010_Resumption_Based/prob_01568_050551__25762018.smt2 |    0.079s | 27.24MiB| unsat | 0 |  |
|Multi_Party_Computation/0023_ETP_RSA_OT/prob_00685_026562__27683552.smt2 |    0.079s | 24.9MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00139_004785__15006864.smt2        |    0.080s | 24.408MiB| unsat | 0 |  |
|Poincare_Disc/0000_Poincare_Lines/prob_00654_025909__6678292.smt2 |    0.080s | 28.384MiB| unsat | 0 |  |
|Probabilistic_Noninterference/0011_Compositionality/prob_02387_085655__31556224.smt2 |    0.080s | 26.96MiB| unsat | 0 |  |
|List_Update/0013_BIT_pairwise/prob_00055_002259__21989260.smt2 |    0.080s | 24.908MiB| unsat | 0 |  |
|Probabilistic_Noninterference/0010_Resumption_Based/prob_01385_044026__25381512.smt2 |    0.081s | 27.976MiB| unsat | 0 |  |
|Probabilistic_Noninterference/0010_Resumption_Based/prob_01119_033732__24604496.smt2 |    0.081s | 26.144MiB| unsat | 0 |  |
|Probabilistic_Noninterference/0010_Resumption_Based/prob_01634_052329__25896576.smt2 |    0.081s | 26.952MiB| unsat | 0 |  |
|Density_Compiler/0000_Density_Predicates/prob_00333_014409__15393406.smt2 |    0.081s | 25.924MiB| unsat | 0 |  |
|Irrationality_J_Hancl/0013_Irrationality_J_Hancl/prob_00443_018586__18232266.smt2 |    0.081s | 24.904MiB| unsat | 0 |  |
|Probabilistic_Noninterference/0011_Compositionality/prob_00237_006770__26620744.smt2 |    0.083s | 27.208MiB| unsat | 0 |  |
|Probabilistic_Noninterference/0010_Resumption_Based/prob_01445_045869__25518594.smt2 |    0.083s | 26.188MiB| unsat | 0 |  |
|First_Welfare_Theorem/0006_Arrow_Debreu_Model/prob_00163_005375__20627578.smt2 |    0.084s | 25.416MiB| unsat | 0 |  |
|First_Welfare_Theorem/0007_Exchange_Economy/prob_00173_005438__25805330.smt2 |    0.084s | 22.356MiB| unsat | 0 |  |
|List_Update/0021_BIT_2comp_on2/prob_01017_040558__26814960.smt2 |    0.084s | 26.444MiB| unsat | 0 |  |
|First_Welfare_Theorem/0006_Arrow_Debreu_Model/prob_00435_017048__24165948.smt2 |    0.086s | 24.396MiB| unsat | 0 |  |
|Ergodic_Theory/0006_Recurrence/prob_01443_071916__18799528.smt2 |    0.086s | 27.236MiB| unsat | 0 |  |
|Transcendence_Series_Hancl_Rucki/0084_Transcendence_Series/prob_00070_002522__16157510.smt2 |    0.087s | 24.644MiB| unsat | 0 |  |
|Probabilistic_Noninterference/0011_Compositionality/prob_01343_046318__29006090.smt2 |    0.087s | 28.74MiB| unsat | 0 |  |
|Prime_Harmonic_Series/0031_Squarefree_Nat/prob_00158_006051__20323806.smt2 |    0.088s | 26.184MiB| unsat | 0 |  |
|Transcendence_Series_Hancl_Rucki/0084_Transcendence_Series/prob_00074_002716__16283448.smt2 |    0.088s | 24.892MiB| unsat | 0 |  |
|Grothendieck_Schemes/0007_Comm_Ring/prob_02463_105643__12336456.smt2 |    0.088s | 27.488MiB| unsat | 0 |  |
|Density_Compiler/0002_PDF_Values/prob_00417_016518__17252458.smt2 |    0.088s | 28.496MiB| unsat | 0 |  |
|Prime_Distribution_Elementary/0008_Partial_Zeta_Bounds/prob_00381_017329__19679326.smt2 |    0.088s | 25.668MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00045_001329__12940668.smt2        |    0.089s | 25.132MiB| unsat | 0 |  |
|Bernoulli/0034_Periodic_Bernpoly/prob_00292_012479__17169906.smt2 |    0.089s | 26.616MiB| unsat | 0 |  |
|Probabilistic_Noninterference/0009_Language_Semantics/prob_00225_007353__19614096.smt2 |    0.090s | 27.456MiB| unsat | 0 |  |
|Grothendieck_Schemes/0007_Comm_Ring/prob_00718_026662__8475614.smt2 |    0.091s | 28.748MiB| unsat | 0 |  |
|Pell/0001_Pell/prob_00732_027720__18807248.smt2              |    0.091s | 25.168MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00155_005469__15602366.smt2        |    0.092s | 23.372MiB| unsat | 0 |  |
|First_Welfare_Theorem/0004_Utility_Functions/prob_00198_005747__18590186.smt2 |    0.092s | 24.9MiB| unsat | 0 |  |
|First_Welfare_Theorem/0008_Private_Ownership_Economy/prob_00047_001384__27659098.smt2 |    0.092s | 25.928MiB| unsat | 0 |  |
|Ergodic_Theory/0006_Recurrence/prob_01085_052490__18529882.smt2 |    0.093s | 30.028MiB| unsat | 0 |  |
|Ergodic_Theory/0006_Recurrence/prob_00864_040571__18349744.smt2 |    0.093s | 24.908MiB| unsat | 0 |  |
|List_Update/0010_BIT/prob_01030_044427__18431996.smt2        |    0.093s | 27.356MiB| unsat | 0 |  |
|First_Welfare_Theorem/0006_Arrow_Debreu_Model/prob_00185_006286__20963850.smt2 |    0.097s | 25.932MiB| unsat | 0 |  |
|Poincare_Disc/0004_Poincare_Between/prob_00464_016818__10695936.smt2 |    0.098s | 26.18MiB| unsat | 0 |  |
|First_Welfare_Theorem/0006_Arrow_Debreu_Model/prob_00109_003286__20117182.smt2 |    0.098s | 25.684MiB| unsat | 0 |  |
|Ergodic_Theory/0003_SG_Library_Complement/prob_00944_035327__15793728.smt2 |    0.098s | 29.26MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00160_005668__15869914.smt2        |    0.099s | 31.4MiB| unsat | 0 |  |
|CryptHOL/0032_Generative_Probabilistic_Value/prob_02972_124533__24181188.smt2 |    0.100s | 27.984MiB| unsat | 0 |  |
|Ergodic_Theory/0005_Measure_Preserving_Transformations/prob_00913_035336__17211948.smt2 |    0.100s | 31.564MiB| unsat | 0 |  |
|List_Update/0019_OPT2/prob_00280_012003__23842038.smt2       |    0.100s | 27.468MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00080_002519__13751410.smt2        |    0.101s | 27.204MiB| unsat | 0 |  |
|First_Welfare_Theorem/0006_Arrow_Debreu_Model/prob_00313_011596__22524684.smt2 |    0.102s | 31.812MiB| unsat | 0 |  |
|First_Welfare_Theorem/0007_Exchange_Economy/prob_00212_007012__26337464.smt2 |    0.102s | 31.816MiB| unsat | 0 |  |
|First_Welfare_Theorem/0007_Exchange_Economy/prob_00182_005780__25980122.smt2 |    0.102s | 30.02MiB| unsat | 0 |  |
|First_Welfare_Theorem/0003_Preferences/prob_00472_015041__15413634.smt2 |    0.103s | 28.052MiB| unsat | 0 |  |
|Ergodic_Theory/0006_Recurrence/prob_00141_006081__17770002.smt2 |    0.103s | 31.164MiB| unsat | 0 |  |
|First_Welfare_Theorem/0007_Exchange_Economy/prob_00154_004721__25605488.smt2 |    0.105s | 29.256MiB| unsat | 0 |  |
|First_Welfare_Theorem/0004_Utility_Functions/prob_00138_004019__18080858.smt2 |    0.108s | 24.156MiB| unsat | 0 |  |
|Poincare_Disc/0003_Poincare_Distance/prob_00863_031847__9774596.smt2 |    0.111s | 30.28MiB| unsat | 0 |  |
|Poincare_Disc/0000_Poincare_Lines/prob_00165_006879__6329206.smt2 |    0.113s | 35.32MiB| unsat | 0 |  |
|First_Welfare_Theorem/0006_Arrow_Debreu_Model/prob_00304_011101__22333288.smt2 |    0.114s | 32.312MiB| unsat | 0 |  |
|Poincare_Disc/0003_Poincare_Distance/prob_01030_038603__9931670.smt2 |    0.119s | 35.736MiB| unsat | 0 |  |
|First_Welfare_Theorem/0006_Arrow_Debreu_Model/prob_00355_013600__23121704.smt2 |    0.120s | 34.112MiB| unsat | 0 |  |
|First_Welfare_Theorem/0007_Exchange_Economy/prob_00245_008459__26514900.smt2 |    0.132s | 38.484MiB| unsat | 0 |  |
|Probabilistic_Noninterference/0009_Language_Semantics/prob_00941_030035__20991340.smt2 |    0.133s | 33.044MiB| unsat | 0 |  |
|Probabilistic_Noninterference/0009_Language_Semantics/prob_00420_013575__20048518.smt2 |    0.151s | 34.336MiB| unsat | 0 |  |
|First_Welfare_Theorem/0004_Utility_Functions/prob_00047_001171__17223338.smt2 |    0.151s | 27.716MiB| unsat | 0 |  |
|Density_Compiler/0004_PDF_Density_Contexts/prob_01054_055351__21820790.smt2 |    0.155s | 43.032MiB| unsat | 0 |  |
|Ergodic_Theory/0003_SG_Library_Complement/prob_01083_041705__15922496.smt2 |    0.166s | 36.008MiB| unsat | 0 |  |
|Ergodic_Theory/0007_Invariants/prob_00413_021191__20068658.smt2 |    0.171s | 36.272MiB| unsat | 0 |  |
|First_Welfare_Theorem/0006_Arrow_Debreu_Model/prob_00238_008497__21625014.smt2 |    0.178s | 40.092MiB| unsat | 0 |  |
|Grothendieck_Schemes/0007_Comm_Ring/prob_02526_107957__12478792.smt2 |    0.228s | 47.156MiB| unsat | 0 |  |
|Grothendieck_Schemes/0007_Comm_Ring/prob_05039_221715__18705350.smt2 |    0.250s | 75.596MiB| unsat | 0 |  |
|CryptHOL/0028_Misc_CryptHOL/prob_02328_091597__19868656.smt2 |    0.255s | 47.508MiB| unsat | 0 |  |
|Polynomial_Interpolation/0003_Missing_Polynomial/prob_00620_021681__16405720.smt2 |    0.277s | 44.464MiB| unsat | 0 |  |
|First_Welfare_Theorem/0007_Exchange_Economy/prob_00321_011678__27454796.smt2 |    0.291s | 82.452MiB| unsat | 0 |  |
|Poincare_Disc/0005_Poincare_Lines_Axis_Intersections/prob_01231_046337__12905712.smt2 |    0.311s | 67.74MiB| unsat | 0 |  |
|Poincare_Disc/0010_Poincare_Tarski/prob_00123_004047__14372770.smt2 |    0.343s | 74.452MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00242_008788__18349028.smt2        |    0.359s | 80.82MiB| unsat | 0 |  |
|Poincare_Disc/0000_Poincare_Lines/prob_01799_065548__7959234.smt2 |    0.361s | 70.772MiB| unsat | 0 |  |
|Ergodic_Theory/0009_Kingman/prob_01344_066736__22776444.smt2 |    0.362s | 80.904MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00175_006281__16334520.smt2        |    0.373s | 75.904MiB| unsat | 0 |  |
|Ergodic_Theory/0009_Kingman/prob_00154_005762__21716024.smt2 |    0.383s | 46.656MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00171_006096__16238272.smt2        |    0.387s | 77.444MiB| unsat | 0 |  |
|Transcendence_Series_Hancl_Rucki/0084_Transcendence_Series/prob_00630_027157__22740804.smt2 |    0.426s | 88.62MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00248_009170__18609884.smt2        |    0.443s | 102.0MiB| unsat | 0 |  |
|Poincare_Disc/0010_Poincare_Tarski/prob_00740_027480__14975414.smt2 |    0.452s | 82.612MiB| unsat | 0 |  |
|Transcendence_Series_Hancl_Rucki/0084_Transcendence_Series/prob_00171_006841__16962510.smt2 |    0.458s | 87.456MiB| unsat | 0 |  |
|First_Welfare_Theorem/0003_Preferences/prob_00434_013854__15200920.smt2 |    0.621s | 86.876MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00115_003827__14500918.smt2        |    0.837s | 138.0MiB| unsat | 0 |  |
|Ergodic_Theory/0008_Ergodicity/prob_00056_001810__21265874.smt2 |    1.084s | 187.0MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00238_008532__18176236.smt2        |    1.158s | 129.0MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00216_007529__17339824.smt2        |    1.160s | 130.0MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00218_007668__17427152.smt2        |    1.349s | 163.0MiB| unsat | 0 |  |
|Triangle/0000_Angles/prob_00220_007813__17514002.smt2        |    2.251s | 269.0MiB| unsat | 0 |  |
|Probabilistic_Noninterference/0011_Compositionality/prob_01482_051102__29267840.smt2 |    2.326s | 355.0MiB| unsat | 0 |  |
|Prime_Distribution_Elementary/0012_Moebius_Mu_Sum/prob_00388_017665__21664948.smt2 |    2.328s | 271.0MiB| unsat | 0 |  |
|Irrationality_J_Hancl/0013_Irrationality_J_Hancl/prob_00772_033530__21842160.smt2 |    2.460s | 187.0MiB| unsat | 0 |  |
|Poincare_Disc/0003_Poincare_Distance/prob_00402_014816__9386210.smt2 |    2.464s | 487.0MiB| unsat | 0 |  |
|First_Welfare_Theorem/0004_Utility_Functions/prob_00172_004977__18365474.smt2 |    2.663s | 105.0MiB| unsat | 0 |  |
|Grothendieck_Schemes/0007_Comm_Ring/prob_00772_030325__8600380.smt2 |    3.101s | 347.0MiB| unsat | 0 |  |
|CryptHOL/0034_GPV_Expectation/prob_00018_000489__29629528.smt2 |    3.450s | 369.0MiB| unsat | 0 |  |
|Poincare_Disc/0003_Poincare_Distance/prob_00763_028628__9685136.smt2 |    5.316s | 2136.0MiB| unsat | 0 |  |
|Poincare_Disc/0000_Poincare_Lines/prob_01224_045964__7349302.smt2 |    6.560s | 919.0MiB| unsat | 0 |  |
|Irrationality_J_Hancl/0013_Irrationality_J_Hancl/prob_00212_008025__15423260.smt2 |    8.097s | 336.0MiB| unsat | 0 |  |
|Bernoulli/0034_Periodic_Bernpoly/prob_00193_007819__16786270.smt2 |   14.333s | 1193.0MiB| unsat | 0 |  |
|Cauchy/0001_CauchysMeanTheorem/prob_00601_019375__12649110.smt2 |   14.784s | 1897.0MiB| unsat | 0 |  |
|Poincare_Disc/0010_Poincare_Tarski/prob_00656_024497__14843968.smt2 |   16.760s | 2066.0MiB| unsat | 0 |  |
|Density_Compiler/0001_PDF_Transformations/prob_00027_000671__15771492.smt2 |   22.823s | 1465.0MiB| unsat | 0 |  |
|Poincare_Disc/0006_Poincare_Perpendicular/prob_00425_015448__13290098.smt2 |   29.012s | 3138.0MiB| unsat | 0 |  |
|Ergodic_Theory/0007_Invariants/prob_00321_016119__19961104.smt2 |   30.008s | 82.604MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Prime_Distribution_Elementary/0012_Moebius_Mu_Sum/prob_00139_005080__21285376.smt2 |   30.009s | 83.32MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|CryptHOL/0032_Generative_Probabilistic_Value/prob_03334_139952__24763988.smt2 |   30.010s | 93.968MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|CryptHOL/0034_GPV_Expectation/prob_01157_062055__31556890.smt2 |   30.012s | 186.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Triangle/0000_Angles/prob_00186_006579__16501644.smt2        |   30.013s | 136.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|CryptHOL/0028_Misc_CryptHOL/prob_02124_081865__19561242.smt2 |   30.015s | 129.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Triangle/0000_Angles/prob_00051_001540__13130622.smt2        |   30.017s | 184.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Density_Compiler/0001_PDF_Transformations/prob_00419_021561__16518114.smt2 |   30.017s | 165.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Density_Compiler/0006_PDF_Target_Semantics/prob_01154_052084__26703506.smt2 |   30.018s | 225.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Grothendieck_Schemes/0007_Comm_Ring/prob_02370_101667__12167840.smt2 |   30.024s | 259.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Irrationality_J_Hancl/0013_Irrationality_J_Hancl/prob_00781_033928__21931022.smt2 |   30.027s | 287.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Prime_Distribution_Elementary/0005_Prime_Distribution_Elementary_Library/prob_00396_015252__17391134.smt2 |   30.028s | 283.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|CryptHOL/0034_GPV_Expectation/prob_00787_040683__31031736.smt2 |   30.029s | 321.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Density_Compiler/0000_Density_Predicates/prob_00274_011423__15262290.smt2 |   30.030s | 427.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|CryptHOL/0032_Generative_Probabilistic_Value/prob_01452_059612__22831570.smt2 |   30.032s | 347.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|First_Welfare_Theorem/0004_Utility_Functions/prob_00155_004504__18223510.smt2 |   30.033s | 374.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|First_Welfare_Theorem/0004_Utility_Functions/prob_00337_010433__19651022.smt2 |   30.037s | 361.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|CryptHOL/0034_GPV_Expectation/prob_00703_036351__30871044.smt2 |   30.038s | 415.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Prime_Distribution_Elementary/0010_Primorial/prob_00087_003500__20234750.smt2 |   30.039s | 500.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Irrationality_J_Hancl/0013_Irrationality_J_Hancl/prob_00158_006204__14777644.smt2 |   30.041s | 429.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0006_Recurrence/prob_02086_107010__19320692.smt2 |   30.042s | 448.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0001_Poincare_Lines_Ideal_Points/prob_00041_001784__8046326.smt2 |   30.043s | 539.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Multi_Party_Computation/0023_ETP_RSA_OT/prob_00225_008297__26518204.smt2 |   30.045s | 548.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Density_Compiler/0004_PDF_Density_Contexts/prob_00739_037570__21088590.smt2 |   30.048s | 512.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Bertrands_Postulate/0004_Bertrand/prob_00831_033282__17816580.smt2 |   30.050s | 528.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Probabilistic_Noninterference/0009_Language_Semantics/prob_00820_026280__20886812.smt2 |   30.051s | 515.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Multi_Party_Computation/0022_ETP_OT/prob_00312_012456__25432354.smt2 |   30.052s | 555.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Triangle/0000_Angles/prob_00222_007972__17600926.smt2        |   30.053s | 606.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0009_Kingman/prob_02076_105791__23318074.smt2 |   30.053s | 520.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0000_Poincare_Lines/prob_01534_056019__7808584.smt2 |   30.055s | 654.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|CryptHOL/0034_GPV_Expectation/prob_00618_030737__30663406.smt2 |   30.055s | 801.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|CryptHOL/0032_Generative_Probabilistic_Value/prob_05769_259415__28311646.smt2 |   30.058s | 787.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Probabilistic_Noninterference/0009_Language_Semantics/prob_01232_040047__21467432.smt2 |   30.059s | 602.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Density_Compiler/0003_PDF_Semantics/prob_00897_036679__18975846.smt2 |   30.060s | 677.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Probabilistic_Noninterference/0011_Compositionality/prob_02474_089274__31787114.smt2 |   30.061s | 667.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Density_Compiler/0001_PDF_Transformations/prob_00164_007550__15972660.smt2 |   30.061s | 839.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Density_Compiler/0006_PDF_Target_Semantics/prob_00807_035693__26237620.smt2 |   30.061s | 714.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0006_Recurrence/prob_02220_114477__19528808.smt2 |   30.063s | 823.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Density_Compiler/0003_PDF_Semantics/prob_00977_040120__19135592.smt2 |   30.064s | 747.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Grothendieck_Schemes/0007_Comm_Ring/prob_04876_211844__18076944.smt2 |   30.065s | 911.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Grothendieck_Schemes/0007_Comm_Ring/prob_04700_202569__17715392.smt2 |   30.065s | 973.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Probabilistic_Noninterference/0011_Compositionality/prob_00050_001158__26171552.smt2 |   30.065s | 782.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0012_Transfer_Operator/prob_00652_036387__25339556.smt2 |   30.068s | 879.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Probabilistic_Noninterference/0009_Language_Semantics/prob_01339_043961__21654932.smt2 |   30.075s | 715.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Probabilistic_Noninterference/0014_Trace_Based/prob_00219_007833__32790660.smt2 |   30.076s | 869.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Bernoulli/0026_Bernoulli/prob_00096_003119__14985828.smt2    |   30.077s | 1048.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0003_Poincare_Distance/prob_00096_004157__9123426.smt2 |   30.079s | 905.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|First_Welfare_Theorem/0003_Preferences/prob_00461_014637__15311806.smt2 |   30.085s | 886.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0005_Measure_Preserving_Transformations/prob_00352_014307__16801792.smt2 |   30.087s | 887.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|List_Update/0019_OPT2/prob_00050_001202__23415968.smt2       |   30.088s | 908.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|List_Update/0010_BIT/prob_00132_005258__17434776.smt2        |   30.089s | 975.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Prime_Distribution_Elementary/0011_Shapiro_Tauberian/prob_00312_012679__20987606.smt2 |   30.091s | 1073.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0000_Poincare_Lines/prob_01321_048824__7554868.smt2 |   30.092s | 967.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|List_Update/0008_Move_to_Front/prob_00074_002294__16145788.smt2 |   30.092s | 1008.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|CryptHOL/0032_Generative_Probabilistic_Value/prob_05881_265356__28500096.smt2 |   30.093s | 1121.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Grothendieck_Schemes/0007_Comm_Ring/prob_00922_038253__8848740.smt2 |   30.097s | 1111.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Polynomial_Interpolation/0000_Missing_Unsorted/prob_00186_007438__12913682.smt2 |   30.099s | 1026.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|CryptHOL/0028_Misc_CryptHOL/prob_01794_067270__19182688.smt2 |   30.102s | 1388.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0006_Recurrence/prob_00963_046040__18442216.smt2 |   30.102s | 1379.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Probabilistic_Noninterference/0011_Compositionality/prob_00364_010882__26971800.smt2 |   30.104s | 1303.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Probabilistic_Noninterference/0009_Language_Semantics/prob_01539_051588__22147296.smt2 |   30.109s | 1098.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Prime_Distribution_Elementary/0012_Moebius_Mu_Sum/prob_00428_019948__21711928.smt2 |   30.113s | 1309.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Density_Compiler/0007_PDF_Target_Density_Contexts/prob_00113_004854__26930326.smt2 |   30.114s | 1223.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Probabilistic_Noninterference/0011_Compositionality/prob_00426_013135__27157136.smt2 |   30.119s | 1294.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0001_Poincare_Lines_Ideal_Points/prob_00217_008523__8449148.smt2 |   30.120s | 1123.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|List_Update/0019_OPT2/prob_00796_038650__24670162.smt2       |   30.121s | 1331.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0005_Poincare_Lines_Axis_Intersections/prob_00046_002036__11395208.smt2 |   30.124s | 1348.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|First_Welfare_Theorem/0004_Utility_Functions/prob_00086_002435__17680284.smt2 |   30.124s | 1206.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Triangle/0000_Angles/prob_00208_007187__16944230.smt2        |   30.128s | 2309.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0000_Poincare_Lines/prob_00820_032853__6737584.smt2 |   30.128s | 1733.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Prime_Distribution_Elementary/0011_Shapiro_Tauberian/prob_00065_002290__20606128.smt2 |   30.131s | 1683.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Transcendence_Series_Hancl_Rucki/0084_Transcendence_Series/prob_00038_001497__15978774.smt2 |   30.136s | 1719.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|First_Welfare_Theorem/0003_Preferences/prob_00394_012413__15066074.smt2 |   30.136s | 1362.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Irrationality_J_Hancl/0013_Irrationality_J_Hancl/prob_00395_016409__17526670.smt2 |   30.138s | 1499.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Triangle/0000_Angles/prob_00159_005627__15780814.smt2        |   30.143s | 2179.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Probabilistic_Noninterference/0009_Language_Semantics/prob_01508_050081__22028704.smt2 |   30.143s | 1452.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|List_Update/0001_Prob_Theory/prob_00496_018045__15219620.smt2 |   30.144s | 1539.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Grothendieck_Schemes/0007_Comm_Ring/prob_04820_209009__17944758.smt2 |   30.146s | 1741.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Density_Compiler/0001_PDF_Transformations/prob_00378_019395__16422978.smt2 |   30.146s | 1718.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Grothendieck_Schemes/0007_Comm_Ring/prob_05465_247432__20085480.smt2 |   30.147s | 1406.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Irrationality_J_Hancl/0013_Irrationality_J_Hancl/prob_00362_014860__17237966.smt2 |   30.147s | 2013.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Bernoulli/0045_Bernoulli_FPS/prob_00499_021309__19275426.smt2 |   30.147s | 1734.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Probabilistic_Noninterference/0010_Resumption_Based/prob_01701_054413__26032312.smt2 |   30.148s | 1814.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Density_Compiler/0004_PDF_Density_Contexts/prob_00094_003824__19764150.smt2 |   30.149s | 1573.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Prime_Distribution_Elementary/0011_Shapiro_Tauberian/prob_00370_015042__21047534.smt2 |   30.149s | 1891.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0003_Poincare_Distance/prob_00298_011097__9306678.smt2 |   30.151s | 1911.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Probabilistic_Noninterference/0011_Compositionality/prob_01559_054717__29539054.smt2 |   30.151s | 1938.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|First_Welfare_Theorem/0006_Arrow_Debreu_Model/prob_00444_017502__24309052.smt2 |   30.151s | 2035.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Irrationality_J_Hancl/0013_Irrationality_J_Hancl/prob_00305_012099__16606612.smt2 |   30.151s | 1761.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0006_Poincare_Perpendicular/prob_00326_012039__13184780.smt2 |   30.153s | 1703.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Multi_Party_Computation/0025_OT14/prob_00723_035077__31789136.smt2 |   30.153s | 1852.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Bernoulli/0026_Bernoulli/prob_00260_009587__15648792.smt2    |   30.157s | 1719.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Probabilistic_Noninterference/0011_Compositionality/prob_00313_009061__26817430.smt2 |   30.161s | 2055.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Irrationality_J_Hancl/0013_Irrationality_J_Hancl/prob_00419_017502__17937076.smt2 |   30.161s | 1687.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|First_Welfare_Theorem/0007_Exchange_Economy/prob_00090_002258__24967246.smt2 |   30.163s | 1694.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|First_Welfare_Theorem/0004_Utility_Functions/prob_00032_000724__17067220.smt2 |   30.163s | 1994.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0003_Poincare_Distance/prob_00202_007847__9224746.smt2 |   30.165s | 2134.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Probabilistic_Noninterference/0011_Compositionality/prob_00110_002928__26316202.smt2 |   30.165s | 2013.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|First_Welfare_Theorem/0007_Exchange_Economy/prob_00200_006501__26157640.smt2 |   30.165s | 1941.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0002_Hyperbolic_Functions/prob_00094_003244__8971320.smt2 |   30.167s | 2595.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|CryptHOL/0034_GPV_Expectation/prob_00370_017101__30182554.smt2 |   30.167s | 2312.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Triangle/0001_Triangle/prob_00128_004818__20181640.smt2      |   30.170s | 2008.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|CryptHOL/0034_GPV_Expectation/prob_00517_025547__30490368.smt2 |   30.170s | 1678.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0003_SG_Library_Complement/prob_00355_012048__15376616.smt2 |   30.170s | 2166.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|First_Welfare_Theorem/0006_Arrow_Debreu_Model/prob_00217_007646__21334304.smt2 |   30.172s | 2075.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|List_Update/0010_BIT/prob_00464_017622__17840330.smt2        |   30.172s | 2059.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|List_Update/0021_BIT_2comp_on2/prob_00914_036597__26572128.smt2 |   30.173s | 2097.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Triangle/0000_Angles/prob_00193_006813__16735838.smt2        |   30.176s | 2133.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|First_Welfare_Theorem/0006_Arrow_Debreu_Model/prob_00423_016449__23978786.smt2 |   30.176s | 2290.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Cauchy/0001_CauchysMeanTheorem/prob_00362_011190__9462410.smt2 |   30.179s | 3497.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0005_Poincare_Lines_Axis_Intersections/prob_00869_032973__12313356.smt2 |   30.180s | 1867.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|CryptHOL/0028_Misc_CryptHOL/prob_02223_086610__19686318.smt2 |   30.181s | 1878.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Irrationality_J_Hancl/0013_Irrationality_J_Hancl/prob_00347_014052__17035432.smt2 |   30.181s | 2246.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0012_Transfer_Operator/prob_00101_005122__24879708.smt2 |   30.182s | 2001.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|First_Welfare_Theorem/0006_Arrow_Debreu_Model/prob_00230_008158__21473788.smt2 |   30.184s | 1835.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Density_Compiler/0001_PDF_Transformations/prob_00222_010426__16074426.smt2 |   30.185s | 2203.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0010_Gouezel_Karlsson/prob_00690_038535__23860096.smt2 |   30.186s | 2088.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Probabilistic_Noninterference/0010_Resumption_Based/prob_01033_031493__24467020.smt2 |   30.187s | 2177.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|First_Welfare_Theorem/0007_Exchange_Economy/prob_00113_003184__25110358.smt2 |   30.187s | 2177.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0006_Recurrence/prob_00417_018859__17969466.smt2 |   30.188s | 2273.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|List_Update/0002_Bit_Strings/prob_00228_010478__15439758.smt2 |   30.188s | 2160.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Cauchy/0001_CauchysMeanTheorem/prob_00582_018576__12406090.smt2 |   30.189s | 2172.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Probabilistic_Noninterference/0012_Syntactic_Criteria/prob_00077_002478__32204280.smt2 |   30.197s | 2406.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Grothendieck_Schemes/0007_Comm_Ring/prob_02695_114316__12926618.smt2 |   30.200s | 2531.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Density_Compiler/0005_PDF_Compiler_Pred/prob_00224_011976__22268420.smt2 |   30.201s | 3068.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|First_Welfare_Theorem/0004_Utility_Functions/prob_00108_003118__17794032.smt2 |   30.202s | 2415.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Triangle/0000_Angles/prob_00098_003214__14178910.smt2        |   30.203s | 2216.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Prime_Distribution_Elementary/0003_Lcm_Nat_Upto/prob_00087_003476__16736208.smt2 |   30.209s | 2183.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|First_Welfare_Theorem/0004_Utility_Functions/prob_00318_009801__19487020.smt2 |   30.210s | 2322.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Cauchy/0001_CauchysMeanTheorem/prob_00516_016371__11460672.smt2 |   30.210s | 3779.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|First_Welfare_Theorem/0004_Utility_Functions/prob_00298_009267__19328794.smt2 |   30.213s | 2345.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Cauchy/0001_CauchysMeanTheorem/prob_00377_011748__9849396.smt2 |   30.213s | 2468.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Prime_Distribution_Elementary/0012_Moebius_Mu_Sum/prob_00495_022762__21800226.smt2 |   30.217s | 2683.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0007_Invariants/prob_00668_033793__20275306.smt2 |   30.217s | 2247.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0010_Gouezel_Karlsson/prob_01358_076054__24384834.smt2 |   30.219s | 2905.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0000_Fekete/prob_00431_018321__15013066.smt2  |   30.220s | 3041.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0004_Poincare_Between/prob_01180_042344__11314978.smt2 |   30.221s | 2172.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Density_Compiler/0006_PDF_Target_Semantics/prob_01090_048757__26580100.smt2 |   30.222s | 2543.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|First_Welfare_Theorem/0007_Exchange_Economy/prob_00130_003756__25284924.smt2 |   30.225s | 3136.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Multi_Party_Computation/0022_ETP_OT/prob_00286_010989__24921056.smt2 |   30.229s | 3101.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Bernoulli/0034_Periodic_Bernpoly/prob_00083_003384__16193274.smt2 |   30.230s | 2802.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|First_Welfare_Theorem/0007_Exchange_Economy/prob_00301_010718__27060766.smt2 |   30.233s | 2437.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Triangle/0000_Angles/prob_00164_005816__15954278.smt2        |   30.235s | 2858.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Triangle/0000_Angles/prob_00058_001799__13297374.smt2        |   30.235s | 2653.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|List_Update/0022_TS/prob_01367_054255__30212328.smt2         |   30.237s | 2589.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Grothendieck_Schemes/0006_Topological_Space/prob_00238_008401__7469626.smt2 |   30.243s | 2430.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Probabilistic_Noninterference/0011_Compositionality/prob_02524_091520__31923830.smt2 |   30.250s | 3029.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Probabilistic_Noninterference/0009_Language_Semantics/prob_01386_045712__21792544.smt2 |   30.253s | 2759.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0009_Kingman/prob_00388_016181__21953906.smt2 |   30.259s | 2940.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Triangle/0000_Angles/prob_00077_002374__13668202.smt2        |   30.260s | 2834.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Triangle/0000_Angles/prob_00246_009058__18525198.smt2        |   30.260s | 2960.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Grothendieck_Schemes/0007_Comm_Ring/prob_05528_250740__20248438.smt2 |   30.262s | 3147.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0001_Poincare_Lines_Ideal_Points/prob_00128_005156__8228798.smt2 |   30.262s | 2653.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Irrationality_J_Hancl/0013_Irrationality_J_Hancl/prob_00535_022434__19405424.smt2 |   30.266s | 2994.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|First_Welfare_Theorem/0003_Preferences/prob_00496_016080__15661980.smt2 |   30.267s | 3741.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Prime_Distribution_Elementary/0009_Summatory_Divisor_Sigma_Bounds/prob_00281_014803__20035136.smt2 |   30.267s | 3657.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0006_Poincare_Perpendicular/prob_00500_017972__13451794.smt2 |   30.270s | 3523.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0005_Poincare_Lines_Axis_Intersections/prob_00514_019162__11852230.smt2 |   30.272s | 2724.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|List_Update/0022_TS/prob_01018_039497__29408430.smt2         |   30.272s | 2944.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Density_Compiler/0004_PDF_Density_Contexts/prob_00556_026860__20668342.smt2 |   30.275s | 3512.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0001_Poincare_Lines_Ideal_Points/prob_00308_011251__8537178.smt2 |   30.278s | 3168.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|First_Welfare_Theorem/0006_Arrow_Debreu_Model/prob_00283_010277__21960850.smt2 |   30.280s | 4455.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|First_Welfare_Theorem/0007_Exchange_Economy/prob_00312_011212__27242088.smt2 |   30.280s | 3363.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0006_Recurrence/prob_00554_025961__18068586.smt2 |   30.284s | 3665.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Triangle/0000_Angles/prob_00166_005925__16144952.smt2        |   30.288s | 3224.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|First_Welfare_Theorem/0006_Arrow_Debreu_Model/prob_00295_010768__22148420.smt2 |   30.290s | 3370.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Multi_Party_Computation/0022_ETP_OT/prob_00336_014374__25604674.smt2 |   30.291s | 4139.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Density_Compiler/0003_PDF_Semantics/prob_00158_006693__18071592.smt2 |   30.293s | 2855.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Triangle/0000_Angles/prob_00136_004629__14911176.smt2        |   30.294s | 3038.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Density_Compiler/0008_PDF_Compiler/prob_00442_024447__28141634.smt2 |   30.295s | 3767.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Probabilistic_Noninterference/0011_Compositionality/prob_02553_092841__32059358.smt2 |   30.297s | 3341.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Cauchy/0001_CauchysMeanTheorem/prob_00667_021755__13213822.smt2 |   30.298s | 3636.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Density_Compiler/0008_PDF_Compiler/prob_00488_026968__28268252.smt2 |   30.301s | 3679.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0010_Poincare_Tarski/prob_00237_008374__14446006.smt2 |   30.302s | 3643.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0007_Invariants/prob_01566_087166__21015824.smt2 |   30.302s | 4111.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0006_Recurrence/prob_01343_066021__18700866.smt2 |   30.303s | 3625.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0007_Invariants/prob_01052_056882__20568120.smt2 |   30.306s | 5268.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Cauchy/0000_CauchySchwarz/prob_00099_002601__5442682.smt2    |   30.306s | 3969.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Density_Compiler/0000_Density_Predicates/prob_00106_003841__15051888.smt2 |   30.307s | 3433.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Multi_Party_Computation/0025_OT14/prob_00371_021874__30604094.smt2 |   30.307s | 3829.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Triangle/0000_Angles/prob_00188_006638__16580436.smt2        |   30.308s | 3845.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|CryptHOL/0028_Misc_CryptHOL/prob_01910_072062__19303108.smt2 |   30.308s | 3608.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0010_Poincare_Tarski/prob_00015_000436__14281284.smt2 |   30.310s | 3890.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Cauchy/0001_CauchysMeanTheorem/prob_00532_016676__11729816.smt2 |   30.310s | 5431.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0004_Poincare_Between/prob_00255_008987__10532680.smt2 |   30.311s | 3758.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Multi_Party_Computation/0026_GMW/prob_00307_015888__32777646.smt2 |   30.315s | 5556.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Triangle/0000_Angles/prob_00032_000847__12631816.smt2        |   30.317s | 3901.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0005_Measure_Preserving_Transformations/prob_00726_028140__17090982.smt2 |   30.317s | 3044.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Prime_Distribution_Elementary/0012_Moebius_Mu_Sum/prob_00353_015554__21617384.smt2 |   30.320s | 3727.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Grothendieck_Schemes/0007_Comm_Ring/prob_02323_099215__12036334.smt2 |   30.323s | 3890.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|List_Update/0012_List_Factoring/prob_00631_027439__20343160.smt2 |   30.325s | 3318.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Probabilistic_Noninterference/0011_Compositionality/prob_00179_004986__26465470.smt2 |   30.330s | 4189.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Irrationality_J_Hancl/0013_Irrationality_J_Hancl/prob_00434_018177__18127332.smt2 |   30.330s | 4145.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Multi_Party_Computation/0026_GMW/prob_00117_005023__32035810.smt2 |   30.330s | 4195.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Triangle/0000_Angles/prob_00048_001464__13036518.smt2        |   30.332s | 3976.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Irrationality_J_Hancl/0013_Irrationality_J_Hancl/prob_00353_014302__17135036.smt2 |   30.332s | 3785.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0004_Poincare_Between/prob_00974_035582__11158806.smt2 |   30.340s | 3680.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Probabilistic_Noninterference/0010_Resumption_Based/prob_01335_041960__25233324.smt2 |   30.341s | 4245.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0007_Invariants/prob_00768_039895__20365060.smt2 |   30.343s | 3951.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Triangle/0000_Angles/prob_00038_001067__12778558.smt2        |   30.347s | 4298.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0009_Kingman/prob_00700_032288__22149832.smt2 |   30.347s | 4312.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Probabilistic_Noninterference/0011_Compositionality/prob_00478_014893__27309312.smt2 |   30.349s | 3956.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Cauchy/0001_CauchysMeanTheorem/prob_00524_016523__11564876.smt2 |   30.352s | 6168.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0003_SG_Library_Complement/prob_00187_005490__15288828.smt2 |   30.353s | 3716.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Multi_Party_Computation/0022_ETP_OT/prob_00421_017972__26052468.smt2 |   30.354s | 4274.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Triangle/0001_Triangle/prob_00146_005754__20672348.smt2      |   30.355s | 4729.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Multi_Party_Computation/0023_ETP_RSA_OT/prob_00548_021107__27360890.smt2 |   30.357s | 4273.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|CryptHOL/0034_GPV_Expectation/prob_00333_015306__30035744.smt2 |   30.359s | 4223.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Bernoulli/0034_Periodic_Bernpoly/prob_00172_007074__16713832.smt2 |   30.361s | 4342.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0009_Kingman/prob_01940_099273__23207710.smt2 |   30.362s | 4031.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Density_Compiler/0008_PDF_Compiler/prob_00166_009595__27559810.smt2 |   30.364s | 4329.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0007_Invariants/prob_01683_094509__21154038.smt2 |   30.365s | 4826.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0006_Recurrence/prob_01562_078403__18888778.smt2 |   30.367s | 4651.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0013_Normalizing_Sequences/prob_00085_004111__25439504.smt2 |   30.369s | 4780.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Probabilistic_Noninterference/0010_Resumption_Based/prob_01188_035723__24736740.smt2 |   30.370s | 4122.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Triangle/0000_Angles/prob_00190_006708__16655116.smt2        |   30.373s | 4454.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0010_Poincare_Tarski/prob_01598_057557__15782202.smt2 |   30.373s | 4138.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0010_Gouezel_Karlsson/prob_01029_057738__24127682.smt2 |   30.384s | 3868.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0010_Poincare_Tarski/prob_00494_017990__14654646.smt2 |   30.385s | 4166.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0005_Poincare_Lines_Axis_Intersections/prob_00951_036233__12520788.smt2 |   30.390s | 4093.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Probabilistic_Noninterference/0014_Trace_Based/prob_00062_002431__32524194.smt2 |   30.390s | 4242.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Prime_Distribution_Elementary/0005_Prime_Distribution_Elementary_Library/prob_00193_006855__17103382.smt2 |   30.390s | 5205.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|CryptHOL/0028_Misc_CryptHOL/prob_01587_058874__18909308.smt2 |   30.394s | 4400.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0012_Transfer_Operator/prob_00381_020909__25115296.smt2 |   30.397s | 4776.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0006_Recurrence/prob_00257_011854__17866844.smt2 |   30.398s | 4633.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0007_Poincare_Circles/prob_00333_012038__13822018.smt2 |   30.399s | 3668.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Grothendieck_Schemes/0007_Comm_Ring/prob_02770_117146__13102388.smt2 |   30.401s | 4463.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Triangle/0000_Angles/prob_00165_005865__16049148.smt2        |   30.402s | 4572.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|First_Welfare_Theorem/0004_Utility_Functions/prob_00121_003531__17924170.smt2 |   30.404s | 4321.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|First_Welfare_Theorem/0006_Arrow_Debreu_Model/prob_00256_009173__21773854.smt2 |   30.406s | 4693.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Multi_Party_Computation/0022_ETP_OT/prob_00299_011738__25200480.smt2 |   30.408s | 5177.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Polynomial_Interpolation/0003_Missing_Polynomial/prob_00557_019337__16250646.smt2 |   30.415s | 4207.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Irrationality_J_Hancl/0013_Irrationality_J_Hancl/prob_00403_016777__17622816.smt2 |   30.422s | 5025.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Bernoulli/0026_Bernoulli/prob_00035_001037__14823946.smt2    |   30.431s | 4487.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|First_Welfare_Theorem/0006_Arrow_Debreu_Model/prob_00204_007175__21162298.smt2 |   30.434s | 4941.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Triangle/0001_Triangle/prob_00133_005016__20356230.smt2      |   30.438s | 5382.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0007_Invariants/prob_01431_079438__20870258.smt2 |   30.440s | 5296.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Triangle/0000_Angles/prob_00114_003793__14438778.smt2        |   30.444s | 5096.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0004_Poincare_Between/prob_00578_020854__10861840.smt2 |   30.446s | 4501.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Triangle/0001_Triangle/prob_00130_004912__20274220.smt2      |   30.447s | 5366.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|List_Update/0019_OPT2/prob_00367_017388__24169644.smt2       |   30.447s | 4305.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Triangle/0000_Angles/prob_00025_000613__12467908.smt2        |   30.456s | 4829.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Multi_Party_Computation/0026_GMW/prob_00348_018229__33000010.smt2 |   30.458s | 7331.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Prime_Distribution_Elementary/0012_Moebius_Mu_Sum/prob_00304_013111__21549076.smt2 |   30.463s | 4687.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0003_Poincare_Distance/prob_01269_047732__10126656.smt2 |   30.468s | 4606.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|First_Welfare_Theorem/0006_Arrow_Debreu_Model/prob_00368_014168__23345856.smt2 |   30.469s | 5417.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Triangle/0001_Triangle/prob_00164_006180__20753524.smt2      |   30.472s | 5547.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Multi_Party_Computation/0025_OT14/prob_00675_032988__31564808.smt2 |   30.479s | 5921.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Prime_Distribution_Elementary/0005_Prime_Distribution_Elementary_Library/prob_00545_021982__17605406.smt2 |   30.481s | 7357.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|List_Update/0010_BIT/prob_01466_068132__19011292.smt2        |   30.481s | 4993.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|First_Welfare_Theorem/0006_Arrow_Debreu_Model/prob_00331_012189__22710714.smt2 |   30.484s | 5076.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0012_Transfer_Operator/prob_00511_028341__25211936.smt2 |   30.484s | 5292.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|First_Welfare_Theorem/0006_Arrow_Debreu_Model/prob_00346_012969__22905330.smt2 |   30.488s | 5858.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Density_Compiler/0000_Density_Predicates/prob_00438_019560__15635664.smt2 |   30.488s | 5239.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Probabilistic_Noninterference/0011_Compositionality/prob_02335_083623__31385608.smt2 |   30.490s | 5077.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Multi_Party_Computation/0025_OT14/prob_00038_001931__28421918.smt2 |   30.494s | 5355.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Bertrands_Postulate/0004_Bertrand/prob_01197_048460__19839176.smt2 |   30.495s | 4776.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Grothendieck_Schemes/0007_Comm_Ring/prob_05600_255192__20540052.smt2 |   30.498s | 5318.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Multi_Party_Computation/0022_ETP_OT/prob_00199_007326__24734122.smt2 |   30.498s | 5864.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Grothendieck_Schemes/0007_Comm_Ring/prob_03557_153570__14994142.smt2 |   30.499s | 5170.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0006_Recurrence/prob_00718_033513__18197032.smt2 |   30.501s | 5236.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Prime_Distribution_Elementary/0013_Selberg_Asymptotic_Formula/prob_00084_003792__21887946.smt2 |   30.515s | 5101.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Triangle/0000_Angles/prob_00090_002887__14011208.smt2        |   30.519s | 8660.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Multi_Party_Computation/0022_ETP_OT/prob_00373_016107__25870084.smt2 |   30.520s | 6662.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Grothendieck_Schemes/0007_Comm_Ring/prob_05128_225779__19020418.smt2 |   30.542s | 5496.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Bernoulli/0034_Periodic_Bernpoly/prob_00159_006347__16602946.smt2 |   30.546s | 6020.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Prime_Distribution_Elementary/0008_Partial_Zeta_Bounds/prob_00335_015442__19615824.smt2 |   30.552s | 6969.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0007_Invariants/prob_01322_072742__20738802.smt2 |   30.557s | 7460.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Prime_Distribution_Elementary/0011_Shapiro_Tauberian/prob_00273_011115__20933730.smt2 |   30.558s | 9316.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Prime_Distribution_Elementary/0012_Moebius_Mu_Sum/prob_00217_009135__21413404.smt2 |   30.559s | 5540.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|List_Update/0010_BIT/prob_01240_055758__18696692.smt2        |   30.559s | 5939.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Multi_Party_Computation/0026_GMW/prob_00060_003072__31850836.smt2 |   30.561s | 7263.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0001_Poincare_Lines_Ideal_Points/prob_00391_014450__8720110.smt2 |   30.564s | 5626.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|CryptHOL/0034_GPV_Expectation/prob_00427_020171__30316956.smt2 |   30.566s | 7571.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0003_Poincare_Distance/prob_01358_050902__10187858.smt2 |   30.570s | 6730.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Irrationality_J_Hancl/0013_Irrationality_J_Hancl/prob_00151_005929__14659406.smt2 |   30.573s | 8194.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|CryptHOL/0034_GPV_Expectation/prob_00229_009941__29862460.smt2 |   30.574s | 7426.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Ergodic_Theory/0009_Kingman/prob_00909_043734__22429890.smt2 |   30.574s | 7452.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Grothendieck_Schemes/0007_Comm_Ring/prob_00861_034679__8719566.smt2 |   30.575s | 7567.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0010_Poincare_Tarski/prob_01692_061102__15878502.smt2 |   30.580s | 8083.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Irrationality_J_Hancl/0013_Irrationality_J_Hancl/prob_00414_017336__17829016.smt2 |   30.580s | 7168.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0000_Poincare_Lines/prob_01012_039616__7043180.smt2 |   30.581s | 6051.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Prime_Distribution_Elementary/0012_Moebius_Mu_Sum/prob_00252_010728__21473410.smt2 |   30.611s | 6824.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Triangle/0000_Angles/prob_00158_005575__15688166.smt2        |   30.614s | 6452.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Multi_Party_Computation/0025_OT14/prob_00324_018548__30311684.smt2 |   30.615s | 7046.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Irrationality_J_Hancl/0013_Irrationality_J_Hancl/prob_00145_005609__14546418.smt2 |   30.625s | 7510.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Bertrands_Postulate/0004_Bertrand/prob_00076_002539__14152536.smt2 |   30.632s | 7811.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Probabilistic_Noninterference/0014_Trace_Based/prob_00141_004759__32674902.smt2 |   30.635s | 7399.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|List_Update/0001_Prob_Theory/prob_00370_014343__15144340.smt2 |   30.659s | 7026.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Multi_Party_Computation/0026_GMW/prob_00279_013124__32485930.smt2 |   30.684s | 8765.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|First_Welfare_Theorem/0006_Arrow_Debreu_Model/prob_00411_015918__23877318.smt2 |   30.695s | 7333.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Poincare_Disc/0005_Poincare_Lines_Axis_Intersections/prob_00410_016040__11714800.smt2 |   30.744s | 7944.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Irrationality_J_Hancl/0013_Irrationality_J_Hancl/prob_00409_017081__17725240.smt2 |   30.767s | 9287.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
|Prime_Distribution_Elementary/0015_PNT_Consequences/prob_00541_021933__22921526.smt2 |   30.981s | 16.287GiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711977272-1-run-all-AYM8QM |
