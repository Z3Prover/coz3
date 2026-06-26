# .

* SAT 0
* UNSAT 26
* TIMEOUT 28
* UNKNOWN 1

* ERRORS 3 (error-1:3)

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: 
Job tag: mariposa-unsat-core-zenodo
Runner: rise-runner-1
Z3 repo: Z3Prover/z3
Z3 commit: 6a62a531819d3c2aa21eaaffa8411aea4d9a2f40
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://zenodo.org/records/20938919/files/unstable_core.tar.zst?download=1
Z3 commit message: qsat: decide quantifier-free goals so qe2 returns sat instead of unknown (fixes iss-7027/small-30) (#9970)

## Summary

Fixes the `iss-7027/small-30` snapshot regression (`Z3Prover/bench`
discussion #2705) **at its root**, instead of working around it by
retuning the LP heuristics.

- **Benchmark:** `inputs/issues/iss-7027/small-30.smt2` —
`(check-sat-using qe2)` over a single `(distinct ...)` of 33 mixed
Int/Real terms.
- The recorded oracle was `unknown`; current `master` produces
`timeout`.

## Root cause

`unknown`/`timeout` are both wrong here: the formula is a `distinct`
over 33 terms (free Int/Real constants plus the literals `0`/`1`), which
is **trivially `sat`** — there are infinitely many distinct reals.

The real bug is in the `qsat` tactic that backs `qe2`. Running
quantifier elimination on a **quantifier-free** formula has nothing to
eliminate, so `qsat` left an undecided residual goal and
`check-sat-using` reported `unknown`. This reproduces on any ground
formula with free variables, e.g.:

```
(declare-fun a () Int)(assert (> a 0))(check-sat-using qe2)   ; -> unknown (should be sat)
```

For `small-30` the QE alternation additionally drove `theory_lra`
integer branch-and-bound down a non-terminating path, surfacing as a
`timeout` under the capture budget (the symptom the `random_hammers`
schedule change happened to expose).

## Fix

Under `check-sat` semantics, top-level free variables are implicitly
existentially quantified. So when the `qsat` input has no quantifiers,
decide satisfiability directly (route through the existing `qsat_sat`
path) instead of producing a residual goal. `qe2`/`qe` now return
`sat`/`unsat` for ground formulas.

QE of genuinely-quantified formulas is unchanged: `apply qe2` on a
quantified goal produces the same projected formula as before (verified
identical to `master`). Only the degenerate quantifier-free case is
affected.

This supersedes the previous approach in this PR (reverting the
`lp.random_hammers` default). That default is left **unchanged**
(`true`), preserving #9958's aggregate QF_LIA benefit. `small-30` now
returns `sat` in ~0.01s regardless of the heuristic schedule, because
the QE machinery no longer runs on this ground instance.

Two changes:
- `src/qe/qsat.cpp`: short-circuit quantifier-free input to the
satisfiability decision path.
- `Z3Prover/bench` `inputs/issues/iss-7027/small-30.expected.out`:
oracle updated `unknown` -> `sat` (to be committed alongside this fix).

## Validation

```
$ z3 small-30.smt2
sat            # ~0.01s

$ echo '(declare-fun a () Int)(assert (> a 0))(check-sat-using qe2)' | z3 -in
sat
$ echo '(declare-fun a () Int)(assert (and (> a 0)(< a 0)))(check-sat-using qe2)' | z3 -in
unsat
```

Full unit-test suite (`test-z3 /a`) passes (92/92). Quantified `qe2`
round-trips (`apply qe2`) match `master` byte-for-byte.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|unstable_core/d_lvbkv-lib-Math-Nonlinear.i.dfy.Impl__NonlinearLemmas.__default.div__denom__ge__1.smt2 |    0.071s | 21.548MiB| unsat | 0 |  |  |
|unstable_core/d_fvbkv-lib-Math-mod_auto_proofs.i.dfy.Impl__Math____mod__auto__proofs__i.__default.lemma__mod__induction__forall2.smt2 |    0.096s | 22.984MiB| unsat | 0 |  |  |
|unstable_core/d_lvbkv-lib-Checksums-BitLemmas.i.dfy.Impl__BitLemmas.__default.eq__from__unpack__LittleEndian__Uint32__eq.smt2 |    0.116s | 23.732MiB| unsat | 0 |  |  |
|unstable_core/d_fvbkv-lib-Base-BitsetLemmas.i.dfy.Impl__BitsetLemmas.__default.set__bit__to__0__self__uint64.smt2 |    0.173s | 37.556MiB| unsat | 0 |  |  |
|unstable_core/d_fvbkv-lib-Base-BitsetLemmas.i.dfy.Impl__BitsetLemmas.__default.set__bit__to__1__self__uint64.smt2 |    0.174s | 37.42MiB| unsat | 0 |  |  |
|unstable_core/d_lvbkv-lib-Lang-LinearSequence.i.dfy.Impl__LinearSequence__i.__default.AllocAndMoveLseq.smt2 |    0.248s | 28.764MiB| unsat | 0 |  |  |
|unstable_core/d_lvbkv-lib-Base-total_order_impl.i.dfy.Impl__Total__Order__Impl.__default.ArrayLargestLtePlus1Linear.smt2 |    0.252s | 27.752MiB| unsat | 0 |  |  |
|unstable_core/d_fvbkv-lib-Marshalling-Math.i.dfy.Impl__Math.__default.lemma__div__denominator.smt2 |    0.345s | 24.3MiB| unsat | 0 |  |  |
|unstable_core/d_fvbkv-lib-Buckets-PackedStringArray.i.dfy.Impl__PackedStringArray.__default.UniqueRepr.smt2 |    0.348s | 31.116MiB| unsat | 0 |  |  |
|unstable_core/d_fvbkv-lib-DataStructures-MutableBtree.i.dfy.Impl__MutableBtree.__default.SubReprLowerBound.smt2 |    0.430s | 31.48MiB| unsat | 0 |  |  |
|unstable_core/d_lvbkv-lib-Marshalling-GenericMarshalling.i.dfy.Impl__GenericMarshalling.__default.MarshallByteArray.smt2 |    0.480s | 36.972MiB| unsat | 0 |  |  |
|unstable_core/d_lvbkv-MapSpec-TSJMap_Refines_ThreeStateVersionedMap.i.dfy.Impl__TSJMap__Refines__ThreeStateVersionedMap.__default.RefinesReplay.smt2 |    0.550s | 36.452MiB| unsat | 0 |  |  |
|unstable_core/d_fvbkv-lib-Marshalling-Math.i.dfy.Impl__Math.__default.lemma__2toX.smt2 |    0.688s | 58.056MiB| unsat | 0 |  |  |
|unstable_core/d_lvbkv-lib-Marshalling-GenericMarshalling.i.dfy.Impl__GenericMarshalling.__default.MarshallArray.smt2 |    0.727s | 39.632MiB| unsat | 0 |  |  |
|unstable_core/d_lvbkv-lib-Marshalling-GenericMarshalling.i.dfy.Impl__GenericMarshalling.__default.MarshallByteArrayInterior.smt2 |    0.887s | 39.512MiB| unsat | 0 |  |  |
|unstable_core/d_fvbkv-lib-Marshalling-GenericMarshalling.i.dfy.Impl__GenericMarshalling.__default.MarshallUint32Array.smt2 |    1.251s | 44.696MiB| unsat | 0 |  |  |
|unstable_core/d_fvbkv-Impl-JournalistModel.i.dfy.CheckWellformed__JournalistModel.__default.append.smt2 |    1.698s | 59.932MiB| unsat | 0 |  |  |
|unstable_core/d_lvbkv-lib-Buckets-PackedKV.i.dfy.Impl__DynamicPkv.DynamicPkv.Append.smt2 |    2.072s | 61.948MiB| unsat | 0 |  |  |
|unstable_core/d_fvbkv-lib-Marshalling-GenericMarshalling.i.dfy.Impl__GenericMarshalling.__default.MarshallArray.smt2 |    2.214s | 59.028MiB| unknown | 0 |  |  |
|unstable_core/d_lvbkv-PivotBetree-PivotBetreeSpecWFNodes.i.dfy.Impl__PivotBetreeSpecWFNodes.__default.ValidSplitWritesInvNodes.smt2 |    2.590s | 96.512MiB| unsat | 0 |  |  |
|unstable_core/fs_dice-queries-ASN1.Low.Base-3.smt2           |    2.715s | 171.0MiB| error-1 | 1 |  |  |
|unstable_core/d_lvbkv-Impl-JournalistImpl.i.dfy.Impl__JournalistImpl.Journalist.append.smt2 |    2.744s | 64.584MiB| unsat | 0 |  |  |
|unstable_core/d_komodo-verified-ptebits.i.dfyImpl___module.__default.lemma__l1ptesmatch.smt2 |    3.455s | 274.0MiB| unsat | 0 |  |  |
|unstable_core/d_fvbkv-lib-Marshalling-GenericMarshalling.i.dfy.Impl__GenericMarshalling.__default.MarshallUint64Array.smt2 |    3.625s | 64.396MiB| unsat | 0 |  |  |
|unstable_core/d_fvbkv-Betree-BetreeInv.i.dfy.Impl__BetreeInv.__default.FlushPreservesLookups.smt2 |    4.520s | 93.196MiB| unsat | 0 |  |  |
|unstable_core/d_fvbkv-lib-Buckets-PackedKV.i.dfy.Impl__PackedKV.__default.BinarySearchQuery.smt2 |    6.524s | 110.0MiB| unsat | 0 |  |  |
|unstable_core/d_lvbkv-lib-DataStructures-MutableBtree.i.dfy.Impl__LMutableBtree.__default.InsertIndex.smt2 |    6.865s | 100.0MiB| unsat | 0 |  |  |
|unstable_core/d_fvbkv-lib-DataStructures-LruImpl.i.dfy.Impl__LruImpl.LruImplQueue.Use.smt2 |   14.794s | 106.0MiB| unsat | 0 |  |  |
|unstable_core/fs_dice-queries-L0.X509.AliasKeyTBS.Subject-7.smt2 |   18.787s | 332.0MiB| error-1 | 1 |  |  |
|unstable_core/fs_dice-queries-L0.X509.AliasKeyTBS.Issuer-7.smt2 |   18.893s | 331.0MiB| error-1 | 1 |  |  |
|unstable_core/d_lvbkv-lib-Buckets-PackedStringArray.i.dfy.Impl__PackedStringArray.DynamicPsa.AppendSeq.smt2 |   20.014s | 144.0MiB| timeout | 0 |  |  |
|unstable_core/d_lvbkv-lib-DataStructures-LinearDList.i.dfy.Impl__DList.DList.InsertAfter.smt2 |   20.020s | 139.0MiB| timeout | 0 |  |  |
|unstable_core/d_fvbkv-lib-DataStructures-BtreeModel.i.dfy.Impl__BtreeModel.__default.SplitIndexAllKeys.smt2 |   20.022s | 205.0MiB| timeout | 0 |  |  |
|unstable_core/d_fvbkv-Impl-IOImpl.i.dfy.Impl__IOImpl.__default.PageInNodeResp.smt2 |   20.022s | 171.0MiB| timeout | 0 |  |  |
|unstable_core/d_komodo-verified-sha-sha256-body-16-xx.gen.dfyImpl___module.__default.va__refined__Body__16__XXUnroller.smt2 |   20.030s | 113.0MiB| timeout | 0 |  |  |
|unstable_core/d_lvbkv-lib-DataStructures-LinearDList.i.dfy.Impl__DList.DList.InsertBefore.smt2 |   20.030s | 131.0MiB| timeout | 0 |  |  |
|unstable_core/d_fvbkv-lib-DataStructures-MutableBtree.i.dfy.Impl__MutableBtree.__default.SubIndex.smt2 |   20.033s | 112.0MiB| timeout | 0 |  |  |
|unstable_core/d_lvbkv-PivotBetree-PivotBetreeSpecRefinement.i.dfy.Impl__PivotBetreeSpecRefinement.__default.SplitChildrenConsistent.smt2 |   20.036s | 130.0MiB| timeout | 0 |  |  |
|unstable_core/d_komodo-verified-verify.gen.dfyImpl___module.__default.va__lemma__svc__returning__verify__inner.smt2 |   20.042s | 188.0MiB| timeout | 0 |  |  |
|unstable_core/d_komodo-verified-sha-sha256-body-00-15.gen.dfyImpl___module.__default.va__refined__Body__00__15UnrolledRecursive.smt2 |   20.043s | 110.0MiB| timeout | 0 |  |  |
|unstable_core/d_lvbkv-lib-Buckets-PackedKV.i.dfy.Impl__PackedKV.__default.BinarySearchQuery.smt2 |   20.043s | 148.0MiB| timeout | 0 |  |  |
|unstable_core/d_fvbkv-ByteBlockCacheSystem-InterpretationDisk.i.dfy.Impl__InterpretationDisk.__default.RefinesProcessWrite.smt2 |   20.044s | 178.0MiB| timeout | 0 |  |  |
|unstable_core/d_lvbkv-lib-DataStructures-LinearContentMutableMap.i.dfy.Impl__LinearContentMutableMap.__default.Realloc.smt2 |   20.044s | 233.0MiB| timeout | 0 |  |  |
|unstable_core/d_komodo-verified-verify.gen.dfyImpl___module.__default.va__lemma__svc__returning__verify.smt2 |   20.045s | 214.0MiB| timeout | 0 |  |  |
|unstable_core/d_fvbkv-Impl-FlushPolicyImpl.i.dfy.Impl__FlushPolicyImpl.__default.runFlushPolicy.smt2 |   20.046s | 195.0MiB| timeout | 0 |  |  |
|unstable_core/d_fvbkv-Impl-QueryImpl.i.dfy.Impl__QueryImpl.__default.queryIterate.smt2 |   20.046s | 193.0MiB| timeout | 0 |  |  |
|unstable_core/d_komodo-verified-verify.gen.dfyImpl___module.__default.va__lemma__svc__returning__verify__inner_k.smt2 |   20.047s | 189.0MiB| timeout | 0 |  |  |
|unstable_core/d_fvbkv-lib-Buckets-KMBPKVOps.i.dfy.Impl__KMBPKVOps.__default.IndexFillDpkv.smt2 |   20.048s | 188.0MiB| timeout | 0 |  |  |
|unstable_core/d_lvbkv-lib-Buckets-BucketsLib.i.dfy.Impl__BucketsLib.__default.WFProperSplitBucketInList.smt2 |   20.051s | 178.0MiB| timeout | 0 |  |  |
|unstable_core/d_fvbkv-ByteBlockCacheSystem-ByteSystem.i.dfy.Impl__ByteSystem.__default.ReqWrite2StepPreservesInv.smt2 |   20.055s | 187.0MiB| timeout | 0 |  |  |
|unstable_core/d_fvbkv-lib-Buckets-PackedStringArray.i.dfy.Impl__PackedStringArray.__default.WFpsaSubSeq.smt2 |   20.056s | 413.0MiB| timeout | 0 |  |  |
|unstable_core/d_lvbkv-Impl-MarshallingImpl.i.dfy.Impl__MarshallingImpl.__default.strictlySortedPivotsToVal.smt2 |   20.057s | 223.0MiB| timeout | 0 |  |  |
|unstable_core/d_lvbkv-Impl-BucketGeneratorModel.i.dfy.Impl__BucketGeneratorModel.__default.GenComposeIsCompose.smt2 |   20.059s | 182.0MiB| timeout | 0 |  |  |
|unstable_core/d_komodo-verified-secprop-conf_ni.i.dfyImpl___module.__default.lemma__initL2PTable__loweq__pdb.smt2 |   20.060s | 187.0MiB| timeout | 0 |  |  |
|unstable_core/d_lvbkv-lib-Buckets-PackedStringArray.i.dfy.Impl__PackedStringArray.__default.psaCanAppendI.smt2 |   20.063s | 279.0MiB| timeout | 0 |  |  |
|unstable_core/d_fvbkv-lib-Math-mod_auto_proofs.i.dfy.Impl__Math____mod__auto__proofs__i.__default.lemma__mod__induction__forall.smt2 |   20.070s | 567.0MiB| timeout | 0 |  |  |
|unstable_core/d_fvbkv-lib-Math-mul.i.dfy.Impl__Math____mul__i.__default.lemma__mul__nonnegative.smt2 |   20.090s | 616.0MiB| timeout | 0 |  |  |
|unstable_core/d_komodo-verified-sha-sha256.i.dfyImpl___module.__default.lemma__SHA256FinalHelper1.smt2 |   20.205s | 2925.0MiB| timeout | 0 |  |  |
