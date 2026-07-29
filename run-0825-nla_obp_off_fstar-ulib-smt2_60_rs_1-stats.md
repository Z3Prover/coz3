# .

* SAT 0
* UNSAT 1
* TIMEOUT 10
* UNKNOWN 0

* ERRORS 239 (error-1:239)

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: fstar-ulib-smt2, levnach/nla-optimize-bounds-in-propagate, optimize_bounds_in_propagate=false, -T:60, seed 1
Job tag: nla_obp_off_fstar-ulib-smt2_60_rs_1
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 7159c38d124cc13be8836e5a662ba95993c416cf
Z3 branch: levnach/nla-optimize-bounds-in-propagate
Z3 options: "-T:60 smt.random_seed=1 smt.arith.nl.optimize_bounds_in_propagate=false"
Z3 inputs: inputs/fstar-ulib-smt2
Z3 commit message: nla: optimize nonlinear variable bounds at the start of core::propagate

Maximize/minimize the variables occurring in monomials over the LP
tableau before interval propagation, mirroring max_min_nl_vars() of
smt.arith.solver=2, which does this on every nonlinear final check.

Without it the interval down-propagation in monomial_bounds only
ratchets towards the implied bound, improving it by a negligible amount
per round, and every intermediate bound re-triggers the integer solver.

