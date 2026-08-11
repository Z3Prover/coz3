# .

* SAT 0
* UNSAT 232
* TIMEOUT 18
* UNKNOWN 0

* ERRORS 0

* BENIGN 231 (model query without model)

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: arith.nl.optimize_bounds_eager=true on fstar-ulib-smt2, branch nla-avoid-grobner-horner (8195d4f77)
Job tag: eager-true-fstar-ulib-smt2
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 8195d4f771ca40f94dd4635c0ea6c9f296144a05
Z3 branch: nla-avoid-grobner-horner
Z3 options: "-T:20 model_validate=true smt.arith.solver=6 smt.arith.nl.optimize_bounds_eager=true"
Z3 inputs: inputs/fstar-ulib-smt2
Z3 commit message: Default the eager bounds mode to on

tr.sh (fstar corpus, seed 33355) had solver=6 averaging 0.28s against
0.07s for solver=2, with the time going to Horner cross-nesting, Grobner
saturation, bounded nlsat and monomial patching on the UInt128/Matrix
families. With the eager mode on the corpus average drops to 0.12s with
identical results, and the 10-check budget contains the QF_NIA exposure:
on a 400-file random QF_NIA sample the flip solves 304 vs 301 with a
0.95 penalized time ratio, and a 300-file QF_NRA sample is unchanged
(265 solved both ways, no flips).

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | INFO |
|------------|----------:|---------:|-------------:| ----------:|------|
|queries-Prims.smt2                                           |    0.085s | 23.5MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Endianness-1.smt2                            |    0.091s | 26.06MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Heap.smt2                                      |    0.499s | 75.192MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.VConfig.smt2                                   |    0.534s | 49.656MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Effect-1.smt2                          |    0.550s | 75.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pervasives.Native.smt2                         |    0.553s | 46.148MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Common.smt2                            |    0.569s | 49.656MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.All.smt2                                       |    0.586s | 75.28MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.PropositionalExtensionality.smt2               |    0.594s | 50.356MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Classical-1.smt2                               |    0.615s | 74.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Classical.smt2                                 |    0.620s | 74.752MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.TSet-1.smt2                                    |    0.630s | 74.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Types.smt2                          |    0.721s | 74.852MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Sequence.Ambient.smt2                          |    0.753s | 74.76MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.OrdMapProps.smt2                               |    0.753s | 74.744MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.Pure.smt2                            |    0.777s | 50.172MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Universe.smt2                                  |    0.781s | 49.96MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Squash.smt2                                    |    0.797s | 49.664MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.OrdSetProps.smt2                               |    0.802s | 74.912MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.IFC.smt2                                       |    0.805s | 74.76MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Order.smt2                                     |    0.811s | 50.276MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Relational.Relational.smt2                     |    0.827s | 75.28MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.TwoLevelHeap.smt2                              |    0.863s | 75.28MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Effect-2.smt2                          |    0.864s | 75.272MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.PredicateExtensionality.smt2                   |    0.879s | 74.784MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Algebra.CommMonoid.Equiv.smt2                  |    0.918s | 74.764MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Algebra.CommMonoid.smt2                        |    0.925s | 50.944MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.MRef.smt2                                      |    0.928s | 75.232MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.FractionalPermission.smt2                      |    0.947s | 74.604MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Option.smt2                                    |    0.971s | 75.608MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.SquashProperties.smt2                          |    0.999s | 74.704MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Result.smt2                            |    1.003s | 75.028MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Ref.smt2                                       |    1.010s | 75.26MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Ghost.smt2                                     |    1.032s | 50.684MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.IndefiniteDescription.smt2                     |    1.033s | 74.8MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.TSet-2.smt2                                    |    1.036s | 74.792MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.List.Pure.Base.smt2                            |    1.078s | 75.264MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Universe.PCM.smt2                              |    1.098s | 75.244MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.WellFounded.smt2                               |    1.098s | 75.016MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Calc.smt2                                      |    1.134s | 74.744MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.MST.smt2                                       |    1.139s | 74.848MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Comment.smt2                                 |    1.144s | 81.476MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.MSTTotal.smt2                                  |    1.150s | 74.492MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Constructive.smt2                              |    1.151s | 50.72MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.PartialMap.smt2                                |    1.161s | 74.764MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Char.smt2                                      |    1.166s | 79.508MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.IntegerIntervals.smt2                          |    1.175s | 76.344MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.SyntaxHelpers.smt2                     |    1.186s | 87.54MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Sequence.Util.smt2                             |    1.223s | 74.752MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.Witnessed.smt2                       |    1.231s | 74.744MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Derived.Lemmas.smt2                 |    1.280s | 85.116MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Util.smt2                                      |    1.288s | 80.14MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.GSet.smt2                                      |    1.299s | 74.66MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.MonotonicCounter.smt2                          |    1.325s | 76.968MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ST.smt2                                        |    1.335s | 75.08MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.NMSTTotal.smt2                                 |    1.347s | 74.872MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.TSet.smt2                                      |    1.374s | 74.744MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Set.smt2                                       |    1.377s | 74.708MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Util.smt2                              |    1.387s | 76.72MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.String.smt2                                    |    1.416s | 84.296MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Matrix2.smt2                                   |    1.417s | 76.896MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.NMST.smt2                                      |    1.420s | 75.008MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Effect.smt2                            |    1.446s | 75.176MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int.Cast.Full.smt2                             |    1.447s | 92.956MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Permutation-2.smt2                         |    1.465s | 80.664MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Error.smt2                                     |    1.503s | 87.84MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Classical.Sugar.smt2                           |    1.597s | 74.744MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Map.smt2                                       |    1.641s | 74.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Fin.smt2                                       |    1.661s | 77.608MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Print.smt2                             |    1.694s | 90.132MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Sequence.Seq.smt2                              |    1.701s | 79.704MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Real.smt2                                      |    1.702s | 50.688MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.BufferCompat.smt2                            |    1.718s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.LexicographicOrdering.smt2                     |    1.727s | 75.268MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Vector.Properties.smt2                         |    1.788s | 82.46MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.FiniteSet.smt2                                 |    1.792s | 76.952MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Crypto.smt2                                    |    1.798s | 138.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Regional.smt2                                |    1.805s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Classical-2.smt2                               |    1.805s | 74.604MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.PCMMap.smt2                                    |    1.816s | 75.884MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.Map.smt2                             |    1.819s | 85.016MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonCommSwaps.smt2                    |    1.861s | 75.824MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.FunctionalExtensionality.smt2                  |    1.891s | 81.928MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.BufferOps.smt2                               |    1.905s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Algebra.CommMonoid.Fold.Nested.smt2            |    1.938s | 96.516MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pervasives.smt2                                |    1.939s | 50.76MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.DependentMap.smt2                              |    1.941s | 74.756MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Typeclasses.smt2                       |    1.964s | 94.772MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tcp.smt2                                       |    1.969s | 138.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.PCM.smt2                                       |    1.986s | 74.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.InteractiveHelpers.Output.smt2                 |    1.999s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Simplifier.smt2                        |    2.045s | 92.768MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.LockCoupling.smt2                              |    2.072s | 152.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Derived.smt2                        |    2.106s | 85.14MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.CancellableSpinLock.smt2                    |    2.146s | 151.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Channel.Protocol.smt2                          |    2.242s | 74.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.GhostReference.smt2                         |    2.281s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.List.smt2                                      |    2.303s | 76.856MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-6.smt2                             |    2.315s | 96.796MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.OrdMap.smt2                                    |    2.317s | 74.988MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.WellFoundedRelation.smt2                       |    2.375s | 76.112MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Effect.M.smt2                                  |    2.376s | 139.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Util.smt2                                   |    2.411s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Channel.Duplex.smt2                            |    2.419s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Loops.Util.smt2                             |    2.422s | 152.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Endianness-2.smt2                            |    2.433s | 138.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Sorted.smt2                                |    2.457s | 76.852MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Math.Lib.smt2                                  |    2.488s | 52.892MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.MonotonicReference.smt2                     |    2.496s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Permutation.smt2                           |    2.528s | 79.636MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Closure.smt2                                   |    2.530s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Equiv.smt2                                 |    2.551s | 82.716MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.WellFounded.Util.smt2                          |    2.692s | 94.804MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int16.smt2                                     |    2.790s | 84.256MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Coercions.smt2                              |    2.827s | 165.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Buffer.smt2                                  |    3.008s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonMonoid.smt2                       |    3.014s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Primitive.ForkJoin.smt2                        |    3.024s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.GhostPCMReference.smt2                         |    3.073s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Vector.Base.smt2                               |    3.081s | 81.432MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int128.smt2                                    |    3.088s | 86.96MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.GhostPCMReference.smt2                      |    3.107s | 157.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Logic.smt2                             |    3.121s | 92.78MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int32.smt2                                     |    3.161s | 83.832MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.DisposableInvariant.smt2                       |    3.170s | 156.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int64.smt2                                     |    3.177s | 84.232MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.DependentMap.smt2                    |    3.177s | 87.116MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Reference.smt2                              |    3.195s | 156.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pointer.Derived2.smt2                          |    3.213s | 140.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Effect.Atomic.smt2                          |    3.261s | 165.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Algebra.CommMonoid.Fold.smt2                   |    3.289s | 84.24MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.InteractiveHelpers.Base.smt2                   |    3.290s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.SpinLock.smt2                               |    3.312s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.BitVector.smt2                              |    3.313s | 152.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.SpinLock.smt2                                  |    3.321s | 152.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Semantics.Instantiate.smt2                     |    3.326s | 143.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int8.smt2                                      |    3.379s | 85.532MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Utils.smt2                                     |    3.441s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Loops.smt2                                     |    3.464s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.List.Tot.Base.smt2                             |    3.501s | 74.764MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Monotonic.Buffer-2.smt2                      |    3.517s | 159.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Literal.smt2                                 |    3.566s | 145.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pointer.Derived3.smt2                          |    3.587s | 141.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.List.Pure.Properties.smt2                      |    3.639s | 77.796MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.MonotonicReference.smt2                        |    3.666s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ReflexiveTransitiveClosure.smt2                |    3.777s | 94.952MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Math.Euclid-1.smt2                             |    3.781s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Loops.smt2                                  |    3.844s | 155.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Effect.Ghost.smt2                           |    3.853s | 167.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-4.smt2                             |    3.859s | 99.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.HyperHeap.smt2                       |    3.944s | 80.164MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Sequence.Permutation.smt2                      |    3.997s | 80.968MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Formula.smt2                        |    4.007s | 90.972MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.OrdSet.smt2                                    |    4.029s | 79.464MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Effect.AtomicAndGhost.smt2                  |    4.050s | 168.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.UninitializedBuffer.smt2                     |    4.083s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.InteractiveHelpers.ExploreTerm.smt2            |    4.119s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Data.smt2                           |    4.168s | 75.8MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Printf.smt2                                    |    4.182s | 137.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.PCMReference.smt2                              |    4.227s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Canon.smt2                             |    4.293s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ConstantTime.Integers.smt2                     |    4.321s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Primitive.ForkJoin.Unix.smt2                   |    4.327s | 155.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonCommMonoidSimple.smt2             |    4.342s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Preorder.smt2                                  |    4.390s | 87.42MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Array.smt2                                     |    4.445s | 87.576MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Effect.smt2                                 |    4.484s | 164.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.BitVector.smt2                                 |    4.509s | 77.732MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.InteractiveHelpers.Effectful.smt2              |    4.525s | 138.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.InteractiveHelpers.PostProcess.smt2            |    4.629s | 154.0MiB| unsat | 0 |  |
|queries-FStar.BigOps.smt2                                    |    4.689s | 168.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.ImmutableBuffer.smt2                         |    4.713s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.ConstBuffer.smt2                             |    4.716s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonCommMonoidSimple.Equiv.smt2       |    4.723s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonCommMonoid.smt2                   |    4.929s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.PrefixFreezableBuffer.smt2                   |    5.025s | 138.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Bytes.smt2                                     |    5.244s | 137.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Array.smt2                                  |    5.277s | 157.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.Seq.smt2                             |    5.400s | 95.644MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Permutation-3.smt2                         |    5.452s | 90.392MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-2.smt2                             |    5.484s | 101.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Arith.smt2                          |    5.527s | 161.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Algebra.Monoid.smt2                            |    5.615s | 312.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.BV.smt2                                        |    5.671s | 84.576MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Integers.smt2                                  |    5.962s | 161.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Regional.Instances.smt2                      |    6.160s | 150.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Monotonic.Buffer-1.smt2                      |    6.192s | 159.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.ToFStarBuffer.smt2                           |    6.351s | 143.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Base.smt2                                  |    6.426s | 82.12MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.UInt64.smt2                                    |    6.501s | 91.188MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Math.Euclid.smt2                               |    6.613s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.Heap.smt2                            |    7.047s | 92.648MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Derived.smt2                           |    7.050s | 96.404MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Endianness.smt2                              |    7.282s | 138.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Printf.smt2                                  |    7.353s | 159.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.PatternMatching.smt2                   |    7.402s | 140.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.BV.smt2                                |    7.422s | 140.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.BufferView.smt2                              |    7.448s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.HyperStack.smt2                      |    7.521s | 92.752MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.UInt32.smt2                                    |    7.626s | 89.236MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Array.Util.smt2                             |    7.926s | 157.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.UInt8.smt2                                     |    7.968s | 89.756MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.List.Tot.Properties.smt2                       |    8.132s | 85.432MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.BufferView.Up.smt2                           |    8.314s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.UInt16.smt2                                    |    8.457s | 90.96MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.HyperStack.ST.smt2                             |    8.604s | 164.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Buffer.Quantifiers.smt2                        |    8.823s | 151.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.TaggedUnion.smt2                               |    8.926s | 146.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Channel.Simplex.smt2                           |    8.981s | 164.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pointer.Base.smt2                              |    9.088s | 139.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Sequence.Base.smt2                             |    9.257s | 98.044MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.BufferNG.smt2                                  |    9.555s | 146.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Matrix.smt2                                    |   10.036s | 103.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pointer.Derived1.smt2                          |   10.111s | 145.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonCommSemiring.smt2                 |   10.142s | 141.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Math.Lemmas.smt2                               |   10.592s | 74.744MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Permutation-1.smt2                         |   11.119s | 169.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.BufferView.Down.smt2                         |   11.995s | 140.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Stepper.smt2                                   |   12.776s | 188.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Array.smt2                                     |   12.922s | 193.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Reference.smt2                                 |   13.290s | 158.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Memory.smt2                                    |   13.917s | 161.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Effect.smt2                                    |   15.097s | 187.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-3.smt2                             |   15.541s | 164.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-7.smt2                             |   15.682s | 181.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Modifies.smt2                                  |   15.688s | 145.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-1.smt2                             |   16.017s | 164.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.HigherReference.smt2                           |   16.380s | 210.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int.Cast.smt2                                  |   17.124s | 102.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Properties.smt2                            |   17.792s | 159.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.EphemeralHashtbl.smt2                       |   17.966s | 178.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Heap-1.smt2                                    |   18.238s | 221.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Math.Fermat.smt2                               |   20.025s | 155.0MiB| timeout | 0 |  |
|queries-Steel.MonotonicHigherReference.smt2                  |   20.038s | 214.0MiB| timeout | 0 |  |
|queries-FStar.ModifiesGen-5.smt2                             |   20.046s | 165.0MiB| timeout | 0 |  |
|queries-FStar.Buffer.smt2                                    |   20.051s | 162.0MiB| timeout | 0 |  |
|queries-Steel.FractionalAnchoredPreorder.smt2                |   20.063s | 191.0MiB| timeout | 0 |  |
|queries-FStar.ModifiesGen.smt2                               |   20.064s | 165.0MiB| timeout | 0 |  |
|queries-FStar.UInt128.smt2                                   |   20.068s | 554.0MiB| timeout | 0 |  |
|queries-FStar.Matrix-1.smt2                                  |   20.068s | 346.0MiB| timeout | 0 |  |
|queries-LowStar.RVector.smt2                                 |   20.073s | 164.0MiB| timeout | 0 |  |
|queries-Steel.Effect.Common.smt2                             |   20.075s | 163.0MiB| timeout | 0 |  |
|queries-FStar.Endianness.smt2                                |   20.076s | 167.0MiB| timeout | 0 |  |
|queries-FStar.UInt.smt2                                      |   20.078s | 101.0MiB| timeout | 0 |  |
|queries-LowStar.Monotonic.Buffer.smt2                        |   20.081s | 164.0MiB| timeout | 0 |  |
|queries-Steel.Effect.Atomic.smt2                             |   20.084s | 186.0MiB| timeout | 0 |  |
|queries-FStar.Int.smt2                                       |   20.107s | 110.0MiB| timeout | 0 |  |
|queries-LowStar.Vector.smt2                                  |   20.113s | 162.0MiB| timeout | 0 |  |
|queries-Steel.Semantics.Hoare.MST.smt2                       |   20.116s | 176.0MiB| timeout | 0 |  |
|queries-Steel.Heap.smt2                                      |   20.118s | 223.0MiB| timeout | 0 |  |
