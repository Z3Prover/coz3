# .

* SAT 0
* UNSAT 233
* TIMEOUT 17
* UNKNOWN 0

* ERRORS 0

* BENIGN 232 (model query without model)

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: arith.nl.optimize_bounds_eager=false on fstar-ulib-smt2, branch nla-avoid-grobner-horner (8195d4f77)
Job tag: eager-false-fstar-ulib-smt2
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 8195d4f771ca40f94dd4635c0ea6c9f296144a05
Z3 branch: nla-avoid-grobner-horner
Z3 options: "-T:20 model_validate=true smt.arith.solver=6 smt.arith.nl.optimize_bounds_eager=false"
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
|queries-LowStar.Endianness-1.smt2                            |    0.140s | 26.072MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Prims.smt2                                           |    0.158s | 23.428MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Heap.smt2                                      |    0.524s | 75.06MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Effect-1.smt2                          |    0.550s | 75.008MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Common.smt2                            |    0.561s | 49.62MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Classical-1.smt2                               |    0.647s | 74.536MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.Pure.smt2                            |    0.691s | 50.044MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.VConfig.smt2                                   |    0.692s | 49.66MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.TSet-1.smt2                                    |    0.692s | 74.812MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pervasives.Native.smt2                         |    0.697s | 46.092MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Sequence.Ambient.smt2                          |    0.715s | 74.708MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.PropositionalExtensionality.smt2               |    0.725s | 50.42MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Universe.smt2                                  |    0.738s | 49.916MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.All.smt2                                       |    0.743s | 75.264MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Types.smt2                          |    0.767s | 74.736MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Order.smt2                                     |    0.783s | 50.504MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.OrdSetProps.smt2                               |    0.788s | 74.776MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Squash.smt2                                    |    0.812s | 49.684MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Classical.smt2                                 |    0.824s | 74.732MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Relational.Relational.smt2                     |    0.829s | 75.26MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.OrdMapProps.smt2                               |    0.843s | 74.74MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Option.smt2                                    |    0.849s | 75.696MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.WellFounded.smt2                               |    0.858s | 75.028MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.TwoLevelHeap.smt2                              |    0.878s | 75.472MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Effect-2.smt2                          |    0.890s | 75.26MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.PredicateExtensionality.smt2                   |    0.893s | 74.76MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.FractionalPermission.smt2                      |    0.906s | 74.74MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Algebra.CommMonoid.smt2                        |    0.942s | 50.94MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Constructive.smt2                              |    0.950s | 50.684MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.PartialMap.smt2                                |    0.952s | 74.732MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.IFC.smt2                                       |    0.959s | 74.716MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Result.smt2                            |    0.970s | 75.028MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Ref.smt2                                       |    1.026s | 75.264MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.SquashProperties.smt2                          |    1.041s | 74.76MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.IndefiniteDescription.smt2                     |    1.044s | 74.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.TSet-2.smt2                                    |    1.058s | 74.752MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Algebra.CommMonoid.Equiv.smt2                  |    1.064s | 74.736MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Ghost.smt2                                     |    1.096s | 50.772MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Universe.PCM.smt2                              |    1.099s | 75.268MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.List.Pure.Base.smt2                            |    1.106s | 75.256MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.IntegerIntervals.smt2                          |    1.109s | 76.348MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Comment.smt2                                 |    1.125s | 81.708MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.MRef.smt2                                      |    1.140s | 75.28MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.MSTTotal.smt2                                  |    1.198s | 74.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Calc.smt2                                      |    1.206s | 74.612MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Char.smt2                                      |    1.212s | 79.384MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Util.smt2                                      |    1.212s | 79.988MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.MonotonicCounter.smt2                          |    1.242s | 77.132MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.NMSTTotal.smt2                                 |    1.260s | 74.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.SyntaxHelpers.smt2                     |    1.285s | 87.364MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Sequence.Util.smt2                             |    1.287s | 74.776MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.GSet.smt2                                      |    1.302s | 74.804MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.TSet.smt2                                      |    1.342s | 74.772MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.MST.smt2                                       |    1.364s | 74.724MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Set.smt2                                       |    1.367s | 74.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Derived.Lemmas.smt2                 |    1.380s | 85.064MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.Witnessed.smt2                       |    1.404s | 74.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.NMST.smt2                                      |    1.421s | 75.052MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ST.smt2                                        |    1.428s | 75.268MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.String.smt2                                    |    1.436s | 84.428MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int.Cast.Full.smt2                             |    1.443s | 93.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Matrix2.smt2                                   |    1.481s | 77.116MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Real.smt2                                      |    1.500s | 50.584MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Effect.smt2                            |    1.513s | 75.048MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Classical.Sugar.smt2                           |    1.530s | 74.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Permutation-2.smt2                         |    1.549s | 80.668MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Error.smt2                                     |    1.581s | 87.984MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Typeclasses.smt2                       |    1.587s | 94.744MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.BufferOps.smt2                               |    1.597s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Fin.smt2                                       |    1.609s | 77.632MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Util.smt2                              |    1.618s | 76.86MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Vector.Properties.smt2                         |    1.648s | 82.508MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Print.smt2                             |    1.673s | 90.188MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Sequence.Seq.smt2                              |    1.674s | 79.812MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Map.smt2                                       |    1.687s | 74.756MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.LockCoupling.smt2                              |    1.722s | 152.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Classical-2.smt2                               |    1.743s | 74.728MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.FunctionalExtensionality.smt2                  |    1.757s | 81.872MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Algebra.CommMonoid.Fold.Nested.smt2            |    1.761s | 96.772MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.Map.smt2                             |    1.798s | 85.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.PCMMap.smt2                                    |    1.824s | 75.888MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.FiniteSet.smt2                                 |    1.843s | 76.864MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tcp.smt2                                       |    1.869s | 138.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Regional.smt2                                |    1.917s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Crypto.smt2                                    |    1.920s | 138.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pervasives.smt2                                |    1.934s | 50.776MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.BufferCompat.smt2                            |    1.987s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonCommSwaps.smt2                    |    2.000s | 75.828MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.LexicographicOrdering.smt2                     |    2.009s | 75.26MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.CancellableSpinLock.smt2                    |    2.039s | 151.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.DependentMap.smt2                              |    2.055s | 74.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Simplifier.smt2                        |    2.065s | 92.708MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.PCM.smt2                                       |    2.073s | 74.792MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-6.smt2                             |    2.104s | 96.796MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Permutation.smt2                           |    2.129s | 79.896MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Channel.Protocol.smt2                          |    2.184s | 74.72MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.MonotonicReference.smt2                     |    2.194s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Sorted.smt2                                |    2.207s | 76.856MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.List.smt2                                      |    2.251s | 76.86MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Math.Lib.smt2                                  |    2.266s | 52.732MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.OrdMap.smt2                                    |    2.269s | 74.952MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Closure.smt2                                   |    2.270s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.GhostReference.smt2                         |    2.341s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.WellFoundedRelation.smt2                       |    2.367s | 76.056MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Loops.Util.smt2                             |    2.400s | 152.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Equiv.smt2                                 |    2.428s | 82.904MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Effect.M.smt2                                  |    2.474s | 139.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.InteractiveHelpers.Output.smt2                 |    2.511s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Channel.Duplex.smt2                            |    2.521s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Derived.smt2                        |    2.524s | 85.232MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.WellFounded.Util.smt2                          |    2.562s | 94.908MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Buffer.smt2                                  |    2.582s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Endianness-2.smt2                            |    2.585s | 137.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Semantics.Instantiate.smt2                     |    2.608s | 143.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Coercions.smt2                              |    2.653s | 165.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Algebra.CommMonoid.Fold.smt2                   |    2.727s | 84.06MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.DependentMap.smt2                    |    2.794s | 86.92MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int16.smt2                                     |    2.798s | 85.188MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int32.smt2                                     |    2.807s | 85.272MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Reference.smt2                              |    2.904s | 156.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int128.smt2                                    |    2.918s | 86.924MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.GhostPCMReference.smt2                         |    2.926s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.MonotonicReference.smt2                        |    2.966s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Utils.smt2                                     |    2.986s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Util.smt2                                   |    2.996s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int8.smt2                                      |    2.997s | 85.048MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Primitive.ForkJoin.smt2                        |    3.091s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.List.Tot.Base.smt2                             |    3.115s | 74.752MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Loops.smt2                                  |    3.212s | 155.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pointer.Derived3.smt2                          |    3.230s | 140.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Literal.smt2                                 |    3.266s | 145.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.SpinLock.smt2                               |    3.273s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Formula.smt2                        |    3.296s | 90.848MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.GhostPCMReference.smt2                      |    3.337s | 157.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonMonoid.smt2                       |    3.340s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ReflexiveTransitiveClosure.smt2                |    3.386s | 94.74MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int64.smt2                                     |    3.387s | 87.032MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pointer.Derived2.smt2                          |    3.389s | 140.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.DisposableInvariant.smt2                       |    3.392s | 157.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Monotonic.Buffer-2.smt2                      |    3.470s | 159.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.BitVector.smt2                              |    3.486s | 152.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Loops.smt2                                     |    3.512s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.HyperHeap.smt2                       |    3.576s | 80.052MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Logic.smt2                             |    3.608s | 93.204MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Vector.Base.smt2                               |    3.616s | 81.624MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.SpinLock.smt2                                  |    3.616s | 152.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.InteractiveHelpers.Base.smt2                   |    3.661s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Effect.Ghost.smt2                           |    3.670s | 167.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.List.Pure.Properties.smt2                      |    3.760s | 77.84MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Effect.Atomic.smt2                          |    3.774s | 165.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ConstantTime.Integers.smt2                     |    3.787s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.PCMReference.smt2                              |    3.803s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.UninitializedBuffer.smt2                     |    3.881s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-4.smt2                             |    3.911s | 99.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonCommMonoidSimple.smt2             |    3.960s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Sequence.Permutation.smt2                      |    3.993s | 80.996MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Preorder.smt2                                  |    4.022s | 87.512MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.InteractiveHelpers.ExploreTerm.smt2            |    4.093s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Primitive.ForkJoin.Unix.smt2                   |    4.122s | 155.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Array.smt2                                     |    4.222s | 87.66MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Effect.smt2                                 |    4.263s | 164.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Data.smt2                           |    4.264s | 75.408MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Printf.smt2                                    |    4.401s | 137.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.BitVector.smt2                                 |    4.403s | 77.884MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Effect.AtomicAndGhost.smt2                  |    4.418s | 168.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.ImmutableBuffer.smt2                         |    4.629s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.OrdSet.smt2                                    |    4.670s | 79.412MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Canon.smt2                             |    4.816s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonCommMonoid.smt2                   |    4.961s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonCommMonoidSimple.Equiv.smt2       |    4.968s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.PrefixFreezableBuffer.smt2                   |    5.055s | 138.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.BigOps.smt2                                    |    5.077s | 168.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Algebra.Monoid.smt2                            |    5.128s | 312.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.InteractiveHelpers.Effectful.smt2              |    5.129s | 138.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-2.smt2                             |    5.197s | 101.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Array.smt2                                  |    5.207s | 157.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.ConstBuffer.smt2                             |    5.267s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.InteractiveHelpers.PostProcess.smt2            |    5.276s | 154.0MiB| unsat | 0 |  |
|queries-LowStar.Monotonic.Buffer-1.smt2                      |    5.471s | 159.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Permutation-3.smt2                         |    5.593s | 90.392MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Arith.smt2                          |    5.690s | 161.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Bytes.smt2                                     |    5.694s | 137.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Math.Euclid.smt2                               |    5.751s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.Seq.smt2                             |    5.982s | 95.556MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Integers.smt2                                  |    6.003s | 161.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Regional.Instances.smt2                      |    6.013s | 150.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.ToFStarBuffer.smt2                           |    6.041s | 143.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.BV.smt2                                        |    6.115s | 84.724MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Endianness.smt2                              |    6.353s | 138.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Base.smt2                                  |    6.359s | 82.016MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Derived.smt2                           |    6.623s | 96.16MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Printf.smt2                                  |    6.799s | 159.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.BV.smt2                                |    6.906s | 140.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Array.Util.smt2                             |    7.300s | 157.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.HyperStack.smt2                      |    7.420s | 92.896MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.Heap.smt2                            |    7.462s | 92.484MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.UInt32.smt2                                    |    7.475s | 88.608MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.UInt16.smt2                                    |    7.513s | 91.476MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.PatternMatching.smt2                   |    7.748s | 140.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.UInt64.smt2                                    |    8.099s | 91.972MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.BufferView.smt2                              |    8.191s | 137.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.UInt8.smt2                                     |    8.329s | 90.844MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.TaggedUnion.smt2                               |    8.397s | 146.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.List.Tot.Properties.smt2                       |    8.460s | 85.58MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.BufferView.Up.smt2                           |    8.503s | 142.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Matrix.smt2                                    |    8.574s | 97.484MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Buffer.Quantifiers.smt2                        |    8.883s | 151.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.BufferNG.smt2                                  |    9.032s | 146.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.HyperStack.ST.smt2                             |    9.291s | 164.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pointer.Base.smt2                              |    9.344s | 139.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Channel.Simplex.smt2                           |    9.414s | 164.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Sequence.Base.smt2                             |    9.745s | 97.916MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonCommSemiring.smt2                 |   10.099s | 141.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pointer.Derived1.smt2                          |   10.626s | 145.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Permutation-1.smt2                         |   11.284s | 169.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Math.Lemmas.smt2                               |   11.501s | 74.752MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.BufferView.Down.smt2                         |   11.912s | 142.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Stepper.smt2                                   |   12.569s | 189.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Array.smt2                                     |   12.620s | 193.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Reference.smt2                                 |   13.070s | 158.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Memory.smt2                                    |   13.962s | 161.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.EphemeralHashtbl.smt2                       |   15.040s | 174.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-7.smt2                             |   15.059s | 181.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Modifies.smt2                                  |   15.318s | 145.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-3.smt2                             |   15.405s | 164.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Effect.smt2                                    |   15.514s | 187.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int.Cast.smt2                                  |   16.715s | 101.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Math.Euclid-1.smt2                             |   16.995s | 1690.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-1.smt2                             |   17.046s | 164.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.HigherReference.smt2                           |   17.485s | 210.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Properties.smt2                            |   17.579s | 159.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Math.Fermat.smt2                               |   18.823s | 151.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Heap-1.smt2                                    |   18.983s | 221.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.UInt.smt2                                      |   20.023s | 104.0MiB| timeout | 0 |  |
|queries-Steel.FractionalAnchoredPreorder.smt2                |   20.029s | 191.0MiB| timeout | 0 |  |
|queries-Steel.Heap.smt2                                      |   20.037s | 226.0MiB| timeout | 0 |  |
|queries-FStar.UInt128.smt2                                   |   20.040s | 130.0MiB| timeout | 0 |  |
|queries-Steel.MonotonicHigherReference.smt2                  |   20.047s | 213.0MiB| timeout | 0 |  |
|queries-Steel.Effect.Common.smt2                             |   20.052s | 163.0MiB| timeout | 0 |  |
|queries-FStar.Matrix-1.smt2                                  |   20.052s | 326.0MiB| timeout | 0 |  |
|queries-FStar.Buffer.smt2                                    |   20.053s | 162.0MiB| timeout | 0 |  |
|queries-FStar.ModifiesGen-5.smt2                             |   20.055s | 165.0MiB| timeout | 0 |  |
|queries-FStar.Endianness.smt2                                |   20.060s | 167.0MiB| timeout | 0 |  |
|queries-LowStar.Vector.smt2                                  |   20.061s | 162.0MiB| timeout | 0 |  |
|queries-Steel.Effect.Atomic.smt2                             |   20.064s | 186.0MiB| timeout | 0 |  |
|queries-FStar.ModifiesGen.smt2                               |   20.065s | 164.0MiB| timeout | 0 |  |
|queries-LowStar.RVector.smt2                                 |   20.067s | 164.0MiB| timeout | 0 |  |
|queries-Steel.Semantics.Hoare.MST.smt2                       |   20.086s | 176.0MiB| timeout | 0 |  |
|queries-FStar.Int.smt2                                       |   20.090s | 114.0MiB| timeout | 0 |  |
|queries-LowStar.Monotonic.Buffer.smt2                        |   20.095s | 164.0MiB| timeout | 0 |  |
