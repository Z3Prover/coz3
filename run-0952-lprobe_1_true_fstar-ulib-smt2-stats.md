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
Job description: fstar-ulib-smt2 with lprobe true
Job tag: lprobe_1_true_fstar-ulib-smt2
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 36738a03948decd13351d874dc573e17965c31ee
Z3 branch: linprobe
Z3 options: "-T:60 smt.arith.nl.linprobe=true"
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
|queries-Prims.smt2                                           |    0.166s | 23.648MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Endianness-1.smt2                            |    0.179s | 26.18MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.VConfig.smt2                                   |    0.444s | 49.664MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pervasives.Native.smt2                         |    0.447s | 46.152MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Heap.smt2                                      |    0.565s | 75.216MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.TSet-1.smt2                                    |    0.572s | 74.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.Pure.smt2                            |    0.595s | 50.192MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Types.smt2                          |    0.614s | 74.74MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Classical-1.smt2                               |    0.627s | 74.752MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Common.smt2                            |    0.634s | 49.664MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Effect-1.smt2                          |    0.636s | 75.012MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Sequence.Ambient.smt2                          |    0.654s | 74.768MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.PropositionalExtensionality.smt2               |    0.758s | 50.176MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Order.smt2                                     |    0.760s | 50.456MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.All.smt2                                       |    0.778s | 75.292MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Universe.smt2                                  |    0.797s | 49.82MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.OrdMapProps.smt2                               |    0.807s | 75.008MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Option.smt2                                    |    0.814s | 75.612MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Classical.smt2                                 |    0.829s | 74.62MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.PartialMap.smt2                                |    0.830s | 74.752MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.FractionalPermission.smt2                      |    0.841s | 74.644MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.TwoLevelHeap.smt2                              |    0.870s | 75.264MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Relational.Relational.smt2                     |    0.875s | 75.12MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Squash.smt2                                    |    0.884s | 49.72MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.OrdSetProps.smt2                               |    0.888s | 74.908MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.SquashProperties.smt2                          |    0.890s | 74.84MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.IndefiniteDescription.smt2                     |    0.919s | 74.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Algebra.CommMonoid.smt2                        |    0.920s | 50.86MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.PredicateExtensionality.smt2                   |    0.924s | 74.768MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Effect-2.smt2                          |    0.937s | 75.244MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.IFC.smt2                                       |    0.969s | 74.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Algebra.CommMonoid.Equiv.smt2                  |    0.975s | 74.544MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.WellFounded.smt2                               |    0.985s | 75.036MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Result.smt2                            |    0.990s | 75.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Universe.PCM.smt2                              |    0.993s | 75.26MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.List.Pure.Base.smt2                            |    1.017s | 75.308MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.MSTTotal.smt2                                  |    1.047s | 74.536MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Constructive.smt2                              |    1.051s | 50.712MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Ref.smt2                                       |    1.064s | 75.288MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Calc.smt2                                      |    1.101s | 74.572MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.TSet-2.smt2                                    |    1.132s | 74.764MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Ghost.smt2                                     |    1.142s | 50.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Comment.smt2                                 |    1.176s | 81.592MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.MRef.smt2                                      |    1.179s | 75.264MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.NMSTTotal.smt2                                 |    1.183s | 74.832MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.IntegerIntervals.smt2                          |    1.230s | 76.352MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.TSet.smt2                                      |    1.234s | 74.724MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Char.smt2                                      |    1.249s | 79.52MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.GSet.smt2                                      |    1.279s | 74.752MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.MonotonicCounter.smt2                          |    1.284s | 77.076MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Util.smt2                                      |    1.304s | 80.152MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.String.smt2                                    |    1.314s | 84.244MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Sequence.Util.smt2                             |    1.318s | 74.932MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int.Cast.Full.smt2                             |    1.320s | 92.98MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Permutation-2.smt2                         |    1.346s | 80.944MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.NMST.smt2                                      |    1.347s | 75.008MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.MST.smt2                                       |    1.348s | 74.816MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Set.smt2                                       |    1.413s | 74.652MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ST.smt2                                        |    1.414s | 75.26MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Util.smt2                              |    1.428s | 76.852MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Algebra.CommMonoid.Fold.Nested.smt2            |    1.433s | 96.536MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.SyntaxHelpers.smt2                     |    1.437s | 87.66MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Classical.Sugar.smt2                           |    1.456s | 74.528MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Map.smt2                                       |    1.498s | 74.8MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Effect.smt2                            |    1.541s | 75.312MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Matrix2.smt2                                   |    1.555s | 77.116MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Fin.smt2                                       |    1.563s | 77.592MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Error.smt2                                     |    1.601s | 88.028MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Sequence.Seq.smt2                              |    1.634s | 79.888MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.FiniteSet.smt2                                 |    1.676s | 76.86MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.Witnessed.smt2                       |    1.680s | 74.736MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Crypto.smt2                                    |    1.686s | 138.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.BufferOps.smt2                               |    1.706s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Vector.Properties.smt2                         |    1.714s | 82.46MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Real.smt2                                      |    1.727s | 50.556MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.DependentMap.smt2                              |    1.753s | 74.8MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Derived.Lemmas.smt2                 |    1.770s | 85.016MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Print.smt2                             |    1.794s | 90.096MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.PCMMap.smt2                                    |    1.822s | 75.92MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Classical-2.smt2                               |    1.843s | 74.752MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.BufferCompat.smt2                            |    1.881s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pervasives.smt2                                |    1.906s | 50.78MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tcp.smt2                                       |    1.914s | 138.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Typeclasses.smt2                       |    1.933s | 94.932MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.CancellableSpinLock.smt2                    |    1.988s | 151.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonCommSwaps.smt2                    |    1.991s | 75.832MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Regional.smt2                                |    1.994s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.Map.smt2                             |    2.011s | 85.036MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.LexicographicOrdering.smt2                     |    2.017s | 75.268MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Permutation.smt2                           |    2.114s | 79.604MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.LockCoupling.smt2                              |    2.141s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.PCM.smt2                                       |    2.167s | 74.656MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Simplifier.smt2                        |    2.182s | 92.76MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.WellFoundedRelation.smt2                       |    2.204s | 76.084MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.FunctionalExtensionality.smt2                  |    2.213s | 81.888MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-6.smt2                             |    2.254s | 96.784MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.List.smt2                                      |    2.298s | 76.856MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Equiv.smt2                                 |    2.317s | 82.796MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Endianness-2.smt2                            |    2.323s | 138.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.OrdMap.smt2                                    |    2.332s | 74.86MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Channel.Protocol.smt2                          |    2.409s | 74.548MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Closure.smt2                                   |    2.422s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Util.smt2                                   |    2.449s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Loops.Util.smt2                             |    2.483s | 152.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Sorted.smt2                                |    2.485s | 76.856MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Math.Lib.smt2                                  |    2.503s | 53.536MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.InteractiveHelpers.Output.smt2                 |    2.556s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.GhostReference.smt2                         |    2.564s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.MonotonicReference.smt2                     |    2.594s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.WellFounded.Util.smt2                          |    2.598s | 94.804MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Buffer.smt2                                  |    2.620s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.MonotonicReference.smt2                        |    2.644s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Effect.M.smt2                                  |    2.705s | 139.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int16.smt2                                     |    2.738s | 85.192MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Reference.smt2                              |    2.753s | 156.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Algebra.CommMonoid.Fold.smt2                   |    2.857s | 83.984MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Derived.smt2                        |    2.864s | 85.256MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Logic.smt2                             |    2.878s | 93.028MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Channel.Duplex.smt2                            |    2.906s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Coercions.smt2                              |    2.966s | 165.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonMonoid.smt2                       |    3.017s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.List.Tot.Base.smt2                             |    3.044s | 74.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Vector.Base.smt2                               |    3.060s | 81.512MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Primitive.ForkJoin.smt2                        |    3.066s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.DependentMap.smt2                    |    3.108s | 87.068MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Loops.smt2                                     |    3.123s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Semantics.Instantiate.smt2                     |    3.133s | 143.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int64.smt2                                     |    3.143s | 86.928MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.GhostPCMReference.smt2                      |    3.143s | 157.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pointer.Derived2.smt2                          |    3.158s | 140.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int32.smt2                                     |    3.164s | 85.972MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.GhostPCMReference.smt2                         |    3.187s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Formula.smt2                        |    3.233s | 90.704MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Effect.Atomic.smt2                          |    3.241s | 166.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int8.smt2                                      |    3.279s | 85.06MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Utils.smt2                                     |    3.303s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.List.Pure.Properties.smt2                      |    3.372s | 77.648MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.SpinLock.smt2                               |    3.377s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pointer.Derived3.smt2                          |    3.399s | 140.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int128.smt2                                    |    3.414s | 86.864MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.HyperHeap.smt2                       |    3.439s | 80.132MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.BitVector.smt2                              |    3.456s | 152.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Effect.Ghost.smt2                           |    3.470s | 167.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Monotonic.Buffer-2.smt2                      |    3.519s | 159.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.DisposableInvariant.smt2                       |    3.542s | 157.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.OrdSet.smt2                                    |    3.552s | 79.392MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.InteractiveHelpers.Base.smt2                   |    3.558s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.SpinLock.smt2                                  |    3.569s | 152.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-4.smt2                             |    3.601s | 100.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Loops.smt2                                  |    3.611s | 155.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Literal.smt2                                 |    3.661s | 145.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ConstantTime.Integers.smt2                     |    3.726s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ReflexiveTransitiveClosure.smt2                |    3.772s | 94.644MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Sequence.Permutation.smt2                      |    3.983s | 80.86MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonCommMonoidSimple.smt2             |    4.019s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.UninitializedBuffer.smt2                     |    4.055s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Preorder.smt2                                  |    4.144s | 87.324MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Array.smt2                                     |    4.277s | 87.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.PCMReference.smt2                              |    4.302s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.InteractiveHelpers.ExploreTerm.smt2            |    4.373s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.BitVector.smt2                                 |    4.387s | 77.5MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Effect.smt2                                 |    4.437s | 164.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Canon.smt2                             |    4.519s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Effect.AtomicAndGhost.smt2                  |    4.575s | 168.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.InteractiveHelpers.PostProcess.smt2            |    4.660s | 154.0MiB| unsat | 0 |  |
|queries-FStar.BigOps.smt2                                    |    4.670s | 168.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonCommMonoid.smt2                   |    4.683s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Printf.smt2                                    |    4.701s | 137.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonCommMonoidSimple.Equiv.smt2       |    4.772s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Data.smt2                           |    4.886s | 75.54MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.ImmutableBuffer.smt2                         |    5.008s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Primitive.ForkJoin.Unix.smt2                   |    5.055s | 155.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.ConstBuffer.smt2                             |    5.131s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Array.smt2                                  |    5.153s | 157.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.PrefixFreezableBuffer.smt2                   |    5.279s | 138.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Monotonic.Buffer-1.smt2                      |    5.307s | 159.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Algebra.Monoid.smt2                            |    5.454s | 312.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.InteractiveHelpers.Effectful.smt2              |    5.585s | 138.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Regional.Instances.smt2                      |    5.693s | 150.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Bytes.smt2                                     |    5.736s | 137.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Arith.smt2                          |    5.773s | 161.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.Seq.smt2                             |    5.820s | 95.648MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.BV.smt2                                        |    5.943s | 84.54MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Permutation-3.smt2                         |    6.239s | 90.42MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Derived.smt2                           |    6.322s | 96.02MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Base.smt2                                  |    6.367s | 82.252MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Integers.smt2                                  |    6.368s | 161.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.ToFStarBuffer.smt2                           |    6.473s | 143.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Printf.smt2                                  |    6.573s | 159.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-2.smt2                             |    6.583s | 101.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Endianness.smt2                              |    6.674s | 138.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.UInt16.smt2                                    |    6.706s | 90.3MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.Heap.smt2                            |    6.988s | 92.62MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.PatternMatching.smt2                   |    6.995s | 140.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.BV.smt2                                |    7.102s | 140.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.HyperStack.smt2                      |    7.209s | 92.8MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Array.Util.smt2                             |    7.298s | 157.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Math.Euclid.smt2                               |    7.409s | 137.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.UInt32.smt2                                    |    7.431s | 89.68MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.UInt64.smt2                                    |    7.916s | 91.256MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.UInt8.smt2                                     |    8.152s | 91.296MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.BufferView.smt2                              |    8.205s | 139.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Channel.Simplex.smt2                           |    8.281s | 164.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Buffer.Quantifiers.smt2                        |    8.601s | 151.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.List.Tot.Properties.smt2                       |    8.637s | 85.6MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.BufferNG.smt2                                  |    8.807s | 146.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.HyperStack.ST.smt2                             |    8.810s | 164.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.BufferView.Up.smt2                           |    8.825s | 142.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.TaggedUnion.smt2                               |    9.059s | 146.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pointer.Base.smt2                              |    9.291s | 139.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Sequence.Base.smt2                             |    9.558s | 98.024MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Matrix.smt2                                    |   10.196s | 99.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pointer.Derived1.smt2                          |   10.469s | 145.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonCommSemiring.smt2                 |   10.531s | 141.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Permutation-1.smt2                         |   10.937s | 169.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Math.Lemmas.smt2                               |   11.180s | 74.8MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Array.smt2                                     |   12.347s | 193.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.BufferView.Down.smt2                         |   12.681s | 140.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Stepper.smt2                                   |   12.986s | 189.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Reference.smt2                                 |   13.870s | 158.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Memory.smt2                                    |   14.229s | 161.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.EphemeralHashtbl.smt2                       |   15.592s | 174.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-3.smt2                             |   15.866s | 164.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-1.smt2                             |   16.107s | 164.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Math.Euclid-1.smt2                             |   16.583s | 1690.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-7.smt2                             |   16.687s | 182.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Modifies.smt2                                  |   16.832s | 145.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int.Cast.smt2                                  |   16.911s | 101.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Effect.smt2                                    |   17.446s | 187.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.HigherReference.smt2                           |   17.513s | 210.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Properties.smt2                            |   17.817s | 160.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Heap-1.smt2                                    |   18.376s | 221.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.FractionalAnchoredPreorder.smt2                |   19.425s | 193.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int.smt2                                       |   22.551s | 113.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.MonotonicHigherReference.smt2                  |   23.194s | 214.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Math.Fermat.smt2                               |   24.323s | 156.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Endianness.smt2                                |   24.880s | 170.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Vector.smt2                                  |   25.385s | 164.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Effect.Common.smt2                             |   27.351s | 170.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Effect.Atomic.smt2                             |   34.161s | 322.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Matrix-1.smt2                                  |   37.949s | 1486.0MiB| unknown | 1 | benign: get-model/get-value after unknown: model is not available |
|queries-FStar.UInt128.smt2                                   |   59.126s | 485.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Buffer.smt2                                    |   60.031s | 182.0MiB| timeout | 0 |  |
|queries-FStar.UInt.smt2                                      |   60.098s | 188.0MiB| timeout | 0 |  |
|queries-Steel.Heap.smt2                                      |   60.111s | 442.0MiB| timeout | 0 |  |
|queries-Steel.Semantics.Hoare.MST.smt2                       |   60.125s | 177.0MiB| timeout | 0 |  |
|queries-FStar.ModifiesGen-5.smt2                             |   60.132s | 217.0MiB| timeout | 0 |  |
|queries-LowStar.Monotonic.Buffer.smt2                        |   60.134s | 330.0MiB| timeout | 0 |  |
|queries-LowStar.RVector.smt2                                 |   60.138s | 325.0MiB| timeout | 0 |  |
|queries-FStar.ModifiesGen.smt2                               |   60.140s | 321.0MiB| timeout | 0 |  |
