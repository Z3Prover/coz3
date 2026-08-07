# .

* SAT 0
* UNSAT 241
* TIMEOUT 8
* UNKNOWN 1

* ERRORS 0

* BENIGN 241 (model query without model)

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: fstar-ulib-smt2 with lprobe false
Job tag: lprobe_1_false_fstar-ulib-smt2
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 36738a03948decd13351d874dc573e17965c31ee
Z3 branch: linprobe
Z3 options: "-T:60 smt.arith.nl.linprobe=false"
Z3 inputs: inputs/fstar-ulib-smt2
Z3 commit message: Fix linprobe perf regression and refactor the probe into a reusable combinator

Perf: remove combined_solver::try_linprobe

It ran the *non-incremental* solver1 only when m_inc_mode or assumptions were
present, i.e. exactly the modes where combined_solver mandates solver2, so
solver1 re-preprocessed the whole assertion stack on every check-sat. Its
wall-clock scoped_timer did not bound that work (linprobe_timeout=1 was slower
than 100), it consumed the caller's rlimit and so changed verdicts, and it made
rlimit-based runs non-deterministic: the same binary on
queries-FStar.UInt128.smt2 returned different unsat counts depending on whether
stdout was redirected or piped.

On the F* ulib queries this is a 3.99x aggregate speedup (224.35s -> 56.29s over
10 files; UInt128 80.2s -> 20.7s, BV 19.9s -> 1.7s) with verdicts identical to
master. The feature itself is unaffected: it lives in the smt tactic, which is
what arith.nl.linprobe documents, and solver1 reaches it via mk_smt_tactic.

Params: declare arith.nl.linprobe_mode and arith.nl.linprobe_timeout

arith.nl.linprobe_mode was read by raw string lookup in four places but never
declared, so it was invisible to -pm and rejected by set-option. The 100ms
timeout was hard-coded twice, in two different libraries.

Refactor

- Move the generic part of linprobe_tactic to tactical.{h,cpp} beside or_else as
  or_else_no_user_propagate(); the class was an or_else reimplementation whose
  only new behaviour was bypassing t1 once user propagators are registered.
  smt_tactic.cpp shrinks from 231 to 100 lines.
- unary_tactical: forward the ten missing user_propagate_* methods so wrappers
  such as using_params do not drop propagator support.
- nla_core: drop the cached m_linprobe flag and use params().arith_nl_linprobe_mode()
  through a new core::linprobe_mode(), matching how every other nla parameter is read.
- theory_lra: replace a per-final-check string parameter lookup with
  m_nla->linprobe_mode().
- mk_smt_tactic_using: restore mk_smt_tactic_core_using as the fallback so
  parallel.enable keeps selecting mk_parallel_smt_tactic.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | INFO |