The pass is guarded by the new option
arith.nl.optimize_bounds_in_propagate (default true) in addition to the
existing arith.nl.optimize_bounds and the
arith.nl.optimize_bounds_lp_max_vars throttle, and it still runs at most
once per scope. The horner.cpp call site is kept for the paths that
reach horner without going through core::propagate; its comment is
updated accordingly.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|queries-Prims.smt2                                           |    0.118s | 23.432MiB| error-1 | 1 |  |  |
|queries-LowStar.Endianness-1.smt2                            |    0.173s | 26.068MiB| error-1 | 1 |  |  |
|queries-FStar.Heap.smt2                                      |    0.517s | 75.268MiB| error-1 | 1 |  |  |
|queries-FStar.Pervasives.Native.smt2                         |    0.562s | 46.124MiB| error-1 | 1 |  |  |
|queries-FStar.TSet-1.smt2                                    |    0.567s | 74.752MiB| error-1 | 1 |  |  |
|queries-FStar.VConfig.smt2                                   |    0.625s | 49.624MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Common.smt2                            |    0.632s | 49.66MiB| error-1 | 1 |  |  |
|queries-FStar.All.smt2                                       |    0.669s | 75.244MiB| error-1 | 1 |  |  |
|queries-FStar.Monotonic.Pure.smt2                            |    0.692s | 50.076MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Effect-1.smt2                          |    0.694s | 75.004MiB| error-1 | 1 |  |  |
|queries-FStar.Sequence.Ambient.smt2                          |    0.711s | 74.752MiB| error-1 | 1 |  |  |
|queries-FStar.OrdMapProps.smt2                               |    0.712s | 74.824MiB| error-1 | 1 |  |  |
|queries-FStar.PropositionalExtensionality.smt2               |    0.751s | 50.188MiB| error-1 | 1 |  |  |
|queries-FStar.Universe.smt2                                  |    0.761s | 49.856MiB| error-1 | 1 |  |  |
|queries-FStar.Classical-1.smt2                               |    0.769s | 74.492MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Types.smt2                          |    0.808s | 74.748MiB| error-1 | 1 |  |  |
|queries-FStar.IFC.smt2                                       |    0.822s | 74.74MiB| error-1 | 1 |  |  |
|queries-FStar.Classical.smt2                                 |    0.824s | 74.492MiB| error-1 | 1 |  |  |
|queries-FStar.Order.smt2                                     |    0.842s | 50.484MiB| error-1 | 1 |  |  |
|queries-FStar.Algebra.CommMonoid.smt2                        |    0.853s | 50.94MiB| error-1 | 1 |  |  |
|queries-FStar.OrdSetProps.smt2                               |    0.855s | 74.824MiB| error-1 | 1 |  |  |
|queries-FStar.Squash.smt2                                    |    0.859s | 49.436MiB| error-1 | 1 |  |  |
|queries-FStar.Option.smt2                                    |    0.864s | 75.508MiB| error-1 | 1 |  |  |
|queries-FStar.Relational.Relational.smt2                     |    0.869s | 75.08MiB| error-1 | 1 |  |  |
|queries-FStar.PredicateExtensionality.smt2                   |    0.878s | 74.748MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Effect-2.smt2                          |    0.888s | 75.264MiB| error-1 | 1 |  |  |
|queries-FStar.TwoLevelHeap.smt2                              |    0.910s | 75.256MiB| error-1 | 1 |  |  |
|queries-Steel.FractionalPermission.smt2                      |    0.970s | 74.768MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Result.smt2                            |    0.976s | 74.856MiB| error-1 | 1 |  |  |
|queries-FStar.SquashProperties.smt2                          |    0.989s | 74.88MiB| error-1 | 1 |  |  |
|queries-FStar.Constructive.smt2                              |    1.008s | 50.648MiB| error-1 | 1 |  |  |
|queries-FStar.Ref.smt2                                       |    1.023s | 75.264MiB| error-1 | 1 |  |  |
|queries-FStar.IndefiniteDescription.smt2                     |    1.028s | 74.792MiB| error-1 | 1 |  |  |
|queries-FStar.Ghost.smt2                                     |    1.049s | 50.676MiB| error-1 | 1 |  |  |
|queries-FStar.List.Pure.Base.smt2                            |    1.057s | 75.264MiB| error-1 | 1 |  |  |
|queries-FStar.WellFounded.smt2                               |    1.075s | 75.056MiB| error-1 | 1 |  |  |
|queries-FStar.Universe.PCM.smt2                              |    1.099s | 75.26MiB| error-1 | 1 |  |  |
|queries-LowStar.Comment.smt2                                 |    1.105s | 81.688MiB| error-1 | 1 |  |  |
|queries-FStar.MSTTotal.smt2                                  |    1.108s | 74.488MiB| error-1 | 1 |  |  |
|queries-FStar.MRef.smt2                                      |    1.108s | 75.264MiB| error-1 | 1 |  |  |
|queries-FStar.TSet-2.smt2                                    |    1.148s | 74.752MiB| error-1 | 1 |  |  |
|queries-FStar.Algebra.CommMonoid.Equiv.smt2                  |    1.163s | 74.528MiB| error-1 | 1 |  |  |
|queries-FStar.PartialMap.smt2                                |    1.189s | 74.584MiB| error-1 | 1 |  |  |
|queries-FStar.Calc.smt2                                      |    1.206s | 74.748MiB| error-1 | 1 |  |  |
|queries-FStar.MST.smt2                                       |    1.221s | 74.776MiB| error-1 | 1 |  |  |
|queries-FStar.Real.smt2                                      |    1.242s | 50.724MiB| error-1 | 1 |  |  |
|queries-FStar.Char.smt2                                      |    1.252s | 79.38MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.SyntaxHelpers.smt2                     |    1.274s | 87.548MiB| error-1 | 1 |  |  |
|queries-FStar.IntegerIntervals.smt2                          |    1.287s | 76.252MiB| error-1 | 1 |  |  |
|queries-FStar.NMSTTotal.smt2                                 |    1.295s | 74.696MiB| error-1 | 1 |  |  |
|queries-FStar.ST.smt2                                        |    1.331s | 75.044MiB| error-1 | 1 |  |  |
|queries-Steel.MonotonicCounter.smt2                          |    1.340s | 77.104MiB| error-1 | 1 |  |  |
|queries-FStar.GSet.smt2                                      |    1.358s | 74.748MiB| error-1 | 1 |  |  |
|queries-FStar.Seq.Permutation-2.smt2                         |    1.365s | 79.764MiB| error-1 | 1 |  |  |
|queries-FStar.Sequence.Util.smt2                             |    1.369s | 74.852MiB| error-1 | 1 |  |  |
|queries-FStar.Util.smt2                                      |    1.373s | 80.2MiB| error-1 | 1 |  |  |
|queries-FStar.Algebra.CommMonoid.Fold.Nested.smt2            |    1.383s | 96.28MiB| error-1 | 1 |  |  |
|queries-FStar.TSet.smt2                                      |    1.390s | 74.768MiB| error-1 | 1 |  |  |
|queries-FStar.Classical.Sugar.smt2                           |    1.400s | 74.496MiB| error-1 | 1 |  |  |
|queries-FStar.Set.smt2                                       |    1.412s | 74.752MiB| error-1 | 1 |  |  |
|queries-FStar.String.smt2                                    |    1.416s | 84.348MiB| error-1 | 1 |  |  |
|queries-FStar.Error.smt2                                     |    1.419s | 87.932MiB| error-1 | 1 |  |  |
|queries-FStar.NMST.smt2                                      |    1.424s | 75.012MiB| error-1 | 1 |  |  |
|queries-FStar.Int.Cast.Full.smt2                             |    1.473s | 92.94MiB| error-1 | 1 |  |  |
|queries-FStar.Vector.Properties.smt2                         |    1.481s | 82.372MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Effect.smt2                            |    1.482s | 75.264MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.CanonCommSwaps.smt2                    |    1.510s | 75.828MiB| error-1 | 1 |  |  |
|queries-FStar.Sequence.Seq.smt2                              |    1.554s | 79.696MiB| error-1 | 1 |  |  |
|queries-FStar.Fin.smt2                                       |    1.630s | 77.508MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Util.smt2                              |    1.654s | 76.604MiB| error-1 | 1 |  |  |
|queries-FStar.Monotonic.Witnessed.smt2                       |    1.657s | 74.496MiB| error-1 | 1 |  |  |
|queries-FStar.DependentMap.smt2                              |    1.692s | 74.536MiB| error-1 | 1 |  |  |
|queries-FStar.Map.smt2                                       |    1.693s | 74.82MiB| error-1 | 1 |  |  |
|queries-FStar.Matrix2.smt2                                   |    1.693s | 76.972MiB| error-1 | 1 |  |  |
|queries-FStar.Monotonic.Map.smt2                             |    1.703s | 85.148MiB| error-1 | 1 |  |  |
|queries-FStar.Classical-2.smt2                               |    1.706s | 74.492MiB| error-1 | 1 |  |  |
|queries-FStar.FiniteSet.smt2                                 |    1.708s | 76.856MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Derived.Lemmas.smt2                 |    1.745s | 85.076MiB| error-1 | 1 |  |  |
|queries-LowStar.BufferOps.smt2                               |    1.792s | 136.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Typeclasses.smt2                       |    1.804s | 94.872MiB| error-1 | 1 |  |  |
|queries-LowStar.Regional.smt2                                |    1.804s | 136.0MiB| error-1 | 1 |  |  |
|queries-FStar.WellFoundedRelation.smt2                       |    1.836s | 76.056MiB| error-1 | 1 |  |  |
|queries-FStar.FunctionalExtensionality.smt2                  |    1.841s | 81.944MiB| error-1 | 1 |  |  |
|queries-FStar.Pervasives.smt2                                |    1.867s | 50.836MiB| error-1 | 1 |  |  |
|queries-Steel.LockCoupling.smt2                              |    1.938s | 153.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-6.smt2                             |    1.943s | 96.788MiB| error-1 | 1 |  |  |
|queries-FStar.Algebra.Monoid.smt2                            |    1.980s | 74.596MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Print.smt2                             |    1.983s | 90.18MiB| error-1 | 1 |  |  |
|queries-FStar.Tcp.smt2                                       |    2.007s | 138.0MiB| error-1 | 1 |  |  |
|queries-FStar.Crypto.smt2                                    |    2.047s | 138.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Simplifier.smt2                        |    2.073s | 92.964MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Lib.smt2                                  |    2.141s | 52.524MiB| error-1 | 1 |  |  |
|queries-LowStar.BufferCompat.smt2                            |    2.168s | 136.0MiB| error-1 | 1 |  |  |
|queries-Steel.ST.CancellableSpinLock.smt2                    |    2.174s | 151.0MiB| error-1 | 1 |  |  |
|queries-FStar.List.smt2                                      |    2.186s | 76.624MiB| error-1 | 1 |  |  |
|queries-FStar.PCM.smt2                                       |    2.215s | 74.796MiB| error-1 | 1 |  |  |
|queries-Steel.PCMMap.smt2                                    |    2.235s | 75.768MiB| error-1 | 1 |  |  |
|queries-FStar.Seq.Permutation.smt2                           |    2.250s | 79.336MiB| error-1 | 1 |  |  |
|queries-LowStar.Endianness-2.smt2                            |    2.286s | 138.0MiB| error-1 | 1 |  |  |
|queries-FStar.Seq.Sorted.smt2                                |    2.290s | 76.856MiB| error-1 | 1 |  |  |
|queries-Steel.ST.MonotonicReference.smt2                     |    2.304s | 153.0MiB| error-1 | 1 |  |  |
|queries-Steel.Effect.M.smt2                                  |    2.322s | 139.0MiB| error-1 | 1 |  |  |
|queries-FStar.InteractiveHelpers.Output.smt2                 |    2.396s | 136.0MiB| error-1 | 1 |  |  |
|queries-Steel.Closure.smt2                                   |    2.458s | 153.0MiB| error-1 | 1 |  |  |
|queries-Steel.ST.Loops.Util.smt2                             |    2.484s | 152.0MiB| error-1 | 1 |  |  |
|queries-Steel.Channel.Protocol.smt2                          |    2.485s | 74.548MiB| error-1 | 1 |  |  |
|queries-Steel.ST.Util.smt2                                   |    2.488s | 155.0MiB| error-1 | 1 |  |  |
|queries-FStar.LexicographicOrdering.smt2                     |    2.498s | 75.26MiB| error-1 | 1 |  |  |
|queries-Steel.ST.GhostReference.smt2                         |    2.529s | 154.0MiB| error-1 | 1 |  |  |
|queries-FStar.OrdMap.smt2                                    |    2.611s | 74.836MiB| error-1 | 1 |  |  |
|queries-FStar.WellFounded.Util.smt2                          |    2.679s | 95.024MiB| error-1 | 1 |  |  |
|queries-Steel.ST.Reference.smt2                              |    2.707s | 156.0MiB| error-1 | 1 |  |  |
|queries-Steel.ST.Coercions.smt2                              |    2.740s | 165.0MiB| error-1 | 1 |  |  |
|queries-FStar.Seq.Equiv.smt2                                 |    2.742s | 80.952MiB| error-1 | 1 |  |  |
|queries-Steel.MonotonicReference.smt2                        |    2.752s | 153.0MiB| error-1 | 1 |  |  |
|queries-LowStar.Buffer.smt2                                  |    2.754s | 136.0MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Derived.smt2                        |    2.767s | 85.592MiB| error-1 | 1 |  |  |
|queries-Steel.Semantics.Instantiate.smt2                     |    2.775s | 143.0MiB| error-1 | 1 |  |  |
|queries-FStar.Monotonic.DependentMap.smt2                    |    2.779s | 86.952MiB| error-1 | 1 |  |  |
|queries-FStar.Algebra.CommMonoid.Fold.smt2                   |    2.803s | 84.112MiB| error-1 | 1 |  |  |
|queries-FStar.Int32.smt2                                     |    2.820s | 84.476MiB| error-1 | 1 |  |  |
|queries-FStar.Int128.smt2                                    |    2.835s | 86.688MiB| error-1 | 1 |  |  |
|queries-FStar.List.Pure.Properties.smt2                      |    2.881s | 77.952MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.CanonMonoid.smt2                       |    2.883s | 136.0MiB| error-1 | 1 |  |  |
|queries-FStar.Int8.smt2                                      |    2.897s | 84.368MiB| error-1 | 1 |  |  |
|queries-Steel.Channel.Duplex.smt2                            |    2.917s | 153.0MiB| error-1 | 1 |  |  |
|queries-Steel.ST.Loops.smt2                                  |    2.980s | 155.0MiB| error-1 | 1 |  |  |
|queries-Steel.Primitive.ForkJoin.smt2                        |    2.988s | 154.0MiB| error-1 | 1 |  |  |
|queries-Steel.GhostPCMReference.smt2                         |    3.093s | 153.0MiB| error-1 | 1 |  |  |
|queries-Steel.ST.Effect.Atomic.smt2                          |    3.149s | 165.0MiB| error-1 | 1 |  |  |
|queries-FStar.Int16.smt2                                     |    3.149s | 86.476MiB| error-1 | 1 |  |  |
|queries-Steel.ST.SpinLock.smt2                               |    3.187s | 154.0MiB| error-1 | 1 |  |  |
|queries-Steel.ST.GhostPCMReference.smt2                      |    3.218s | 157.0MiB| error-1 | 1 |  |  |
|queries-Steel.Loops.smt2                                     |    3.231s | 154.0MiB| error-1 | 1 |  |  |
|queries-Steel.Utils.smt2                                     |    3.273s | 154.0MiB| error-1 | 1 |  |  |
|queries-FStar.Vector.Base.smt2                               |    3.322s | 81.548MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Logic.smt2                             |    3.360s | 93.032MiB| error-1 | 1 |  |  |
|queries-FStar.Int64.smt2                                     |    3.374s | 83.896MiB| error-1 | 1 |  |  |
|queries-FStar.ReflexiveTransitiveClosure.smt2                |    3.396s | 94.784MiB| error-1 | 1 |  |  |
|queries-LowStar.Monotonic.Buffer-2.smt2                      |    3.404s | 159.0MiB| error-1 | 1 |  |  |
|queries-Steel.ST.BitVector.smt2                              |    3.485s | 153.0MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Formula.smt2                        |    3.492s | 90.648MiB| error-1 | 1 |  |  |
|queries-FStar.Monotonic.HyperHeap.smt2                       |    3.511s | 80.172MiB| error-1 | 1 |  |  |
|queries-LowStar.Literal.smt2                                 |    3.515s | 145.0MiB| error-1 | 1 |  |  |
|queries-FStar.Pointer.Derived3.smt2                          |    3.522s | 140.0MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Euclid-1.smt2                             |    3.567s | 136.0MiB| error-1 | 1 |  |  |
|queries-FStar.Pointer.Derived2.smt2                          |    3.593s | 140.0MiB| error-1 | 1 |  |  |
|queries-Steel.ST.Effect.Ghost.smt2                           |    3.616s | 167.0MiB| error-1 | 1 |  |  |
|queries-FStar.Array.smt2                                     |    3.620s | 84.54MiB| error-1 | 1 |  |  |
|queries-Steel.DisposableInvariant.smt2                       |    3.651s | 156.0MiB| error-1 | 1 |  |  |
|queries-FStar.InteractiveHelpers.Base.smt2                   |    3.675s | 136.0MiB| error-1 | 1 |  |  |
|queries-Steel.SpinLock.smt2                                  |    3.733s | 152.0MiB| error-1 | 1 |  |  |
|queries-Steel.Primitive.ForkJoin.Unix.smt2                   |    3.813s | 155.0MiB| error-1 | 1 |  |  |
|queries-FStar.OrdSet.smt2                                    |    3.898s | 79.328MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-4.smt2                             |    3.977s | 99.0MiB| error-1 | 1 |  |  |
|queries-Steel.PCMReference.smt2                              |    4.080s | 153.0MiB| error-1 | 1 |  |  |
|queries-LowStar.UninitializedBuffer.smt2                     |    4.104s | 136.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.CanonCommMonoidSimple.smt2             |    4.128s | 136.0MiB| error-1 | 1 |  |  |
|queries-Steel.Preorder.smt2                                  |    4.139s | 87.332MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Data.smt2                           |    4.229s | 75.412MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Canon.smt2                             |    4.278s | 136.0MiB| error-1 | 1 |  |  |
|queries-FStar.ConstantTime.Integers.smt2                     |    4.361s | 136.0MiB| error-1 | 1 |  |  |
|queries-FStar.List.Tot.Base.smt2                             |    4.377s | 74.492MiB| error-1 | 1 |  |  |
|queries-FStar.Printf.smt2                                    |    4.407s | 137.0MiB| error-1 | 1 |  |  |
|queries-FStar.InteractiveHelpers.Effectful.smt2              |    4.407s | 138.0MiB| error-1 | 1 |  |  |
|queries-LowStar.ConstBuffer.smt2                             |    4.438s | 136.0MiB| error-1 | 1 |  |  |
|queries-FStar.Sequence.Permutation.smt2                      |    4.467s | 82.32MiB| error-1 | 1 |  |  |
|queries-FStar.BigOps.smt2                                    |    4.481s | 168.0MiB| error-1 | 1 |  |  |
|queries-FStar.InteractiveHelpers.PostProcess.smt2            |    4.577s | 155.0MiB| unsat | 0 |  |  |
|queries-FStar.BitVector.smt2                                 |    4.588s | 78.056MiB| error-1 | 1 |  |  |
|queries-LowStar.ImmutableBuffer.smt2                         |    4.648s | 136.0MiB| error-1 | 1 |  |  |
|queries-FStar.InteractiveHelpers.ExploreTerm.smt2            |    4.704s | 136.0MiB| error-1 | 1 |  |  |
|queries-Steel.ST.Effect.AtomicAndGhost.smt2                  |    4.708s | 168.0MiB| error-1 | 1 |  |  |
|queries-LowStar.PrefixFreezableBuffer.smt2                   |    4.747s | 138.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.CanonCommMonoid.smt2                   |    4.814s | 136.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.CanonCommMonoidSimple.Equiv.smt2       |    4.832s | 136.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-2.smt2                             |    4.873s | 99.0MiB| error-1 | 1 |  |  |
|queries-Steel.ST.Effect.smt2                                 |    5.134s | 164.0MiB| error-1 | 1 |  |  |
|queries-Steel.ST.Array.smt2                                  |    5.317s | 157.0MiB| error-1 | 1 |  |  |
|queries-FStar.Monotonic.Seq.smt2                             |    5.345s | 92.948MiB| error-1 | 1 |  |  |
|queries-FStar.BV.smt2                                        |    5.725s | 84.396MiB| error-1 | 1 |  |  |
|queries-FStar.Seq.Permutation-3.smt2                         |    5.916s | 94.188MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Arith.smt2                          |    5.931s | 162.0MiB| error-1 | 1 |  |  |
|queries-FStar.Bytes.smt2                                     |    5.964s | 137.0MiB| error-1 | 1 |  |  |
|queries-LowStar.Regional.Instances.smt2                      |    5.993s | 151.0MiB| error-1 | 1 |  |  |
|queries-FStar.Seq.Base.smt2                                  |    6.188s | 80.924MiB| error-1 | 1 |  |  |
|queries-FStar.Integers.smt2                                  |    6.199s | 161.0MiB| error-1 | 1 |  |  |
|queries-LowStar.Endianness.smt2                              |    6.326s | 138.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Derived.smt2                           |    6.537s | 95.96MiB| error-1 | 1 |  |  |
|queries-LowStar.Printf.smt2                                  |    6.588s | 159.0MiB| error-1 | 1 |  |  |
|queries-LowStar.ToFStarBuffer.smt2                           |    6.653s | 142.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.BV.smt2                                |    6.683s | 140.0MiB| error-1 | 1 |  |  |
|queries-LowStar.Monotonic.Buffer-1.smt2                      |    7.444s | 159.0MiB| error-1 | 1 |  |  |
|queries-FStar.Monotonic.HyperStack.smt2                      |    7.711s | 93.08MiB| error-1 | 1 |  |  |
|queries-FStar.UInt32.smt2                                    |    7.721s | 89.104MiB| error-1 | 1 |  |  |
|queries-FStar.UInt8.smt2                                     |    7.843s | 91.048MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.PatternMatching.smt2                   |    7.899s | 140.0MiB| error-1 | 1 |  |  |
|queries-Steel.ST.Array.Util.smt2                             |    7.915s | 157.0MiB| error-1 | 1 |  |  |
|queries-Steel.Channel.Simplex.smt2                           |    8.009s | 164.0MiB| error-1 | 1 |  |  |
|queries-FStar.UInt16.smt2                                    |    8.040s | 90.964MiB| error-1 | 1 |  |  |
|queries-LowStar.BufferView.smt2                              |    8.208s | 139.0MiB| error-1 | 1 |  |  |
|queries-FStar.Monotonic.Heap.smt2                            |    8.311s | 91.72MiB| error-1 | 1 |  |  |
|queries-LowStar.BufferView.Up.smt2                           |    8.325s | 137.0MiB| error-1 | 1 |  |  |
|queries-FStar.List.Tot.Properties.smt2                       |    8.347s | 84.936MiB| error-1 | 1 |  |  |
|queries-FStar.Buffer.Quantifiers.smt2                        |    8.424s | 150.0MiB| error-1 | 1 |  |  |
|queries-FStar.HyperStack.ST.smt2                             |    8.889s | 164.0MiB| error-1 | 1 |  |  |
|queries-FStar.TaggedUnion.smt2                               |    8.904s | 148.0MiB| error-1 | 1 |  |  |
|queries-FStar.UInt64.smt2                                    |    8.945s | 93.232MiB| error-1 | 1 |  |  |
|queries-FStar.BufferNG.smt2                                  |    9.015s | 145.0MiB| error-1 | 1 |  |  |
|queries-FStar.Pointer.Base.smt2                              |    9.245s | 139.0MiB| error-1 | 1 |  |  |
|queries-FStar.Sequence.Base.smt2                             |    9.567s | 93.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.CanonCommSemiring.smt2                 |   10.318s | 141.0MiB| error-1 | 1 |  |  |
|queries-FStar.Pointer.Derived1.smt2                          |   10.357s | 144.0MiB| error-1 | 1 |  |  |
|queries-FStar.Seq.Permutation-1.smt2                         |   10.434s | 164.0MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Lemmas.smt2                               |   10.687s | 74.748MiB| error-1 | 1 |  |  |
|queries-FStar.Matrix.smt2                                    |   11.466s | 105.0MiB| error-1 | 1 |  |  |
|queries-Steel.Array.smt2                                     |   11.998s | 189.0MiB| error-1 | 1 |  |  |
|queries-LowStar.BufferView.Down.smt2                         |   12.630s | 144.0MiB| error-1 | 1 |  |  |
|queries-Steel.Stepper.smt2                                   |   13.150s | 189.0MiB| error-1 | 1 |  |  |
|queries-Steel.Reference.smt2                                 |   13.403s | 158.0MiB| error-1 | 1 |  |  |
|queries-Steel.Memory.smt2                                    |   13.491s | 162.0MiB| error-1 | 1 |  |  |
|queries-FStar.Modifies.smt2                                  |   15.000s | 144.0MiB| error-1 | 1 |  |  |
|queries-Steel.Effect.smt2                                    |   15.552s | 187.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-1.smt2                             |   15.668s | 166.0MiB| error-1 | 1 |  |  |
|queries-Steel.ST.EphemeralHashtbl.smt2                       |   15.746s | 178.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-3.smt2                             |   15.791s | 164.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-7.smt2                             |   15.814s | 181.0MiB| error-1 | 1 |  |  |
|queries-Steel.HigherReference.smt2                           |   16.511s | 210.0MiB| error-1 | 1 |  |  |
|queries-FStar.Seq.Properties.smt2                            |   17.427s | 160.0MiB| error-1 | 1 |  |  |
|queries-FStar.Int.Cast.smt2                                  |   17.683s | 104.0MiB| error-1 | 1 |  |  |
|queries-Steel.Heap-1.smt2                                    |   17.817s | 251.0MiB| error-1 | 1 |  |  |
|queries-FStar.Int.smt2                                       |   18.507s | 108.0MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Fermat.smt2                               |   19.215s | 152.0MiB| error-1 | 1 |  |  |
|queries-Steel.FractionalAnchoredPreorder.smt2                |   20.479s | 183.0MiB| error-1 | 1 |  |  |
|queries-Steel.MonotonicHigherReference.smt2                  |   20.727s | 210.0MiB| error-1 | 1 |  |  |
|queries-FStar.Endianness.smt2                                |   23.840s | 168.0MiB| error-1 | 1 |  |  |
|queries-LowStar.Vector.smt2                                  |   25.210s | 163.0MiB| error-1 | 1 |  |  |
|queries-Steel.Effect.Common.smt2                             |   27.988s | 170.0MiB| error-1 | 1 |  |  |
|queries-Steel.Effect.Atomic.smt2                             |   33.153s | 322.0MiB| error-1 | 1 |  |  |
|queries-FStar.Matrix-1.smt2                                  |   39.730s | 1678.0MiB| error-1 | 1 |  |  |
|queries-FStar.Buffer.smt2                                    |   60.032s | 202.0MiB| timeout | 0 |  |  |
|queries-FStar.UInt.smt2                                      |   60.035s | 191.0MiB| timeout | 0 |  |  |
|queries-Steel.Semantics.Hoare.MST.smt2                       |   60.060s | 177.0MiB| timeout | 0 |  |  |
|queries-FStar.ModifiesGen-5.smt2                             |   60.072s | 224.0MiB| timeout | 0 |  |  |
|queries-Steel.Heap.smt2                                      |   60.086s | 414.0MiB| timeout | 0 |  |  |
|queries-LowStar.Monotonic.Buffer.smt2                        |   60.093s | 325.0MiB| timeout | 0 |  |  |
|queries-LowStar.RVector.smt2                                 |   60.096s | 328.0MiB| timeout | 0 |  |  |
|queries-FStar.ModifiesGen.smt2                               |   60.098s | 329.0MiB| timeout | 0 |  |  |
|queries-FStar.UInt128.smt2                                   |   60.112s | 351.0MiB| timeout | 0 |  |  |
|queries-FStar.Math.Euclid.smt2                               |   60.167s | 971.0MiB| timeout | 0 |  |  |
