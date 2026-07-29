# .

* SAT 0
* UNSAT 1
* TIMEOUT 8
* UNKNOWN 0

* ERRORS 241 (error-1:241)

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: fstar-ulib-smt2, levnach/nla-optimize-bounds-in-propagate, optimize_bounds_in_propagate=true, -T:60, seed 1
Job tag: nla_obp_on_fstar-ulib-smt2_60_rs_1
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 7159c38d124cc13be8836e5a662ba95993c416cf
Z3 branch: levnach/nla-optimize-bounds-in-propagate
Z3 options: "-T:60 smt.random_seed=1 smt.arith.nl.optimize_bounds_in_propagate=true"
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
|queries-LowStar.Endianness-1.smt2                            |    0.176s | 26.14MiB| error-1 | 1 |  |  |
|queries-Prims.smt2                                           |    0.189s | 23.432MiB| error-1 | 1 |  |  |
|queries-FStar.Pervasives.Native.smt2                         |    0.440s | 46.092MiB| error-1 | 1 |  |  |
|queries-FStar.VConfig.smt2                                   |    0.473s | 49.616MiB| error-1 | 1 |  |  |
|queries-FStar.Classical-1.smt2                               |    0.507s | 74.624MiB| error-1 | 1 |  |  |
|queries-FStar.Heap.smt2                                      |    0.537s | 75.024MiB| error-1 | 1 |  |  |
|queries-FStar.Monotonic.Pure.smt2                            |    0.541s | 50.164MiB| error-1 | 1 |  |  |
|queries-FStar.TSet-1.smt2                                    |    0.584s | 74.744MiB| error-1 | 1 |  |  |
|queries-FStar.IFC.smt2                                       |    0.664s | 74.752MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Types.smt2                          |    0.672s | 74.744MiB| error-1 | 1 |  |  |
|queries-FStar.PropositionalExtensionality.smt2               |    0.690s | 50.3MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Effect-1.smt2                          |    0.694s | 75.028MiB| error-1 | 1 |  |  |
|queries-FStar.Sequence.Ambient.smt2                          |    0.703s | 74.748MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Common.smt2                            |    0.715s | 49.664MiB| error-1 | 1 |  |  |
|queries-FStar.PredicateExtensionality.smt2                   |    0.734s | 74.748MiB| error-1 | 1 |  |  |
|queries-FStar.All.smt2                                       |    0.737s | 75.208MiB| error-1 | 1 |  |  |
|queries-FStar.Universe.smt2                                  |    0.754s | 49.788MiB| error-1 | 1 |  |  |
|queries-FStar.IndefiniteDescription.smt2                     |    0.783s | 74.744MiB| error-1 | 1 |  |  |
|queries-FStar.Classical.smt2                                 |    0.784s | 74.54MiB| error-1 | 1 |  |  |
|queries-FStar.TwoLevelHeap.smt2                              |    0.795s | 75.26MiB| error-1 | 1 |  |  |
|queries-FStar.Algebra.CommMonoid.smt2                        |    0.839s | 50.94MiB| error-1 | 1 |  |  |
|queries-FStar.OrdMapProps.smt2                               |    0.849s | 74.744MiB| error-1 | 1 |  |  |
|queries-FStar.Squash.smt2                                    |    0.863s | 49.66MiB| error-1 | 1 |  |  |
|queries-FStar.Order.smt2                                     |    0.869s | 50.288MiB| error-1 | 1 |  |  |
|queries-FStar.Relational.Relational.smt2                     |    0.879s | 75.012MiB| error-1 | 1 |  |  |
|queries-Steel.FractionalPermission.smt2                      |    0.882s | 74.768MiB| error-1 | 1 |  |  |
|queries-FStar.OrdSetProps.smt2                               |    0.891s | 74.752MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Effect-2.smt2                          |    0.893s | 75.256MiB| error-1 | 1 |  |  |
|queries-FStar.Option.smt2                                    |    0.905s | 75.56MiB| error-1 | 1 |  |  |
|queries-FStar.SquashProperties.smt2                          |    0.907s | 74.744MiB| error-1 | 1 |  |  |
|queries-FStar.WellFounded.smt2                               |    0.924s | 75.008MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Result.smt2                            |    0.945s | 74.84MiB| error-1 | 1 |  |  |
|queries-FStar.TSet.smt2                                      |    0.971s | 74.628MiB| error-1 | 1 |  |  |
|queries-FStar.Ref.smt2                                       |    0.973s | 75.312MiB| error-1 | 1 |  |  |
|queries-FStar.Constructive.smt2                              |    0.976s | 50.7MiB| error-1 | 1 |  |  |
|queries-FStar.Algebra.CommMonoid.Equiv.smt2                  |    1.037s | 74.756MiB| error-1 | 1 |  |  |
|queries-FStar.List.Pure.Base.smt2                            |    1.048s | 75.24MiB| error-1 | 1 |  |  |
|queries-FStar.TSet-2.smt2                                    |    1.048s | 74.756MiB| error-1 | 1 |  |  |
|queries-FStar.Sequence.Util.smt2                             |    1.083s | 74.692MiB| error-1 | 1 |  |  |
|queries-FStar.Universe.PCM.smt2                              |    1.085s | 75.268MiB| error-1 | 1 |  |  |
|queries-FStar.Ghost.smt2                                     |    1.088s | 50.684MiB| error-1 | 1 |  |  |
|queries-FStar.MRef.smt2                                      |    1.139s | 75.264MiB| error-1 | 1 |  |  |
|queries-Steel.MonotonicCounter.smt2                          |    1.172s | 77.08MiB| error-1 | 1 |  |  |
|queries-FStar.Seq.Permutation-2.smt2                         |    1.176s | 79.888MiB| error-1 | 1 |  |  |
|queries-LowStar.Comment.smt2                                 |    1.184s | 81.716MiB| error-1 | 1 |  |  |
|queries-FStar.Calc.smt2                                      |    1.222s | 74.7MiB| error-1 | 1 |  |  |
|queries-FStar.GSet.smt2                                      |    1.225s | 74.656MiB| error-1 | 1 |  |  |
|queries-FStar.PartialMap.smt2                                |    1.230s | 74.668MiB| error-1 | 1 |  |  |
|queries-FStar.MSTTotal.smt2                                  |    1.263s | 74.596MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Effect.smt2                            |    1.275s | 75.264MiB| error-1 | 1 |  |  |
|queries-FStar.Int.Cast.Full.smt2                             |    1.280s | 92.812MiB| error-1 | 1 |  |  |
|queries-FStar.Monotonic.Witnessed.smt2                       |    1.286s | 74.588MiB| error-1 | 1 |  |  |
|queries-FStar.Char.smt2                                      |    1.296s | 79.436MiB| error-1 | 1 |  |  |
|queries-FStar.NMSTTotal.smt2                                 |    1.308s | 74.7MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.SyntaxHelpers.smt2                     |    1.316s | 87.584MiB| error-1 | 1 |  |  |
|queries-FStar.NMST.smt2                                      |    1.338s | 75.004MiB| error-1 | 1 |  |  |
|queries-FStar.Util.smt2                                      |    1.340s | 80.116MiB| error-1 | 1 |  |  |
|queries-FStar.MST.smt2                                       |    1.390s | 74.788MiB| error-1 | 1 |  |  |
|queries-FStar.Set.smt2                                       |    1.392s | 74.748MiB| error-1 | 1 |  |  |
|queries-FStar.Matrix2.smt2                                   |    1.411s | 76.852MiB| error-1 | 1 |  |  |
|queries-FStar.ST.smt2                                        |    1.413s | 75.024MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Util.smt2                              |    1.429s | 76.604MiB| error-1 | 1 |  |  |
|queries-FStar.IntegerIntervals.smt2                          |    1.437s | 76.084MiB| error-1 | 1 |  |  |
|queries-FStar.Error.smt2                                     |    1.452s | 87.832MiB| error-1 | 1 |  |  |
|queries-FStar.Map.smt2                                       |    1.471s | 74.788MiB| error-1 | 1 |  |  |
|queries-FStar.String.smt2                                    |    1.478s | 84.344MiB| error-1 | 1 |  |  |
|queries-FStar.Fin.smt2                                       |    1.567s | 77.512MiB| error-1 | 1 |  |  |
|queries-FStar.Real.smt2                                      |    1.572s | 50.62MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Derived.Lemmas.smt2                 |    1.584s | 84.952MiB| error-1 | 1 |  |  |
|queries-FStar.Sequence.Seq.smt2                              |    1.615s | 79.784MiB| error-1 | 1 |  |  |
|queries-FStar.Classical.Sugar.smt2                           |    1.617s | 74.712MiB| error-1 | 1 |  |  |
|queries-FStar.PCM.smt2                                       |    1.682s | 74.756MiB| error-1 | 1 |  |  |
|queries-FStar.Algebra.Monoid.smt2                            |    1.756s | 74.524MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Print.smt2                             |    1.765s | 90.164MiB| error-1 | 1 |  |  |
|queries-FStar.Vector.Properties.smt2                         |    1.776s | 82.556MiB| error-1 | 1 |  |  |
|queries-FStar.FiniteSet.smt2                                 |    1.784s | 76.756MiB| error-1 | 1 |  |  |
|queries-FStar.Classical-2.smt2                               |    1.800s | 74.748MiB| error-1 | 1 |  |  |
|queries-FStar.Algebra.CommMonoid.Fold.Nested.smt2            |    1.808s | 96.284MiB| error-1 | 1 |  |  |
|queries-LowStar.BufferCompat.smt2                            |    1.847s | 136.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.CanonCommSwaps.smt2                    |    1.851s | 75.828MiB| error-1 | 1 |  |  |
|queries-FStar.List.smt2                                      |    1.895s | 76.616MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Typeclasses.smt2                       |    1.909s | 94.92MiB| error-1 | 1 |  |  |
|queries-FStar.Tcp.smt2                                       |    1.924s | 138.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Simplifier.smt2                        |    1.933s | 92.852MiB| error-1 | 1 |  |  |
|queries-FStar.Pervasives.smt2                                |    1.944s | 50.736MiB| error-1 | 1 |  |  |
|queries-FStar.Monotonic.Map.smt2                             |    1.994s | 85.008MiB| error-1 | 1 |  |  |
|queries-FStar.WellFoundedRelation.smt2                       |    2.005s | 76.108MiB| error-1 | 1 |  |  |
|queries-FStar.Crypto.smt2                                    |    2.039s | 138.0MiB| error-1 | 1 |  |  |
|queries-Steel.Channel.Protocol.smt2                          |    2.081s | 74.716MiB| error-1 | 1 |  |  |
|queries-Steel.ST.GhostReference.smt2                         |    2.161s | 154.0MiB| error-1 | 1 |  |  |
|queries-FStar.DependentMap.smt2                              |    2.186s | 74.528MiB| error-1 | 1 |  |  |
|queries-FStar.Seq.Permutation.smt2                           |    2.220s | 79.164MiB| error-1 | 1 |  |  |
|queries-Steel.ST.MonotonicReference.smt2                     |    2.222s | 153.0MiB| error-1 | 1 |  |  |
|queries-FStar.Seq.Sorted.smt2                                |    2.227s | 76.852MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-6.smt2                             |    2.232s | 96.776MiB| error-1 | 1 |  |  |
|queries-FStar.FunctionalExtensionality.smt2                  |    2.236s | 82.052MiB| error-1 | 1 |  |  |
|queries-Steel.PCMMap.smt2                                    |    2.242s | 75.82MiB| error-1 | 1 |  |  |
|queries-LowStar.Regional.smt2                                |    2.260s | 136.0MiB| error-1 | 1 |  |  |
|queries-LowStar.BufferOps.smt2                               |    2.262s | 136.0MiB| error-1 | 1 |  |  |
|queries-Steel.ST.CancellableSpinLock.smt2                    |    2.280s | 151.0MiB| error-1 | 1 |  |  |
|queries-Steel.Effect.M.smt2                                  |    2.321s | 139.0MiB| error-1 | 1 |  |  |
|queries-Steel.LockCoupling.smt2                              |    2.331s | 153.0MiB| error-1 | 1 |  |  |
|queries-FStar.LexicographicOrdering.smt2                     |    2.382s | 75.328MiB| error-1 | 1 |  |  |
|queries-FStar.OrdMap.smt2                                    |    2.476s | 74.908MiB| error-1 | 1 |  |  |
|queries-Steel.Primitive.ForkJoin.smt2                        |    2.508s | 154.0MiB| error-1 | 1 |  |  |
|queries-Steel.ST.Loops.Util.smt2                             |    2.524s | 152.0MiB| error-1 | 1 |  |  |
|queries-Steel.Closure.smt2                                   |    2.579s | 153.0MiB| error-1 | 1 |  |  |
|queries-LowStar.Endianness-2.smt2                            |    2.601s | 138.0MiB| error-1 | 1 |  |  |
|queries-FStar.Algebra.CommMonoid.Fold.smt2                   |    2.608s | 84.2MiB| error-1 | 1 |  |  |
|queries-FStar.WellFounded.Util.smt2                          |    2.609s | 95.0MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Lib.smt2                                  |    2.646s | 53.42MiB| error-1 | 1 |  |  |
|queries-FStar.Seq.Equiv.smt2                                 |    2.697s | 81.188MiB| error-1 | 1 |  |  |
|queries-Steel.Utils.smt2                                     |    2.703s | 154.0MiB| error-1 | 1 |  |  |
|queries-Steel.ST.Util.smt2                                   |    2.771s | 155.0MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Derived.smt2                        |    2.771s | 85.564MiB| error-1 | 1 |  |  |
|queries-LowStar.Buffer.smt2                                  |    2.782s | 136.0MiB| error-1 | 1 |  |  |
|queries-Steel.MonotonicReference.smt2                        |    2.795s | 153.0MiB| error-1 | 1 |  |  |
|queries-Steel.Semantics.Instantiate.smt2                     |    2.820s | 143.0MiB| error-1 | 1 |  |  |
|queries-Steel.ST.Coercions.smt2                              |    2.828s | 165.0MiB| error-1 | 1 |  |  |
|queries-FStar.Int8.smt2                                      |    2.851s | 83.596MiB| error-1 | 1 |  |  |
|queries-FStar.InteractiveHelpers.Output.smt2                 |    2.881s | 136.0MiB| error-1 | 1 |  |  |
|queries-Steel.ST.Reference.smt2                              |    2.887s | 156.0MiB| error-1 | 1 |  |  |
|queries-FStar.Int128.smt2                                    |    2.896s | 85.268MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Logic.smt2                             |    2.901s | 92.944MiB| error-1 | 1 |  |  |
|queries-FStar.Monotonic.DependentMap.smt2                    |    2.907s | 87.088MiB| error-1 | 1 |  |  |
|queries-FStar.Vector.Base.smt2                               |    3.010s | 81.56MiB| error-1 | 1 |  |  |
|queries-LowStar.Literal.smt2                                 |    3.015s | 144.0MiB| error-1 | 1 |  |  |
|queries-Steel.Channel.Duplex.smt2                            |    3.029s | 153.0MiB| error-1 | 1 |  |  |
|queries-FStar.Int32.smt2                                     |    3.043s | 83.512MiB| error-1 | 1 |  |  |
|queries-Steel.Loops.smt2                                     |    3.043s | 154.0MiB| error-1 | 1 |  |  |
|queries-Steel.ST.BitVector.smt2                              |    3.065s | 152.0MiB| error-1 | 1 |  |  |
|queries-FStar.InteractiveHelpers.Base.smt2                   |    3.075s | 136.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.CanonMonoid.smt2                       |    3.104s | 136.0MiB| error-1 | 1 |  |  |
|queries-FStar.Pointer.Derived3.smt2                          |    3.118s | 140.0MiB| error-1 | 1 |  |  |
|queries-Steel.GhostPCMReference.smt2                         |    3.173s | 153.0MiB| error-1 | 1 |  |  |
|queries-FStar.List.Pure.Properties.smt2                      |    3.182s | 77.972MiB| error-1 | 1 |  |  |
|queries-Steel.ST.SpinLock.smt2                               |    3.219s | 154.0MiB| error-1 | 1 |  |  |
|queries-Steel.DisposableInvariant.smt2                       |    3.340s | 157.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-4.smt2                             |    3.357s | 99.0MiB| error-1 | 1 |  |  |
|queries-Steel.ST.Loops.smt2                                  |    3.394s | 155.0MiB| error-1 | 1 |  |  |
|queries-FStar.Int16.smt2                                     |    3.433s | 83.704MiB| error-1 | 1 |  |  |
|queries-Steel.SpinLock.smt2                                  |    3.438s | 152.0MiB| error-1 | 1 |  |  |
|queries-Steel.ST.Effect.Atomic.smt2                          |    3.470s | 166.0MiB| error-1 | 1 |  |  |
|queries-Steel.ST.GhostPCMReference.smt2                      |    3.471s | 157.0MiB| error-1 | 1 |  |  |
|queries-FStar.Pointer.Derived2.smt2                          |    3.510s | 140.0MiB| error-1 | 1 |  |  |
|queries-FStar.Int64.smt2                                     |    3.546s | 83.696MiB| error-1 | 1 |  |  |
|queries-LowStar.Monotonic.Buffer-2.smt2                      |    3.551s | 160.0MiB| error-1 | 1 |  |  |
|queries-FStar.Monotonic.HyperHeap.smt2                       |    3.594s | 79.952MiB| error-1 | 1 |  |  |
|queries-FStar.OrdSet.smt2                                    |    3.607s | 79.46MiB| error-1 | 1 |  |  |
|queries-FStar.Array.smt2                                     |    3.608s | 84.652MiB| error-1 | 1 |  |  |
|queries-FStar.ReflexiveTransitiveClosure.smt2                |    3.633s | 94.76MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Formula.smt2                        |    3.771s | 90.908MiB| error-1 | 1 |  |  |
|queries-LowStar.UninitializedBuffer.smt2                     |    3.842s | 136.0MiB| error-1 | 1 |  |  |
|queries-Steel.Preorder.smt2                                  |    3.932s | 87.328MiB| error-1 | 1 |  |  |
|queries-FStar.Sequence.Permutation.smt2                      |    4.034s | 81.912MiB| error-1 | 1 |  |  |
|queries-FStar.List.Tot.Base.smt2                             |    4.052s | 74.496MiB| error-1 | 1 |  |  |
|queries-Steel.ST.Effect.Ghost.smt2                           |    4.069s | 167.0MiB| error-1 | 1 |  |  |
|queries-Steel.Primitive.ForkJoin.Unix.smt2                   |    4.116s | 155.0MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Data.smt2                           |    4.155s | 75.472MiB| error-1 | 1 |  |  |
|queries-FStar.InteractiveHelpers.ExploreTerm.smt2            |    4.200s | 136.0MiB| error-1 | 1 |  |  |
|queries-FStar.ConstantTime.Integers.smt2                     |    4.210s | 136.0MiB| error-1 | 1 |  |  |
|queries-Steel.ST.Effect.smt2                                 |    4.262s | 164.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.CanonCommMonoidSimple.smt2             |    4.420s | 136.0MiB| error-1 | 1 |  |  |
|queries-FStar.BitVector.smt2                                 |    4.473s | 77.948MiB| error-1 | 1 |  |  |
|queries-Steel.PCMReference.smt2                              |    4.497s | 153.0MiB| error-1 | 1 |  |  |
|queries-FStar.Printf.smt2                                    |    4.572s | 137.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Canon.smt2                             |    4.620s | 136.0MiB| error-1 | 1 |  |  |
|queries-FStar.InteractiveHelpers.Effectful.smt2              |    4.699s | 138.0MiB| error-1 | 1 |  |  |
|queries-FStar.Bytes.smt2                                     |    4.823s | 137.0MiB| error-1 | 1 |  |  |
|queries-FStar.InteractiveHelpers.PostProcess.smt2            |    4.827s | 155.0MiB| unsat | 0 |  |  |
|queries-Steel.ST.Effect.AtomicAndGhost.smt2                  |    4.887s | 168.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.CanonCommMonoidSimple.Equiv.smt2       |    4.993s | 136.0MiB| error-1 | 1 |  |  |
|queries-LowStar.PrefixFreezableBuffer.smt2                   |    4.994s | 138.0MiB| error-1 | 1 |  |  |
|queries-LowStar.ImmutableBuffer.smt2                         |    5.015s | 136.0MiB| error-1 | 1 |  |  |
|queries-Steel.ST.Array.smt2                                  |    5.063s | 156.0MiB| error-1 | 1 |  |  |
|queries-FStar.BigOps.smt2                                    |    5.091s | 168.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-2.smt2                             |    5.133s | 99.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.CanonCommMonoid.smt2                   |    5.182s | 136.0MiB| error-1 | 1 |  |  |
|queries-LowStar.ConstBuffer.smt2                             |    5.568s | 136.0MiB| error-1 | 1 |  |  |
|queries-FStar.Reflection.Arith.smt2                          |    5.584s | 162.0MiB| error-1 | 1 |  |  |
|queries-FStar.Monotonic.Seq.smt2                             |    5.623s | 92.928MiB| error-1 | 1 |  |  |
|queries-FStar.Seq.Permutation-3.smt2                         |    5.695s | 93.976MiB| error-1 | 1 |  |  |
|queries-FStar.UInt32.smt2                                    |    5.786s | 86.54MiB| error-1 | 1 |  |  |
|queries-LowStar.Regional.Instances.smt2                      |    5.793s | 151.0MiB| error-1 | 1 |  |  |
|queries-FStar.BV.smt2                                        |    5.928s | 84.38MiB| error-1 | 1 |  |  |
|queries-FStar.Integers.smt2                                  |    5.989s | 161.0MiB| error-1 | 1 |  |  |
|queries-FStar.Seq.Base.smt2                                  |    5.990s | 80.992MiB| error-1 | 1 |  |  |
|queries-LowStar.ToFStarBuffer.smt2                           |    6.009s | 142.0MiB| error-1 | 1 |  |  |
|queries-FStar.UInt64.smt2                                    |    6.227s | 88.248MiB| error-1 | 1 |  |  |
|queries-FStar.UInt8.smt2                                     |    6.815s | 90.116MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.Derived.smt2                           |    6.822s | 96.056MiB| error-1 | 1 |  |  |
|queries-LowStar.Endianness.smt2                              |    6.945s | 138.0MiB| error-1 | 1 |  |  |
|queries-FStar.UInt16.smt2                                    |    6.964s | 90.552MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.PatternMatching.smt2                   |    7.054s | 140.0MiB| error-1 | 1 |  |  |
|queries-LowStar.Printf.smt2                                  |    7.146s | 161.0MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.BV.smt2                                |    7.655s | 140.0MiB| error-1 | 1 |  |  |
|queries-LowStar.Monotonic.Buffer-1.smt2                      |    7.685s | 159.0MiB| error-1 | 1 |  |  |
|queries-Steel.ST.Array.Util.smt2                             |    7.850s | 157.0MiB| error-1 | 1 |  |  |
|queries-FStar.Matrix.smt2                                    |    8.092s | 96.78MiB| error-1 | 1 |  |  |
|queries-FStar.Monotonic.HyperStack.smt2                      |    8.103s | 93.148MiB| error-1 | 1 |  |  |
|queries-FStar.List.Tot.Properties.smt2                       |    8.198s | 85.052MiB| error-1 | 1 |  |  |
|queries-LowStar.BufferView.smt2                              |    8.221s | 139.0MiB| error-1 | 1 |  |  |
|queries-FStar.Buffer.Quantifiers.smt2                        |    8.582s | 150.0MiB| error-1 | 1 |  |  |
|queries-FStar.TaggedUnion.smt2                               |    8.678s | 148.0MiB| error-1 | 1 |  |  |
|queries-FStar.Monotonic.Heap.smt2                            |    8.697s | 91.948MiB| error-1 | 1 |  |  |
|queries-Steel.Channel.Simplex.smt2                           |    8.920s | 164.0MiB| error-1 | 1 |  |  |
|queries-LowStar.BufferView.Up.smt2                           |    8.933s | 139.0MiB| error-1 | 1 |  |  |
|queries-FStar.HyperStack.ST.smt2                             |    9.040s | 164.0MiB| error-1 | 1 |  |  |
|queries-FStar.BufferNG.smt2                                  |    9.096s | 145.0MiB| error-1 | 1 |  |  |
|queries-FStar.Pointer.Base.smt2                              |    9.125s | 139.0MiB| error-1 | 1 |  |  |
|queries-FStar.Sequence.Base.smt2                             |    9.406s | 92.88MiB| error-1 | 1 |  |  |
|queries-FStar.Tactics.CanonCommSemiring.smt2                 |   10.531s | 141.0MiB| error-1 | 1 |  |  |
|queries-FStar.Seq.Permutation-1.smt2                         |   10.910s | 164.0MiB| error-1 | 1 |  |  |
|queries-FStar.Pointer.Derived1.smt2                          |   10.924s | 146.0MiB| error-1 | 1 |  |  |
|queries-Steel.Array.smt2                                     |   11.367s | 189.0MiB| error-1 | 1 |  |  |
|queries-LowStar.BufferView.Down.smt2                         |   12.396s | 141.0MiB| error-1 | 1 |  |  |
|queries-Steel.Stepper.smt2                                   |   13.289s | 189.0MiB| error-1 | 1 |  |  |
|queries-Steel.Memory.smt2                                    |   13.341s | 162.0MiB| error-1 | 1 |  |  |
|queries-Steel.Reference.smt2                                 |   13.402s | 158.0MiB| error-1 | 1 |  |  |
|queries-FStar.Modifies.smt2                                  |   14.851s | 144.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-3.smt2                             |   15.277s | 164.0MiB| error-1 | 1 |  |  |
|queries-Steel.Effect.smt2                                    |   15.337s | 187.0MiB| error-1 | 1 |  |  |
|queries-FStar.Int.Cast.smt2                                  |   16.153s | 100.0MiB| error-1 | 1 |  |  |
|queries-Steel.HigherReference.smt2                           |   16.279s | 209.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-7.smt2                             |   16.565s | 181.0MiB| error-1 | 1 |  |  |
|queries-FStar.ModifiesGen-1.smt2                             |   16.708s | 166.0MiB| error-1 | 1 |  |  |
|queries-Steel.ST.EphemeralHashtbl.smt2                       |   16.842s | 180.0MiB| error-1 | 1 |  |  |
|queries-Steel.Heap-1.smt2                                    |   16.930s | 251.0MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Lemmas.smt2                               |   17.247s | 100.0MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Euclid-1.smt2                             |   17.391s | 1728.0MiB| error-1 | 1 |  |  |
|queries-FStar.Seq.Properties.smt2                            |   18.052s | 160.0MiB| error-1 | 1 |  |  |
|queries-Steel.FractionalAnchoredPreorder.smt2                |   20.951s | 183.0MiB| error-1 | 1 |  |  |
|queries-Steel.MonotonicHigherReference.smt2                  |   21.482s | 210.0MiB| error-1 | 1 |  |  |
|queries-FStar.Int.smt2                                       |   22.644s | 177.0MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Fermat.smt2                               |   23.377s | 153.0MiB| error-1 | 1 |  |  |
|queries-FStar.Endianness.smt2                                |   24.199s | 168.0MiB| error-1 | 1 |  |  |
|queries-LowStar.Vector.smt2                                  |   25.775s | 165.0MiB| error-1 | 1 |  |  |
|queries-Steel.Effect.Common.smt2                             |   28.191s | 170.0MiB| error-1 | 1 |  |  |
|queries-Steel.Effect.Atomic.smt2                             |   33.766s | 322.0MiB| error-1 | 1 |  |  |
|queries-FStar.Matrix-1.smt2                                  |   36.395s | 1702.0MiB| error-1 | 1 |  |  |
|queries-FStar.Buffer.smt2                                    |   40.942s | 195.0MiB| error-1 | 1 |  |  |
|queries-FStar.Math.Euclid.smt2                               |   53.490s | 1082.0MiB| error-1 | 1 |  |  |
|queries-Steel.Semantics.Hoare.MST.smt2                       |   60.053s | 177.0MiB| timeout | 0 |  |  |
|queries-FStar.UInt.smt2                                      |   60.071s | 164.0MiB| timeout | 0 |  |  |
|queries-LowStar.RVector.smt2                                 |   60.082s | 325.0MiB| timeout | 0 |  |  |
|queries-LowStar.Monotonic.Buffer.smt2                        |   60.098s | 327.0MiB| timeout | 0 |  |  |
|queries-FStar.ModifiesGen-5.smt2                             |   60.098s | 234.0MiB| timeout | 0 |  |  |
|queries-Steel.Heap.smt2                                      |   60.100s | 423.0MiB| timeout | 0 |  |  |
|queries-FStar.ModifiesGen.smt2                               |   60.109s | 328.0MiB| timeout | 0 |  |  |
|queries-FStar.UInt128.smt2                                   |   60.156s | 370.0MiB| timeout | 0 |  |  |