|------------|----------:|---------:|-------------:| ----------:|------|
|queries-Prims.smt2                                           |    0.112s | 23.436MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Endianness-1.smt2                            |    0.200s | 26.028MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Common.smt2                            |    0.474s | 49.68MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.VConfig.smt2                                   |    0.493s | 49.656MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Effect-1.smt2                          |    0.519s | 75.404MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Heap.smt2                                      |    0.551s | 75.052MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Classical-1.smt2                               |    0.610s | 74.624MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pervasives.Native.smt2                         |    0.633s | 46.284MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Squash.smt2                                    |    0.662s | 49.66MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.TSet-1.smt2                                    |    0.700s | 74.78MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.Pure.smt2                            |    0.702s | 50.172MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Order.smt2                                     |    0.713s | 50.46MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.PropositionalExtensionality.smt2               |    0.715s | 50.428MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Universe.smt2                                  |    0.769s | 49.788MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Sequence.Ambient.smt2                          |    0.773s | 74.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.OrdSetProps.smt2                               |    0.798s | 74.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Types.smt2                          |    0.810s | 74.788MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Classical.smt2                                 |    0.813s | 74.524MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.All.smt2                                       |    0.824s | 75.256MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.OrdMapProps.smt2                               |    0.824s | 74.852MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Algebra.CommMonoid.smt2                        |    0.842s | 50.932MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.IFC.smt2                                       |    0.843s | 74.752MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.TwoLevelHeap.smt2                              |    0.850s | 75.264MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Option.smt2                                    |    0.873s | 75.652MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Effect-2.smt2                          |    0.874s | 75.264MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.PredicateExtensionality.smt2                   |    0.883s | 74.796MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Relational.Relational.smt2                     |    0.907s | 75.112MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.FractionalPermission.smt2                      |    0.939s | 74.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Result.smt2                            |    0.957s | 75.016MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Constructive.smt2                              |    0.983s | 50.664MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Universe.PCM.smt2                              |    0.989s | 75.232MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Ref.smt2                                       |    0.994s | 75.26MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.SquashProperties.smt2                          |    1.004s | 74.856MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.IndefiniteDescription.smt2                     |    1.066s | 74.852MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Ghost.smt2                                     |    1.070s | 50.684MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.List.Pure.Base.smt2                            |    1.084s | 75.136MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.WellFounded.smt2                               |    1.106s | 75.008MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Algebra.CommMonoid.Equiv.smt2                  |    1.125s | 74.624MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.TSet-2.smt2                                    |    1.132s | 74.792MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.NMST.smt2                                      |    1.163s | 75.032MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Util.smt2                                      |    1.185s | 80.16MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.MSTTotal.smt2                                  |    1.189s | 74.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.MRef.smt2                                      |    1.200s | 75.304MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.PartialMap.smt2                                |    1.205s | 74.784MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.GSet.smt2                                      |    1.205s | 74.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Comment.smt2                                 |    1.212s | 81.72MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Char.smt2                                      |    1.215s | 79.384MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Calc.smt2                                      |    1.225s | 74.78MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Sequence.Util.smt2                             |    1.240s | 74.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ST.smt2                                        |    1.285s | 75.252MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.NMSTTotal.smt2                                 |    1.302s | 74.68MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.TSet.smt2                                      |    1.312s | 74.74MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.SyntaxHelpers.smt2                     |    1.333s | 87.62MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Matrix2.smt2                                   |    1.348s | 76.996MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int.Cast.Full.smt2                             |    1.353s | 92.84MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.MonotonicCounter.smt2                          |    1.354s | 77.132MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.MST.smt2                                       |    1.366s | 74.78MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Set.smt2                                       |    1.373s | 74.744MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Permutation-2.smt2                         |    1.377s | 80.92MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Real.smt2                                      |    1.434s | 50.62MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.IntegerIntervals.smt2                          |    1.449s | 76.34MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.Witnessed.smt2                       |    1.514s | 74.752MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Util.smt2                              |    1.515s | 76.688MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Effect.smt2                            |    1.547s | 75.148MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Error.smt2                                     |    1.558s | 87.996MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Classical.Sugar.smt2                           |    1.566s | 74.68MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonCommSwaps.smt2                    |    1.586s | 75.832MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Fin.smt2                                       |    1.600s | 77.584MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.String.smt2                                    |    1.603s | 84.368MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Map.smt2                                       |    1.606s | 74.744MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.BufferOps.smt2                               |    1.678s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Sequence.Seq.smt2                              |    1.711s | 79.892MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Regional.smt2                                |    1.772s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Print.smt2                             |    1.783s | 90.02MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Derived.Lemmas.smt2                 |    1.796s | 85.052MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.List.smt2                                      |    1.797s | 76.792MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.FunctionalExtensionality.smt2                  |    1.831s | 81.936MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.FiniteSet.smt2                                 |    1.831s | 76.78MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Typeclasses.smt2                       |    1.835s | 94.74MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Classical-2.smt2                               |    1.865s | 74.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Vector.Properties.smt2                         |    1.888s | 82.452MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.DependentMap.smt2                              |    1.954s | 74.796MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.GhostReference.smt2                         |    1.983s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Algebra.CommMonoid.Fold.Nested.smt2            |    1.987s | 96.584MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.Map.smt2                             |    2.011s | 84.976MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.PCM.smt2                                       |    2.033s | 74.756MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.BufferCompat.smt2                            |    2.047s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Crypto.smt2                                    |    2.106s | 138.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Simplifier.smt2                        |    2.116s | 92.88MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.LexicographicOrdering.smt2                     |    2.137s | 75.3MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.MonotonicReference.smt2                     |    2.173s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.PCMMap.smt2                                    |    2.197s | 75.804MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Effect.M.smt2                                  |    2.229s | 139.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.InteractiveHelpers.Output.smt2                 |    2.242s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.LockCoupling.smt2                              |    2.291s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.CancellableSpinLock.smt2                    |    2.299s | 151.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Loops.Util.smt2                             |    2.305s | 152.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tcp.smt2                                       |    2.317s | 138.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Sorted.smt2                                |    2.353s | 76.992MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Primitive.ForkJoin.smt2                        |    2.380s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.OrdMap.smt2                                    |    2.386s | 74.836MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Endianness-2.smt2                            |    2.390s | 138.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.WellFounded.Util.smt2                          |    2.403s | 94.744MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pervasives.smt2                                |    2.471s | 50.832MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.WellFoundedRelation.smt2                       |    2.520s | 76.08MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Closure.smt2                                   |    2.550s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Derived.smt2                        |    2.565s | 85.16MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Channel.Protocol.smt2                          |    2.589s | 74.74MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-6.smt2                             |    2.608s | 96.788MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Permutation.smt2                           |    2.616s | 79.504MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Buffer.smt2                                  |    2.683s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.DependentMap.smt2                    |    2.718s | 87.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Channel.Duplex.smt2                            |    2.762s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Equiv.smt2                                 |    2.784s | 82.76MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Logic.smt2                             |    2.805s | 93.204MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pointer.Derived2.smt2                          |    2.909s | 140.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.GhostPCMReference.smt2                         |    2.919s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Math.Lib.smt2                                  |    2.925s | 53.692MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Semantics.Instantiate.smt2                     |    2.926s | 143.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.MonotonicReference.smt2                        |    2.971s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Loops.smt2                                  |    2.978s | 155.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonMonoid.smt2                       |    3.065s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Util.smt2                                   |    3.099s | 155.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Algebra.CommMonoid.Fold.smt2                   |    3.107s | 84.072MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pointer.Derived3.smt2                          |    3.120s | 140.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Utils.smt2                                     |    3.126s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int8.smt2                                      |    3.136s | 85.012MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Loops.smt2                                     |    3.142s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Vector.Base.smt2                               |    3.151s | 81.596MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Literal.smt2                                 |    3.166s | 145.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Effect.Atomic.smt2                          |    3.187s | 165.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int16.smt2                                     |    3.197s | 85.332MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.GhostPCMReference.smt2                      |    3.203s | 156.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.DisposableInvariant.smt2                       |    3.212s | 156.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Reference.smt2                              |    3.215s | 156.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.SpinLock.smt2                               |    3.235s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Coercions.smt2                              |    3.264s | 165.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.BitVector.smt2                              |    3.285s | 152.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-4.smt2                             |    3.317s | 100.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Monotonic.Buffer-2.smt2                      |    3.352s | 159.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Effect.Ghost.smt2                           |    3.367s | 167.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ReflexiveTransitiveClosure.smt2                |    3.382s | 94.74MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.List.Pure.Properties.smt2                      |    3.438s | 77.7MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int128.smt2                                    |    3.525s | 87.056MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int32.smt2                                     |    3.561s | 86.04MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int64.smt2                                     |    3.574s | 86.792MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.InteractiveHelpers.Base.smt2                   |    3.595s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.HyperHeap.smt2                       |    3.630s | 79.904MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Formula.smt2                        |    3.649s | 90.652MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.SpinLock.smt2                                  |    3.680s | 152.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ConstantTime.Integers.smt2                     |    3.731s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Primitive.ForkJoin.Unix.smt2                   |    3.786s | 155.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.List.Tot.Base.smt2                             |    3.817s | 74.8MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Printf.smt2                                    |    3.907s | 137.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Array.smt2                                     |    3.918s | 87.828MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonCommMonoidSimple.smt2             |    3.936s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Preorder.smt2                                  |    3.957s | 87.36MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.BitVector.smt2                                 |    4.012s | 77.368MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Data.smt2                           |    4.025s | 75.584MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Effect.AtomicAndGhost.smt2                  |    4.044s | 168.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.InteractiveHelpers.ExploreTerm.smt2            |    4.251s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Sequence.Permutation.smt2                      |    4.301s | 81.124MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Canon.smt2                             |    4.307s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.UninitializedBuffer.smt2                     |    4.389s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonCommMonoidSimple.Equiv.smt2       |    4.439s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.OrdSet.smt2                                    |    4.441s | 79.444MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.InteractiveHelpers.Effectful.smt2              |    4.491s | 138.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonCommMonoid.smt2                   |    4.511s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.PCMReference.smt2                              |    4.540s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.ConstBuffer.smt2                             |    4.858s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.BigOps.smt2                                    |    4.909s | 168.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.ImmutableBuffer.smt2                         |    4.956s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Effect.smt2                                 |    5.092s | 164.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Array.smt2                                  |    5.096s | 156.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.InteractiveHelpers.PostProcess.smt2            |    5.141s | 155.0MiB| unsat | 0 |  |
|queries-FStar.Monotonic.Seq.smt2                             |    5.156s | 95.512MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Bytes.smt2                                     |    5.343s | 137.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-2.smt2                             |    5.524s | 101.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Permutation-3.smt2                         |    5.536s | 90.476MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.PrefixFreezableBuffer.smt2                   |    5.616s | 138.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Monotonic.Buffer-1.smt2                      |    5.663s | 159.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.BV.smt2                                        |    5.682s | 84.42MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Algebra.Monoid.smt2                            |    5.684s | 312.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Regional.Instances.smt2                      |    5.740s | 150.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Arith.smt2                          |    5.958s | 161.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Base.smt2                                  |    6.129s | 82.492MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Integers.smt2                                  |    6.244s | 161.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Endianness.smt2                              |    6.504s | 138.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.ToFStarBuffer.smt2                           |    6.591s | 143.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Derived.smt2                           |    6.654s | 96.032MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Printf.smt2                                  |    6.788s | 159.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.BV.smt2                                |    7.144s | 140.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.PatternMatching.smt2                   |    7.150s | 141.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.UInt32.smt2                                    |    7.332s | 89.884MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Math.Euclid.smt2                               |    7.344s | 137.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.HyperStack.smt2                      |    7.360s | 92.804MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.Heap.smt2                            |    7.696s | 92.5MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Array.Util.smt2                             |    7.712s | 157.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.UInt16.smt2                                    |    7.761s | 90.336MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.UInt64.smt2                                    |    7.890s | 91.428MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.List.Tot.Properties.smt2                       |    8.065s | 85.532MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Channel.Simplex.smt2                           |    8.167s | 164.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.BufferView.smt2                              |    8.798s | 139.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Buffer.Quantifiers.smt2                        |    8.843s | 151.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.UInt8.smt2                                     |    8.986s | 91.164MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.HyperStack.ST.smt2                             |    9.079s | 164.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.BufferNG.smt2                                  |    9.088s | 146.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.TaggedUnion.smt2                               |    9.098s | 146.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pointer.Base.smt2                              |    9.308s | 139.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.BufferView.Up.smt2                           |    9.395s | 143.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Sequence.Base.smt2                             |    9.525s | 97.944MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonCommSemiring.smt2                 |   10.227s | 141.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pointer.Derived1.smt2                          |   10.685s | 145.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Matrix.smt2                                    |   10.777s | 100.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Permutation-1.smt2                         |   11.612s | 169.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Math.Lemmas.smt2                               |   11.701s | 74.78MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Array.smt2                                     |   12.291s | 193.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Stepper.smt2                                   |   12.833s | 189.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.BufferView.Down.smt2                         |   13.222s | 140.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Memory.smt2                                    |   13.656s | 161.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Reference.smt2                                 |   13.895s | 158.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.EphemeralHashtbl.smt2                       |   14.750s | 174.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-7.smt2                             |   15.204s | 181.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Modifies.smt2                                  |   15.385s | 145.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-3.smt2                             |   16.068s | 164.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int.Cast.smt2                                  |   16.604s | 101.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Effect.smt2                                    |   16.620s | 187.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-1.smt2                             |   17.097s | 164.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Properties.smt2                            |   18.136s | 160.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Math.Euclid-1.smt2                             |   18.142s | 1690.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.HigherReference.smt2                           |   18.994s | 210.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Heap-1.smt2                                    |   19.099s | 221.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.FractionalAnchoredPreorder.smt2                |   19.615s | 193.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int.smt2                                       |   22.552s | 113.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.MonotonicHigherReference.smt2                  |   23.146s | 215.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Math.Fermat.smt2                               |   24.391s | 157.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Endianness.smt2                                |   24.498s | 170.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Vector.smt2                                  |   25.132s | 164.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Effect.Common.smt2                             |   27.435s | 171.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Effect.Atomic.smt2                             |   34.529s | 322.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Matrix-1.smt2                                  |   38.216s | 1486.0MiB| unknown | 1 | benign: get-model/get-value after unknown: model is not available |
|queries-FStar.UInt128.smt2                                   |   59.802s | 485.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-5.smt2                             |   60.038s | 217.0MiB| timeout | 0 |  |
|queries-FStar.Buffer.smt2                                    |   60.056s | 182.0MiB| timeout | 0 |  |
|queries-Steel.Semantics.Hoare.MST.smt2                       |   60.074s | 177.0MiB| timeout | 0 |  |
|queries-FStar.UInt.smt2                                      |   60.078s | 188.0MiB| timeout | 0 |  |
|queries-Steel.Heap.smt2                                      |   60.086s | 443.0MiB| timeout | 0 |  |
|queries-LowStar.Monotonic.Buffer.smt2                        |   60.102s | 330.0MiB| timeout | 0 |  |
|queries-LowStar.RVector.smt2                                 |   60.111s | 326.0MiB| timeout | 0 |  |
|queries-FStar.ModifiesGen.smt2                               |   60.129s | 321.0MiB| timeout | 0 |  |
