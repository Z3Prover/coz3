# .

* SAT 0
* UNSAT 82
* TIMEOUT 0
* UNKNOWN 0

* ERRORS 205 (signal-15:205)

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-10-28 17:58:17 UTC
Job description: Pinned full public LPAR cohort; limit; master; 30s solver and Ramon limits; four independent solver workers; not the paper's filtered cohort or four-core portfolio
Job tag: lpar26-20261028-30s-r3-master-limit
Runner: rise-runner-1
Z3 repo: Z3Prover/z3
Z3 commit: 2b25abc65ca59569450f56ea1c988638083cb800
Z3 branch: master
Z3 options: "-T:30 -st"
Ramon options: "--limit-time 30"
Input preset: alasca-lpar-2026
Z3 inputs: https://github.com/vprover/vampire_benchmarks/archive/6563baa75aea885cee14db5d6dfcff1cf3b50a1c.tar.gz alasca-lpar-2026/limit
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
|crafted/lim_c.smt2                                           |    0.013s | 20.552MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f4_a3_c8_v2__2.smt2  |    0.013s | 20.844MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f4_a3_c8_v8__0.smt2  |    0.013s | 20.804MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f8_a3_c16_v2__7.smt2 |    0.014s | 20.804MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f8_a3_c16_v2__3.smt2 |    0.014s | 20.808MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f4_a3_c8_v2__9.smt2  |    0.014s | 20.804MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f8_a3_c16_v2__1.smt2 |    0.014s | 20.808MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f8_a3_c16_v8__0.smt2 |    0.014s | 20.808MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f8_a3_c16_v2__2.smt2 |    0.015s | 20.804MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f8_a3_c16_v8__1.smt2 |    0.015s | 20.304MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f4_a3_c8_v8__4.smt2  |    0.015s | 20.8MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f8_a3_c16_v2__9.smt2 |    0.025s | 20.784MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f4_a3_c8_v2__2.smt2  |    0.026s | 20.568MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f8_a3_c16_v8__6.smt2 |    0.027s | 20.86MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f8_a3_c16_v8__5.smt2 |    0.027s | 20.56MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f8_a3_c16_v8__5.smt2 |    0.027s | 20.804MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f8_a3_c16_v2__8.smt2 |    0.028s | 20.808MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f8_a3_c16_v8__4.smt2 |    0.028s | 20.552MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f8_a3_c16_v8__3.smt2 |    0.028s | 20.56MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f8_a3_c16_v2__5.smt2 |    0.028s | 20.812MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f8_a3_c16_v2__6.smt2 |    0.028s | 20.556MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f4_a3_c8_v2__0.smt2  |    0.028s | 20.944MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f4_a3_c8_v2__6.smt2  |    0.029s | 20.804MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f4_a3_c8_v2__7.smt2  |    0.029s | 20.804MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f8_a3_c16_v2__6.smt2 |    0.029s | 20.808MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f8_a3_c16_v8__8.smt2 |    0.029s | 20.552MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f8_a3_c16_v8__8.smt2 |    0.029s | 20.804MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f8_a3_c16_v8__4.smt2 |    0.029s | 20.804MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f4_a3_c8_v2__7.smt2  |    0.029s | 20.56MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f4_a3_c8_v8__1.smt2  |    0.029s | 20.58MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f4_a3_c8_v2__1.smt2  |    0.029s | 20.552MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f8_a3_c16_v8__7.smt2 |    0.029s | 20.688MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f4_a3_c8_v2__4.smt2  |    0.029s | 20.824MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f4_a3_c8_v2__5.smt2  |    0.029s | 20.804MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f8_a3_c16_v2__8.smt2 |    0.029s | 20.556MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f4_a3_c8_v2__3.smt2  |    0.029s | 21.028MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f8_a3_c16_v2__0.smt2 |    0.030s | 20.552MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f4_a3_c8_v8__3.smt2  |    0.030s | 20.792MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f8_a3_c16_v2__1.smt2 |    0.030s | 20.556MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f4_a3_c8_v8__9.smt2  |    0.030s | 20.844MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f4_a3_c8_v8__4.smt2  |    0.030s | 20.804MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f4_a3_c8_v8__7.smt2  |    0.030s | 20.56MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f8_a3_c16_v2__9.smt2 |    0.031s | 20.56MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f8_a3_c16_v8__6.smt2 |    0.031s | 20.816MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f8_a3_c16_v8__2.smt2 |    0.031s | 20.804MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f4_a3_c8_v2__3.smt2  |    0.031s | 20.804MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f8_a3_c16_v2__4.smt2 |    0.031s | 20.804MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f4_a3_c8_v8__0.smt2  |    0.031s | 20.916MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f8_a3_c16_v8__0.smt2 |    0.031s | 20.552MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f4_a3_c8_v2__4.smt2  |    0.031s | 20.552MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f4_a3_c8_v8__8.smt2  |    0.031s | 20.804MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f8_a3_c16_v8__9.smt2 |    0.032s | 20.556MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f4_a3_c8_v2__9.smt2  |    0.032s | 21.036MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f4_a3_c8_v2__8.smt2  |    0.032s | 20.552MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f4_a3_c8_v8__7.smt2  |    0.032s | 20.556MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f4_a3_c8_v8__2.smt2  |    0.032s | 20.784MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f8_a3_c16_v2__7.smt2 |    0.032s | 20.8MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f4_a3_c8_v2__8.smt2  |    0.032s | 20.816MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f8_a3_c16_v8__1.smt2 |    0.032s | 20.8MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f8_a3_c16_v2__0.smt2 |    0.032s | 20.804MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f4_a3_c8_v8__5.smt2  |    0.033s | 21.032MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f8_a3_c16_v8__3.smt2 |    0.033s | 20.832MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f4_a3_c8_v8__5.smt2  |    0.033s | 20.804MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f4_a3_c8_v2__5.smt2  |    0.033s | 20.804MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f8_a3_c16_v8__9.smt2 |    0.033s | 20.804MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f4_a3_c8_v8__6.smt2  |    0.033s | 20.78MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f8_a3_c16_v2__3.smt2 |    0.033s | 20.8MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f4_a3_c8_v2__6.smt2  |    0.033s | 20.6MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f4_a3_c8_v8__6.smt2  |    0.034s | 20.804MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f4_a3_c8_v2__0.smt2  |    0.034s | 20.8MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f8_a3_c16_v2__4.smt2 |    0.034s | 20.8MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f4_a3_c8_v8__3.smt2  |    0.035s | 20.776MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f4_a3_c8_v8__2.smt2  |    0.035s | 20.56MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f8_a3_c16_v2__2.smt2 |    0.035s | 21.064MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f8_a3_c16_v2__5.smt2 |    0.035s | 20.556MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f4_a3_c8_v8__8.smt2  |    0.035s | 20.804MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f4_a3_c8_v8__1.smt2  |    0.035s | 20.8MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f4_a3_c8_v8__9.smt2  |    0.035s | 20.56MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz64_f8_a3_c16_v8__7.smt2 |    0.036s | 20.808MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f8_a3_c16_v8__2.smt2 |    0.037s | 20.8MiB| unsat | 0 |  |
|irrelevant_vars/lim_irrelevant_var_sz16_f4_a3_c8_v2__1.smt2  |    0.037s | 20.552MiB| unsat | 0 |  |
|const_mul/lim_-20_84_x.smt2                                  |    0.404s | 29.092MiB| unsat | 0 |  |
|crafted/lim_k_fx.smt2                                        |   30.003s | 30.756MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz16__4.smt2          |   30.006s | 69.04MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz8__1.smt2           |   30.006s | 71.052MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m128__2.smt2                    |   30.006s | 62.108MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|const_mul/lim_-79_8_x.smt2                                   |   30.007s | 81.992MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz32__8.smt2          |   30.007s | 90.168MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz16__7.smt2          |   30.007s | 66.14MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz32__6.smt2          |   30.007s | 75.036MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz16__9.smt2          |   30.007s | 69.332MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz16__6.smt2          |   30.007s | 76.192MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz16__1.smt2          |   30.007s | 76.944MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz16__5.smt2          |   30.007s | 81.772MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz8__7.smt2           |   30.007s | 79.652MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz16__2.smt2          |   30.007s | 69.212MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz32__4.smt2          |   30.007s | 70.72MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz32__3.smt2          |   30.007s | 68.916MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz32__9.smt2          |   30.007s | 75.5MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz8__6.smt2           |   30.007s | 71.516MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz64__3.smt2          |   30.007s | 84.78MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m128__5.smt2                    |   30.007s | 72.348MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m32__6.smt2                     |   30.007s | 80.356MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m32__8.smt2                     |   30.007s | 61.3MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m64__0.smt2                     |   30.007s | 82.224MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m8__2.smt2                      |   30.007s | 78.484MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m16__1.smt2                     |   30.007s | 76.16MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m32__8.smt2                     |   30.007s | 75.704MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m8__6.smt2                      |   30.007s | 80.772MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m64__2.smt2                     |   30.007s | 69.408MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m32__5.smt2                     |   30.007s | 74.212MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m32__6.smt2                     |   30.007s | 69.424MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m32__7.smt2                     |   30.007s | 83.26MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m32__3.smt2                     |   30.007s | 81.06MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m8__0.smt2                      |   30.007s | 70.512MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|const_mul/lim_-43_100_x.smt2                                 |   30.008s | 86.3MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|const_mul/lim_-90_24_x.smt2                                  |   30.008s | 90.688MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|const_mul/lim_-3_64_x.smt2                                   |   30.008s | 88.596MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|const_mul/lim_96_82_x.smt2                                   |   30.008s | 84.076MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz16__6.smt2          |   30.008s | 87.476MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz64__1.smt2          |   30.008s | 83.832MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz32__5.smt2          |   30.008s | 88.568MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz8__0.smt2           |   30.008s | 75.604MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz16__3.smt2          |   30.008s | 77.492MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz32__0.smt2          |   30.008s | 86.832MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz16__5.smt2          |   30.008s | 86.176MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz64__6.smt2          |   30.008s | 83.064MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz64__2.smt2          |   30.008s | 79.544MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz32__7.smt2          |   30.008s | 81.4MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz16__2.smt2          |   30.008s | 89.42MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz16__8.smt2          |   30.008s | 69.404MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz16__4.smt2          |   30.008s | 92.364MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz64__9.smt2          |   30.008s | 84.86MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz32__5.smt2          |   30.008s | 88.72MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz16__0.smt2          |   30.008s | 77.268MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz8__8.smt2           |   30.008s | 85.376MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz32__8.smt2          |   30.008s | 84.672MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz64__4.smt2          |   30.008s | 84.796MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz64__5.smt2          |   30.008s | 83.576MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|crafted/lim_fgx.smt2                                         |   30.008s | 96.468MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m32__3.smt2                     |   30.008s | 86.848MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m64__7.smt2                     |   30.008s | 90.972MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m32__2.smt2                     |   30.008s | 81.236MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m16__9.smt2                     |   30.008s | 87.016MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m128__6.smt2                    |   30.008s | 91.356MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m64__6.smt2                     |   30.008s | 91.216MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m8__3.smt2                      |   30.008s | 80.488MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m16__5.smt2                     |   30.008s | 77.8MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m32__7.smt2                     |   30.008s | 81.588MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m8__8.smt2                      |   30.008s | 83.932MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m64__4.smt2                     |   30.008s | 87.588MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m64__0.smt2                     |   30.008s | 87.264MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m128__7.smt2                    |   30.008s | 94.984MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m64__3.smt2                     |   30.008s | 95.3MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m64__2.smt2                     |   30.008s | 99.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m64__1.smt2                     |   30.008s | 77.18MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|const_mul/lim_14_100_x.smt2                                  |   30.009s | 93.564MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|const_mul/lim_68_92_x.smt2                                   |   30.009s | 128.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|const_mul/lim_-75_89_x.smt2                                  |   30.009s | 107.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|const_mul/lim_44_17_x.smt2                                   |   30.009s | 97.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz16__3.smt2          |   30.009s | 110.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz32__1.smt2          |   30.009s | 91.808MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz16__8.smt2          |   30.009s | 90.012MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz16__1.smt2          |   30.009s | 85.676MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz8__5.smt2           |   30.009s | 80.952MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz64__0.smt2          |   30.009s | 81.528MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz16__7.smt2          |   30.009s | 90.74MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz16__0.smt2          |   30.009s | 99.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz8__4.smt2           |   30.009s | 100.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz16__9.smt2          |   30.009s | 99.884MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz32__1.smt2          |   30.009s | 89.596MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz8__3.smt2           |   30.009s | 88.688MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz64__7.smt2          |   30.009s | 80.76MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz32__7.smt2          |   30.009s | 86.34MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz32__2.smt2          |   30.009s | 87.872MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz32__9.smt2          |   30.009s | 91.232MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz8__9.smt2           |   30.009s | 98.108MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz64__8.smt2          |   30.009s | 86.328MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz32__2.smt2          |   30.009s | 89.692MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz32__0.smt2          |   30.009s | 93.88MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m16__7.smt2                     |   30.009s | 99.032MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m16__8.smt2                     |   30.009s | 91.344MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m64__1.smt2                     |   30.009s | 97.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m64__5.smt2                     |   30.009s | 100.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m32__1.smt2                     |   30.009s | 98.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m64__6.smt2                     |   30.009s | 97.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m16__6.smt2                     |   30.009s | 86.56MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m32__3.smt2                     |   30.009s | 98.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m128__4.smt2                    |   30.009s | 100.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m32__5.smt2                     |   30.009s | 98.356MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m16__7.smt2                     |   30.009s | 92.052MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m128__3.smt2                    |   30.009s | 104.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m64__9.smt2                     |   30.009s | 102.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m16__6.smt2                     |   30.009s | 98.548MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m64__9.smt2                     |   30.009s | 107.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m128__9.smt2                    |   30.009s | 86.648MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m16__0.smt2                     |   30.009s | 97.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m32__7.smt2                     |   30.009s | 106.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|const_mul/lim_-6_88_x.smt2                                   |   30.010s | 119.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|const_mul/lim_67_79_x.smt2                                   |   30.010s | 120.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz64__1.smt2          |   30.010s | 114.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz8__2.smt2           |   30.010s | 98.252MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz32__6.smt2          |   30.010s | 100.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz64__4.smt2          |   30.010s | 117.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz64__5.smt2          |   30.010s | 116.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz64__3.smt2          |   30.010s | 119.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz32__4.smt2          |   30.010s | 96.66MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz64__6.smt2          |   30.010s | 118.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz32__3.smt2          |   30.010s | 108.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m64__7.smt2                     |   30.010s | 100.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m32__0.smt2                     |   30.010s | 107.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m16__1.smt2                     |   30.010s | 113.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m64__3.smt2                     |   30.010s | 109.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m32__8.smt2                     |   30.010s | 111.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m16__5.smt2                     |   30.010s | 134.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m16__3.smt2                     |   30.010s | 123.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m16__4.smt2                     |   30.010s | 107.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m64__5.smt2                     |   30.010s | 121.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m32__4.smt2                     |   30.010s | 132.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m64__8.smt2                     |   30.010s | 142.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|const_mul/lim_-70_86_x.smt2                                  |   30.011s | 132.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz64__8.smt2          |   30.011s | 118.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz32__0.smt2          |   30.011s | 119.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz64__0.smt2          |   30.011s | 122.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz64__7.smt2          |   30.011s | 117.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz32__5.smt2          |   30.011s | 125.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz32__8.smt2          |   30.011s | 136.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz64__2.smt2          |   30.011s | 118.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f4_sz64__9.smt2          |   30.011s | 118.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|crafted/lim_sum.smt2                                         |   30.011s | 142.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|crafted/lim_x.smt2                                           |   30.011s | 167.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m8__5.smt2                      |   30.011s | 124.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m128__1.smt2                    |   30.011s | 152.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m32__1.smt2                     |   30.011s | 118.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m64__8.smt2                     |   30.011s | 120.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m16__0.smt2                     |   30.011s | 109.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m32__5.smt2                     |   30.011s | 141.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m32__9.smt2                     |   30.011s | 133.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m16__3.smt2                     |   30.011s | 139.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m8__4.smt2                      |   30.011s | 143.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m32__4.smt2                     |   30.011s | 127.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz32__2.smt2          |   30.012s | 135.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz32__9.smt2          |   30.012s | 145.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz32__7.smt2          |   30.012s | 109.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m128__8.smt2                    |   30.012s | 153.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m32__6.smt2                     |   30.012s | 137.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m32__0.smt2                     |   30.012s | 156.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m32__1.smt2                     |   30.012s | 142.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m32__4.smt2                     |   30.012s | 140.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m16__2.smt2                     |   30.012s | 150.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m16__4.smt2                     |   30.012s | 150.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m32__9.smt2                     |   30.012s | 157.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|const_mul/lim_-93_47_x.smt2                                  |   30.013s | 151.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|const_mul/lim_-26_59_x.smt2                                  |   30.013s | 208.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz32__4.smt2          |   30.013s | 162.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz32__1.smt2          |   30.013s | 171.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz32__6.smt2          |   30.013s | 144.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f2_sz32__3.smt2          |   30.013s | 128.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m32__9.smt2                     |   30.013s | 147.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m32__2.smt2                     |   30.013s | 164.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m8__9.smt2                      |   30.013s | 145.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m16__8.smt2                     |   30.013s | 180.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|const_mul/lim_69_40_x.smt2                                   |   30.014s | 173.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m16__9.smt2                     |   30.014s | 155.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m128__0.smt2                    |   30.014s | 170.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m32__0.smt2                     |   30.014s | 169.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n8_m64__4.smt2                     |   30.014s | 166.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|const_mul/lim_81_56_x.smt2                                   |   30.015s | 222.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|const_mul/lim_-70_41_x.smt2                                  |   30.015s | 228.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|const_mul/lim_-98_53_x.smt2                                  |   30.015s | 226.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|crafted/lim_fxy.smt2                                         |   30.015s | 196.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz128__5.smt2         |   30.016s | 216.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz128__0.smt2         |   30.016s | 215.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz128__7.smt2         |   30.016s | 221.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz128__4.smt2         |   30.016s | 213.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m16__2.smt2                     |   30.016s | 246.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m8__7.smt2                      |   30.016s | 239.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n2_m8__1.smt2                      |   30.016s | 239.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|const_mul/lim_87_76_x.smt2                                   |   30.017s | 228.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz128__8.smt2         |   30.017s | 215.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz128__9.smt2         |   30.017s | 202.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz128__3.smt2         |   30.017s | 214.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz128__6.smt2         |   30.017s | 200.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|linear_limit/linear_limit_n4_m32__2.smt2                     |   30.017s | 245.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz128__1.smt2         |   30.018s | 210.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|composition_tower/composition_tower_f8_sz128__2.smt2         |   30.018s | 211.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
|crafted/lim_f_greater_lim_greater.smt2                       |   30.018s | 235.0MiB| signal-15 | 143 | ramon: cwd /home/ghrunner/actions-runner/_work/_temp/ramon-40711929234-1-run-all-gq6IyU |
