# .

* SAT 750
* UNSAT 1364
* TIMEOUT 325
* UNKNOWN 0

* ERRORS 0

# Meta data

<pre>
Ramon benchmark for Z3
-
Experiment date and time: 2026-10-28 19:39:41 UTC
Job description: Pinned full public LPAR cohort; LRA; master; 30s solver and Ramon limits; four independent solver workers; not the paper's filtered cohort or four-core portfolio
Job tag: lpar26-20261028-30s-r3-master-LRA
Runner: rise-runner-1
Z3 repo: Z3Prover/z3
Z3 commit: 2b25abc65ca59569450f56ea1c988638083cb800
Z3 branch: master
Z3 options: "-T:30 -st"
Ramon options: "--limit-time 30"
Input preset: alasca-lpar-2026
Z3 inputs: https://github.com/vprover/vampire_benchmarks/archive/6563baa75aea885cee14db5d6dfcff1cf3b50a1c.tar.gz alasca-lpar-2026/LRA
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
Report correction: regenerated from original raw logs with Z3Prover/bench@1250328d4; explicit Ramon time limits count as timeouts.
</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | INFO |
|------------|----------:|---------:|-------------:| ----------:|------|
|keymaera/intersection-example-simple.proof-node86313.smt2    |    0.011s | 19.536MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node431299.smt2   |    0.011s | 19.784MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node462599.smt2   |    0.011s | 19.532MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node22058.smt2    |    0.011s | 19.78MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_4_2.smt2                                |    0.012s | 19.528MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_3_12.smt2                               |    0.012s | 19.632MiB| unsat | 0 |  |
|tptp/ARI396=1.smt2                                           |    0.012s | 20.036MiB| unsat | 0 |  |
|tptp/ARI369=1.smt2                                           |    0.012s | 19.784MiB| unsat | 0 |  |
|tptp/ARI400=1.smt2                                           |    0.012s | 20.04MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node398905.smt2   |    0.012s | 19.78MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node25230.smt2    |    0.012s | 19.784MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node681835.smt2   |    0.012s | 20.036MiB| unsat | 0 |  |
|keymaera/water_tank-node18188.smt2                           |    0.012s | 19.948MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node679744.smt2   |    0.012s | 19.784MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node59054.smt2    |    0.012s | 19.8MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node404555.smt2   |    0.012s | 19.436MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node75451.smt2    |    0.012s | 19.784MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node22930.smt2    |    0.012s | 19.776MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node26275.smt2    |    0.012s | 19.772MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node396678.smt2   |    0.012s | 19.78MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node58285.smt2    |    0.012s | 19.784MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node419176.smt2   |    0.012s | 19.768MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node84760.smt2    |    0.012s | 19.056MiB| unsat | 0 |  |
|keymaera/water_tank-node29317.smt2                           |    0.012s | 19.54MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node683817.smt2   |    0.012s | 19.78MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node94741.smt2    |    0.012s | 19.788MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node22267.smt2    |    0.012s | 19.296MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node684031.smt2   |    0.012s | 19.28MiB| unsat | 0 |  |
|keymaera/water_tank-node11781.smt2                           |    0.012s | 20.036MiB| unsat | 0 |  |
|keymaera/water_tank-node19206.smt2                           |    0.012s | 20.036MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node432116.smt2   |    0.012s | 19.78MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node53427.smt2    |    0.012s | 20.032MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node404096.smt2   |    0.012s | 19.784MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node431411.smt2   |    0.012s | 19.444MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node679020.smt2   |    0.012s | 19.304MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node461247.smt2   |    0.012s | 20.036MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node683187.smt2   |    0.012s | 19.772MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node16571.smt2    |    0.012s | 19.78MiB| unsat | 0 |  |
|keymaera/water_tank-node23091.smt2                           |    0.012s | 19.784MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node6512.smt2     |    0.012s | 19.456MiB| unsat | 0 |  |
|keymaera/water_tank-node22289.smt2                           |    0.012s | 20.036MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node47263.smt2    |    0.012s | 19.78MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node23139.smt2    |    0.012s | 19.328MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node464724.smt2   |    0.012s | 19.896MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node6298.smt2     |    0.012s | 19.316MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node432437.smt2   |    0.012s | 19.78MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node454003.smt2   |    0.012s | 19.776MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node681947.smt2   |    0.012s | 19.52MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node76467.smt2    |    0.012s | 19.792MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node396892.smt2   |    0.012s | 19.784MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node679290.smt2   |    0.012s | 19.792MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node85544.smt2    |    0.012s | 19.516MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node463847.smt2   |    0.012s | 20.04MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node54507.smt2    |    0.012s | 20.032MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node5741.smt2     |    0.012s | 19.784MiB| unsat | 0 |  |
|keymaera/water_tank-node27679.smt2                           |    0.012s | 19.8MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node53134.smt2    |    0.012s | 19.788MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node62694.smt2    |    0.012s | 19.788MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node403143.smt2   |    0.012s | 19.528MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node71583.smt2    |    0.012s | 20.04MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node25994.smt2    |    0.012s | 19.78MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node422254.smt2   |    0.012s | 19.276MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node432325.smt2   |    0.012s | 19.416MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node70716.smt2    |    0.012s | 19.764MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node76579.smt2    |    0.012s | 20.036MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node57616.smt2    |    0.012s | 19.996MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node460167.smt2   |    0.012s | 20.04MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node16357.smt2    |    0.012s | 19.796MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node688613.smt2   |    0.012s | 19.74MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node24909.smt2    |    0.012s | 19.784MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node427730.smt2   |    0.012s | 19.524MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node99416.smt2    |    0.012s | 20.056MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node683401.smt2   |    0.012s | 19.524MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node407008.smt2   |    0.012s | 19.784MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node684597.smt2   |    0.012s | 19.78MiB| unsat | 0 |  |
|keymaera/water_tank-node30158.smt2                           |    0.012s | 19.632MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node98400.smt2    |    0.012s | 19.536MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node93665.smt2    |    0.012s | 19.536MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node54186.smt2    |    0.012s | 19.528MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node431090.smt2   |    0.012s | 19.792MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node63249.smt2    |    0.012s | 19.78MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node460926.smt2   |    0.012s | 19.536MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node70925.smt2    |    0.012s | 20.08MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node404749.smt2   |    0.012s | 19.32MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node396021.smt2   |    0.012s | 19.532MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node412241.smt2   |    0.012s | 19.772MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node47049.smt2    |    0.012s | 19.564MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_286.smt2                  |    0.013s | 20.56MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_007.smt2                  |    0.013s | 20.808MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_005.smt2                  |    0.013s | 20.592MiB| unsat | 0 |  |
|tptp/ARI357=1.smt2                                           |    0.013s | 20.036MiB| unsat | 0 |  |
|tptp/ARI376=1.smt2                                           |    0.013s | 19.784MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node2545.smt2     |    0.013s | 20.544MiB| unsat | 0 |  |
|keymaera/water_tank-node14909.smt2                           |    0.013s | 20.296MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node406687.smt2   |    0.013s | 20.036MiB| unsat | 0 |  |
|keymaera/water_tank-node22259.smt2                           |    0.013s | 20.58MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node93874.smt2    |    0.013s | 20.116MiB| unsat | 0 |  |
|keymaera/water_tank-node31056.smt2                           |    0.013s | 20.304MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node405951.smt2   |    0.013s | 19.832MiB| unsat | 0 |  |
|keymaera/water_tank-node30102.smt2                           |    0.013s | 20.368MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node680933.smt2   |    0.013s | 20.04MiB| unsat | 0 |  |
|keymaera/water_tank-node10155.smt2                           |    0.013s | 20.548MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node403405.smt2   |    0.013s | 19.968MiB| unsat | 0 |  |
|keymaera/water_tank-node9350.smt2                            |    0.013s | 20.424MiB| unsat | 0 |  |
|keymaera/water_tank-node27654.smt2                           |    0.013s | 20.548MiB| unsat | 0 |  |
|keymaera/water_tank-node10186.smt2                           |    0.013s | 19.784MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node406896.smt2   |    0.013s | 19.548MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node43527.smt2    |    0.013s | 20.336MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node98721.smt2    |    0.013s | 20.04MiB| unsat | 0 |  |
|keymaera/water_tank-node5019.smt2                            |    0.013s | 20.296MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node461135.smt2   |    0.013s | 20.032MiB| unsat | 0 |  |
|keymaera/water_tank-node16055.smt2                           |    0.013s | 20.296MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node464056.smt2   |    0.013s | 20.04MiB| unsat | 0 |  |
|keymaera/water_tank-node16086.smt2                           |    0.013s | 20.52MiB| unsat | 0 |  |
|keymaera/water_tank-node28824.smt2                           |    0.013s | 20.3MiB| unsat | 0 |  |
|keymaera/water_tank-node12555.smt2                           |    0.013s | 20.404MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node394346.smt2   |    0.013s | 20.296MiB| unsat | 0 |  |
|keymaera/water_tank-node25619.smt2                           |    0.013s | 20.548MiB| unsat | 0 |  |
|keymaera/water_tank-node23061.smt2                           |    0.013s | 20.3MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node464836.smt2   |    0.013s | 20.032MiB| unsat | 0 |  |
|keymaera/water_tank-node11755.smt2                           |    0.013s | 20.708MiB| unsat | 0 |  |
|keymaera/water_tank-node12581.smt2                           |    0.013s | 19.78MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node44607.smt2    |    0.013s | 20.6MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node54395.smt2    |    0.013s | 20.016MiB| unsat | 0 |  |
|keymaera/water_tank-node24628.smt2                           |    0.013s | 20.088MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node465124.smt2   |    0.013s | 20.032MiB| unsat | 0 |  |
|keymaera/water_tank-node28519.smt2                           |    0.013s | 20.3MiB| unsat | 0 |  |
|keymaera/water_tank-node26754.smt2                           |    0.013s | 20.548MiB| unsat | 0 |  |
|keymaera/water_tank-node23030.smt2                           |    0.013s | 20.552MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node408839.smt2   |    0.013s | 20.544MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node459874.smt2   |    0.013s | 20.04MiB| unsat | 0 |  |
|keymaera/water_tank-node13885.smt2                           |    0.013s | 20.548MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node81556.smt2    |    0.013s | 20.296MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_110.smt2                  |    0.014s | 21.06MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_193.smt2                  |    0.014s | 20.804MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_297.smt2                  |    0.014s | 21.064MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_051.smt2                  |    0.014s | 20.812MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_058.smt2                  |    0.014s | 21.06MiB| sat | 0 |  |
|scholl-smt08/model/model_6_17.smt2                           |    0.014s | 20.928MiB| sat | 0 |  |
|scholl-smt08/model/model_5_6.smt2                            |    0.014s | 20.812MiB| sat | 0 |  |
|scholl-smt08/model/model_6_4.smt2                            |    0.014s | 20.56MiB| sat | 0 |  |
|scholl-smt08/model/model_6_14.smt2                           |    0.014s | 20.8MiB| sat | 0 |  |
|scholl-smt08/model/model_6_26.smt2                           |    0.014s | 20.808MiB| sat | 0 |  |
|scholl-smt08/model/model_6_24.smt2                           |    0.014s | 21.056MiB| sat | 0 |  |
|scholl-smt08/model/model_5_24.smt2                           |    0.014s | 21.056MiB| sat | 0 |  |
|scholl-smt08/model/model_5_1.smt2                            |    0.014s | 20.8MiB| sat | 0 |  |
|scholl-smt08/model/model_6_8.smt2                            |    0.014s | 20.808MiB| sat | 0 |  |
|scholl-smt08/model/model_6_10.smt2                           |    0.014s | 21.06MiB| sat | 0 |  |
|scholl-smt08/model/model_6_15.smt2                           |    0.014s | 21.064MiB| sat | 0 |  |
|scholl-smt08/model/model_4_3.smt2                            |    0.014s | 21.064MiB| sat | 0 |  |
|scholl-smt08/model/model_6_13.smt2                           |    0.014s | 20.552MiB| sat | 0 |  |
|scholl-smt08/model/model_6_9.smt2                            |    0.014s | 21.056MiB| sat | 0 |  |
|scholl-smt08/model/model_5_28.smt2                           |    0.014s | 21.06MiB| sat | 0 |  |
|scholl-smt08/model/model_6_11.smt2                           |    0.014s | 20.804MiB| sat | 0 |  |
|scholl-smt08/model/model_6_32.smt2                           |    0.014s | 20.92MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_38.smt2                         |    0.014s | 20.808MiB| unsat | 0 |  |
|tptp/ARI375=1.smt2                                           |    0.014s | 20.556MiB| unsat | 0 |  |
|tptp/ARI348=1.smt2                                           |    0.014s | 20.804MiB| unsat | 0 |  |
|tptp/ARI356=1.smt2                                           |    0.014s | 20.624MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node12702.smt2    |    0.014s | 20.3MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node450267.smt2   |    0.014s | 20.344MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node81001.smt2    |    0.014s | 20.548MiB| unsat | 0 |  |
|keymaera/water_tank-node25679.smt2                           |    0.014s | 20.048MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node451347.smt2   |    0.014s | 20.616MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node80787.smt2    |    0.014s | 20.344MiB| unsat | 0 |  |
|keymaera/water_tank-node29292.smt2                           |    0.014s | 20.544MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node409496.smt2   |    0.014s | 20.548MiB| unsat | 0 |  |
|keymaera/water_tank-node22228.smt2                           |    0.014s | 20.552MiB| unsat | 0 |  |
|keymaera/water_tank-node16056.smt2                           |    0.014s | 21.06MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node679466.smt2   |    0.014s | 20.416MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node80003.smt2    |    0.014s | 20.304MiB| unsat | 0 |  |
|keymaera/water_tank-node5020.smt2                            |    0.014s | 20.812MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node11645.smt2    |    0.014s | 20.352MiB| unsat | 0 |  |
|keymaera/water_tank-node21141.smt2                           |    0.014s | 20.3MiB| unsat | 0 |  |
|keymaera/water_tank-node27709.smt2                           |    0.014s | 20.296MiB| unsat | 0 |  |
|keymaera/water_tank-node28520.smt2                           |    0.014s | 20.816MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node81770.smt2    |    0.014s | 20.544MiB| unsat | 0 |  |
|keymaera/water_tank-node10156.smt2                           |    0.014s | 21.06MiB| unsat | 0 |  |
|keymaera/water_tank-node24658.smt2                           |    0.014s | 20.544MiB| unsat | 0 |  |
|keymaera/water_tank-node14879.smt2                           |    0.014s | 20.344MiB| unsat | 0 |  |
|keymaera/water_tank-node30870.smt2                           |    0.014s | 20.544MiB| unsat | 0 |  |
|keymaera/water_tank-node28794.smt2                           |    0.014s | 20.808MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node393141.smt2   |    0.014s | 20.296MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node11938.smt2    |    0.014s | 20.508MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_214.smt2                  |    0.015s | 21.064MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_079.smt2                  |    0.015s | 20.848MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_101.smt2                  |    0.015s | 20.808MiB| unsat | 0 |  |
|scholl-smt08/model/model_4_2.smt2                            |    0.015s | 20.812MiB| sat | 0 |  |
|scholl-smt08/model/model_5_7.smt2                            |    0.015s | 20.808MiB| sat | 0 |  |
|scholl-smt08/model/model_5_11.smt2                           |    0.015s | 21.06MiB| sat | 0 |  |
|scholl-smt08/model/model_6_37.smt2                           |    0.015s | 20.816MiB| sat | 0 |  |
|scholl-smt08/model/model_6_44.smt2                           |    0.015s | 21.064MiB| sat | 0 |  |
|scholl-smt08/model/model_6_38.smt2                           |    0.015s | 20.696MiB| sat | 0 |  |
|scholl-smt08/model/model_6_50.smt2                           |    0.015s | 21.06MiB| sat | 0 |  |
|scholl-smt08/model/model_6_48.smt2                           |    0.015s | 20.56MiB| sat | 0 |  |
|scholl-smt08/model/model_6_2.smt2                            |    0.015s | 20.8MiB| sat | 0 |  |
|scholl-smt08/model/model_6_35.smt2                           |    0.015s | 21.056MiB| sat | 0 |  |
|scholl-smt08/model/model_6_30.smt2                           |    0.015s | 20.812MiB| sat | 0 |  |
|scholl-smt08/model/model_5_18.smt2                           |    0.015s | 21.056MiB| sat | 0 |  |
|scholl-smt08/model/model_6_23.smt2                           |    0.015s | 20.808MiB| sat | 0 |  |
|scholl-smt08/model/model_6_29.smt2                           |    0.015s | 20.808MiB| sat | 0 |  |
|scholl-smt08/model/model_5_22.smt2                           |    0.015s | 21.06MiB| sat | 0 |  |
|scholl-smt08/model/model_6_46.smt2                           |    0.015s | 21.056MiB| sat | 0 |  |
|scholl-smt08/model/model_5_25.smt2                           |    0.015s | 20.556MiB| sat | 0 |  |
|scholl-smt08/model/model_5_20.smt2                           |    0.015s | 21.056MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_18.smt2                         |    0.015s | 20.76MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_17.smt2                         |    0.015s | 21.06MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_3.smt2                          |    0.015s | 20.812MiB| sat | 0 |  |
|keymaera/water_tank-node12556.smt2                           |    0.015s | 20.708MiB| unsat | 0 |  |
|keymaera/water_tank-node19175.smt2                           |    0.015s | 20.564MiB| unsat | 0 |  |
|keymaera/water_tank-node30869.smt2                           |    0.015s | 20.812MiB| unsat | 0 |  |
|keymaera/water_tank-node11756.smt2                           |    0.015s | 20.808MiB| unsat | 0 |  |
|keymaera/water_tank-node30103.smt2                           |    0.015s | 21.056MiB| unsat | 0 |  |
|keymaera/water_tank-node23031.smt2                           |    0.015s | 20.752MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node13023.smt2    |    0.015s | 20.608MiB| unsat | 0 |  |
|keymaera/water_tank-node7173.smt2                            |    0.015s | 21.064MiB| unsat | 0 |  |
|keymaera/water_tank-node27653.smt2                           |    0.015s | 20.496MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node12911.smt2    |    0.015s | 20.564MiB| unsat | 0 |  |
|keymaera/water_tank-node24597.smt2                           |    0.015s | 20.804MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node94532.smt2    |    0.015s | 19.836MiB| unsat | 0 |  |
|keymaera/water_tank-node18157.smt2                           |    0.015s | 20.72MiB| unsat | 0 |  |
|keymaera/water_tank-node14878.smt2                           |    0.015s | 20.448MiB| unsat | 0 |  |
|keymaera/water_tank-node25618.smt2                           |    0.015s | 20.56MiB| unsat | 0 |  |
|keymaera/water_tank-node9351.smt2                            |    0.015s | 20.8MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_150.smt2                  |    0.016s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_100.smt2                  |    0.016s | 21.316MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_281.smt2                  |    0.016s | 21.064MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_4_6.smt2                                |    0.016s | 21.06MiB| unsat | 0 |  |
|scholl-smt08/model/model_6_62.smt2                           |    0.016s | 21.056MiB| sat | 0 |  |
|scholl-smt08/model/model_6_55.smt2                           |    0.016s | 20.812MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_37.smt2                         |    0.016s | 20.804MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_219.smt2                  |    0.017s | 19.38MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_296.smt2                  |    0.017s | 20.828MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_110.smt2                  |    0.017s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_101.smt2                  |    0.017s | 21.316MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_097.smt2                  |    0.017s | 21.188MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_166.smt2                  |    0.017s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_114.smt2                  |    0.017s | 21.028MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_085.smt2                  |    0.017s | 21.064MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_6_10.smt2                               |    0.017s | 21.4MiB| sat | 0 |  |
|scholl-smt08/RND/RND_3_11.smt2                               |    0.017s | 21.06MiB| sat | 0 |  |
|scholl-smt08/RND/RND_6_8.smt2                                |    0.017s | 21.072MiB| sat | 0 |  |
|scholl-smt08/model/model_6_71.smt2                           |    0.017s | 21.124MiB| sat | 0 |  |
|scholl-smt08/model/model_6_76.smt2                           |    0.017s | 21.316MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_9.smt2                          |    0.017s | 20.816MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_46.smt2                         |    0.017s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_096.smt2                  |    0.018s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_199.smt2                  |    0.018s | 21.072MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_145.smt2                  |    0.018s | 21.064MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_023.smt2                  |    0.018s | 21.22MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_093.smt2                  |    0.018s | 21.064MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_278.smt2                  |    0.018s | 21.052MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_035.smt2                  |    0.018s | 21.172MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_032.smt2                  |    0.018s | 21.14MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_4_7.smt2                                |    0.018s | 21.068MiB| unsat | 0 |  |
|scholl-smt08/model/model_5_52.smt2                           |    0.018s | 21.32MiB| sat | 0 |  |
|scholl-smt08/model/model_6_79.smt2                           |    0.018s | 21.236MiB| sat | 0 |  |
|scholl-smt08/model/model_6_66.smt2                           |    0.018s | 21.312MiB| sat | 0 |  |
|scholl-smt08/model/model_5_46.smt2                           |    0.018s | 21.336MiB| sat | 0 |  |
|scholl-smt08/model/model_5_56.smt2                           |    0.018s | 21.492MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_28.smt2                         |    0.018s | 21.312MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_021.smt2                  |    0.019s | 21.068MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_008.smt2                  |    0.019s | 21.32MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_245.smt2                  |    0.019s | 20.808MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_088.smt2                  |    0.019s | 20.804MiB| unsat | 0 |  |
|scholl-smt08/model/model_5_58.smt2                           |    0.019s | 21.32MiB| sat | 0 |  |
|scholl-smt08/model/model_5_48.smt2                           |    0.019s | 21.572MiB| sat | 0 |  |
|scholl-smt08/model/model_5_47.smt2                           |    0.019s | 21.572MiB| sat | 0 |  |
|scholl-smt08/model/model_6_69.smt2                           |    0.019s | 21.316MiB| sat | 0 |  |
|scholl-smt08/model/model_5_61.smt2                           |    0.019s | 21.324MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_46.smt2                         |    0.019s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_094.smt2                  |    0.020s | 21.308MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_273.smt2                  |    0.020s | 20.82MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_031.smt2                  |    0.020s | 21.316MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_158.smt2                  |    0.020s | 21.32MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_070.smt2                  |    0.020s | 21.316MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_261.smt2                  |    0.020s | 20.992MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_178.smt2                  |    0.020s | 20.824MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_257.smt2                  |    0.020s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_226.smt2                  |    0.020s | 21.324MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_038.smt2                  |    0.020s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_247.smt2                  |    0.020s | 20.96MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_265.smt2                  |    0.020s | 21.064MiB| sat | 0 |  |
|scholl-smt08/RND/RND_4_18.smt2                               |    0.020s | 21.604MiB| sat | 0 |  |
|scholl-smt08/model/model_5_37.smt2                           |    0.020s | 21.596MiB| sat | 0 |  |
|scholl-smt08/model/model_5_62.smt2                           |    0.020s | 21.316MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_1.smt2                          |    0.020s | 21.316MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_033.smt2                  |    0.021s | 21.136MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_068.smt2                  |    0.021s | 21.056MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_005.smt2                  |    0.021s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_243.smt2                  |    0.021s | 20.968MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_037.smt2                  |    0.021s | 21.068MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_248.smt2                  |    0.021s | 21.324MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_290.smt2                  |    0.021s | 21.064MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_191.smt2                  |    0.021s | 20.812MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_4_13.smt2                               |    0.021s | 19.644MiB| sat | 0 |  |
|scholl-smt08/model/model_5_57.smt2                           |    0.021s | 21.488MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_202.smt2                  |    0.022s | 21.056MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_055.smt2                  |    0.022s | 20.944MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_141.smt2                  |    0.022s | 20.804MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_214.smt2                  |    0.022s | 19.78MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_091.smt2                  |    0.022s | 21.632MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_263.smt2                  |    0.022s | 21.324MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_081.smt2                  |    0.022s | 21.32MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_234.smt2                  |    0.022s | 21.076MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_048.smt2                  |    0.022s | 21.28MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_064.smt2                  |    0.022s | 20.556MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_3_5.smt2                                |    0.022s | 19.776MiB| unsat | 0 |  |
|scholl-smt08/model/model_5_60.smt2                           |    0.022s | 21.576MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_287.smt2                  |    0.023s | 20.808MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_140.smt2                  |    0.023s | 19.78MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_077.smt2                  |    0.023s | 19.78MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_214.smt2                  |    0.023s | 19.528MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_193.smt2                  |    0.023s | 20.436MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_185.smt2                  |    0.023s | 19.784MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_055.smt2                  |    0.023s | 20.808MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_237.smt2                  |    0.023s | 20.804MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_287.smt2                  |    0.023s | 21.064MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_202.smt2                  |    0.023s | 21.06MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_054.smt2                  |    0.023s | 21.576MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_006.smt2                  |    0.023s | 19.552MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_090.smt2                  |    0.023s | 21.312MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_011.smt2                  |    0.023s | 20.824MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_034.smt2                  |    0.023s | 21.072MiB| unsat | 0 |  |
|scholl-smt08/model/model_5_55.smt2                           |    0.023s | 21.32MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_27.smt2                         |    0.023s | 20.868MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_23.smt2                         |    0.023s | 19.54MiB| sat | 0 |  |
|tptp/ARI361=1.smt2                                           |    0.023s | 19.956MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_071.smt2                  |    0.024s | 21.056MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_286.smt2                  |    0.024s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_051.smt2                  |    0.024s | 20.944MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_247.smt2                  |    0.024s | 20.56MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_122.smt2                  |    0.024s | 21.568MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_287.smt2                  |    0.024s | 19.7MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_026.smt2                  |    0.024s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_019.smt2                  |    0.024s | 21.52MiB| sat | 0 |  |
|scholl-smt08/RND/RND_3_15.smt2                               |    0.024s | 21.22MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_3_8.smt2                                |    0.024s | 20.296MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_29.smt2                         |    0.024s | 21.068MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_18.smt2                         |    0.024s | 20.808MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node409710.smt2   |    0.024s | 20.66MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_073.smt2                  |    0.025s | 20.812MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_079.smt2                  |    0.025s | 21.064MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_058.smt2                  |    0.025s | 20.812MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_297.smt2                  |    0.025s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_140.smt2                  |    0.025s | 19.78MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_273.smt2                  |    0.025s | 21.124MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_170.smt2                  |    0.025s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_067.smt2                  |    0.025s | 21.06MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_282.smt2                  |    0.025s | 20.952MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_215.smt2                  |    0.025s | 20.816MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_241.smt2                  |    0.025s | 21.34MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_188.smt2                  |    0.025s | 21.572MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_094.smt2                  |    0.025s | 21.06MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_4_22.smt2                               |    0.025s | 20.04MiB| unsat | 0 |  |
|scholl-smt08/model/model_6_20.smt2                           |    0.025s | 20.652MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_19.smt2                         |    0.025s | 20.556MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_13.smt2                         |    0.025s | 19.78MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_14.smt2                         |    0.025s | 21.064MiB| sat | 0 |  |
|tptp/ARI389=1.smt2                                           |    0.025s | 19.784MiB| unsat | 0 |  |
|keymaera/water_tank-node8199.smt2                            |    0.025s | 20.548MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node422040.smt2   |    0.025s | 19.532MiB| unsat | 0 |  |
|keymaera/water_tank-node22229.smt2                           |    0.025s | 20.808MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node451133.smt2   |    0.025s | 20.044MiB| unsat | 0 |  |
|keymaera/water_tank-node13855.smt2                           |    0.025s | 20.296MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_140.smt2                  |    0.026s | 19.536MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_011.smt2                  |    0.026s | 20.808MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_006.smt2                  |    0.026s | 20.8MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_178.smt2                  |    0.026s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_243.smt2                  |    0.026s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_219.smt2                  |    0.026s | 20.812MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_272.smt2                  |    0.026s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_218.smt2                  |    0.026s | 20.96MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_071.smt2                  |    0.026s | 19.784MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_073.smt2                  |    0.026s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_114.smt2                  |    0.026s | 19.684MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_155.smt2                  |    0.026s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_160.smt2                  |    0.026s | 21.068MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_120.smt2                  |    0.026s | 21.08MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_245.smt2                  |    0.026s | 21.6MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_276.smt2                  |    0.026s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_218.smt2                  |    0.026s | 20.932MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_018.smt2                  |    0.026s | 21.064MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_4_5.smt2                                |    0.026s | 21.06MiB| sat | 0 |  |
|scholl-smt08/RND/RND_3_2.smt2                                |    0.026s | 20.812MiB| sat | 0 |  |
|scholl-smt08/model/model_6_25.smt2                           |    0.026s | 20.824MiB| sat | 0 |  |
|scholl-smt08/model/model_6_67.smt2                           |    0.026s | 21.316MiB| sat | 0 |  |
|scholl-smt08/model/model_6_33.smt2                           |    0.026s | 20.968MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_47.smt2                         |    0.026s | 20.3MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_25.smt2                         |    0.026s | 19.528MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node2331.smt2     |    0.026s | 20.544MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_064.smt2                  |    0.027s | 19.784MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_014.smt2                  |    0.027s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_297.smt2                  |    0.027s | 21.06MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_286.smt2                  |    0.027s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_291.smt2                  |    0.027s | 21.324MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_237.smt2                  |    0.027s | 19.536MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_218.smt2                  |    0.027s | 20.808MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_079.smt2                  |    0.027s | 20.808MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_058.smt2                  |    0.027s | 20.812MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_186.smt2                  |    0.027s | 21.476MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_230.smt2                  |    0.027s | 21.32MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_099.smt2                  |    0.027s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_069.smt2                  |    0.027s | 20.808MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_6_12.smt2                               |    0.027s | 19.78MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_3_9.smt2                                |    0.027s | 21.096MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_3_19.smt2                               |    0.027s | 19.284MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_4_8.smt2                                |    0.027s | 21.012MiB| sat | 0 |  |
|scholl-smt08/RND/RND_3_1.smt2                                |    0.027s | 21.06MiB| sat | 0 |  |
|scholl-smt08/model/model_6_27.smt2                           |    0.027s | 21.076MiB| sat | 0 |  |
|scholl-smt08/model/model_5_34.smt2                           |    0.027s | 21.828MiB| sat | 0 |  |
|scholl-smt08/model/model_6_12.smt2                           |    0.027s | 20.804MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_26.smt2                         |    0.027s | 20.86MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_13.smt2                         |    0.027s | 19.78MiB| sat | 0 |  |
|tptp/ARI384=1.smt2                                           |    0.027s | 19.548MiB| unsat | 0 |  |
|keymaera/water_tank-node9381.smt2                            |    0.027s | 19.784MiB| unsat | 0 |  |
|keymaera/water_tank-node28793.smt2                           |    0.027s | 20.296MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_284.smt2                  |    0.028s | 20.808MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_168.smt2                  |    0.028s | 20.3MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_285.smt2                  |    0.028s | 21.472MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_005.smt2                  |    0.028s | 20.808MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_155.smt2                  |    0.028s | 21.056MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_247.smt2                  |    0.028s | 20.82MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_202.smt2                  |    0.028s | 19.78MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_064.smt2                  |    0.028s | 19.78MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_291.smt2                  |    0.028s | 21.188MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_133.smt2                  |    0.028s | 21.312MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_235.smt2                  |    0.028s | 21.32MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_041.smt2                  |    0.028s | 21.064MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_266.smt2                  |    0.028s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_228.smt2                  |    0.028s | 21.172MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_134.smt2                  |    0.028s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_283.smt2                  |    0.028s | 21.324MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_091.smt2                  |    0.028s | 21.056MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_3_28.smt2                               |    0.028s | 19.788MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_6_6.smt2                                |    0.028s | 21.316MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_3_13.smt2                               |    0.028s | 21.316MiB| sat | 0 |  |
|scholl-smt08/RND/RND_3_3.smt2                                |    0.028s | 21.06MiB| sat | 0 |  |
|scholl-smt08/RND/RND_4_12.smt2                               |    0.028s | 19.78MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_6_2.smt2                                |    0.028s | 19.78MiB| unsat | 0 |  |
|scholl-smt08/model/model_6_52.smt2                           |    0.028s | 21.288MiB| sat | 0 |  |
|scholl-smt08/model/model_6_70.smt2                           |    0.028s | 21.064MiB| sat | 0 |  |
|scholl-smt08/model/model_6_65.smt2                           |    0.028s | 21.12MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_9.smt2                          |    0.028s | 21.064MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_8.smt2                          |    0.028s | 20.808MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_8.smt2                          |    0.028s | 21.064MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_4.smt2                          |    0.028s | 21.06MiB| sat | 0 |  |
|tptp/ARI383=1.smt2                                           |    0.028s | 20.556MiB| unsat | 0 |  |
|tptp/ARI370=1.smt2                                           |    0.028s | 20.036MiB| unsat | 0 |  |
|tptp/ARI349=1.smt2                                           |    0.028s | 20.036MiB| unsat | 0 |  |
|tptp/NUM907=1.smt2                                           |    0.028s | 20.552MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node75772.smt2    |    0.028s | 19.804MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node85758.smt2    |    0.028s | 19.568MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_006.smt2                  |    0.029s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_284.smt2                  |    0.029s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_202.smt2                  |    0.029s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_005.smt2                  |    0.029s | 21.06MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_296.smt2                  |    0.029s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_155.smt2                  |    0.029s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_058.smt2                  |    0.029s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_193.smt2                  |    0.029s | 20.808MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_215.smt2                  |    0.029s | 21.312MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_051.smt2                  |    0.029s | 20.808MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_068.smt2                  |    0.029s | 20.828MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_011.smt2                  |    0.029s | 20.808MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_064.smt2                  |    0.029s | 20.812MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_207.smt2                  |    0.029s | 19.276MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_006.smt2                  |    0.029s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_104.smt2                  |    0.029s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_007.smt2                  |    0.029s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_192.smt2                  |    0.029s | 21.312MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_035.smt2                  |    0.029s | 21.592MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_008.smt2                  |    0.029s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_014.smt2                  |    0.029s | 19.528MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_297.smt2                  |    0.029s | 20.332MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_216.smt2                  |    0.029s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_245.smt2                  |    0.029s | 19.784MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_276.smt2                  |    0.029s | 21.072MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_284.smt2                  |    0.029s | 20.804MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_168.smt2                  |    0.029s | 19.528MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_142.smt2                  |    0.029s | 20.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_211.smt2                  |    0.029s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_141.smt2                  |    0.029s | 21.08MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_284.smt2                  |    0.029s | 20.792MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_079.smt2                  |    0.029s | 20.808MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_144.smt2                  |    0.029s | 21.316MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_260.smt2                  |    0.029s | 21.324MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_014.smt2                  |    0.029s | 20.724MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_070.smt2                  |    0.029s | 21.016MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_082.smt2                  |    0.029s | 21.056MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_289.smt2                  |    0.029s | 21.072MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_300.smt2                  |    0.029s | 20.812MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_6_17.smt2                               |    0.029s | 20.812MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_4_1.smt2                                |    0.029s | 20.812MiB| sat | 0 |  |
|scholl-smt08/RND/RND_4_4.smt2                                |    0.029s | 19.78MiB| sat | 0 |  |
|scholl-smt08/model/model_6_56.smt2                           |    0.029s | 21.06MiB| sat | 0 |  |
|scholl-smt08/model/model_5_10.smt2                           |    0.029s | 20.804MiB| sat | 0 |  |
|scholl-smt08/model/model_6_7.smt2                            |    0.029s | 20.816MiB| sat | 0 |  |
|scholl-smt08/model/model_6_61.smt2                           |    0.029s | 21.06MiB| sat | 0 |  |
|scholl-smt08/model/model_6_45.smt2                           |    0.029s | 20.816MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_20.smt2                         |    0.029s | 21.06MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_33.smt2                         |    0.029s | 19.784MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_7.smt2                          |    0.029s | 20.808MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_23.smt2                         |    0.029s | 20.808MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_40.smt2                         |    0.029s | 21.684MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_6.smt2                          |    0.029s | 21.32MiB| sat | 0 |  |
|tptp/ARI397=1.smt2                                           |    0.029s | 20.804MiB| unsat | 0 |  |
|tptp/ARI398=1.smt2                                           |    0.029s | 20.036MiB| unsat | 0 |  |
|tptp/ARI355=1.smt2                                           |    0.029s | 20.8MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node44393.smt2    |    0.029s | 20.148MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node681626.smt2   |    0.029s | 20.032MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node58499.smt2    |    0.029s | 19.688MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node403899.smt2   |    0.029s | 20.04MiB| unsat | 0 |  |
|keymaera/water_tank-node19176.smt2                           |    0.029s | 20.548MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node62480.smt2    |    0.029s | 19.776MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_101.smt2                  |    0.030s | 21.328MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_051.smt2                  |    0.030s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_068.smt2                  |    0.030s | 19.78MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_033.smt2                  |    0.030s | 21.324MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_141.smt2                  |    0.030s | 19.536MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_064.smt2                  |    0.030s | 20.812MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_014.smt2                  |    0.030s | 21.06MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_092.smt2                  |    0.030s | 21.472MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_002.smt2                  |    0.030s | 21.064MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_147.smt2                  |    0.030s | 21.312MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_028.smt2                  |    0.030s | 21.012MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_057.smt2                  |    0.030s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_040.smt2                  |    0.030s | 21.312MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_192.smt2                  |    0.030s | 21.296MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_016.smt2                  |    0.030s | 21.06MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_3_4.smt2                                |    0.030s | 21.056MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_3_21.smt2                               |    0.030s | 21.728MiB| sat | 0 |  |
|scholl-smt08/model/model_5_31.smt2                           |    0.030s | 20.808MiB| sat | 0 |  |
|scholl-smt08/model/model_6_1.smt2                            |    0.030s | 20.552MiB| sat | 0 |  |
|scholl-smt08/model/model_4_1.smt2                            |    0.030s | 20.808MiB| sat | 0 |  |
|scholl-smt08/model/model_5_4.smt2                            |    0.030s | 21.06MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_43.smt2                         |    0.030s | 21.516MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_30.smt2                         |    0.030s | 19.748MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_15.smt2                         |    0.030s | 19.776MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_5.smt2                          |    0.030s | 19.784MiB| unsat | 0 |  |
|tptp/ARI388=1.smt2                                           |    0.030s | 20.808MiB| unsat | 0 |  |
|keymaera/water_tank-node29291.smt2                           |    0.030s | 21.076MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_193.smt2                  |    0.031s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_297.smt2                  |    0.031s | 20.808MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_286.smt2                  |    0.031s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_218.smt2                  |    0.031s | 20.936MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_007.smt2                  |    0.031s | 21.056MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_073.smt2                  |    0.031s | 20.896MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_014.smt2                  |    0.031s | 20.584MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_261.smt2                  |    0.031s | 19.536MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_255.smt2                  |    0.031s | 21.588MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_243.smt2                  |    0.031s | 19.776MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_284.smt2                  |    0.031s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_168.smt2                  |    0.031s | 19.78MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_011.smt2                  |    0.031s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_155.smt2                  |    0.031s | 20.812MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_058.smt2                  |    0.031s | 20.556MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_015.smt2                  |    0.031s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_006.smt2                  |    0.031s | 19.532MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_285.smt2                  |    0.031s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_055.smt2                  |    0.031s | 20.82MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_237.smt2                  |    0.031s | 21.064MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_200.smt2                  |    0.031s | 21.312MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_029.smt2                  |    0.031s | 21.072MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_018.smt2                  |    0.031s | 21.324MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_180.smt2                  |    0.031s | 21.1MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_233.smt2                  |    0.031s | 21.568MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_017.smt2                  |    0.031s | 21.584MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_104.smt2                  |    0.031s | 20.788MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_227.smt2                  |    0.031s | 20.82MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_084.smt2                  |    0.031s | 21.288MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_046.smt2                  |    0.031s | 20.94MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_188.smt2                  |    0.031s | 21.316MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_4_3.smt2                                |    0.031s | 19.78MiB| sat | 0 |  |
|scholl-smt08/RND/RND_3_20.smt2                               |    0.031s | 21.372MiB| sat | 0 |  |
|scholl-smt08/RND/RND_3_10.smt2                               |    0.031s | 19.508MiB| sat | 0 |  |
|scholl-smt08/model/model_5_3.smt2                            |    0.031s | 20.812MiB| sat | 0 |  |
|scholl-smt08/model/model_5_16.smt2                           |    0.031s | 20.804MiB| sat | 0 |  |
|scholl-smt08/model/model_5_23.smt2                           |    0.031s | 20.56MiB| sat | 0 |  |
|scholl-smt08/model/model_6_49.smt2                           |    0.031s | 20.808MiB| sat | 0 |  |
|scholl-smt08/model/model_5_43.smt2                           |    0.031s | 21.116MiB| sat | 0 |  |
|scholl-smt08/model/model_5_2.smt2                            |    0.031s | 20.56MiB| sat | 0 |  |
|scholl-smt08/model/model_6_73.smt2                           |    0.031s | 21.072MiB| sat | 0 |  |
|scholl-smt08/model/model_6_22.smt2                           |    0.031s | 20.936MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_37.smt2                         |    0.031s | 20.804MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_42.smt2                         |    0.031s | 20.036MiB| unsat | 0 |  |
|tptp/ARI382=1.smt2                                           |    0.031s | 20.804MiB| unsat | 0 |  |
|keymaera/water_tank-node24598.smt2                           |    0.031s | 20.296MiB| unsat | 0 |  |
|keymaera/water_tank-node21166.smt2                           |    0.031s | 20.04MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node99528.smt2    |    0.031s | 19.668MiB| unsat | 0 |  |
|keymaera/water_tank-node30895.smt2                           |    0.031s | 20.036MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_055.smt2                  |    0.032s | 21.088MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_007.smt2                  |    0.032s | 20.992MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_273.smt2                  |    0.032s | 20.816MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_216.smt2                  |    0.032s | 21.316MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_247.smt2                  |    0.032s | 20.908MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_015.smt2                  |    0.032s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_193.smt2                  |    0.032s | 20.828MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_273.smt2                  |    0.032s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_160.smt2                  |    0.032s | 21.328MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_286.smt2                  |    0.032s | 19.776MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_090.smt2                  |    0.032s | 21.48MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_005.smt2                  |    0.032s | 20.56MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_085.smt2                  |    0.032s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_272.smt2                  |    0.032s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_007.smt2                  |    0.032s | 20.816MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_069.smt2                  |    0.032s | 21.316MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_078.smt2                  |    0.032s | 21.072MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_171.smt2                  |    0.032s | 21.312MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_216.smt2                  |    0.032s | 21.336MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_094.smt2                  |    0.032s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_184.smt2                  |    0.032s | 21.312MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_140.smt2                  |    0.032s | 19.54MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_175.smt2                  |    0.032s | 20.604MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_009.smt2                  |    0.032s | 20.812MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_4_11.smt2                               |    0.032s | 21.828MiB| sat | 0 |  |
|scholl-smt08/RND/RND_4_10.smt2                               |    0.032s | 20.56MiB| sat | 0 |  |
|scholl-smt08/RND/RND_4_9.smt2                                |    0.032s | 21.316MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_6_7.smt2                                |    0.032s | 21.064MiB| unsat | 0 |  |
|scholl-smt08/model/model_6_5.smt2                            |    0.032s | 21.064MiB| sat | 0 |  |
|scholl-smt08/model/model_6_6.smt2                            |    0.032s | 21.056MiB| sat | 0 |  |
|scholl-smt08/model/model_6_68.smt2                           |    0.032s | 21.316MiB| sat | 0 |  |
|scholl-smt08/model/model_5_54.smt2                           |    0.032s | 21.572MiB| sat | 0 |  |
|scholl-smt08/model/model_6_16.smt2                           |    0.032s | 20.556MiB| sat | 0 |  |
|scholl-smt08/model/model_6_31.smt2                           |    0.032s | 20.808MiB| sat | 0 |  |
|scholl-smt08/model/model_6_64.smt2                           |    0.032s | 21.052MiB| sat | 0 |  |
|scholl-smt08/model/model_5_50.smt2                           |    0.032s | 21.32MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_5.smt2                          |    0.032s | 19.776MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_7.smt2                          |    0.032s | 19.588MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_11.smt2                         |    0.032s | 21.064MiB| sat | 0 |  |
|tptp/ARI362=1.smt2                                           |    0.032s | 20.568MiB| unsat | 0 |  |
|keymaera/water_tank-node31001.smt2                           |    0.032s | 21.06MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node733141.smt2   |    0.032s | 19.776MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node680612.smt2   |    0.032s | 20.036MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node15800.smt2    |    0.032s | 19.536MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node679178.smt2   |    0.032s | 19.788MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_243.smt2                  |    0.033s | 19.528MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_237.smt2                  |    0.033s | 19.528MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_141.smt2                  |    0.033s | 21.092MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_170.smt2                  |    0.033s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_053.smt2                  |    0.033s | 20.812MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_170.smt2                  |    0.033s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_200.smt2                  |    0.033s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_287.smt2                  |    0.033s | 20.808MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_071.smt2                  |    0.033s | 20.808MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_073.smt2                  |    0.033s | 19.784MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_147.smt2                  |    0.033s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_053.smt2                  |    0.033s | 21.064MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_219.smt2                  |    0.033s | 20.816MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_011.smt2                  |    0.033s | 20.912MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_085.smt2                  |    0.033s | 21.204MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_118.smt2                  |    0.033s | 21.324MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_124.smt2                  |    0.033s | 21.32MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_277.smt2                  |    0.033s | 20.808MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_181.smt2                  |    0.033s | 21.296MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_190.smt2                  |    0.033s | 21.568MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_168.smt2                  |    0.033s | 20.892MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_146.smt2                  |    0.033s | 21.144MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_202.smt2                  |    0.033s | 20.808MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_295.smt2                  |    0.033s | 21.108MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_118.smt2                  |    0.033s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_029.smt2                  |    0.033s | 21.152MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_192.smt2                  |    0.033s | 21.312MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_179.smt2                  |    0.033s | 21.316MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_3_17.smt2                               |    0.033s | 21.064MiB| sat | 0 |  |
|scholl-smt08/RND/RND_6_5.smt2                                |    0.033s | 21.32MiB| sat | 0 |  |
|scholl-smt08/RND/RND_3_7.smt2                                |    0.033s | 20.884MiB| unsat | 0 |  |
|scholl-smt08/model/model_5_13.smt2                           |    0.033s | 21.06MiB| sat | 0 |  |
|scholl-smt08/model/model_6_59.smt2                           |    0.033s | 21.06MiB| sat | 0 |  |
|scholl-smt08/model/model_5_21.smt2                           |    0.033s | 21.06MiB| sat | 0 |  |
|scholl-smt08/model/model_6_40.smt2                           |    0.033s | 21.06MiB| sat | 0 |  |
|scholl-smt08/model/model_6_18.smt2                           |    0.033s | 21.06MiB| sat | 0 |  |
|scholl-smt08/model/model_6_42.smt2                           |    0.033s | 20.82MiB| sat | 0 |  |
|scholl-smt08/model/model_6_53.smt2                           |    0.033s | 21.244MiB| sat | 0 |  |
|scholl-smt08/model/model_6_21.smt2                           |    0.033s | 20.952MiB| sat | 0 |  |
|scholl-smt08/model/model_6_28.smt2                           |    0.033s | 20.808MiB| sat | 0 |  |
|scholl-smt08/model/model_6_34.smt2                           |    0.033s | 21.06MiB| sat | 0 |  |
|scholl-smt08/model/model_6_57.smt2                           |    0.033s | 21.012MiB| sat | 0 |  |
|scholl-smt08/model/model_6_19.smt2                           |    0.033s | 21.064MiB| sat | 0 |  |
|scholl-smt08/model/model_5_42.smt2                           |    0.033s | 21.572MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_2.smt2                          |    0.033s | 21.064MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_44.smt2                         |    0.033s | 20.04MiB| unsat | 0 |  |
|keymaera/water_tank-node31000.smt2                           |    0.033s | 20.548MiB| unsat | 0 |  |
|keymaera/water_tank-node8198.smt2                            |    0.033s | 20.68MiB| unsat | 0 |  |
|keymaera/water_tank-node7174.smt2                            |    0.033s | 20.572MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node464515.smt2   |    0.033s | 20.036MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node25118.smt2    |    0.033s | 19.76MiB| unsat | 0 |  |
|keymaera/water_tank-node13854.smt2                           |    0.033s | 20.816MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_287.smt2                  |    0.034s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_218.smt2                  |    0.034s | 20.808MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_051.smt2                  |    0.034s | 20.816MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_079.smt2                  |    0.034s | 20.812MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_068.smt2                  |    0.034s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_055.smt2                  |    0.034s | 20.808MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_002.smt2                  |    0.034s | 21.312MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_147.smt2                  |    0.034s | 21.316MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_237.smt2                  |    0.034s | 21.06MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_141.smt2                  |    0.034s | 21.076MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_094.smt2                  |    0.034s | 21.32MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_101.smt2                  |    0.034s | 21.06MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_189.smt2                  |    0.034s | 21.328MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_216.smt2                  |    0.034s | 21.316MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_277.smt2                  |    0.034s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_145.smt2                  |    0.034s | 21.064MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_094.smt2                  |    0.034s | 21.572MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_220.smt2                  |    0.034s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_276.smt2                  |    0.034s | 21.312MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_101.smt2                  |    0.034s | 20.82MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_099.smt2                  |    0.034s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_243.smt2                  |    0.034s | 20.556MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_192.smt2                  |    0.034s | 20.812MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_296.smt2                  |    0.034s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_015.smt2                  |    0.034s | 20.816MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_008.smt2                  |    0.034s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_013.smt2                  |    0.034s | 21.324MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_060.smt2                  |    0.034s | 21.056MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_252.smt2                  |    0.034s | 21.32MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_291.smt2                  |    0.034s | 21.064MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_133.smt2                  |    0.034s | 21.312MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_244.smt2                  |    0.034s | 21.568MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_071.smt2                  |    0.034s | 21.056MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_082.smt2                  |    0.034s | 21.28MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_219.smt2                  |    0.034s | 19.792MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_035.smt2                  |    0.034s | 21.316MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_170.smt2                  |    0.034s | 20.812MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_187.smt2                  |    0.034s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_111.smt2                  |    0.034s | 21.316MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_015.smt2                  |    0.034s | 21.064MiB| unsat | 0 |  |
|scholl-smt08/model/model_6_39.smt2                           |    0.034s | 21.06MiB| sat | 0 |  |
|scholl-smt08/model/model_6_3.smt2                            |    0.034s | 20.548MiB| sat | 0 |  |
|scholl-smt08/model/model_5_8.smt2                            |    0.034s | 21.064MiB| sat | 0 |  |
|scholl-smt08/model/model_6_78.smt2                           |    0.034s | 21.08MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_55.smt2                         |    0.034s | 20.8MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_10.smt2                         |    0.034s | 21.04MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_3.smt2                          |    0.034s | 20.868MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_27.smt2                         |    0.034s | 21.344MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_10.smt2                         |    0.034s | 21.288MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_19.smt2                         |    0.034s | 21.288MiB| sat | 0 |  |
|tptp/NUM903=1.smt2                                           |    0.034s | 19.52MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node43741.smt2    |    0.034s | 20.296MiB| unsat | 0 |  |
|keymaera/water_tank-node31031.smt2                           |    0.034s | 20.548MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node3202.smt2     |    0.034s | 20.352MiB| unsat | 0 |  |
|keymaera/water_tank-node18158.smt2                           |    0.034s | 20.296MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node412027.smt2   |    0.034s | 19.536MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node412898.smt2   |    0.034s | 19.532MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_178.smt2                  |    0.035s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_071.smt2                  |    0.035s | 20.816MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_254.smt2                  |    0.035s | 19.532MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_090.smt2                  |    0.035s | 21.572MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_060.smt2                  |    0.035s | 21.332MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_178.smt2                  |    0.035s | 19.78MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_053.smt2                  |    0.035s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_285.smt2                  |    0.035s | 21.072MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_232.smt2                  |    0.035s | 21.332MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_170.smt2                  |    0.035s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_178.smt2                  |    0.035s | 20.816MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_080.smt2                  |    0.035s | 19.536MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_022.smt2                  |    0.035s | 21.588MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_137.smt2                  |    0.035s | 21.064MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_274.smt2                  |    0.035s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_127.smt2                  |    0.035s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_213.smt2                  |    0.035s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_148.smt2                  |    0.035s | 21.572MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_061.smt2                  |    0.035s | 21.572MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_075.smt2                  |    0.035s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_196.smt2                  |    0.035s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_027.smt2                  |    0.035s | 21.32MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_115.smt2                  |    0.035s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_235.smt2                  |    0.035s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_117.smt2                  |    0.035s | 21.24MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_062.smt2                  |    0.035s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_023.smt2                  |    0.035s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_098.smt2                  |    0.035s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_170.smt2                  |    0.035s | 20.816MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_017.smt2                  |    0.035s | 21.32MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_4_20.smt2                               |    0.035s | 21.668MiB| sat | 0 |  |
|scholl-smt08/RND/RND_3_18.smt2                               |    0.035s | 20.548MiB| sat | 0 |  |
|scholl-smt08/RND/RND_4_30.smt2                               |    0.035s | 19.788MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_3_27.smt2                               |    0.035s | 21.324MiB| sat | 0 |  |
|scholl-smt08/RND/RND_3_16.smt2                               |    0.035s | 21.572MiB| sat | 0 |  |
|scholl-smt08/model/model_5_14.smt2                           |    0.035s | 20.556MiB| sat | 0 |  |
|scholl-smt08/model/model_5_5.smt2                            |    0.035s | 20.808MiB| sat | 0 |  |
|scholl-smt08/model/model_5_19.smt2                           |    0.035s | 21.06MiB| sat | 0 |  |
|scholl-smt08/model/model_5_26.smt2                           |    0.035s | 20.812MiB| sat | 0 |  |
|scholl-smt08/model/model_6_77.smt2                           |    0.035s | 21.32MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_15.smt2                         |    0.035s | 21.316MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_39.smt2                         |    0.035s | 22.592MiB| sat | 0 |  |
|keymaera/intersection-example-simple.proof-node427939.smt2   |    0.035s | 19.528MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node419390.smt2   |    0.035s | 19.784MiB| unsat | 0 |  |
|keymaera/water_tank-node21140.smt2                           |    0.035s | 20.88MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node680821.smt2   |    0.035s | 19.788MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node688433.smt2   |    0.035s | 19.544MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_168.smt2                  |    0.036s | 20.824MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_219.smt2                  |    0.036s | 20.812MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_133.smt2                  |    0.036s | 21.316MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_272.smt2                  |    0.036s | 21.44MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_192.smt2                  |    0.036s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_008.smt2                  |    0.036s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_200.smt2                  |    0.036s | 20.816MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_185.smt2                  |    0.036s | 21.328MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_256.smt2                  |    0.036s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_105.smt2                  |    0.036s | 20.992MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_288.smt2                  |    0.036s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_089.smt2                  |    0.036s | 21.316MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_177.smt2                  |    0.036s | 21.564MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_073.smt2                  |    0.036s | 19.78MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_162.smt2                  |    0.036s | 21.16MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_240.smt2                  |    0.036s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_068.smt2                  |    0.036s | 20.804MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_203.smt2                  |    0.036s | 21.068MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_214.smt2                  |    0.036s | 21.292MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_004.smt2                  |    0.036s | 21.312MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_250.smt2                  |    0.036s | 21.576MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_049.smt2                  |    0.036s | 21.316MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_300.smt2                  |    0.036s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_123.smt2                  |    0.036s | 20.812MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_081.smt2                  |    0.036s | 21.22MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_026.smt2                  |    0.036s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_148.smt2                  |    0.036s | 20.812MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_266.smt2                  |    0.036s | 21.056MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_298.smt2                  |    0.036s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_003.smt2                  |    0.036s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_228.smt2                  |    0.036s | 20.708MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_280.smt2                  |    0.036s | 21.312MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_275.smt2                  |    0.036s | 20.812MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_223.smt2                  |    0.036s | 21.072MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_6_3.smt2                                |    0.036s | 21.064MiB| sat | 0 |  |
|scholl-smt08/RND/RND_6_4.smt2                                |    0.036s | 21.568MiB| sat | 0 |  |
|scholl-smt08/RND/RND_3_25.smt2                               |    0.036s | 21.572MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_6_37.smt2                               |    0.036s | 20.812MiB| unsat | 0 |  |
|scholl-smt08/model/model_5_64.smt2                           |    0.036s | 21.572MiB| sat | 0 |  |
|scholl-smt08/model/model_6_47.smt2                           |    0.036s | 21.056MiB| sat | 0 |  |
|scholl-smt08/model/model_5_59.smt2                           |    0.036s | 21.312MiB| sat | 0 |  |
|scholl-smt08/model/model_5_41.smt2                           |    0.036s | 21.476MiB| sat | 0 |  |
|scholl-smt08/model/model_6_36.smt2                           |    0.036s | 21.06MiB| sat | 0 |  |
|scholl-smt08/model/model_6_80.smt2                           |    0.036s | 21.316MiB| sat | 0 |  |
|scholl-smt08/model/model_6_51.smt2                           |    0.036s | 20.808MiB| sat | 0 |  |
|scholl-smt08/model/model_4_6.smt2                            |    0.036s | 21.22MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_28.smt2                         |    0.036s | 20.82MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_12.smt2                         |    0.036s | 21.552MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_38.smt2                         |    0.036s | 21.568MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_4.smt2                          |    0.036s | 20.816MiB| sat | 0 |  |
|keymaera/intersection-example-simple.proof-node453137.smt2   |    0.036s | 19.536MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node98609.smt2    |    0.036s | 20.04MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node428602.smt2   |    0.036s | 19.536MiB| unsat | 0 |  |
|keymaera/water_tank-node30133.smt2                           |    0.036s | 20.3MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node75660.smt2    |    0.036s | 20.04MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_053.smt2                  |    0.037s | 20.812MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_110.smt2                  |    0.037s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_276.smt2                  |    0.037s | 21.296MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_110.smt2                  |    0.037s | 20.812MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_103.smt2                  |    0.037s | 21.288MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_117.smt2                  |    0.037s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_133.smt2                  |    0.037s | 21.472MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_122.smt2                  |    0.037s | 21.32MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_215.smt2                  |    0.037s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_016.smt2                  |    0.037s | 21.588MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_140.smt2                  |    0.037s | 20.936MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_126.smt2                  |    0.037s | 21.576MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_169.smt2                  |    0.037s | 21.324MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_164.smt2                  |    0.037s | 21.32MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_224.smt2                  |    0.037s | 21.064MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_044.smt2                  |    0.037s | 21.34MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_107.smt2                  |    0.037s | 21.576MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_217.smt2                  |    0.037s | 21.324MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_195.smt2                  |    0.037s | 21.312MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_155.smt2                  |    0.037s | 21.06MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_024.smt2                  |    0.037s | 21.08MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_185.smt2                  |    0.037s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_274.smt2                  |    0.037s | 20.812MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_241.smt2                  |    0.037s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_151.smt2                  |    0.037s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_074.smt2                  |    0.037s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_059.smt2                  |    0.037s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_063.smt2                  |    0.037s | 21.056MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_4_17.smt2                               |    0.037s | 21.324MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_6_14.smt2                               |    0.037s | 21.984MiB| sat | 0 |  |
|scholl-smt08/RND/RND_3_24.smt2                               |    0.037s | 22.604MiB| unsat | 0 |  |
|scholl-smt08/model/model_4_5.smt2                            |    0.037s | 21.06MiB| sat | 0 |  |
|scholl-smt08/model/model_6_72.smt2                           |    0.037s | 21.584MiB| sat | 0 |  |
|scholl-smt08/model/model_6_43.smt2                           |    0.037s | 21.068MiB| sat | 0 |  |
|scholl-smt08/model/model_5_63.smt2                           |    0.037s | 21.328MiB| sat | 0 |  |
|scholl-smt08/model/model_5_51.smt2                           |    0.037s | 21.32MiB| sat | 0 |  |
|scholl-smt08/model/model_6_60.smt2                           |    0.037s | 20.864MiB| sat | 0 |  |
|scholl-smt08/model/model_5_38.smt2                           |    0.037s | 21.58MiB| sat | 0 |  |
|scholl-smt08/model/model_5_53.smt2                           |    0.037s | 21.068MiB| sat | 0 |  |
|scholl-smt08/model/model_6_41.smt2                           |    0.037s | 21.064MiB| sat | 0 |  |
|scholl-smt08/model/model_5_45.smt2                           |    0.037s | 21.328MiB| sat | 0 |  |
|scholl-smt08/model/model_6_75.smt2                           |    0.037s | 21.312MiB| sat | 0 |  |
|scholl-smt08/model/model_5_17.smt2                           |    0.037s | 21.06MiB| sat | 0 |  |
|scholl-smt08/model/model_6_58.smt2                           |    0.037s | 21.06MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_33.smt2                         |    0.037s | 21.572MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_32.smt2                         |    0.037s | 20.076MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_50.smt2                         |    0.037s | 22.088MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_17.smt2                         |    0.037s | 20.812MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_20.smt2                         |    0.037s | 21.452MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_21.smt2                         |    0.037s | 21.828MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_58.smt2                         |    0.037s | 21.064MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node5527.smt2     |    0.037s | 19.66MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node46397.smt2    |    0.037s | 19.532MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node453789.smt2   |    0.037s | 19.528MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node71792.smt2    |    0.037s | 19.536MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_247.smt2                  |    0.038s | 21.292MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_291.smt2                  |    0.038s | 21.068MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_049.smt2                  |    0.038s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_156.smt2                  |    0.038s | 21.488MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_135.smt2                  |    0.038s | 21.444MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_109.smt2                  |    0.038s | 21.32MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_270.smt2                  |    0.038s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_077.smt2                  |    0.038s | 21.488MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_074.smt2                  |    0.038s | 21.072MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_003.smt2                  |    0.038s | 21.072MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_232.smt2                  |    0.038s | 21.316MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_083.smt2                  |    0.038s | 21.112MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_143.smt2                  |    0.038s | 21.064MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_207.smt2                  |    0.038s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_138.smt2                  |    0.038s | 20.816MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_246.smt2                  |    0.038s | 21.332MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_163.smt2                  |    0.038s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_168.smt2                  |    0.038s | 21.208MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_040.smt2                  |    0.038s | 20.828MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_240.smt2                  |    0.038s | 20.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_232.smt2                  |    0.038s | 20.812MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_175.smt2                  |    0.038s | 21.312MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_085.smt2                  |    0.038s | 20.812MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_198.smt2                  |    0.038s | 21.072MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_4_16.smt2                               |    0.038s | 21.832MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_6_27.smt2                               |    0.038s | 21.584MiB| unsat | 0 |  |
|scholl-smt08/model/model_5_39.smt2                           |    0.038s | 21.58MiB| sat | 0 |  |
|scholl-smt08/model/model_5_9.smt2                            |    0.038s | 20.852MiB| sat | 0 |  |
|scholl-smt08/model/model_6_54.smt2                           |    0.038s | 20.824MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_32.smt2                         |    0.038s | 20.804MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_58.smt2                         |    0.038s | 21.328MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_11.smt2                         |    0.038s | 21.072MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_29.smt2                         |    0.038s | 21.82MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_25.smt2                         |    0.038s | 21.576MiB| unsat | 0 |  |
|tptp/ARI371=1.smt2                                           |    0.038s | 20.556MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node15586.smt2    |    0.038s | 19.536MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_181.smt2                  |    0.039s | 20.82MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_200.smt2                  |    0.039s | 21.296MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_189.smt2                  |    0.039s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_104.smt2                  |    0.039s | 21.324MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_258.smt2                  |    0.039s | 21.34MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_066.smt2                  |    0.039s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_152.smt2                  |    0.039s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_053.smt2                  |    0.039s | 21.316MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_056.smt2                  |    0.039s | 21.328MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_292.smt2                  |    0.039s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_174.smt2                  |    0.039s | 21.316MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_172.smt2                  |    0.039s | 21.552MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_052.smt2                  |    0.039s | 21.572MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_280.smt2                  |    0.039s | 21.424MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_204.smt2                  |    0.039s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_006.smt2                  |    0.039s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_078.smt2                  |    0.039s | 21.072MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_065.smt2                  |    0.039s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_164.smt2                  |    0.039s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_156.smt2                  |    0.039s | 21.072MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_239.smt2                  |    0.039s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_051.smt2                  |    0.039s | 21.312MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_249.smt2                  |    0.039s | 20.816MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_283.smt2                  |    0.039s | 21.316MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_3_30.smt2                               |    0.039s | 21.824MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_3_14.smt2                               |    0.039s | 21.316MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_3_6.smt2                                |    0.039s | 20.82MiB| sat | 0 |  |
|scholl-smt08/model/model_6_74.smt2                           |    0.039s | 21.076MiB| sat | 0 |  |
|scholl-smt08/model/model_4_4.smt2                            |    0.039s | 20.812MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_44.smt2                         |    0.039s | 22.848MiB| sat | 0 |  |
|keymaera/intersection-example-simple.proof-node450481.smt2   |    0.039s | 20.396MiB| unsat | 0 |  |
|keymaera/water_tank-node25649.smt2                           |    0.039s | 19.908MiB| unsat | 0 |  |
|keymaera/intersection-example-simple.proof-node462796.smt2   |    0.039s | 19.536MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_103.smt2                  |    0.040s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_069.smt2                  |    0.040s | 21.072MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_137.smt2                  |    0.040s | 21.572MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_147.smt2                  |    0.040s | 21.068MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_145.smt2                  |    0.040s | 21.128MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_123.smt2                  |    0.040s | 21.232MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_138.smt2                  |    0.040s | 21.216MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_043.smt2                  |    0.040s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_239.smt2                  |    0.040s | 21.572MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_139.smt2                  |    0.040s | 21.572MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_121.smt2                  |    0.040s | 21.372MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_045.smt2                  |    0.040s | 21.316MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_047.smt2                  |    0.040s | 21.572MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_016.smt2                  |    0.040s | 21.368MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_062.smt2                  |    0.040s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_113.smt2                  |    0.040s | 20.812MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_179.smt2                  |    0.040s | 21.32MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_030.smt2                  |    0.040s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_205.smt2                  |    0.040s | 21.18MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_007.smt2                  |    0.040s | 20.812MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_247.smt2                  |    0.040s | 21.08MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_021.smt2                  |    0.040s | 20.808MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_083.smt2                  |    0.040s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_058.smt2                  |    0.040s | 20.812MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_254.smt2                  |    0.040s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_149.smt2                  |    0.040s | 21.064MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_6_21.smt2                               |    0.040s | 21.196MiB| sat | 0 |  |
|scholl-smt08/model/model_6_63.smt2                           |    0.040s | 21.064MiB| sat | 0 |  |
|scholl-smt08/model/model_5_49.smt2                           |    0.040s | 21.324MiB| sat | 0 |  |
|scholl-smt08/model/model_5_35.smt2                           |    0.040s | 21.58MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_35.smt2                         |    0.040s | 21.832MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_48.smt2                         |    0.040s | 22.08MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_57.smt2                         |    0.040s | 21.984MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_16.smt2                         |    0.040s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_145.smt2                  |    0.041s | 21.296MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_085.smt2                  |    0.041s | 21.072MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_186.smt2                  |    0.041s | 21.576MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_150.smt2                  |    0.041s | 21.468MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_215.smt2                  |    0.041s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_217.smt2                  |    0.041s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_016.smt2                  |    0.041s | 21.568MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_060.smt2                  |    0.041s | 21.14MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_117.smt2                  |    0.041s | 21.576MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_157.smt2                  |    0.041s | 21.396MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_176.smt2                  |    0.041s | 21.324MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_201.smt2                  |    0.041s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_057.smt2                  |    0.041s | 21.312MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_127.smt2                  |    0.041s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_268.smt2                  |    0.041s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_061.smt2                  |    0.041s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_231.smt2                  |    0.041s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_134.smt2                  |    0.041s | 21.304MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_242.smt2                  |    0.041s | 21.312MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_004.smt2                  |    0.041s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_259.smt2                  |    0.041s | 21.276MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_290.smt2                  |    0.041s | 20.9MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_222.smt2                  |    0.041s | 21.184MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_4_14.smt2                               |    0.041s | 21.328MiB| sat | 0 |  |
|scholl-smt08/RND/RND_4_15.smt2                               |    0.041s | 21.828MiB| sat | 0 |  |
|scholl-smt08/RND/RND_6_18.smt2                               |    0.041s | 21.584MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_34.smt2                         |    0.041s | 21.832MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_54.smt2                         |    0.041s | 25.092MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_296.smt2                  |    0.042s | 21.476MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_115.smt2                  |    0.042s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_116.smt2                  |    0.042s | 21.576MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_242.smt2                  |    0.042s | 21.628MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_262.smt2                  |    0.042s | 21.316MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_236.smt2                  |    0.042s | 21.14MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_154.smt2                  |    0.042s | 21.324MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_264.smt2                  |    0.042s | 21.568MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_197.smt2                  |    0.042s | 21.576MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_282.smt2                  |    0.042s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_025.smt2                  |    0.042s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_147.smt2                  |    0.042s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_109.smt2                  |    0.042s | 21.008MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_144.smt2                  |    0.042s | 21.052MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_203.smt2                  |    0.042s | 21.292MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_4_28.smt2                               |    0.042s | 21.576MiB| sat | 0 |  |
|scholl-smt08/RND/RND_6_9.smt2                                |    0.042s | 21.828MiB| sat | 0 |  |
|scholl-smt08/model/model_5_40.smt2                           |    0.042s | 21.572MiB| sat | 0 |  |
|scholl-smt08/model/model_5_33.smt2                           |    0.042s | 21.332MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_31.smt2                         |    0.042s | 22.596MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_099.smt2                  |    0.043s | 21.32MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_065.smt2                  |    0.043s | 21.064MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_153.smt2                  |    0.043s | 21.328MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_255.smt2                  |    0.043s | 20.808MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_183.smt2                  |    0.043s | 21.316MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_298.smt2                  |    0.043s | 21.32MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_249.smt2                  |    0.043s | 21.32MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_125.smt2                  |    0.043s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_296.smt2                  |    0.043s | 21.32MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_167.smt2                  |    0.043s | 21.072MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_022.smt2                  |    0.043s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_252.smt2                  |    0.043s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_145.smt2                  |    0.043s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_089.smt2                  |    0.043s | 21.312MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_073.smt2                  |    0.043s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_146.smt2                  |    0.043s | 21.072MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_086.smt2                  |    0.043s | 21.064MiB| unsat | 0 |  |
|scholl-smt08/model/model_5_36.smt2                           |    0.043s | 21.58MiB| sat | 0 |  |
|scholl-smt08/model/model_5_44.smt2                           |    0.043s | 21.572MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_14.smt2                         |    0.043s | 21.328MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_53.smt2                         |    0.043s | 22.596MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_24.smt2                         |    0.043s | 21.328MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_137.smt2                  |    0.044s | 21.572MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_226.smt2                  |    0.044s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_024.smt2                  |    0.044s | 21.448MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_076.smt2                  |    0.044s | 21.588MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_036.smt2                  |    0.044s | 21.54MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_267.smt2                  |    0.044s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_033.smt2                  |    0.044s | 21.568MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_153.smt2                  |    0.044s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_041.smt2                  |    0.044s | 21.184MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_226.smt2                  |    0.044s | 20.812MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_279.smt2                  |    0.044s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_219.smt2                  |    0.044s | 21.076MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_3_29.smt2                               |    0.044s | 21.576MiB| sat | 0 |  |
|scholl-smt08/RND/RND_3_22.smt2                               |    0.044s | 21.624MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_6_11.smt2                               |    0.044s | 22.336MiB| sat | 0 |  |
|keymaera/intersection-example-simple.proof-node80217.smt2    |    0.044s | 20.552MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_176.smt2                  |    0.045s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_069.smt2                  |    0.045s | 21.32MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_090.smt2                  |    0.045s | 21.592MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_106.smt2                  |    0.045s | 21.316MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_072.smt2                  |    0.045s | 21.064MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_116.smt2                  |    0.045s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_050.smt2                  |    0.045s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_214.smt2                  |    0.045s | 21.312MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_265.smt2                  |    0.045s | 20.812MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_277.smt2                  |    0.046s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_102.smt2                  |    0.046s | 21.82MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_010.smt2                  |    0.046s | 21.576MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_119.smt2                  |    0.046s | 21.576MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_104.smt2                  |    0.046s | 21.324MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_136.smt2                  |    0.046s | 21.072MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_272.smt2                  |    0.046s | 21.32MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_059.smt2                  |    0.046s | 20.836MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_088.smt2                  |    0.046s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_263.smt2                  |    0.046s | 20.816MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_215.smt2                  |    0.046s | 20.992MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_181.smt2                  |    0.046s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_045.smt2                  |    0.046s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_071.smt2                  |    0.046s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_293.smt2                  |    0.046s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_269.smt2                  |    0.046s | 20.804MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_052.smt2                  |    0.046s | 21.28MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_064.smt2                  |    0.046s | 20.816MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_6_40.smt2                               |    0.046s | 21.308MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_35.smt2                         |    0.046s | 22.356MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_003.smt2                  |    0.047s | 21.316MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_009.smt2                  |    0.047s | 21.076MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_198.smt2                  |    0.047s | 21.588MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_285.smt2                  |    0.047s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_075.smt2                  |    0.047s | 21.312MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_115.smt2                  |    0.047s | 21.064MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_6_1.smt2                                |    0.047s | 20.812MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_43.smt2                         |    0.047s | 21.42MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_122.smt2                  |    0.048s | 21.58MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_231.smt2                  |    0.048s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_189.smt2                  |    0.048s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_207.smt2                  |    0.048s | 21.572MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_080.smt2                  |    0.048s | 21.316MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_117.smt2                  |    0.048s | 21.072MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_208.smt2                  |    0.048s | 21.512MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_212.smt2                  |    0.048s | 21.572MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_032.smt2                  |    0.048s | 21.328MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_189.smt2                  |    0.048s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_169.smt2                  |    0.048s | 20.564MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_157.smt2                  |    0.048s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_255.smt2                  |    0.048s | 21.072MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_177.smt2                  |    0.048s | 21.076MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_212.smt2                  |    0.048s | 21.316MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_4_23.smt2                               |    0.048s | 22.508MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_59.smt2                         |    0.048s | 23.76MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_256.smt2                  |    0.049s | 21.328MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_278.smt2                  |    0.049s | 21.332MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_209.smt2                  |    0.049s | 21.324MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_293.smt2                  |    0.049s | 21.572MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_128.smt2                  |    0.049s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_131.smt2                  |    0.049s | 21.328MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_031.smt2                  |    0.049s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_092.smt2                  |    0.049s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_002.smt2                  |    0.049s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_159.smt2                  |    0.049s | 21.312MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_194.smt2                  |    0.049s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_038.smt2                  |    0.049s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_002.smt2                  |    0.050s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_206.smt2                  |    0.050s | 21.32MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_025.smt2                  |    0.050s | 21.576MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_220.smt2                  |    0.050s | 21.724MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_229.smt2                  |    0.050s | 21.2MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_269.smt2                  |    0.050s | 21.568MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_285.smt2                  |    0.050s | 21.324MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_231.smt2                  |    0.050s | 21.568MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_087.smt2                  |    0.050s | 21.324MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_6_23.smt2                               |    0.050s | 22.856MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_210.smt2                  |    0.051s | 21.568MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_175.smt2                  |    0.051s | 21.82MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_039.smt2                  |    0.051s | 21.328MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_139.smt2                  |    0.051s | 21.088MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_238.smt2                  |    0.051s | 20.812MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_114.smt2                  |    0.051s | 21.072MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_087.smt2                  |    0.051s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_264.smt2                  |    0.051s | 21.316MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_6_13.smt2                               |    0.051s | 21.836MiB| sat | 0 |  |
|scholl-smt08/RND/RND_4_24.smt2                               |    0.051s | 22.616MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_68.smt2                         |    0.051s | 23.364MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_215.smt2                  |    0.052s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_217.smt2                  |    0.052s | 21.328MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_129.smt2                  |    0.052s | 21.52MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_105.smt2                  |    0.052s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_113.smt2                  |    0.052s | 21.072MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_039.smt2                  |    0.052s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_251.smt2                  |    0.053s | 21.568MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_104.smt2                  |    0.054s | 21.32MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_254.smt2                  |    0.054s | 21.324MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_253.smt2                  |    0.054s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_044.smt2                  |    0.054s | 21.312MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_056.smt2                  |    0.054s | 20.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_284.smt2                  |    0.054s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_292.smt2                  |    0.054s | 21.072MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_180.smt2                  |    0.054s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_166.smt2                  |    0.055s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_226.smt2                  |    0.055s | 21.468MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_276.smt2                  |    0.055s | 20.816MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_106.smt2                  |    0.055s | 21.312MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_195.smt2                  |    0.055s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_129.smt2                  |    0.055s | 21.272MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_281.smt2                  |    0.055s | 21.316MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_50.smt2                         |    0.055s | 23.368MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_22.smt2                         |    0.055s | 21.872MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_225.smt2                  |    0.056s | 21.332MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_182.smt2                  |    0.056s | 21.632MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_161.smt2                  |    0.056s | 21.552MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_060.smt2                  |    0.056s | 21.072MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_189.smt2                  |    0.056s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_001.smt2                  |    0.056s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_116.smt2                  |    0.057s | 21.48MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_043.smt2                  |    0.057s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_135.smt2                  |    0.057s | 20.976MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_288.smt2                  |    0.057s | 21.072MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_121.smt2                  |    0.057s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_272.smt2                  |    0.057s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_267.smt2                  |    0.057s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_204.smt2                  |    0.057s | 20.984MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_008.smt2                  |    0.057s | 21.332MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_035.smt2                  |    0.058s | 21.576MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_165.smt2                  |    0.058s | 21.568MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_151.smt2                  |    0.058s | 21.32MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_046.smt2                  |    0.058s | 21.572MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_176.smt2                  |    0.058s | 21.312MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_271.smt2                  |    0.058s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_095.smt2                  |    0.058s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_033.smt2                  |    0.059s | 21.324MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_299.smt2                  |    0.059s | 21.32MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_076.smt2                  |    0.059s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_224.smt2                  |    0.059s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_141.smt2                  |    0.059s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_299.smt2                  |    0.059s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_184.smt2                  |    0.059s | 21.328MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_111.smt2                  |    0.059s | 21.072MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_4_19.smt2                               |    0.059s | 21.828MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_41.smt2                         |    0.059s | 23.372MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_137.smt2                  |    0.060s | 21.576MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_175.smt2                  |    0.060s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_271.smt2                  |    0.060s | 21.468MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_128.smt2                  |    0.060s | 20.984MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_060.smt2                  |    0.061s | 21.32MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_226.smt2                  |    0.061s | 21.572MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_130.smt2                  |    0.061s | 21.32MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_279.smt2                  |    0.061s | 21.32MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_158.smt2                  |    0.061s | 21.548MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_262.smt2                  |    0.061s | 21.264MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_037.smt2                  |    0.061s | 21.308MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_233.smt2                  |    0.061s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_167.smt2                  |    0.061s | 21.072MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_39.smt2                         |    0.061s | 21.632MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_48.smt2                         |    0.061s | 23.516MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_076.smt2                  |    0.062s | 21.736MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_220.smt2                  |    0.062s | 21.324MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_003.smt2                  |    0.062s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_132.smt2                  |    0.062s | 21.704MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_132.smt2                  |    0.062s | 21.312MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_3_23.smt2                               |    0.062s | 22.344MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_177.smt2                  |    0.063s | 21.568MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_126.smt2                  |    0.063s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_142.smt2                  |    0.063s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_160.smt2                  |    0.063s | 21.096MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_234.smt2                  |    0.063s | 21.072MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_103.smt2                  |    0.064s | 21.332MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_177.smt2                  |    0.064s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_251.smt2                  |    0.064s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_287.smt2                  |    0.064s | 21.204MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_098.smt2                  |    0.065s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_166.smt2                  |    0.065s | 21.324MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_103.smt2                  |    0.065s | 21.372MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_137.smt2                  |    0.065s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_286.smt2                  |    0.065s | 21.204MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_237.smt2                  |    0.065s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_202.smt2                  |    0.065s | 21.08MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_12.smt2                         |    0.065s | 21.804MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_225.smt2                  |    0.066s | 21.068MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_6_30.smt2                               |    0.066s | 23.116MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_6.smt2                          |    0.066s | 21.576MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_102.smt2                  |    0.067s | 21.584MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_066.smt2                  |    0.067s | 20.82MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_010.smt2                  |    0.067s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_186.smt2                  |    0.068s | 21.068MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_123.smt2                  |    0.068s | 21.716MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_179.smt2                  |    0.068s | 21.824MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_102.smt2                  |    0.068s | 21.332MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_122.smt2                  |    0.068s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_136.smt2                  |    0.068s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_097.smt2                  |    0.068s | 21.308MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_277.smt2                  |    0.069s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_019.smt2                  |    0.069s | 21.224MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_3_26.smt2                               |    0.069s | 22.208MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_201.smt2                  |    0.070s | 21.848MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_114.smt2                  |    0.070s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_002.smt2                  |    0.070s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_275.smt2                  |    0.070s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_143.smt2                  |    0.070s | 21.064MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_49.smt2                         |    0.070s | 22.86MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_52.smt2                         |    0.070s | 23.636MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_039.smt2                  |    0.072s | 21.568MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_003.smt2                  |    0.072s | 21.584MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_150.smt2                  |    0.072s | 21.564MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_108.smt2                  |    0.072s | 21.324MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_102.smt2                  |    0.072s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_140.smt2                  |    0.072s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_256.smt2                  |    0.073s | 21.08MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_159.smt2                  |    0.073s | 21.32MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_236.smt2                  |    0.073s | 21.04MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_222.smt2                  |    0.074s | 21.828MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_221.smt2                  |    0.074s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_030.smt2                  |    0.074s | 21.32MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_095.smt2                  |    0.074s | 21.58MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_21.smt2                         |    0.074s | 22.088MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_162.smt2                  |    0.075s | 21.316MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_60.smt2                         |    0.075s | 26.924MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_291.smt2                  |    0.076s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_103.smt2                  |    0.076s | 21.308MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_232.smt2                  |    0.077s | 21.464MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_160.smt2                  |    0.077s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_256.smt2                  |    0.077s | 21.572MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_49.smt2                         |    0.077s | 23.876MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_26.smt2                         |    0.077s | 22.096MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_112.smt2                  |    0.078s | 21.576MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_173.smt2                  |    0.078s | 21.076MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_077.smt2                  |    0.078s | 21.068MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_55.smt2                         |    0.078s | 24.012MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_290.smt2                  |    0.079s | 21.572MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_296.smt2                  |    0.079s | 21.312MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_050.smt2                  |    0.079s | 21.32MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_4_21.smt2                               |    0.079s | 24.056MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_016.smt2                  |    0.080s | 21.08MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_123.smt2                  |    0.080s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_055.smt2                  |    0.080s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_014.smt2                  |    0.080s | 21.212MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_263.smt2                  |    0.081s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_150.smt2                  |    0.081s | 20.968MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_263.smt2                  |    0.081s | 21.872MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_221.smt2                  |    0.081s | 21.072MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_222.smt2                  |    0.082s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_149.smt2                  |    0.083s | 21.836MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_227.smt2                  |    0.084s | 21.832MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_124.smt2                  |    0.084s | 21.32MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_22.smt2                         |    0.084s | 22.088MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_175.smt2                  |    0.085s | 21.072MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_217.smt2                  |    0.085s | 21.324MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_4_27.smt2                               |    0.086s | 23.344MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_183.smt2                  |    0.087s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_093.smt2                  |    0.087s | 21.308MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_035.smt2                  |    0.088s | 21.644MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_176.smt2                  |    0.088s | 21.624MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_042.smt2                  |    0.088s | 21.824MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_16.smt2                         |    0.090s | 21.824MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_117.smt2                  |    0.091s | 21.584MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_166.smt2                  |    0.091s | 21.076MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_216.smt2                  |    0.092s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_110.smt2                  |    0.092s | 21.308MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_120.smt2                  |    0.093s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_176.smt2                  |    0.094s | 21.824MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_297.smt2                  |    0.095s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_102.smt2                  |    0.096s | 21.576MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_290.smt2                  |    0.096s | 21.572MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_42.smt2                         |    0.096s | 23.628MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_220.smt2                  |    0.097s | 21.576MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_242.smt2                  |    0.097s | 21.58MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_256.smt2                  |    0.097s | 21.312MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_51.smt2                         |    0.097s | 27.428MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_161.smt2                  |    0.098s | 21.828MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_012.smt2                  |    0.098s | 21.076MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_114.smt2                  |    0.099s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_179.smt2                  |    0.099s | 21.568MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_069.smt2                  |    0.100s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_155.smt2                  |    0.101s | 20.992MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_099.smt2                  |    0.102s | 21.608MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_032.smt2                  |    0.102s | 21.576MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_1.smt2                          |    0.102s | 21.528MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_122.smt2                  |    0.103s | 21.328MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_049.smt2                  |    0.103s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_242.smt2                  |    0.103s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_258.smt2                  |    0.103s | 21.324MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_268.smt2                  |    0.104s | 21.576MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_173.smt2                  |    0.105s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_089.smt2                  |    0.106s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_039.smt2                  |    0.107s | 21.832MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_154.smt2                  |    0.107s | 21.072MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_238.smt2                  |    0.108s | 21.58MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_231.smt2                  |    0.109s | 21.576MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_221.smt2                  |    0.109s | 21.828MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_160.smt2                  |    0.110s | 21.58MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_201.smt2                  |    0.110s | 21.58MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_236.smt2                  |    0.112s | 21.724MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_013.smt2                  |    0.112s | 21.308MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_115.smt2                  |    0.113s | 21.328MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_289.smt2                  |    0.113s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_099.smt2                  |    0.114s | 21.236MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_221.smt2                  |    0.118s | 21.568MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_049.smt2                  |    0.118s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_125.smt2                  |    0.119s | 21.312MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_100.smt2                  |    0.120s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_232.smt2                  |    0.121s | 21.324MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_097.smt2                  |    0.121s | 21.364MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_088.smt2                  |    0.121s | 21.844MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_244.smt2                  |    0.121s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_190.smt2                  |    0.122s | 21.824MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_026.smt2                  |    0.122s | 21.328MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_214.smt2                  |    0.123s | 21.576MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_163.smt2                  |    0.126s | 21.584MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_34.smt2                         |    0.126s | 22.572MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_053.smt2                  |    0.128s | 20.816MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_295.smt2                  |    0.128s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_178.smt2                  |    0.129s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_116.smt2                  |    0.130s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_161.smt2                  |    0.130s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_086.smt2                  |    0.132s | 21.324MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_67.smt2                         |    0.135s | 44.56MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_002.smt2                  |    0.136s | 21.576MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_62.smt2                         |    0.136s | 29.82MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_123.smt2                  |    0.137s | 21.332MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_049.smt2                  |    0.137s | 21.312MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_6_26.smt2                               |    0.140s | 23.624MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_299.smt2                  |    0.141s | 21.58MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_107.smt2                  |    0.142s | 21.588MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_265.smt2                  |    0.142s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_068.smt2                  |    0.142s | 21.316MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_2.smt2                          |    0.143s | 21.328MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_107.smt2                  |    0.144s | 21.32MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_4_26.smt2                               |    0.146s | 24.116MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_36.smt2                         |    0.146s | 23.224MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_228.smt2                  |    0.150s | 21.324MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_130.smt2                  |    0.150s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_228.smt2                  |    0.154s | 21.828MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_209.smt2                  |    0.155s | 21.32MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_30.smt2                         |    0.155s | 22.348MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_24.smt2                         |    0.155s | 22.084MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_221.smt2                  |    0.156s | 22.1MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_255.smt2                  |    0.157s | 21.58MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_290.smt2                  |    0.157s | 21.62MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_236.smt2                  |    0.158s | 21.516MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_205.smt2                  |    0.159s | 21.58MiB| sat | 0 |  |
|scholl-smt08/RND/RND_6_33.smt2                               |    0.159s | 27.148MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_225.smt2                  |    0.161s | 21.584MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_126.smt2                  |    0.161s | 21.576MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_196.smt2                  |    0.161s | 21.744MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_6_15.smt2                               |    0.161s | 22.088MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_126.smt2                  |    0.164s | 21.32MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_156.smt2                  |    0.167s | 21.476MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_078.smt2                  |    0.168s | 21.58MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_133.smt2                  |    0.168s | 21.06MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_107.smt2                  |    0.170s | 21.828MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_018.smt2                  |    0.170s | 21.824MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_260.smt2                  |    0.172s | 21.724MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_274.smt2                  |    0.173s | 21.232MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_012.smt2                  |    0.173s | 21.816MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_294.smt2                  |    0.174s | 21.832MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_6_20.smt2                               |    0.175s | 22.604MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_042.smt2                  |    0.176s | 21.664MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_255.smt2                  |    0.177s | 21.576MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_263.smt2                  |    0.180s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_295.smt2                  |    0.181s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_098.smt2                  |    0.184s | 21.824MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_097.smt2                  |    0.184s | 21.576MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_36.smt2                         |    0.184s | 23.252MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_40.smt2                         |    0.186s | 22.86MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_185.smt2                  |    0.187s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_206.smt2                  |    0.187s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_086.smt2                  |    0.188s | 21.844MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_191.smt2                  |    0.188s | 22.164MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_277.smt2                  |    0.190s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_042.smt2                  |    0.191s | 21.584MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_47.smt2                         |    0.195s | 22.336MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_116.smt2                  |    0.198s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_163.smt2                  |    0.199s | 21.58MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_036.smt2                  |    0.199s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_213.smt2                  |    0.201s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_026.smt2                  |    0.202s | 21.212MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_039.smt2                  |    0.202s | 21.588MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_066.smt2                  |    0.203s | 22.156MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_086.smt2                  |    0.204s | 21.58MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_59.smt2                         |    0.206s | 26.092MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_45.smt2                         |    0.206s | 24.772MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_213.smt2                  |    0.208s | 21.508MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_066.smt2                  |    0.209s | 21.576MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_149.smt2                  |    0.209s | 21.832MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_001.smt2                  |    0.211s | 21.8MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_045.smt2                  |    0.212s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_030.smt2                  |    0.214s | 21.828MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_063.smt2                  |    0.215s | 21.82MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_53.smt2                         |    0.218s | 23.364MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_021.smt2                  |    0.219s | 21.576MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_60.smt2                         |    0.220s | 28.832MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_235.smt2                  |    0.221s | 21.576MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_223.smt2                  |    0.221s | 21.832MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_037.smt2                  |    0.223s | 22.084MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_300.smt2                  |    0.223s | 21.828MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_197.smt2                  |    0.230s | 21.312MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_032.smt2                  |    0.231s | 21.328MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_148.smt2                  |    0.235s | 22.08MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_152.smt2                  |    0.236s | 21.032MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_149.smt2                  |    0.245s | 21.956MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_115.smt2                  |    0.246s | 21.928MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_274.smt2                  |    0.249s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_201.smt2                  |    0.249s | 21.576MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_083.smt2                  |    0.249s | 21.96MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_072.smt2                  |    0.250s | 21.592MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_057.smt2                  |    0.258s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_027.smt2                  |    0.258s | 22.08MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_255.smt2                  |    0.261s | 21.576MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_213.smt2                  |    0.264s | 21.576MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_235.smt2                  |    0.265s | 21.592MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_075.smt2                  |    0.265s | 21.836MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_231.smt2                  |    0.266s | 21.324MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_037.smt2                  |    0.269s | 21.608MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_275.smt2                  |    0.271s | 22.088MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_161.smt2                  |    0.271s | 21.636MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_54.smt2                         |    0.275s | 27.572MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_031.smt2                  |    0.279s | 21.592MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_210.smt2                  |    0.280s | 21.324MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_242.smt2                  |    0.281s | 21.576MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_092.smt2                  |    0.281s | 21.828MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_299.smt2                  |    0.286s | 21.584MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_084.smt2                  |    0.286s | 21.96MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_156.smt2                  |    0.287s | 21.576MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_131.smt2                  |    0.288s | 21.828MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_030.smt2                  |    0.293s | 21.828MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_028.smt2                  |    0.296s | 20.816MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_236.smt2                  |    0.299s | 21.828MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_026.smt2                  |    0.300s | 21.58MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_054.smt2                  |    0.311s | 21.832MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_57.smt2                         |    0.311s | 25.612MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_083.smt2                  |    0.313s | 21.708MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_023.smt2                  |    0.318s | 21.832MiB| sat | 0 |  |
|scholl-smt08/RND/RND_6_16.smt2                               |    0.320s | 22.852MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_298.smt2                  |    0.327s | 22.316MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_3_56.smt2                         |    0.328s | 28.708MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_066.smt2                  |    0.330s | 22.056MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_289.smt2                  |    0.331s | 21.976MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_031.smt2                  |    0.331s | 21.728MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_163.smt2                  |    0.337s | 21.584MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_217.smt2                  |    0.338s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_246.smt2                  |    0.339s | 22.34MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_004.smt2                  |    0.340s | 21.572MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_046.smt2                  |    0.341s | 21.828MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_079.smt2                  |    0.343s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_161.smt2                  |    0.344s | 21.576MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_027.smt2                  |    0.348s | 22.316MiB| sat | 0 |  |
|scholl-smt08/RND/RND_6_22.smt2                               |    0.348s | 25.664MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_144.smt2                  |    0.352s | 21.576MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_004.smt2                  |    0.353s | 21.8MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_165.smt2                  |    0.357s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_144.smt2                  |    0.358s | 21.848MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_186.smt2                  |    0.362s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_142.smt2                  |    0.365s | 21.58MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_032.smt2                  |    0.365s | 21.84MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_278.smt2                  |    0.367s | 22.08MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_092.smt2                  |    0.368s | 21.328MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_4_25.smt2                               |    0.370s | 23.592MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_274.smt2                  |    0.371s | 22.088MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_41.smt2                         |    0.371s | 24.112MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_037.smt2                  |    0.377s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_205.smt2                  |    0.380s | 21.328MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_152.smt2                  |    0.382s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_021.smt2                  |    0.382s | 22.136MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_65.smt2                         |    0.383s | 30.94MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_212.smt2                  |    0.388s | 21.836MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_31.smt2                         |    0.391s | 23.02MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_203.smt2                  |    0.392s | 21.784MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_020.smt2                  |    0.393s | 21.836MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_033.smt2                  |    0.396s | 22.1MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_269.smt2                  |    0.397s | 21.832MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_021.smt2                  |    0.399s | 21.828MiB| sat | 0 |  |
|scholl-smt08/RND/RND_6_24.smt2                               |    0.400s | 23.88MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_228.smt2                  |    0.405s | 21.58MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_207.smt2                  |    0.406s | 21.584MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_028.smt2                  |    0.406s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_078.smt2                  |    0.407s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_046.smt2                  |    0.407s | 21.828MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_041.smt2                  |    0.407s | 21.712MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_196.smt2                  |    0.410s | 21.828MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_250.smt2                  |    0.417s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_243.smt2                  |    0.420s | 21.312MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_143.smt2                  |    0.421s | 22.084MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_223.smt2                  |    0.421s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_126.smt2                  |    0.422s | 21.58MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_238.smt2                  |    0.427s | 21.576MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_163.smt2                  |    0.436s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_047.smt2                  |    0.436s | 21.844MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_017.smt2                  |    0.439s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_086.smt2                  |    0.448s | 22.076MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_132.smt2                  |    0.450s | 21.588MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_124.smt2                  |    0.452s | 21.832MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_210.smt2                  |    0.452s | 21.868MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_013.smt2                  |    0.461s | 21.596MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_210.smt2                  |    0.470s | 21.324MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_179.smt2                  |    0.473s | 21.576MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_119.smt2                  |    0.473s | 21.184MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_259.smt2                  |    0.475s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_147.smt2                  |    0.476s | 22.852MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_244.smt2                  |    0.476s | 21.332MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_186.smt2                  |    0.478s | 21.328MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_063.smt2                  |    0.479s | 22.364MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_075.smt2                  |    0.482s | 22.08MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_282.smt2                  |    0.488s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_042.smt2                  |    0.493s | 22.056MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_089.smt2                  |    0.494s | 22.1MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_080.smt2                  |    0.496s | 21.588MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_161.smt2                  |    0.497s | 21.58MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_254.smt2                  |    0.501s | 22.088MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_098.smt2                  |    0.501s | 22.344MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_257.smt2                  |    0.519s | 22.096MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_6_19.smt2                               |    0.522s | 22.348MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_018.smt2                  |    0.523s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_067.smt2                  |    0.533s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_196.smt2                  |    0.535s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_230.smt2                  |    0.536s | 21.996MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_298.smt2                  |    0.537s | 21.576MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_241.smt2                  |    0.537s | 22.1MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_017.smt2                  |    0.538s | 21.744MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_045.smt2                  |    0.540s | 21.58MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_093.smt2                  |    0.550s | 21.84MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_193.smt2                  |    0.554s | 21.068MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_162.smt2                  |    0.555s | 22.076MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_225.smt2                  |    0.559s | 22.1MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_028.smt2                  |    0.561s | 21.656MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_232.smt2                  |    0.565s | 21.576MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_098.smt2                  |    0.567s | 21.828MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_112.smt2                  |    0.568s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_212.smt2                  |    0.569s | 21.328MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_127.smt2                  |    0.571s | 21.832MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_63.smt2                         |    0.572s | 27.608MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_188.smt2                  |    0.587s | 22.088MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_082.smt2                  |    0.601s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_012.smt2                  |    0.605s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_072.smt2                  |    0.605s | 20.952MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_080.smt2                  |    0.606s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_119.smt2                  |    0.615s | 22.088MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_054.smt2                  |    0.624s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_087.smt2                  |    0.628s | 21.84MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_70.smt2                         |    0.629s | 61.364MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_252.smt2                  |    0.631s | 21.84MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_238.smt2                  |    0.633s | 21.976MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_057.smt2                  |    0.634s | 21.324MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_241.smt2                  |    0.636s | 22.088MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_257.smt2                  |    0.637s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_264.smt2                  |    0.644s | 22.088MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_151.smt2                  |    0.646s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_150.smt2                  |    0.646s | 22.008MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_143.smt2                  |    0.652s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_096.smt2                  |    0.656s | 21.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_044.smt2                  |    0.662s | 21.98MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_135.smt2                  |    0.663s | 21.916MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_238.smt2                  |    0.668s | 21.584MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_275.smt2                  |    0.668s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_062.smt2                  |    0.669s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_249.smt2                  |    0.670s | 21.58MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_282.smt2                  |    0.674s | 21.732MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_087.smt2                  |    0.683s | 21.58MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_152.smt2                  |    0.685s | 21.844MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_154.smt2                  |    0.691s | 21.708MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_225.smt2                  |    0.709s | 21.58MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_205.smt2                  |    0.711s | 22.328MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_212.smt2                  |    0.718s | 21.576MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_239.smt2                  |    0.733s | 22.34MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_057.smt2                  |    0.739s | 21.58MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_265.smt2                  |    0.743s | 22.088MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_223.smt2                  |    0.749s | 21.804MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_129.smt2                  |    0.750s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_043.smt2                  |    0.754s | 22.092MiB| sat | 0 |  |
|20230331-polyv/mit-projhh-miss1.smt2                         |    0.754s | 39.784MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_258.smt2                  |    0.776s | 21.592MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_132.smt2                  |    0.778s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_283.smt2                  |    0.790s | 21.828MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_269.smt2                  |    0.801s | 21.616MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_258.smt2                  |    0.803s | 21.584MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_227.smt2                  |    0.804s | 21.848MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_252.smt2                  |    0.813s | 21.876MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_294.smt2                  |    0.815s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_183.smt2                  |    0.818s | 21.58MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_025.smt2                  |    0.819s | 22.092MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_51.smt2                         |    0.824s | 30.22MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_172.smt2                  |    0.833s | 21.86MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_205.smt2                  |    0.835s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_093.smt2                  |    0.842s | 21.84MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_268.smt2                  |    0.844s | 22.088MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_181.smt2                  |    0.845s | 21.584MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_095.smt2                  |    0.845s | 21.9MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_184.smt2                  |    0.846s | 21.836MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_024.smt2                  |    0.849s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_199.smt2                  |    0.867s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_196.smt2                  |    0.875s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_022.smt2                  |    0.876s | 21.84MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_041.smt2                  |    0.877s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_070.smt2                  |    0.879s | 21.828MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_130.smt2                  |    0.897s | 22.208MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_295.smt2                  |    0.903s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_162.smt2                  |    0.911s | 21.836MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_207.smt2                  |    0.932s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_266.smt2                  |    0.934s | 22.112MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_012.smt2                  |    0.936s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_083.smt2                  |    0.940s | 22.344MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_072.smt2                  |    0.943s | 21.828MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_257.smt2                  |    0.951s | 22.336MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_254.smt2                  |    0.955s | 22.336MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_047.smt2                  |    0.972s | 21.576MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_154.smt2                  |    0.979s | 22.088MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_273.smt2                  |    0.983s | 21.008MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_270.smt2                  |    0.985s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_169.smt2                  |    0.990s | 21.672MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_090.smt2                  |    0.990s | 21.932MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_261.smt2                  |    0.992s | 22.32MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_056.smt2                  |    0.992s | 22.4MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_271.smt2                  |    0.996s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_177.smt2                  |    0.997s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_105.smt2                  |    0.997s | 21.76MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_281.smt2                  |    1.001s | 21.58MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_142.smt2                  |    1.003s | 22.084MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_206.smt2                  |    1.006s | 21.58MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_054.smt2                  |    1.011s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_195.smt2                  |    1.024s | 21.584MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_125.smt2                  |    1.024s | 22.1MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_223.smt2                  |    1.027s | 22.084MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_260.smt2                  |    1.046s | 22.088MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_283.smt2                  |    1.046s | 21.832MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_206.smt2                  |    1.048s | 21.588MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_230.smt2                  |    1.051s | 21.316MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_294.smt2                  |    1.056s | 21.828MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_038.smt2                  |    1.061s | 22.084MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_241.smt2                  |    1.072s | 22.088MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_067.smt2                  |    1.073s | 22.084MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_198.smt2                  |    1.074s | 22.084MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_060.smt2                  |    1.085s | 21.792MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_076.smt2                  |    1.095s | 21.832MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_249.smt2                  |    1.102s | 22.24MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_100.smt2                  |    1.106s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_143.smt2                  |    1.107s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_148.smt2                  |    1.108s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_091.smt2                  |    1.119s | 21.828MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_248.smt2                  |    1.142s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_056.smt2                  |    1.156s | 21.844MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_072.smt2                  |    1.167s | 21.58MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_56.smt2                         |    1.167s | 33.952MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_127.smt2                  |    1.178s | 22.344MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_269.smt2                  |    1.181s | 22.08MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_199.smt2                  |    1.182s | 22.192MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_063.smt2                  |    1.189s | 21.584MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_230.smt2                  |    1.190s | 22.088MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_124.smt2                  |    1.193s | 21.832MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_010.smt2                  |    1.195s | 22.096MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_218.smt2                  |    1.198s | 21.892MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_025.smt2                  |    1.201s | 21.84MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_4_29.smt2                               |    1.203s | 24.568MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_278.smt2                  |    1.223s | 22.088MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_280.smt2                  |    1.230s | 21.844MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_260.smt2                  |    1.233s | 21.832MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_156.smt2                  |    1.243s | 21.588MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_69.smt2                         |    1.258s | 35.724MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_136.smt2                  |    1.277s | 22.34MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_154.smt2                  |    1.287s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_004.smt2                  |    1.293s | 22.34MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_293.smt2                  |    1.310s | 21.58MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_106.smt2                  |    1.312s | 21.92MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_210.smt2                  |    1.313s | 21.972MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_195.smt2                  |    1.331s | 21.96MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_013.smt2                  |    1.334s | 21.836MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_045.smt2                  |    1.338s | 22.084MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_45.smt2                         |    1.339s | 25.356MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_294.smt2                  |    1.341s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_124.smt2                  |    1.343s | 21.832MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_289.smt2                  |    1.343s | 21.58MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_146.smt2                  |    1.352s | 21.58MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_017.smt2                  |    1.354s | 21.932MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_078.smt2                  |    1.368s | 22.86MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_268.smt2                  |    1.373s | 22.084MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_148.smt2                  |    1.373s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_244.smt2                  |    1.377s | 21.576MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_235.smt2                  |    1.378s | 21.588MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_248.smt2                  |    1.414s | 21.828MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_208.smt2                  |    1.421s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_100.smt2                  |    1.427s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_173.smt2                  |    1.430s | 22.764MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_113.smt2                  |    1.437s | 22.34MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_184.smt2                  |    1.447s | 22.084MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_174.smt2                  |    1.449s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_089.smt2                  |    1.469s | 22.336MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_227.smt2                  |    1.488s | 22.088MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_251.smt2                  |    1.491s | 22.088MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_144.smt2                  |    1.500s | 22.0MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_244.smt2                  |    1.502s | 22.088MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_074.smt2                  |    1.508s | 22.088MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_001.smt2                  |    1.530s | 22.312MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_211.smt2                  |    1.542s | 21.664MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_288.smt2                  |    1.553s | 22.592MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_135.smt2                  |    1.559s | 21.58MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_233.smt2                  |    1.584s | 21.856MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_270.smt2                  |    1.585s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_008.smt2                  |    1.591s | 22.108MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_188.smt2                  |    1.610s | 21.84MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir1/formula_194.smt2                  |    1.619s | 21.584MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_203.smt2                  |    1.630s | 22.108MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_146.smt2                  |    1.636s | 21.832MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_299.smt2                  |    1.674s | 21.652MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_190.smt2                  |    1.674s | 22.088MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_100.smt2                  |    1.681s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_044.smt2                  |    1.698s | 21.832MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_52.smt2                         |    1.708s | 30.824MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_138.smt2                  |    1.719s | 21.828MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_293.smt2                  |    1.723s | 21.844MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_059.smt2                  |    1.731s | 21.964MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_300.smt2                  |    1.745s | 22.348MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_125.smt2                  |    1.795s | 22.084MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_093.smt2                  |    1.796s | 21.484MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_023.smt2                  |    1.814s | 21.832MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_264.smt2                  |    1.815s | 22.084MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_080.smt2                  |    1.824s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_261.smt2                  |    1.825s | 21.716MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_047.smt2                  |    1.836s | 21.836MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_6_34.smt2                               |    1.838s | 31.96MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_203.smt2                  |    1.844s | 22.24MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_070.smt2                  |    1.872s | 22.34MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_191.smt2                  |    1.875s | 22.084MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_167.smt2                  |    1.876s | 22.092MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_190.smt2                  |    1.911s | 21.584MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_258.smt2                  |    1.918s | 21.844MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_088.smt2                  |    1.918s | 21.832MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_260.smt2                  |    1.924s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_152.smt2                  |    1.925s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_249.smt2                  |    1.949s | 21.708MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_065.smt2                  |    1.950s | 21.832MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_279.smt2                  |    1.967s | 22.344MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_292.smt2                  |    1.981s | 21.776MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_131.smt2                  |    2.014s | 21.828MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_108.smt2                  |    2.016s | 21.58MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_111.smt2                  |    2.019s | 21.824MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_070.smt2                  |    2.039s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_136.smt2                  |    2.079s | 22.088MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_259.smt2                  |    2.094s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_295.smt2                  |    2.100s | 22.34MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_229.smt2                  |    2.119s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_250.smt2                  |    2.127s | 22.264MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_185.smt2                  |    2.139s | 22.092MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_142.smt2                  |    2.153s | 22.34MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_151.smt2                  |    2.157s | 22.088MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_048.smt2                  |    2.161s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_109.smt2                  |    2.209s | 22.568MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_061.smt2                  |    2.220s | 22.092MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_158.smt2                  |    2.246s | 21.604MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_043.smt2                  |    2.259s | 22.088MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_134.smt2                  |    2.288s | 22.12MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_087.smt2                  |    2.295s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_267.smt2                  |    2.310s | 21.844MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_120.smt2                  |    2.317s | 21.576MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_111.smt2                  |    2.361s | 21.684MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_038.smt2                  |    2.380s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_111.smt2                  |    2.386s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_180.smt2                  |    2.389s | 22.06MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_245.smt2                  |    2.427s | 22.088MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_289.smt2                  |    2.433s | 22.084MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_248.smt2                  |    2.433s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_130.smt2                  |    2.443s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_129.smt2                  |    2.446s | 21.64MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_054.smt2                  |    2.459s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_261.smt2                  |    2.460s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_269.smt2                  |    2.477s | 22.084MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_169.smt2                  |    2.498s | 22.344MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_081.smt2                  |    2.528s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_194.smt2                  |    2.574s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_158.smt2                  |    2.576s | 21.584MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_111.smt2                  |    2.610s | 21.576MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_233.smt2                  |    2.612s | 21.84MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_128.smt2                  |    2.619s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_204.smt2                  |    2.620s | 21.58MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_125.smt2                  |    2.651s | 21.556MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_280.smt2                  |    2.677s | 21.58MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_211.smt2                  |    2.699s | 21.58MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_091.smt2                  |    2.708s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_174.smt2                  |    2.774s | 21.752MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_005.smt2                  |    2.775s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_077.smt2                  |    2.817s | 22.096MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_64.smt2                         |    2.826s | 40.268MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_248.smt2                  |    2.827s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_234.smt2                  |    2.846s | 21.736MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_101.smt2                  |    2.858s | 21.936MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_268.smt2                  |    2.883s | 22.092MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_171.smt2                  |    2.891s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_074.smt2                  |    2.902s | 22.096MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_120.smt2                  |    2.918s | 21.828MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_139.smt2                  |    2.954s | 22.1MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_108.smt2                  |    2.961s | 21.976MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_128.smt2                  |    2.987s | 21.844MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_208.smt2                  |    2.989s | 21.912MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_085.smt2                  |    3.005s | 22.084MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_271.smt2                  |    3.012s | 21.58MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_259.smt2                  |    3.027s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_105.smt2                  |    3.030s | 22.192MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_091.smt2                  |    3.063s | 21.58MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_062.smt2                  |    3.091s | 21.752MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_254.smt2                  |    3.106s | 22.088MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_229.smt2                  |    3.167s | 21.684MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_234.smt2                  |    3.219s | 22.092MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_052.smt2                  |    3.223s | 22.34MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_279.smt2                  |    3.239s | 22.088MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_088.smt2                  |    3.293s | 21.58MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_181.smt2                  |    3.293s | 21.584MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_034.smt2                  |    3.304s | 22.34MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_108.smt2                  |    3.377s | 22.344MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_6_36.smt2                               |    3.382s | 29.372MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_108.smt2                  |    3.384s | 22.228MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_081.smt2                  |    3.407s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_036.smt2                  |    3.417s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_227.smt2                  |    3.426s | 22.024MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_119.smt2                  |    3.429s | 21.78MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_252.smt2                  |    3.443s | 21.992MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_240.smt2                  |    3.452s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_096.smt2                  |    3.454s | 21.916MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_220.smt2                  |    3.457s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_139.smt2                  |    3.498s | 21.58MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_262.smt2                  |    3.503s | 22.084MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_118.smt2                  |    3.506s | 21.58MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_139.smt2                  |    3.534s | 21.576MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_250.smt2                  |    3.579s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_040.smt2                  |    3.603s | 22.088MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_022.smt2                  |    3.632s | 22.172MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_280.smt2                  |    3.669s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_047.smt2                  |    3.672s | 21.564MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_180.smt2                  |    3.707s | 22.096MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_253.smt2                  |    3.764s | 21.9MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_077.smt2                  |    3.772s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_052.smt2                  |    3.852s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_011.smt2                  |    3.852s | 21.7MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_267.smt2                  |    3.893s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_279.smt2                  |    3.896s | 22.1MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_029.smt2                  |    3.949s | 22.084MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_065.smt2                  |    3.950s | 22.152MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_120.smt2                  |    3.962s | 22.084MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_167.smt2                  |    3.966s | 21.868MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_046.smt2                  |    4.044s | 21.968MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_275.smt2                  |    4.084s | 21.58MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_282.smt2                  |    4.151s | 21.9MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_092.smt2                  |    4.216s | 21.58MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_172.smt2                  |    4.282s | 22.096MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_298.smt2                  |    4.327s | 22.356MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_191.smt2                  |    4.345s | 21.84MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_157.smt2                  |    4.350s | 22.088MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_253.smt2                  |    4.354s | 21.948MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_209.smt2                  |    4.406s | 22.088MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_230.smt2                  |    4.432s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_109.smt2                  |    4.489s | 21.996MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_159.smt2                  |    4.528s | 22.348MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_139.smt2                  |    4.530s | 22.088MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_024.smt2                  |    4.613s | 21.836MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_146.smt2                  |    4.641s | 22.084MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_075.smt2                  |    4.670s | 22.424MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_019.smt2                  |    4.712s | 21.84MiB| sat | 0 |  |
|scholl-smt08/RND/RND_6_29.smt2                               |    4.787s | 25.276MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_067.smt2                  |    4.809s | 21.832MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_113.smt2                  |    4.882s | 22.092MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_245.smt2                  |    4.891s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_044.smt2                  |    4.920s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_174.smt2                  |    4.980s | 22.132MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_224.smt2                  |    5.029s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_293.smt2                  |    5.101s | 22.092MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_190.smt2                  |    5.103s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_121.smt2                  |    5.139s | 21.844MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_194.smt2                  |    5.162s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_095.smt2                  |    5.179s | 21.992MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_019.smt2                  |    5.192s | 22.092MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_131.smt2                  |    5.215s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_197.smt2                  |    5.223s | 22.348MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_097.smt2                  |    5.337s | 22.952MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_257.smt2                  |    5.470s | 22.1MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_171.smt2                  |    5.504s | 21.844MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_211.smt2                  |    5.508s | 22.092MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_164.smt2                  |    5.508s | 22.096MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_224.smt2                  |    5.531s | 22.344MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_132.smt2                  |    5.558s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_229.smt2                  |    5.581s | 22.336MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_022.smt2                  |    5.584s | 21.844MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_030.smt2                  |    5.623s | 22.344MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_173.smt2                  |    5.682s | 22.86MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_165.smt2                  |    5.689s | 22.34MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_027.smt2                  |    5.754s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_009.smt2                  |    5.819s | 22.092MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_180.smt2                  |    5.883s | 21.732MiB| sat | 0 |  |
|20230331-polyv/cp6hv-miss1v.smt2                             |    6.102s | 29.776MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_153.smt2                  |    6.117s | 22.08MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_162.smt2                  |    6.148s | 22.356MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_040.smt2                  |    6.157s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_034.smt2                  |    6.162s | 21.704MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_151.smt2                  |    6.171s | 21.836MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_042.smt2                  |    6.177s | 21.064MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_281.smt2                  |    6.288s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_041.smt2                  |    6.377s | 22.084MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_135.smt2                  |    6.416s | 21.844MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_213.smt2                  |    6.441s | 22.828MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_198.smt2                  |    6.532s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_211.smt2                  |    6.561s | 21.844MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_259.smt2                  |    6.577s | 22.084MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_149.smt2                  |    6.600s | 22.088MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_092.smt2                  |    6.603s | 21.828MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_158.smt2                  |    6.743s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_173.smt2                  |    6.883s | 22.336MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_127.smt2                  |    6.891s | 22.084MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_084.smt2                  |    6.917s | 22.08MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_020.smt2                  |    6.938s | 21.836MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_050.smt2                  |    6.967s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_209.smt2                  |    7.049s | 21.836MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_159.smt2                  |    7.090s | 21.972MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_038.smt2                  |    7.148s | 21.98MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_253.smt2                  |    7.149s | 22.088MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_281.smt2                  |    7.190s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_199.smt2                  |    7.200s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_045.smt2                  |    7.264s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_159.smt2                  |    7.337s | 22.244MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_020.smt2                  |    7.342s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_115.smt2                  |    7.377s | 21.836MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_191.smt2                  |    7.381s | 22.6MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_240.smt2                  |    7.391s | 22.1MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_065.smt2                  |    7.472s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_283.smt2                  |    7.484s | 21.996MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_131.smt2                  |    7.561s | 22.096MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_182.smt2                  |    7.657s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_171.smt2                  |    7.753s | 22.348MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_181.smt2                  |    7.779s | 22.096MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_270.smt2                  |    7.892s | 22.096MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_096.smt2                  |    7.926s | 22.096MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_292.smt2                  |    7.986s | 22.348MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_138.smt2                  |    8.062s | 22.092MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_233.smt2                  |    8.156s | 21.84MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_271.smt2                  |    8.246s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_182.smt2                  |    8.285s | 22.344MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_6_28.smt2                               |    8.459s | 26.964MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_076.smt2                  |    8.501s | 21.836MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_034.smt2                  |    8.543s | 21.844MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_063.smt2                  |    8.548s | 22.86MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_112.smt2                  |    8.614s | 22.348MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_106.smt2                  |    8.628s | 21.832MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_259.smt2                  |    8.768s | 21.996MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_028.smt2                  |    8.813s | 21.78MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_015.smt2                  |    8.865s | 21.856MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_001.smt2                  |    8.878s | 22.856MiB| unsat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_66.smt2                         |    9.382s | 63.996MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_262.smt2                  |    9.492s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_024.smt2                  |    9.515s | 21.832MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_294.smt2                  |    9.610s | 21.972MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_246.smt2                  |    9.615s | 21.832MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_128.smt2                  |    9.669s | 21.84MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_6_25.smt2                               |    9.673s | 26.036MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_164.smt2                  |    9.760s | 22.092MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_048.smt2                  |    9.769s | 22.084MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_020.smt2                  |    9.970s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_027.smt2                  |    9.992s | 21.932MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_187.smt2                  |   10.163s | 21.976MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_200.smt2                  |   10.873s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_121.smt2                  |   11.234s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_107.smt2                  |   11.293s | 22.096MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_253.smt2                  |   11.393s | 22.348MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_059.smt2                  |   11.490s | 22.14MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_6_39.smt2                               |   11.722s | 45.424MiB| sat | 0 |  |
|scholl-smt08/RND/RND_6_31.smt2                               |   11.748s | 35.356MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_229.smt2                  |   11.901s | 22.084MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_172.smt2                  |   11.997s | 22.556MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_240.smt2                  |   12.323s | 22.088MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_157.smt2                  |   12.625s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_129.smt2                  |   12.642s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_208.smt2                  |   12.953s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_048.smt2                  |   13.327s | 21.832MiB| unsat | 0 |  |
|scholl-smt08/RND/RND_6_38.smt2                               |   13.405s | 31.204MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_288.smt2                  |   13.420s | 22.088MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_251.smt2                  |   14.933s | 22.348MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_067.smt2                  |   14.983s | 22.092MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_300.smt2                  |   15.007s | 22.596MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_019.smt2                  |   15.023s | 22.096MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_262.smt2                  |   15.082s | 22.6MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_018.smt2                  |   15.333s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_084.smt2                  |   15.428s | 22.092MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_177.smt2                  |   15.507s | 22.348MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_082.smt2                  |   15.738s | 22.048MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_024.smt2                  |   16.141s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_025.smt2                  |   16.178s | 22.084MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_096.smt2                  |   16.275s | 22.124MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_184.smt2                  |   16.419s | 22.86MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_061.smt2                  |   16.674s | 22.476MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_204.smt2                  |   16.692s | 23.152MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_256.smt2                  |   16.865s | 21.832MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_105.smt2                  |   17.172s | 21.976MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_089.smt2                  |   17.483s | 22.084MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir4/formula_200.smt2                  |   17.493s | 22.224MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_288.smt2                  |   17.678s | 22.864MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_138.smt2                  |   17.909s | 22.096MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_182.smt2                  |   17.984s | 23.012MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_291.smt2                  |   18.223s | 22.024MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_062.smt2                  |   18.861s | 22.092MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_209.smt2                  |   18.937s | 22.088MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_061.smt2                  |   19.080s | 22.092MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_277.smt2                  |   19.536s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_187.smt2                  |   20.281s | 22.628MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_270.smt2                  |   20.347s | 22.62MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_112.smt2                  |   20.393s | 22.036MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_118.smt2                  |   20.482s | 22.34MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_266.smt2                  |   20.816s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_204.smt2                  |   21.050s | 22.344MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_010.smt2                  |   22.096s | 22.008MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_196.smt2                  |   22.098s | 22.104MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_081.smt2                  |   23.247s | 22.64MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_198.smt2                  |   23.416s | 23.016MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir3/formula_222.smt2                  |   23.737s | 22.352MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_187.smt2                  |   23.816s | 22.096MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_222.smt2                  |   24.343s | 22.096MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_251.smt2                  |   24.820s | 22.092MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_165.smt2                  |   24.910s | 22.352MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_174.smt2                  |   25.473s | 22.348MiB| sat | 0 |  |
|scholl-smt08/RNDPRE/RNDPRE_4_61.smt2                         |   25.747s | 76.292MiB| sat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_112.smt2                  |   26.206s | 22.352MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_121.smt2                  |   26.310s | 21.836MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir2/formula_029.smt2                  |   26.452s | 23.244MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_191.smt2                  |   26.466s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_278.smt2                  |   27.426s | 22.096MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_019.smt2                  |   28.811s | 22.252MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_136.smt2                  |   29.166s | 22.344MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir5/formula_188.smt2                  |   29.872s | 21.84MiB| unsat | 0 |  |
|2010-Monniaux-QE/mjollnir6/formula_207.smt2                  |   30.003s | 21.984MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_126.smt2                  |   30.003s | 21.572MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_142.smt2                  |   30.003s | 22.096MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_067.smt2                  |   30.003s | 21.836MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_258.smt2                  |   30.003s | 21.836MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_282.smt2                  |   30.003s | 21.072MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_030.smt2                  |   30.003s | 21.568MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_273.smt2                  |   30.003s | 21.32MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_205.smt2                  |   30.003s | 21.832MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_042.smt2                  |   30.003s | 21.964MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_160.smt2                  |   30.003s | 21.568MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_102.smt2                  |   30.003s | 21.604MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_096.smt2                  |   30.003s | 21.84MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_013.smt2                  |   30.003s | 22.348MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_185.smt2                  |   30.003s | 22.22MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_225.smt2                  |   30.003s | 21.568MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_235.smt2                  |   30.003s | 21.58MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_123.smt2                  |   30.003s | 21.568MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_031.smt2                  |   30.003s | 21.832MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_158.smt2                  |   30.003s | 21.58MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_186.smt2                  |   30.003s | 21.58MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_022.smt2                  |   30.003s | 21.584MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_091.smt2                  |   30.003s | 21.58MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_066.smt2                  |   30.003s | 21.332MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_138.smt2                  |   30.003s | 21.828MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_169.smt2                  |   30.003s | 22.096MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_054.smt2                  |   30.003s | 21.744MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_010.smt2                  |   30.003s | 21.824MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_112.smt2                  |   30.003s | 21.828MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_070.smt2                  |   30.003s | 21.736MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_165.smt2                  |   30.003s | 22.084MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_120.smt2                  |   30.003s | 21.832MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_252.smt2                  |   30.003s | 21.58MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_263.smt2                  |   30.003s | 21.72MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_261.smt2                  |   30.003s | 21.576MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_150.smt2                  |   30.003s | 21.32MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_108.smt2                  |   30.003s | 21.312MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_078.smt2                  |   30.003s | 21.712MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_199.smt2                  |   30.003s | 21.832MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_137.smt2                  |   30.003s | 21.632MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_100.smt2                  |   30.003s | 21.552MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_274.smt2                  |   30.003s | 21.964MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_251.smt2                  |   30.003s | 22.092MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_157.smt2                  |   30.003s | 21.584MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_065.smt2                  |   30.003s | 21.588MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_043.smt2                  |   30.003s | 21.58MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_122.smt2                  |   30.003s | 21.84MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_171.smt2                  |   30.003s | 22.096MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_153.smt2                  |   30.003s | 21.552MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_025.smt2                  |   30.003s | 22.088MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_076.smt2                  |   30.003s | 21.832MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_257.smt2                  |   30.003s | 21.472MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_119.smt2                  |   30.003s | 21.968MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_164.smt2                  |   30.003s | 21.46MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_210.smt2                  |   30.003s | 22.08MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_262.smt2                  |   30.003s | 21.824MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_118.smt2                  |   30.003s | 21.572MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_216.smt2                  |   30.003s | 22.084MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_241.smt2                  |   30.003s | 21.58MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_110.smt2                  |   30.003s | 21.832MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_209.smt2                  |   30.003s | 21.728MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_183.smt2                  |   30.003s | 21.712MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_124.smt2                  |   30.003s | 22.084MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_224.smt2                  |   30.003s | 21.836MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_156.smt2                  |   30.003s | 22.088MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_103.smt2                  |   30.003s | 21.488MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_173.smt2                  |   30.003s | 22.084MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_044.smt2                  |   30.003s | 21.824MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_159.smt2                  |   30.003s | 21.58MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_028.smt2                  |   30.003s | 21.58MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_104.smt2                  |   30.003s | 21.58MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_239.smt2                  |   30.003s | 22.348MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_182.smt2                  |   30.003s | 21.94MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_080.smt2                  |   30.003s | 21.832MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_201.smt2                  |   30.003s | 21.828MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_135.smt2                  |   30.003s | 22.348MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_117.smt2                  |   30.003s | 21.556MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_145.smt2                  |   30.003s | 21.064MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_094.smt2                  |   30.003s | 21.712MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_057.smt2                  |   30.003s | 21.836MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_105.smt2                  |   30.003s | 21.552MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_130.smt2                  |   30.003s | 21.584MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_107.smt2                  |   30.003s | 22.244MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_133.smt2                  |   30.003s | 21.336MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_245.smt2                  |   30.003s | 21.84MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_299.smt2                  |   30.003s | 21.58MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_238.smt2                  |   30.003s | 21.828MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_176.smt2                  |   30.003s | 21.572MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_244.smt2                  |   30.003s | 21.324MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_041.smt2                  |   30.003s | 21.66MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_081.smt2                  |   30.003s | 21.832MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_152.smt2                  |   30.003s | 21.588MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_109.smt2                  |   30.003s | 21.328MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_246.smt2                  |   30.003s | 22.04MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_226.smt2                  |   30.003s | 21.58MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_270.smt2                  |   30.003s | 21.308MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_220.smt2                  |   30.003s | 22.06MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_136.smt2                  |   30.003s | 23.112MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_288.smt2                  |   30.003s | 21.748MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_276.smt2                  |   30.003s | 21.064MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_234.smt2                  |   30.003s | 22.084MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_127.smt2                  |   30.003s | 21.584MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_056.smt2                  |   30.003s | 21.572MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_121.smt2                  |   30.003s | 21.832MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_294.smt2                  |   30.003s | 22.084MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_230.smt2                  |   30.003s | 21.828MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_106.smt2                  |   30.003s | 21.84MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_190.smt2                  |   30.003s | 21.496MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_236.smt2                  |   30.003s | 21.828MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_272.smt2                  |   30.003s | 21.58MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_154.smt2                  |   30.003s | 21.32MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_020.smt2                  |   30.003s | 21.584MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_213.smt2                  |   30.003s | 22.088MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_163.smt2                  |   30.003s | 22.86MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_026.smt2                  |   30.003s | 21.572MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_151.smt2                  |   30.003s | 21.724MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_292.smt2                  |   30.003s | 21.844MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_184.smt2                  |   30.003s | 22.104MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_174.smt2                  |   30.003s | 22.6MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_040.smt2                  |   30.003s | 21.588MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_194.smt2                  |   30.003s | 21.324MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_279.smt2                  |   30.003s | 21.576MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_148.smt2                  |   30.003s | 21.496MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_229.smt2                  |   30.003s | 21.592MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_208.smt2                  |   30.003s | 23.112MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_217.smt2                  |   30.003s | 21.58MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_074.smt2                  |   30.003s | 21.828MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_293.smt2                  |   30.003s | 21.072MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_266.smt2                  |   30.003s | 21.584MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_059.smt2                  |   30.003s | 22.088MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_298.smt2                  |   30.003s | 21.828MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_029.smt2                  |   30.003s | 21.828MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_003.smt2                  |   30.003s | 21.576MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_097.smt2                  |   30.003s | 21.568MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_132.smt2                  |   30.003s | 22.16MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_172.smt2                  |   30.003s | 21.732MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_047.smt2                  |   30.003s | 21.324MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_016.smt2                  |   30.003s | 21.976MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_195.smt2                  |   30.003s | 21.828MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_146.smt2                  |   30.003s | 21.596MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_099.smt2                  |   30.003s | 21.328MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_048.smt2                  |   30.003s | 21.584MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_228.smt2                  |   30.003s | 21.32MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_082.smt2                  |   30.003s | 22.064MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_249.smt2                  |   30.003s | 21.832MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_285.smt2                  |   30.003s | 21.576MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_192.smt2                  |   30.003s | 21.576MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_227.smt2                  |   30.003s | 22.092MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_268.smt2                  |   30.003s | 21.32MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_061.smt2                  |   30.003s | 21.416MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_090.smt2                  |   30.003s | 21.34MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_166.smt2                  |   30.003s | 21.832MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_036.smt2                  |   30.003s | 21.84MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_162.smt2                  |   30.003s | 21.828MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_129.smt2                  |   30.003s | 21.58MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_062.smt2                  |   30.003s | 21.584MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_075.smt2                  |   30.003s | 21.836MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_240.smt2                  |   30.003s | 21.832MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_114.smt2                  |   30.003s | 21.324MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_069.smt2                  |   30.003s | 22.344MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_035.smt2                  |   30.003s | 21.936MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_021.smt2                  |   30.003s | 21.668MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_231.smt2                  |   30.003s | 21.96MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_018.smt2                  |   30.003s | 22.084MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_072.smt2                  |   30.003s | 21.588MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_084.smt2                  |   30.003s | 21.576MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_023.smt2                  |   30.003s | 21.976MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_093.smt2                  |   30.003s | 21.84MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_027.smt2                  |   30.003s | 21.828MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_116.smt2                  |   30.003s | 21.312MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_113.smt2                  |   30.003s | 21.652MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_212.smt2                  |   30.003s | 22.1MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_271.smt2                  |   30.003s | 21.832MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_083.smt2                  |   30.003s | 21.356MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_098.smt2                  |   30.003s | 21.568MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_037.smt2                  |   30.003s | 21.932MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_087.smt2                  |   30.003s | 21.328MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_134.smt2                  |   30.003s | 21.32MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_144.smt2                  |   30.003s | 21.464MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_203.smt2                  |   30.003s | 22.076MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_242.smt2                  |   30.003s | 21.828MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_265.smt2                  |   30.003s | 21.828MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_004.smt2                  |   30.003s | 21.332MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_032.smt2                  |   30.003s | 21.648MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_267.smt2                  |   30.003s | 21.964MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_180.smt2                  |   30.003s | 21.328MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_189.smt2                  |   30.003s | 21.84MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_289.smt2                  |   30.003s | 21.984MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_221.smt2                  |   30.003s | 21.976MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_187.smt2                  |   30.003s | 21.576MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_250.smt2                  |   30.003s | 21.312MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_095.smt2                  |   30.003s | 21.844MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_034.smt2                  |   30.003s | 21.572MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_088.smt2                  |   30.003s | 21.58MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_046.smt2                  |   30.003s | 21.576MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_128.smt2                  |   30.003s | 21.872MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_179.smt2                  |   30.003s | 22.088MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_175.smt2                  |   30.003s | 21.064MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_131.smt2                  |   30.003s | 21.58MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_052.smt2                  |   30.003s | 21.828MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_260.smt2                  |   30.003s | 22.032MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_125.smt2                  |   30.003s | 22.336MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_063.smt2                  |   30.003s | 21.832MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_233.smt2                  |   30.003s | 21.836MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_086.smt2                  |   30.003s | 21.828MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_280.smt2                  |   30.003s | 22.064MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_204.smt2                  |   30.003s | 21.584MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_015.smt2                  |   30.003s | 22.22MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_012.smt2                  |   30.003s | 21.576MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_275.smt2                  |   30.003s | 21.584MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_167.smt2                  |   30.003s | 21.58MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_049.smt2                  |   30.003s | 21.84MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_253.smt2                  |   30.003s | 22.092MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_050.smt2                  |   30.003s | 21.32MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_248.smt2                  |   30.003s | 21.836MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_281.smt2                  |   30.003s | 21.58MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_143.smt2                  |   30.003s | 21.736MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_223.smt2                  |   30.003s | 21.46MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_001.smt2                  |   30.003s | 21.576MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_017.smt2                  |   30.003s | 21.828MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_039.smt2                  |   30.003s | 22.344MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_009.smt2                  |   30.003s | 21.824MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_290.smt2                  |   30.003s | 21.584MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_188.smt2                  |   30.003s | 21.876MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_300.smt2                  |   30.003s | 21.84MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_283.smt2                  |   30.003s | 21.904MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_198.smt2                  |   30.003s | 21.828MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_222.smt2                  |   30.003s | 21.836MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_295.smt2                  |   30.003s | 21.576MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_264.smt2                  |   30.003s | 21.848MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_197.smt2                  |   30.003s | 21.84MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_149.smt2                  |   30.003s | 21.832MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir2/formula_031.smt2                  |   30.003s | 23.116MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir2/formula_010.smt2                  |   30.003s | 23.804MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir2/formula_171.smt2                  |   30.003s | 23.116MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir2/formula_239.smt2                  |   30.003s | 23.456MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir2/formula_182.smt2                  |   30.003s | 23.12MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir2/formula_201.smt2                  |   30.003s | 25.936MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir2/formula_040.smt2                  |   30.003s | 22.864MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir2/formula_074.smt2                  |   30.003s | 23.568MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir2/formula_266.smt2                  |   30.003s | 22.604MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir2/formula_059.smt2                  |   30.003s | 23.092MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir2/formula_048.smt2                  |   30.003s | 23.116MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir2/formula_038.smt2                  |   30.003s | 24.708MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir2/formula_166.smt2                  |   30.003s | 24.096MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir2/formula_036.smt2                  |   30.003s | 22.864MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir2/formula_084.smt2                  |   30.003s | 22.64MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir2/formula_113.smt2                  |   30.003s | 23.66MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir2/formula_265.smt2                  |   30.003s | 23.196MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir2/formula_267.smt2                  |   30.003s | 22.864MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir2/formula_187.smt2                  |   30.003s | 22.864MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir2/formula_050.smt2                  |   30.003s | 23.412MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir2/formula_009.smt2                  |   30.003s | 23.752MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir2/formula_264.smt2                  |   30.003s | 22.82MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir2/formula_197.smt2                  |   30.003s | 22.864MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_211.smt2                  |   30.003s | 22.604MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_013.smt2                  |   30.003s | 22.092MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_169.smt2                  |   30.003s | 23.128MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_199.smt2                  |   30.003s | 22.232MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_157.smt2                  |   30.003s | 22.092MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_043.smt2                  |   30.003s | 22.096MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_153.smt2                  |   30.003s | 22.348MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_119.smt2                  |   30.003s | 22.868MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_164.smt2                  |   30.003s | 22.352MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_118.smt2                  |   30.003s | 22.332MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_183.smt2                  |   30.003s | 22.352MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_224.smt2                  |   30.003s | 22.096MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_130.smt2                  |   30.003s | 22.344MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_109.smt2                  |   30.003s | 21.076MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_246.smt2                  |   30.003s | 22.348MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_234.smt2                  |   30.003s | 21.956MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_056.smt2                  |   30.003s | 22.096MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_020.smt2                  |   30.003s | 22.184MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_292.smt2                  |   30.003s | 22.86MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_194.smt2                  |   30.003s | 22.356MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_029.smt2                  |   30.003s | 22.128MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_172.smt2                  |   30.003s | 22.344MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_195.smt2                  |   30.003s | 23.1MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_082.smt2                  |   30.003s | 22.504MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_036.smt2                  |   30.003s | 22.348MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_134.smt2                  |   30.003s | 22.396MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_250.smt2                  |   30.003s | 22.348MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_095.smt2                  |   30.003s | 22.504MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_052.smt2                  |   30.003s | 22.196MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_167.smt2                  |   30.003s | 22.092MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_001.smt2                  |   30.003s | 22.348MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_009.smt2                  |   30.003s | 22.4MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_197.smt2                  |   30.003s | 22.344MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir3/formula_153.smt2                  |   30.003s | 23.292MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir3/formula_183.smt2                  |   30.003s | 23.332MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir3/formula_246.smt2                  |   30.003s | 23.264MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir3/formula_106.smt2                  |   30.003s | 23.2MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir3/formula_208.smt2                  |   30.003s | 23.148MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir3/formula_023.smt2                  |   30.003s | 23.5MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir3/formula_134.smt2                  |   30.003s | 22.348MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir3/formula_034.smt2                  |   30.003s | 22.86MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir3/formula_050.smt2                  |   30.003s | 23.012MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir1/formula_012.smt2                  |   30.003s | 25.248MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_278.smt2                  |   30.004s | 21.58MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_206.smt2                  |   30.004s | 21.58MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_077.smt2                  |   30.004s | 21.32MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir6/formula_033.smt2                  |   30.004s | 21.328MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir2/formula_165.smt2                  |   30.004s | 22.688MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_206.smt2                  |   30.004s | 22.864MiB| timeout | 143 | Ramon wall-clock time limit reached |
|2010-Monniaux-QE/mjollnir5/formula_239.smt2                  |   30.004s | 22.352MiB| timeout | 143 | Ramon wall-clock time limit reached |
|scholl-smt08/RND/RND_6_32.smt2                               |   30.004s | 34.784MiB| timeout | 143 | Ramon wall-clock time limit reached |
|20230331-polyv/cp5hv.smt2                                    |   30.004s | 32.448MiB| timeout | 143 | Ramon wall-clock time limit reached |
|20230331-polyv/cp6hv-miss1h.smt2                             |   30.004s | 37.5MiB| timeout | 143 | Ramon wall-clock time limit reached |
|scholl-smt08/RND/RND_6_35.smt2                               |   30.005s | 37.908MiB| timeout | 143 | Ramon wall-clock time limit reached |
|20230331-polyv/h5projh3-h4projh3.smt2                        |   30.005s | 55.5MiB| timeout | 143 | Ramon wall-clock time limit reached |
|20230331-polyv/cp6hv.smt2                                    |   30.005s | 42.396MiB| timeout | 143 | Ramon wall-clock time limit reached |
|20230331-polyv/mit71-checkpred1.smt2                         |   30.005s | 39.888MiB| timeout | 143 | Ramon wall-clock time limit reached |
|20230331-polyv/mit-projhh.smt2                               |   30.006s | 61.788MiB| timeout | 143 | Ramon wall-clock time limit reached |
|20230331-polyv/mit-checkpred1.smt2                           |   30.007s | 73.212MiB| timeout | 143 | Ramon wall-clock time limit reached |
|20230331-polyv/mit-checkpred726.smt2                         |   30.007s | 90.348MiB| timeout | 143 | Ramon wall-clock time limit reached |
|20230331-polyv/mit-projhv-miss1.smt2                         |   30.009s | 131.0MiB| timeout | 143 | Ramon wall-clock time limit reached |
|20230331-polyv/h5hv.smt2                                     |   30.010s | 146.0MiB| timeout | 143 | Ramon wall-clock time limit reached |
|20230331-polyv/mit-projhv.smt2                               |   30.011s | 143.0MiB| timeout | 143 | Ramon wall-clock time limit reached |
|20230331-polyv/h5projh-h4h.smt2                              |   30.011s | 157.0MiB| timeout | 143 | Ramon wall-clock time limit reached |
|20230331-polyv/h5projh-h4v.smt2                              |   30.020s | 289.0MiB| timeout | 143 | Ramon wall-clock time limit reached |
|20230331-polyv/bv7hv-miss1v.smt2                             |   30.027s | 519.0MiB| timeout | 143 | Ramon wall-clock time limit reached |
|20230331-polyv/bv7hv.smt2                                    |   30.028s | 584.0MiB| timeout | 143 | Ramon wall-clock time limit reached |
|20230331-polyv/bv7hv-miss1h.smt2                             |   30.029s | 540.0MiB| timeout | 143 | Ramon wall-clock time limit reached |
|20230331-polyv/mithv.smt2                                    |   30.054s | 1187.0MiB| timeout | 143 | Ramon wall-clock time limit reached |
|20230331-polyv/mithv-miss1v.smt2                             |   30.061s | 1239.0MiB| timeout | 143 | Ramon wall-clock time limit reached |
