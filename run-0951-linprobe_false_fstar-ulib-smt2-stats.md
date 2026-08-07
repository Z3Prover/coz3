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
Job description: fstar-ulib-smt2 with linprobe false
Job tag: linprobe_false_fstar-ulib-smt2
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 98bb6245ca74b76025d7e2a6300aaec6dfb20f90
Z3 branch: linprobe
Z3 options: "-T:60 smt.arith.nl.linprobe=false"
Z3 inputs: inputs/fstar-ulib-smt2
Z3 commit message: Add linprobe mode: short monomial linearization probe before SMT

Introduce an optional "linprobe" pre-solving phase that runs a short,
time-bounded SMT check restricted to linear monomial bound propagation
before falling back to the full solver.

- nla_core: add m_linprobe flag driven by arith.nl.linprobe_mode; in
  linprobe mode use monomial_bounds::propagate_linear_bounds instead of
  propagate_changed_bounds.
- monomial_bounds: honor the resource limit inside the linear-bound and
  LP-row propagation loops so the probe can be interrupted.
- theory_lra: return FC_CONTINUE when NLA made progress in linprobe mode,
  and bail out early when the context became inconsistent.
- smt_tactic: wrap the SMT tactic in a new linprobe_tactic that first runs
  a time-bounded probe with non-linear features disabled and falls back to
  the regular tactic on failure; user-propagator callbacks disable the probe.
- combined_solver: try the same bounded probe on solver1 before the normal
  path, and track whether user-propagator callbacks are registered.
- smt_params_helper: add the arith.nl.linprobe parameter.
- test: cover linprobe propagation of newly linear monomials.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | INFO |
|------------|----------:|---------:|-------------:| ----------:|------|
|queries-Prims.smt2                                           |    0.117s | 23.44MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Endianness-1.smt2                            |    0.175s | 26.004MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pervasives.Native.smt2                         |    0.452s | 46.18MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Common.smt2                            |    0.464s | 49.504MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.PropositionalExtensionality.smt2               |    0.483s | 50.284MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.VConfig.smt2                                   |    0.496s | 49.712MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Heap.smt2                                      |    0.508s | 75.044MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.Pure.smt2                            |    0.529s | 50.196MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Classical-1.smt2                               |    0.530s | 74.7MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.TSet-1.smt2                                    |    0.609s | 74.796MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Order.smt2                                     |    0.635s | 50.428MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Effect-1.smt2                          |    0.636s | 75.104MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.All.smt2                                       |    0.654s | 75.176MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.IFC.smt2                                       |    0.665s | 74.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.TwoLevelHeap.smt2                              |    0.728s | 75.256MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Sequence.Ambient.smt2                          |    0.754s | 74.776MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Algebra.CommMonoid.smt2                        |    0.754s | 50.944MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Universe.smt2                                  |    0.763s | 49.912MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.OrdSetProps.smt2                               |    0.781s | 74.744MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Types.smt2                          |    0.807s | 74.732MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Classical.smt2                                 |    0.809s | 74.636MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.OrdMapProps.smt2                               |    0.826s | 74.772MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Squash.smt2                                    |    0.864s | 49.728MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.FractionalPermission.smt2                      |    0.881s | 74.744MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Option.smt2                                    |    0.888s | 75.728MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.PredicateExtensionality.smt2                   |    0.898s | 74.732MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Relational.Relational.smt2                     |    0.903s | 75.104MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Effect-2.smt2                          |    0.909s | 75.26MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Ghost.smt2                                     |    0.929s | 50.776MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Ref.smt2                                       |    0.975s | 75.248MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.IndefiniteDescription.smt2                     |    0.982s | 74.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.TSet-2.smt2                                    |    0.994s | 74.792MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.SquashProperties.smt2                          |    1.003s | 74.796MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Result.smt2                            |    1.018s | 74.996MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Universe.PCM.smt2                              |    1.029s | 75.308MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.List.Pure.Base.smt2                            |    1.043s | 75.212MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.WellFounded.smt2                               |    1.067s | 75.004MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.TSet.smt2                                      |    1.082s | 74.764MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.MRef.smt2                                      |    1.120s | 75.26MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Algebra.CommMonoid.Equiv.smt2                  |    1.124s | 74.612MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Calc.smt2                                      |    1.137s | 74.752MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Comment.smt2                                 |    1.149s | 81.664MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Constructive.smt2                              |    1.159s | 50.68MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.MonotonicCounter.smt2                          |    1.208s | 77.108MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.MSTTotal.smt2                                  |    1.214s | 74.736MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.GSet.smt2                                      |    1.223s | 74.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Char.smt2                                      |    1.236s | 79.664MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.SyntaxHelpers.smt2                     |    1.265s | 87.608MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Util.smt2                                      |    1.281s | 79.94MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.PartialMap.smt2                                |    1.282s | 74.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.NMSTTotal.smt2                                 |    1.296s | 74.78MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Sequence.Util.smt2                             |    1.297s | 74.764MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.MST.smt2                                       |    1.315s | 74.756MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.IntegerIntervals.smt2                          |    1.340s | 76.288MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Util.smt2                              |    1.370s | 76.648MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int.Cast.Full.smt2                             |    1.370s | 93.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Effect.smt2                            |    1.374s | 75.112MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.NMST.smt2                                      |    1.380s | 74.836MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ST.smt2                                        |    1.391s | 75.288MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Set.smt2                                       |    1.428s | 74.708MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.Witnessed.smt2                       |    1.437s | 74.608MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.String.smt2                                    |    1.443s | 84.328MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Matrix2.smt2                                   |    1.462s | 76.912MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Error.smt2                                     |    1.471s | 88.044MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Sequence.Seq.smt2                              |    1.483s | 80.024MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Map.smt2                                       |    1.515s | 74.616MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Permutation-2.smt2                         |    1.581s | 80.912MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Crypto.smt2                                    |    1.622s | 138.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Real.smt2                                      |    1.626s | 50.556MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Classical.Sugar.smt2                           |    1.633s | 74.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Typeclasses.smt2                       |    1.645s | 94.968MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Fin.smt2                                       |    1.656s | 77.692MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Classical-2.smt2                               |    1.714s | 74.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Derived.Lemmas.smt2                 |    1.734s | 85.068MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Vector.Properties.smt2                         |    1.742s | 82.384MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Print.smt2                             |    1.760s | 90.148MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.FiniteSet.smt2                                 |    1.771s | 76.86MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.Map.smt2                             |    1.795s | 85.044MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Algebra.CommMonoid.Fold.Nested.smt2            |    1.806s | 96.508MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.DependentMap.smt2                              |    1.883s | 74.568MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.PCM.smt2                                       |    1.890s | 74.78MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.CancellableSpinLock.smt2                    |    1.943s | 151.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.BufferCompat.smt2                            |    1.950s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonCommSwaps.smt2                    |    1.955s | 75.856MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.List.smt2                                      |    1.990s | 76.872MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.BufferOps.smt2                               |    2.051s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Channel.Protocol.smt2                          |    2.135s | 74.692MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Endianness-2.smt2                            |    2.143s | 138.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.FunctionalExtensionality.smt2                  |    2.165s | 81.94MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.PCMMap.smt2                                    |    2.179s | 75.952MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.LexicographicOrdering.smt2                     |    2.181s | 75.216MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Regional.smt2                                |    2.193s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.MonotonicReference.smt2                     |    2.196s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Closure.smt2                                   |    2.222s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.WellFoundedRelation.smt2                       |    2.223s | 75.984MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Simplifier.smt2                        |    2.288s | 92.888MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tcp.smt2                                       |    2.293s | 138.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.OrdMap.smt2                                    |    2.324s | 75.004MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.LockCoupling.smt2                              |    2.343s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Permutation.smt2                           |    2.380s | 79.568MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.GhostReference.smt2                         |    2.430s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonMonoid.smt2                       |    2.448s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pervasives.smt2                                |    2.481s | 50.82MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Sorted.smt2                                |    2.488s | 76.712MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Math.Lib.smt2                                  |    2.537s | 53.724MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Effect.M.smt2                                  |    2.549s | 139.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Util.smt2                                   |    2.554s | 155.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Equiv.smt2                                 |    2.559s | 82.66MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Derived.smt2                        |    2.579s | 85.236MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-6.smt2                             |    2.602s | 96.792MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.DependentMap.smt2                    |    2.655s | 87.016MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int8.smt2                                      |    2.682s | 84.956MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.WellFounded.Util.smt2                          |    2.688s | 95.048MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Reference.smt2                              |    2.737s | 156.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.InteractiveHelpers.Output.smt2                 |    2.741s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Loops.Util.smt2                             |    2.763s | 152.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Channel.Duplex.smt2                            |    2.857s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pointer.Derived2.smt2                          |    2.878s | 140.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Coercions.smt2                              |    2.893s | 165.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.GhostPCMReference.smt2                      |    2.895s | 156.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Effect.Atomic.smt2                          |    2.905s | 165.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Primitive.ForkJoin.smt2                        |    2.929s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ReflexiveTransitiveClosure.smt2                |    2.935s | 94.488MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int128.smt2                                    |    2.972s | 87.1MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Semantics.Instantiate.smt2                     |    2.973s | 143.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.SpinLock.smt2                                  |    3.077s | 152.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Loops.smt2                                  |    3.086s | 155.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Effect.Ghost.smt2                           |    3.100s | 167.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int64.smt2                                     |    3.197s | 87.044MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Utils.smt2                                     |    3.271s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Algebra.CommMonoid.Fold.smt2                   |    3.276s | 84.216MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int16.smt2                                     |    3.299s | 85.528MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Buffer.smt2                                  |    3.315s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.GhostPCMReference.smt2                         |    3.324s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Literal.smt2                                 |    3.325s | 145.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Monotonic.Buffer-2.smt2                      |    3.379s | 159.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int32.smt2                                     |    3.428s | 86.248MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Logic.smt2                             |    3.448s | 92.944MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.DisposableInvariant.smt2                       |    3.460s | 157.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.List.Tot.Base.smt2                             |    3.468s | 74.688MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Vector.Base.smt2                               |    3.498s | 81.652MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.SpinLock.smt2                               |    3.550s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.List.Pure.Properties.smt2                      |    3.580s | 77.628MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-4.smt2                             |    3.616s | 100.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Primitive.ForkJoin.Unix.smt2                   |    3.621s | 155.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.MonotonicReference.smt2                        |    3.635s | 153.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Loops.smt2                                     |    3.648s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.BitVector.smt2                              |    3.698s | 152.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Formula.smt2                        |    3.720s | 90.644MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Sequence.Permutation.smt2                      |    3.841s | 80.92MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pointer.Derived3.smt2                          |    3.874s | 141.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.UninitializedBuffer.smt2                     |    3.900s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.InteractiveHelpers.Base.smt2                   |    3.907s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.HyperHeap.smt2                       |    3.924s | 80.136MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.BitVector.smt2                                 |    3.979s | 77.688MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Effect.smt2                                 |    4.061s | 164.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ConstantTime.Integers.smt2                     |    4.066s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.PCMReference.smt2                              |    4.143s | 154.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonCommMonoidSimple.smt2             |    4.243s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Canon.smt2                             |    4.306s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Data.smt2                           |    4.357s | 75.684MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Preorder.smt2                                  |    4.361s | 87.58MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.BigOps.smt2                                    |    4.448s | 168.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Array.smt2                                     |    4.483s | 87.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.OrdSet.smt2                                    |    4.498s | 79.476MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.ImmutableBuffer.smt2                         |    4.588s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonCommMonoid.smt2                   |    4.738s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.PrefixFreezableBuffer.smt2                   |    4.777s | 138.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.InteractiveHelpers.ExploreTerm.smt2            |    4.806s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Effect.AtomicAndGhost.smt2                  |    4.866s | 168.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonCommMonoidSimple.Equiv.smt2       |    4.883s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Printf.smt2                                    |    4.889s | 137.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.InteractiveHelpers.PostProcess.smt2            |    4.987s | 155.0MiB| unsat | 0 |  |
|queries-FStar.InteractiveHelpers.Effectful.smt2              |    5.016s | 138.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Algebra.Monoid.smt2                            |    5.088s | 312.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.ConstBuffer.smt2                             |    5.229s | 136.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Array.smt2                                  |    5.387s | 157.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-2.smt2                             |    5.395s | 102.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Bytes.smt2                                     |    5.484s | 137.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Reflection.Arith.smt2                          |    5.528s | 161.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Permutation-3.smt2                         |    5.666s | 90.56MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Regional.Instances.smt2                      |    6.071s | 151.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Integers.smt2                                  |    6.076s | 161.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.Seq.smt2                             |    6.085s | 95.428MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Monotonic.Buffer-1.smt2                      |    6.172s | 159.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.BV.smt2                                        |    6.189s | 84.444MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.ToFStarBuffer.smt2                           |    6.291s | 143.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Math.Euclid.smt2                               |    6.795s | 137.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.BV.smt2                                |    6.836s | 140.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Base.smt2                                  |    6.853s | 82.044MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.Derived.smt2                           |    6.924s | 95.744MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.PatternMatching.smt2                   |    6.932s | 140.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Endianness.smt2                              |    7.081s | 138.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.UInt16.smt2                                    |    7.105s | 90.396MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.UInt32.smt2                                    |    7.135s | 89.748MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Printf.smt2                                  |    7.272s | 159.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.Heap.smt2                            |    7.508s | 92.42MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.Array.Util.smt2                             |    7.598s | 157.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Monotonic.HyperStack.smt2                      |    7.730s | 92.712MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.UInt64.smt2                                    |    8.032s | 91.528MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.List.Tot.Properties.smt2                       |    8.108s | 85.54MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Channel.Simplex.smt2                           |    8.164s | 164.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.UInt8.smt2                                     |    8.337s | 91.78MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.BufferView.smt2                              |    8.450s | 139.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Buffer.Quantifiers.smt2                        |    8.589s | 151.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.BufferView.Up.smt2                           |    8.863s | 142.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.TaggedUnion.smt2                               |    8.907s | 146.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.BufferNG.smt2                                  |    9.102s | 146.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pointer.Base.smt2                              |    9.182s | 139.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.HyperStack.ST.smt2                             |    9.275s | 164.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Tactics.CanonCommSemiring.smt2                 |   10.068s | 141.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Sequence.Base.smt2                             |   10.247s | 97.776MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Pointer.Derived1.smt2                          |   10.673s | 146.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Math.Lemmas.smt2                               |   10.888s | 74.8MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Matrix.smt2                                    |   10.965s | 100.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Permutation-1.smt2                         |   11.056s | 169.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Array.smt2                                     |   12.373s | 193.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.BufferView.Down.smt2                         |   12.630s | 140.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Stepper.smt2                                   |   12.829s | 188.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Memory.smt2                                    |   13.511s | 161.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Reference.smt2                                 |   13.852s | 158.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-3.smt2                             |   15.150s | 164.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Modifies.smt2                                  |   15.347s | 145.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.ST.EphemeralHashtbl.smt2                       |   15.427s | 174.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Effect.smt2                                    |   15.445s | 188.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-7.smt2                             |   15.745s | 181.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int.Cast.smt2                                  |   16.426s | 101.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.ModifiesGen-1.smt2                             |   16.586s | 164.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Math.Euclid-1.smt2                             |   16.894s | 1690.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.HigherReference.smt2                           |   17.647s | 210.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Heap-1.smt2                                    |   18.160s | 221.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Seq.Properties.smt2                            |   18.450s | 160.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.FractionalAnchoredPreorder.smt2                |   21.077s | 191.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Int.smt2                                       |   22.413s | 113.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.MonotonicHigherReference.smt2                  |   22.827s | 214.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Math.Fermat.smt2                               |   24.190s | 156.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Endianness.smt2                                |   24.567s | 170.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-LowStar.Vector.smt2                                  |   24.985s | 164.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Effect.Common.smt2                             |   27.209s | 171.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-Steel.Effect.Atomic.smt2                             |   33.656s | 322.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Matrix-1.smt2                                  |   37.879s | 1486.0MiB| unknown | 1 | benign: get-model/get-value after unknown: model is not available |
|queries-FStar.UInt128.smt2                                   |   58.780s | 485.0MiB| unsat | 1 | benign: get-model/get-value after unsat: model is not available |
|queries-FStar.Buffer.smt2                                    |   60.032s | 202.0MiB| timeout | 0 |  |
|queries-Steel.Semantics.Hoare.MST.smt2                       |   60.074s | 177.0MiB| timeout | 0 |  |
|queries-LowStar.RVector.smt2                                 |   60.088s | 326.0MiB| timeout | 0 |  |
|queries-FStar.UInt.smt2                                      |   60.089s | 188.0MiB| timeout | 0 |  |
|queries-FStar.ModifiesGen.smt2                               |   60.092s | 322.0MiB| timeout | 0 |  |
|queries-FStar.ModifiesGen-5.smt2                             |   60.098s | 218.0MiB| timeout | 0 |  |
|queries-Steel.Heap.smt2                                      |   60.103s | 442.0MiB| timeout | 0 |  |
|queries-LowStar.Monotonic.Buffer.smt2                        |   60.104s | 331.0MiB| timeout | 0 |  |
