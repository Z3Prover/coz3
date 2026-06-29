# .

* SAT 0
* UNSAT 0
* TIMEOUT 47
* UNKNOWN 0

* ERRORS 504 (error:374, parser:130)

# Meta data

<pre>
Ramon benchmark for Z3
-
Job description: Triggered by CoZ3 Benchmark Runner | Benchmark suite: https://tptp.org/TPTP/Distribution/TPTP-v9.2.1.tgz Problems/ALG | Source list: benchmarks-tptp.txt
Job tag: coz3-https-tptp.org-TPTP-Distribution-TPTP-v9.2.1.tgz-Problems-
Runner: rise-runner-2
Z3 repo: Z3Prover/z3
Z3 commit: 56bf04e30a9cfd601b1dcb4159604e81929a02ee
Z3 branch: master
Z3 options: "-T:20 model_validate=true"
Z3 inputs: https://tptp.org/TPTP/Distribution/TPTP-v9.2.1.tgz Problems/ALG
Z3 commit message: Fix qe-lite de Bruijn reindexing after bounded quantifier expansion (#9996)

`qe-lite` could produce malformed formulas when expanding bounded
quantifiers under nested binders, leaving outer de Bruijn indices
unshifted after eliminating an inner quantifier (e.g., `(:var 1)`
escaping capture). This change fixes index normalization in that rewrite
path and adds a regression for the reported forall/exists arithmetic
case.

- **Rewrite correctness in bounded quantifier expansion**
- In `src/qe/lite/qe_lite_tactic.cpp`, after substituting bounded
variables in payload conjuncts, apply `inv_var_shifter(num_decls)` so
outer bound variables are reindexed relative to the removed binder.
- This preserves quantifier structure correctness when
`try_expand_bounded_quantifier` eliminates an inner quantifier.

- **Regression coverage for the reported pattern**
- In `src/test/smt_context.cpp`, add a focused quantified arithmetic
formula matching the bug shape:
    - outer `forall (x, x4)`
    - inner `exists (y)`
    - mixed inequalities that trigger qe-lite bounded expansion
- Assert the formula is unsatisfiable, preventing reintroduction of
invalid index handling in this path.

```c++
inst = vs(p, subst_map.size(), subst_map.data());
shift(inst, num_decls, inst); // reindex outer de Bruijn vars after eliminating inner quantifier
```

---------

Co-authored-by: copilot-swe-agent[bot] <198982749+Copilot@users.noreply.github.com>

</pre>


# Statistics
|FILE                                                         |TIME     |MEM        | STATUS   | EXIT | STDOUT | STDERR | 
|------------|----------:|---------:|-------------:| ----------:|--------|--------| 
|ALG251^2.p                                                   |    0.021s | 19.52MiB| parser | 103 |  |  |
|ALG221+3.p                                                   |    0.021s | 19.732MiB| parser | 103 |  |  |
|ALG269^1.p                                                   |    0.021s | 19.656MiB| parser | 103 |  |  |
|ALG251^3.p                                                   |    0.022s | 19.536MiB| parser | 103 |  |  |
|ALG229+2.p                                                   |    0.023s | 19.696MiB| parser | 103 |  |  |
|ALG253^1.p                                                   |    0.023s | 19.464MiB| parser | 103 |  |  |
|ALG259^2.p                                                   |    0.023s | 19.596MiB| parser | 103 |  |  |
|ALG297^5.p                                                   |    0.026s | 20.328MiB| error | 0 |  |  |
|ALG082+1.p                                                   |    0.027s | 20.3MiB| error | 0 |  |  |
|ALG267^1.p                                                   |    0.027s | 19.656MiB| parser | 103 |  |  |
|ALG184+1.p                                                   |    0.027s | 20.364MiB| error | 0 |  |  |
|ALG224+2.p                                                   |    0.027s | 19.672MiB| parser | 103 |  |  |
|ALG021+1.p                                                   |    0.027s | 20.112MiB| error | 0 |  |  |
|ALG019+1.p                                                   |    0.028s | 21.272MiB| error | 0 |  |  |
|ALG440-1.p                                                   |    0.028s | 20.924MiB| error | 0 |  |  |
|ALG001-1.p                                                   |    0.028s | 19.596MiB| parser | 103 |  |  |
|ALG073+1.p                                                   |    0.028s | 20.592MiB| error | 0 |  |  |
|ALG232+3.p                                                   |    0.028s | 19.544MiB| parser | 103 |  |  |
|ALG186+1.p                                                   |    0.029s | 20.336MiB| error | 0 |  |  |
|ALG204+1.p                                                   |    0.030s | 20.664MiB| error | 0 |  |  |
|ALG226+3.p                                                   |    0.030s | 19.704MiB| parser | 103 |  |  |
|ALG203+1.p                                                   |    0.030s | 20.68MiB| error | 0 |  |  |
|ALG268^5.p                                                   |    0.032s | 19.732MiB| parser | 103 |  |  |
|ALG020+1.p                                                   |    0.033s | 20.112MiB| error | 0 |  |  |
|ALG260^1.p                                                   |    0.033s | 19.628MiB| parser | 103 |  |  |
|ALG331-1.p                                                   |    0.033s | 21.2MiB| error | 0 |  |  |
|ALG230+1.p                                                   |    0.034s | 20.88MiB| error | 0 |  |  |
|ALG260^2.p                                                   |    0.034s | 19.856MiB| parser | 103 |  |  |
|ALG071+1.p                                                   |    0.035s | 21.504MiB| error | 0 |  |  |
|ALG257^2.p                                                   |    0.036s | 19.596MiB| parser | 103 |  |  |
|ALG161+1.p                                                   |    0.037s | 20.324MiB| error | 0 |  |  |
|ALG025+1.p                                                   |    0.038s | 19.976MiB| error | 0 |  |  |
|ALG226+2.p                                                   |    0.038s | 19.656MiB| parser | 103 |  |  |
|ALG255^1.p                                                   |    0.039s | 19.6MiB| parser | 103 |  |  |
|ALG207+1.p                                                   |    0.039s | 20.236MiB| error | 0 |  |  |
|ALG333-1.p                                                   |    0.039s | 21.456MiB| error | 0 |  |  |
|ALG254^1.p                                                   |    0.040s | 19.688MiB| parser | 103 |  |  |
|ALG254^2.p                                                   |    0.040s | 19.628MiB| parser | 103 |  |  |
|ALG135+1.p                                                   |    0.040s | 20.244MiB| error | 0 |  |  |
|ALG217+3.p                                                   |    0.041s | 19.652MiB| parser | 103 |  |  |
|ALG224+3.p                                                   |    0.041s | 19.592MiB| parser | 103 |  |  |
|ALG223+4.p                                                   |    0.041s | 19.956MiB| parser | 103 |  |  |
|ALG016^7.p                                                   |    0.041s | 20.104MiB| parser | 103 |  |  |
|ALG012-1.p                                                   |    0.042s | 20.292MiB| error | 0 |  |  |
|ALG094+1.p                                                   |    0.042s | 20.096MiB| error | 0 |  |  |
|ALG220+1.p                                                   |    0.042s | 20.824MiB| error | 0 |  |  |
|ALG265^2.p                                                   |    0.043s | 19.572MiB| parser | 103 |  |  |
|ALG023+1.p                                                   |    0.045s | 20.4MiB| error | 0 |  |  |
|ALG226+4.p                                                   |    0.046s | 19.808MiB| parser | 103 |  |  |
|ALG221+2.p                                                   |    0.046s | 19.588MiB| parser | 103 |  |  |
|ALG323-1.p                                                   |    0.046s | 21.428MiB| error | 0 |  |  |
|ALG318-1.p                                                   |    0.046s | 21.408MiB| error | 0 |  |  |
|ALG130+1.p                                                   |    0.047s | 20.108MiB| error | 0 |  |  |
|ALG018^7.p                                                   |    0.047s | 19.6MiB| parser | 103 |  |  |
|ALG252^2.p                                                   |    0.047s | 19.64MiB| parser | 103 |  |  |
|ALG023^7.p                                                   |    0.048s | 19.604MiB| parser | 103 |  |  |
|ALG060+1.p                                                   |    0.049s | 20.34MiB| error | 0 |  |  |
|ALG233+2.p                                                   |    0.049s | 19.904MiB| parser | 103 |  |  |
|ALG228+1.p                                                   |    0.050s | 20.968MiB| error | 0 |  |  |
|ALG017^7.p                                                   |    0.050s | 19.632MiB| parser | 103 |  |  |
|ALG087+1.p                                                   |    0.050s | 20.356MiB| error | 0 |  |  |
|ALG443+1.p                                                   |    0.050s | 19.884MiB| parser | 103 |  |  |
|ALG020^7.p                                                   |    0.050s | 19.608MiB| parser | 103 |  |  |
|ALG223+2.p                                                   |    0.051s | 19.572MiB| parser | 103 |  |  |
|ALG228+4.p                                                   |    0.051s | 19.876MiB| parser | 103 |  |  |
|ALG216+4.p                                                   |    0.052s | 19.656MiB| parser | 103 |  |  |
|ALG103+1.p                                                   |    0.052s | 20.664MiB| error | 0 |  |  |
|ALG392-1.p                                                   |    0.053s | 20.96MiB| error | 0 |  |  |
|ALG306-1.p                                                   |    0.053s | 21.184MiB| error | 0 |  |  |
|ALG235-1.p                                                   |    0.053s | 20.448MiB| error | 0 |  |  |
|ALG268^6.p                                                   |    0.053s | 19.828MiB| parser | 103 |  |  |
|ALG251^1.p                                                   |    0.053s | 19.748MiB| parser | 103 |  |  |
|ALG079+1.p                                                   |    0.053s | 20.172MiB| error | 0 |  |  |
|ALG015+1.p                                                   |    0.053s | 20.924MiB| error | 0 |  |  |
|ALG219+3.p                                                   |    0.053s | 19.6MiB| parser | 103 |  |  |
|ALG261^1.p                                                   |    0.054s | 19.588MiB| parser | 103 |  |  |
|ALG002-1.p                                                   |    0.054s | 20.356MiB| error | 0 |  |  |
|ALG248^1.p                                                   |    0.054s | 19.612MiB| parser | 103 |  |  |
|ALG001^5.p                                                   |    0.054s | 20.444MiB| error | 0 |  |  |
|ALG081+1.p                                                   |    0.054s | 20.444MiB| error | 0 |  |  |
|ALG245-1.p                                                   |    0.054s | 22.484MiB| error | 0 |  |  |
|ALG266^2.p                                                   |    0.054s | 19.688MiB| parser | 103 |  |  |
|ALG142+1.p                                                   |    0.055s | 20.24MiB| error | 0 |  |  |
|ALG210+2.p                                                   |    0.055s | 21.696MiB| error | 0 |  |  |
|ALG173+1.p                                                   |    0.055s | 19.96MiB| error | 0 |  |  |
|ALG080+1.p                                                   |    0.055s | 20.388MiB| error | 0 |  |  |
|ALG014^7.p                                                   |    0.055s | 20.324MiB| parser | 103 |  |  |
|ALG223+3.p                                                   |    0.055s | 19.636MiB| parser | 103 |  |  |
|ALG215+1.p                                                   |    0.055s | 20.652MiB| error | 0 |  |  |
|ALG390-1.p                                                   |    0.055s | 20.496MiB| error | 0 |  |  |
|ALG228+2.p                                                   |    0.055s | 19.556MiB| parser | 103 |  |  |
|ALG139+1.p                                                   |    0.056s | 20.108MiB| error | 0 |  |  |
|ALG334-1.p                                                   |    0.056s | 21.58MiB| error | 0 |  |  |
|ALG039+1.p                                                   |    0.056s | 19.908MiB| error | 0 |  |  |
|ALG222+3.p                                                   |    0.056s | 19.968MiB| parser | 103 |  |  |
|ALG036+1.p                                                   |    0.056s | 19.724MiB| error | 0 |  |  |
|ALG110+1.p                                                   |    0.056s | 20.608MiB| error | 0 |  |  |
|ALG212+1.p                                                   |    0.056s | 19.812MiB| parser | 103 |  |  |
|ALG320-1.p                                                   |    0.056s | 21.36MiB| error | 0 |  |  |
|ALG178+1.p                                                   |    0.056s | 21.44MiB| error | 0 |  |  |
|ALG444^1.p                                                   |    0.057s | 19.636MiB| parser | 103 |  |  |
|ALG001-2.p                                                   |    0.057s | 19.652MiB| parser | 103 |  |  |
|ALG234+3.p                                                   |    0.057s | 19.872MiB| parser | 103 |  |  |
|ALG124+1.p                                                   |    0.057s | 20.836MiB| error | 0 |  |  |
|ALG041+1.p                                                   |    0.057s | 21.588MiB| error | 0 |  |  |
|ALG240-1.p                                                   |    0.058s | 22.208MiB| error | 0 |  |  |
|ALG217+2.p                                                   |    0.058s | 19.652MiB| parser | 103 |  |  |
|ALG295^5.p                                                   |    0.058s | 21.212MiB| error | 0 |  |  |
|ALG217+1.p                                                   |    0.058s | 21.032MiB| error | 0 |  |  |
|ALG068+1.p                                                   |    0.058s | 20.388MiB| error | 0 |  |  |
|ALG277^5.p                                                   |    0.058s | 22.636MiB| error | 0 |  |  |
|ALG317-1.p                                                   |    0.058s | 21.472MiB| error | 0 |  |  |
|ALG321-1.p                                                   |    0.059s | 21.408MiB| error | 0 |  |  |
|ALG176+1.p                                                   |    0.059s | 20.604MiB| error | 0 |  |  |
|ALG248^2.p                                                   |    0.059s | 19.772MiB| parser | 103 |  |  |
|ALG040+1.p                                                   |    0.060s | 20.62MiB| error | 0 |  |  |
|ALG216+1.p                                                   |    0.060s | 20.852MiB| error | 0 |  |  |
|ALG234+2.p                                                   |    0.060s | 19.496MiB| parser | 103 |  |  |
|ALG231+4.p                                                   |    0.060s | 19.644MiB| parser | 103 |  |  |
|ALG399-1.p                                                   |    0.060s | 21.344MiB| error | 0 |  |  |
|ALG231+3.p                                                   |    0.060s | 19.884MiB| parser | 103 |  |  |
|ALG171+1.p                                                   |    0.060s | 19.896MiB| error | 0 |  |  |
|ALG092+1.p                                                   |    0.061s | 20.136MiB| error | 0 |  |  |
|ALG224+1.p                                                   |    0.061s | 20.664MiB| error | 0 |  |  |
|ALG220+4.p                                                   |    0.061s | 19.488MiB| parser | 103 |  |  |
|ALG309-1.p                                                   |    0.061s | 21.248MiB| error | 0 |  |  |
|ALG187+1.p                                                   |    0.062s | 20.1MiB| error | 0 |  |  |
|ALG228+3.p                                                   |    0.062s | 19.612MiB| parser | 103 |  |  |
|ALG028+1.p                                                   |    0.062s | 19.84MiB| error | 0 |  |  |
|ALG042+1.p                                                   |    0.062s | 20.22MiB| error | 0 |  |  |
|ALG214+4.p                                                   |    0.062s | 19.628MiB| parser | 103 |  |  |
|ALG058+1.p                                                   |    0.062s | 20.624MiB| error | 0 |  |  |
|ALG206+1.p                                                   |    0.062s | 20.54MiB| error | 0 |  |  |
|ALG270^5.p                                                   |    0.062s | 20.12MiB| error | 0 |  |  |
|ALG313-1.p                                                   |    0.062s | 21.408MiB| error | 0 |  |  |
|ALG267^2.p                                                   |    0.063s | 19.452MiB| parser | 103 |  |  |
|ALG009-1.p                                                   |    0.063s | 19.664MiB| parser | 103 |  |  |
|ALG327-1.p                                                   |    0.063s | 21.776MiB| error | 0 |  |  |
|ALG220+3.p                                                   |    0.063s | 19.496MiB| parser | 103 |  |  |
|ALG250^3.p                                                   |    0.063s | 19.636MiB| parser | 103 |  |  |
|ALG312-1.p                                                   |    0.063s | 21.324MiB| error | 0 |  |  |
|ALG021^7.p                                                   |    0.063s | 19.884MiB| parser | 103 |  |  |
|ALG264^1.p                                                   |    0.063s | 19.844MiB| parser | 103 |  |  |
|ALG031+1.p                                                   |    0.064s | 20.512MiB| error | 0 |  |  |
|ALG168+1.p                                                   |    0.064s | 21.984MiB| error | 0 |  |  |
|ALG256^1.p                                                   |    0.064s | 19.46MiB| parser | 103 |  |  |
|ALG108+1.p                                                   |    0.065s | 20.628MiB| error | 0 |  |  |
|ALG315-1.p                                                   |    0.065s | 21.38MiB| error | 0 |  |  |
|ALG200+1.p                                                   |    0.066s | 19.828MiB| error | 0 |  |  |
|ALG230+3.p                                                   |    0.066s | 19.592MiB| parser | 103 |  |  |
|ALG017+1.p                                                   |    0.066s | 19.64MiB| error | 0 |  |  |
|ALG182+1.p                                                   |    0.066s | 20.252MiB| error | 0 |  |  |
|ALG136+1.p                                                   |    0.066s | 20.108MiB| error | 0 |  |  |
|ALG132+1.p                                                   |    0.066s | 20.332MiB| error | 0 |  |  |
|ALG175+1.p                                                   |    0.066s | 20.276MiB| error | 0 |  |  |
|ALG307-1.p                                                   |    0.067s | 21.1MiB| error | 0 |  |  |
|ALG015^7.p                                                   |    0.067s | 19.852MiB| parser | 103 |  |  |
|ALG268^2.p                                                   |    0.067s | 19.608MiB| parser | 103 |  |  |
|ALG311-1.p                                                   |    0.067s | 21.4MiB| error | 0 |  |  |
|ALG227+3.p                                                   |    0.067s | 19.872MiB| parser | 103 |  |  |
|ALG064+1.p                                                   |    0.067s | 20.552MiB| error | 0 |  |  |
|ALG013-1.p                                                   |    0.067s | 20.424MiB| error | 0 |  |  |
|ALG314-1.p                                                   |    0.067s | 21.348MiB| error | 0 |  |  |
|ALG213+1.p                                                   |    0.067s | 19.992MiB| parser | 103 |  |  |
|ALG152+1.p                                                   |    0.067s | 20.284MiB| error | 0 |  |  |
|ALG125+1.p                                                   |    0.067s | 20.768MiB| error | 0 |  |  |
|ALG216+2.p                                                   |    0.068s | 19.712MiB| parser | 103 |  |  |
|ALG264^3.p                                                   |    0.068s | 19.88MiB| parser | 103 |  |  |
|ALG030+1.p                                                   |    0.068s | 20.824MiB| error | 0 |  |  |
|ALG043+1.p                                                   |    0.068s | 20.064MiB| error | 0 |  |  |
|ALG121+1.p                                                   |    0.068s | 20.688MiB| error | 0 |  |  |
|ALG192+1.p                                                   |    0.068s | 20.852MiB| error | 0 |  |  |
|ALG231+2.p                                                   |    0.068s | 19.596MiB| parser | 103 |  |  |
|ALG234+4.p                                                   |    0.068s | 19.616MiB| parser | 103 |  |  |
|ALG076+1.p                                                   |    0.069s | 20.424MiB| error | 0 |  |  |
|ALG029+1.p                                                   |    0.069s | 21.448MiB| error | 0 |  |  |
|ALG170+1.p                                                   |    0.069s | 20.52MiB| error | 0 |  |  |
|ALG189+1.p                                                   |    0.069s | 20.088MiB| error | 0 |  |  |
|ALG249^3.p                                                   |    0.069s | 19.58MiB| parser | 103 |  |  |
|ALG181+1.p                                                   |    0.069s | 20.244MiB| error | 0 |  |  |
|ALG441-1.p                                                   |    0.069s | 21.172MiB| error | 0 |  |  |
|ALG193+1.p                                                   |    0.070s | 20.616MiB| error | 0 |  |  |
|ALG304-1.p                                                   |    0.070s | 21.972MiB| error | 0 |  |  |
|ALG257^1.p                                                   |    0.070s | 19.496MiB| parser | 103 |  |  |
|ALG056+1.p                                                   |    0.070s | 20.496MiB| error | 0 |  |  |
|ALG252^1.p                                                   |    0.070s | 19.888MiB| parser | 103 |  |  |
|ALG054+1.p                                                   |    0.070s | 20.38MiB| error | 0 |  |  |
|ALG218+2.p                                                   |    0.071s | 19.528MiB| parser | 103 |  |  |
|ALG153+1.p                                                   |    0.071s | 20.4MiB| error | 0 |  |  |
|ALG248^3.p                                                   |    0.071s | 19.788MiB| parser | 103 |  |  |
|ALG393-1.p                                                   |    0.071s | 20.5MiB| error | 0 |  |  |
|ALG112+1.p                                                   |    0.071s | 20.704MiB| error | 0 |  |  |
|ALG111+1.p                                                   |    0.071s | 20.66MiB| error | 0 |  |  |
|ALG165+1.p                                                   |    0.071s | 20.576MiB| error | 0 |  |  |
|ALG353-1.p                                                   |    0.071s | 20.492MiB| error | 0 |  |  |
|ALG213-10.p                                                  |    0.071s | 19.564MiB| parser | 103 |  |  |
|ALG066+1.p                                                   |    0.071s | 20.552MiB| error | 0 |  |  |
|ALG215+3.p                                                   |    0.072s | 19.72MiB| parser | 103 |  |  |
|ALG219+2.p                                                   |    0.072s | 19.644MiB| parser | 103 |  |  |
|ALG214+2.p                                                   |    0.072s | 19.628MiB| parser | 103 |  |  |
|ALG144+1.p                                                   |    0.072s | 20.324MiB| error | 0 |  |  |
|ALG147+1.p                                                   |    0.072s | 20.376MiB| error | 0 |  |  |
|ALG229+4.p                                                   |    0.072s | 19.548MiB| parser | 103 |  |  |
|ALG221+4.p                                                   |    0.072s | 19.848MiB| parser | 103 |  |  |
|ALG233+4.p                                                   |    0.073s | 20.1MiB| parser | 103 |  |  |
|ALG220+2.p                                                   |    0.073s | 19.628MiB| parser | 103 |  |  |
|ALG022+1.p                                                   |    0.073s | 20.224MiB| error | 0 |  |  |
|ALG310-1.p                                                   |    0.073s | 21.424MiB| error | 0 |  |  |
|ALG287^5.p                                                   |    0.073s | 20.388MiB| error | 0 |  |  |
|ALG148+1.p                                                   |    0.073s | 20.24MiB| error | 0 |  |  |
|ALG232+2.p                                                   |    0.073s | 19.624MiB| parser | 103 |  |  |
|ALG247^2.p                                                   |    0.073s | 19.6MiB| parser | 103 |  |  |
|ALG085+1.p                                                   |    0.073s | 20.368MiB| error | 0 |  |  |
|ALG131+1.p                                                   |    0.073s | 20.364MiB| error | 0 |  |  |
|ALG256^2.p                                                   |    0.073s | 19.632MiB| parser | 103 |  |  |
|ALG219+4.p                                                   |    0.074s | 19.72MiB| parser | 103 |  |  |
|ALG266^1.p                                                   |    0.074s | 19.456MiB| parser | 103 |  |  |
|ALG057+1.p                                                   |    0.074s | 20.416MiB| error | 0 |  |  |
|ALG194+1.p                                                   |    0.074s | 21.1MiB| error | 0 |  |  |
|ALG188+1.p                                                   |    0.074s | 20.3MiB| error | 0 |  |  |
|ALG303-1.p                                                   |    0.075s | 21.392MiB| error | 0 |  |  |
|ALG078+1.p                                                   |    0.075s | 20.26MiB| error | 0 |  |  |
|ALG199+1.p                                                   |    0.075s | 19.82MiB| error | 0 |  |  |
|ALG038+1.p                                                   |    0.075s | 20.596MiB| error | 0 |  |  |
|ALG022^7.p                                                   |    0.075s | 19.54MiB| parser | 103 |  |  |
|ALG033+1.p                                                   |    0.075s | 20.284MiB| error | 0 |  |  |
|ALG218+3.p                                                   |    0.076s | 19.608MiB| parser | 103 |  |  |
|ALG205+1.p                                                   |    0.076s | 20.456MiB| error | 0 |  |  |
|ALG050+1.p                                                   |    0.077s | 21.352MiB| error | 0 |  |  |
|ALG329-1.p                                                   |    0.077s | 21.496MiB| error | 0 |  |  |
|ALG269^3.p                                                   |    0.077s | 19.552MiB| parser | 103 |  |  |
|ALG097+1.p                                                   |    0.078s | 20.632MiB| error | 0 |  |  |
|ALG159+1.p                                                   |    0.078s | 20.296MiB| error | 0 |  |  |
|ALG225+2.p                                                   |    0.078s | 19.692MiB| parser | 103 |  |  |
|ALG069-10.p                                                  |    0.078s | 21.692MiB| error | 0 |  |  |
|ALG102+1.p                                                   |    0.079s | 20.748MiB| error | 0 |  |  |
|ALG202+1.p                                                   |    0.079s | 21.2MiB| error | 0 |  |  |
|ALG325-1.p                                                   |    0.079s | 21.388MiB| error | 0 |  |  |
|ALG104+1.p                                                   |    0.079s | 20.988MiB| error | 0 |  |  |
|ALG442-1.p                                                   |    0.079s | 20.964MiB| error | 0 |  |  |
|ALG210+1.p                                                   |    0.079s | 21.692MiB| error | 0 |  |  |
|ALG226+1.p                                                   |    0.079s | 20.656MiB| error | 0 |  |  |
|ALG209+1.p                                                   |    0.080s | 20.324MiB| error | 0 |  |  |
|ALG154+1.p                                                   |    0.080s | 20.444MiB| error | 0 |  |  |
|ALG118+1.p                                                   |    0.080s | 20.912MiB| error | 0 |  |  |
|ALG167+1.p                                                   |    0.080s | 20.892MiB| error | 0 |  |  |
|ALG409-1.p                                                   |    0.080s | 25.456MiB| error | 0 |  |  |
|ALG229+1.p                                                   |    0.080s | 20.872MiB| error | 0 |  |  |
|ALG268^3.p                                                   |    0.080s | 19.612MiB| parser | 103 |  |  |
|ALG322-1.p                                                   |    0.080s | 21.34MiB| error | 0 |  |  |
|ALG177+1.p                                                   |    0.080s | 20.696MiB| error | 0 |  |  |
|ALG369-1.p                                                   |    0.080s | 20.244MiB| error | 0 |  |  |
|ALG436-1.p                                                   |    0.081s | 20.056MiB| error | 0 |  |  |
|ALG098+1.p                                                   |    0.081s | 20.716MiB| error | 0 |  |  |
|ALG308-1.p                                                   |    0.081s | 21.62MiB| error | 0 |  |  |
|ALG233+3.p                                                   |    0.081s | 19.644MiB| parser | 103 |  |  |
|ALG151+1.p                                                   |    0.082s | 20.624MiB| error | 0 |  |  |
|ALG179+1.p                                                   |    0.082s | 21.436MiB| error | 0 |  |  |
|ALG407-1.p                                                   |    0.082s | 21.044MiB| error | 0 |  |  |
|ALG269^2.p                                                   |    0.082s | 19.616MiB| parser | 103 |  |  |
|ALG156+1.p                                                   |    0.082s | 20.36MiB| error | 0 |  |  |
|ALG348-1.p                                                   |    0.082s | 24.144MiB| error | 0 |  |  |
|ALG143+1.p                                                   |    0.082s | 20.304MiB| error | 0 |  |  |
|ALG305-1.p                                                   |    0.083s | 21.132MiB| error | 0 |  |  |
|ALG253^2.p                                                   |    0.083s | 19.456MiB| parser | 103 |  |  |
|ALG224+4.p                                                   |    0.083s | 19.64MiB| parser | 103 |  |  |
|ALG227+1.p                                                   |    0.083s | 20.864MiB| error | 0 |  |  |
|ALG214+3.p                                                   |    0.083s | 19.632MiB| parser | 103 |  |  |
|ALG286^5.p                                                   |    0.083s | 20.42MiB| error | 0 |  |  |
|ALG217+4.p                                                   |    0.083s | 19.592MiB| parser | 103 |  |  |
|ALG216+3.p                                                   |    0.083s | 19.48MiB| parser | 103 |  |  |
|ALG384-1.p                                                   |    0.083s | 22.516MiB| error | 0 |  |  |
|ALG215+4.p                                                   |    0.084s | 19.596MiB| parser | 103 |  |  |
|ALG063+1.p                                                   |    0.084s | 20.264MiB| error | 0 |  |  |
|ALG095+1.p                                                   |    0.084s | 20.224MiB| error | 0 |  |  |
|ALG225+4.p                                                   |    0.084s | 19.704MiB| parser | 103 |  |  |
|ALG133+1.p                                                   |    0.085s | 20.324MiB| error | 0 |  |  |
|ALG037+1.p                                                   |    0.085s | 20.9MiB| error | 0 |  |  |
|ALG047+1.p                                                   |    0.085s | 20.56MiB| error | 0 |  |  |
|ALG107+1.p                                                   |    0.085s | 20.692MiB| error | 0 |  |  |
|ALG229+3.p                                                   |    0.086s | 19.592MiB| parser | 103 |  |  |
|ALG439-1.p                                                   |    0.086s | 25.048MiB| error | 0 |  |  |
|ALG030-10.p                                                  |    0.087s | 20.984MiB| error | 0 |  |  |
|ALG259^1.p                                                   |    0.087s | 19.632MiB| parser | 103 |  |  |
|ALG397-1.p                                                   |    0.087s | 20.548MiB| error | 0 |  |  |
|ALG138+1.p                                                   |    0.087s | 20.148MiB| error | 0 |  |  |
|ALG232+1.p                                                   |    0.087s | 20.884MiB| error | 0 |  |  |
|ALG222+2.p                                                   |    0.088s | 19.944MiB| parser | 103 |  |  |
|ALG101+1.p                                                   |    0.088s | 20.684MiB| error | 0 |  |  |
|ALG120+1.p                                                   |    0.088s | 20.62MiB| error | 0 |  |  |
|ALG113+1.p                                                   |    0.088s | 20.888MiB| error | 0 |  |  |
|ALG208+1.p                                                   |    0.088s | 20.332MiB| error | 0 |  |  |
|ALG106+1.p                                                   |    0.088s | 20.632MiB| error | 0 |  |  |
|ALG046+1.p                                                   |    0.088s | 20.216MiB| error | 0 |  |  |
|ALG261^2.p                                                   |    0.088s | 19.6MiB| parser | 103 |  |  |
|ALG123+1.p                                                   |    0.089s | 20.62MiB| error | 0 |  |  |
|ALG160+1.p                                                   |    0.089s | 20.384MiB| error | 0 |  |  |
|ALG349-1.p                                                   |    0.089s | 22.56MiB| error | 0 |  |  |
|ALG091+1.p                                                   |    0.089s | 20.144MiB| error | 0 |  |  |
|ALG214+1.p                                                   |    0.089s | 20.756MiB| error | 0 |  |  |
|ALG045+1.p                                                   |    0.089s | 20.544MiB| error | 0 |  |  |
|ALG117+1.p                                                   |    0.089s | 20.664MiB| error | 0 |  |  |
|ALG316-1.p                                                   |    0.090s | 21.216MiB| error | 0 |  |  |
|ALG341-1.p                                                   |    0.090s | 23.124MiB| error | 0 |  |  |
|ALG158+1.p                                                   |    0.090s | 20.356MiB| error | 0 |  |  |
|ALG350-1.p                                                   |    0.090s | 21.736MiB| error | 0 |  |  |
|ALG128+1.p                                                   |    0.090s | 20.792MiB| error | 0 |  |  |
|ALG358-1.p                                                   |    0.090s | 23.012MiB| error | 0 |  |  |
|ALG001-3.p                                                   |    0.090s | 20.116MiB| parser | 103 |  |  |
|ALG324-1.p                                                   |    0.090s | 21.384MiB| error | 0 |  |  |
|ALG127+1.p                                                   |    0.091s | 20.58MiB| error | 0 |  |  |
|ALG414-1.p                                                   |    0.091s | 20.46MiB| error | 0 |  |  |
|ALG169+1.p                                                   |    0.091s | 22.016MiB| error | 0 |  |  |
|ALG084+1.p                                                   |    0.091s | 20.364MiB| error | 0 |  |  |
|ALG268^1.p                                                   |    0.091s | 19.544MiB| parser | 103 |  |  |
|ALG137+1.p                                                   |    0.091s | 20.272MiB| error | 0 |  |  |
|ALG146+1.p                                                   |    0.091s | 20.136MiB| error | 0 |  |  |
|ALG044+1.p                                                   |    0.091s | 19.972MiB| error | 0 |  |  |
|ALG328-1.p                                                   |    0.091s | 21.496MiB| error | 0 |  |  |
|ALG083+1.p                                                   |    0.092s | 20.144MiB| error | 0 |  |  |
|ALG231+1.p                                                   |    0.092s | 20.864MiB| error | 0 |  |  |
|ALG218+4.p                                                   |    0.093s | 19.636MiB| parser | 103 |  |  |
|ALG232+4.p                                                   |    0.093s | 19.652MiB| parser | 103 |  |  |
|ALG394-1.p                                                   |    0.093s | 20.996MiB| error | 0 |  |  |
|ALG340-1.p                                                   |    0.093s | 22.384MiB| error | 0 |  |  |
|ALG404-1.p                                                   |    0.093s | 20.756MiB| error | 0 |  |  |
|ALG065+1.p                                                   |    0.093s | 20.292MiB| error | 0 |  |  |
|ALG437-1.p                                                   |    0.093s | 23.952MiB| error | 0 |  |  |
|ALG032+1.p                                                   |    0.093s | 20.404MiB| error | 0 |  |  |
|ALG233+1.p                                                   |    0.093s | 20.896MiB| error | 0 |  |  |
|ALG223+1.p                                                   |    0.094s | 20.624MiB| error | 0 |  |  |
|ALG164+1.p                                                   |    0.094s | 20.296MiB| error | 0 |  |  |
|ALG258^2.p                                                   |    0.094s | 19.7MiB| parser | 103 |  |  |
|ALG089+1.p                                                   |    0.094s | 20.36MiB| error | 0 |  |  |
|ALG332-1.p                                                   |    0.094s | 21.392MiB| error | 0 |  |  |
|ALG172+1.p                                                   |    0.094s | 19.74MiB| error | 0 |  |  |
|ALG222+4.p                                                   |    0.095s | 19.672MiB| parser | 103 |  |  |
|ALG162+1.p                                                   |    0.095s | 20.644MiB| error | 0 |  |  |
|ALG268^4.p                                                   |    0.095s | 19.64MiB| parser | 103 |  |  |
|ALG326-1.p                                                   |    0.095s | 21.428MiB| error | 0 |  |  |
|ALG129+1.p                                                   |    0.096s | 20.82MiB| error | 0 |  |  |
|ALG126+1.p                                                   |    0.096s | 20.684MiB| error | 0 |  |  |
|ALG114+1.p                                                   |    0.096s | 20.432MiB| error | 0 |  |  |
|ALG411-1.p                                                   |    0.096s | 22.712MiB| error | 0 |  |  |
|ALG410-1.p                                                   |    0.096s | 23.5MiB| error | 0 |  |  |
|ALG230+2.p                                                   |    0.096s | 19.524MiB| parser | 103 |  |  |
|ALG109+1.p                                                   |    0.097s | 20.924MiB| error | 0 |  |  |
|ALG157+1.p                                                   |    0.097s | 20.5MiB| error | 0 |  |  |
|ALG230+4.p                                                   |    0.097s | 19.896MiB| parser | 103 |  |  |
|ALG201+1.p                                                   |    0.097s | 20.956MiB| error | 0 |  |  |
|ALG100+1.p                                                   |    0.097s | 20.556MiB| error | 0 |  |  |
|ALG219+1.p                                                   |    0.097s | 21.136MiB| error | 0 |  |  |
|ALG061+1.p                                                   |    0.098s | 20.424MiB| error | 0 |  |  |
|ALG141+1.p                                                   |    0.098s | 20.264MiB| error | 0 |  |  |
|ALG431-1.p                                                   |    0.098s | 20.424MiB| error | 0 |  |  |
|ALG368-1.p                                                   |    0.098s | 20.772MiB| error | 0 |  |  |
|ALG263^3.p                                                   |    0.098s | 19.596MiB| parser | 103 |  |  |
|ALG225+3.p                                                   |    0.098s | 19.624MiB| parser | 103 |  |  |
|ALG052+1.p                                                   |    0.098s | 21.172MiB| error | 0 |  |  |
|ALG422-1.p                                                   |    0.099s | 23.908MiB| error | 0 |  |  |
|ALG014+1.p                                                   |    0.099s | 20.012MiB| error | 0 |  |  |
|ALG381-1.p                                                   |    0.099s | 23.424MiB| error | 0 |  |  |
|ALG190+1.p                                                   |    0.100s | 20.524MiB| error | 0 |  |  |
|ALG218+1.p                                                   |    0.100s | 20.884MiB| error | 0 |  |  |
|ALG225+1.p                                                   |    0.101s | 20.484MiB| error | 0 |  |  |
|ALG351-1.p                                                   |    0.101s | 21.908MiB| error | 0 |  |  |
|ALG149+1.p                                                   |    0.101s | 20.324MiB| error | 0 |  |  |
|ALG420-1.p                                                   |    0.101s | 22.852MiB| error | 0 |  |  |
|ALG090+1.p                                                   |    0.102s | 20.004MiB| error | 0 |  |  |
|ALG432-1.p                                                   |    0.103s | 24.344MiB| error | 0 |  |  |
|ALG263^1.p                                                   |    0.104s | 20.052MiB| parser | 103 |  |  |
|ALG067+1.p                                                   |    0.104s | 20.396MiB| error | 0 |  |  |
|ALG122+1.p                                                   |    0.105s | 20.616MiB| error | 0 |  |  |
|ALG150+1.p                                                   |    0.105s | 20.304MiB| error | 0 |  |  |
|ALG134+1.p                                                   |    0.105s | 20.484MiB| error | 0 |  |  |
|ALG163+1.p                                                   |    0.105s | 20.436MiB| error | 0 |  |  |
|ALG227+4.p                                                   |    0.106s | 19.528MiB| parser | 103 |  |  |
|ALG262^2.p                                                   |    0.106s | 19.436MiB| parser | 103 |  |  |
|ALG077+1.p                                                   |    0.106s | 20.268MiB| error | 0 |  |  |
|ALG330-1.p                                                   |    0.106s | 21.524MiB| error | 0 |  |  |
|ALG343-1.p                                                   |    0.106s | 24.136MiB| error | 0 |  |  |
|ALG222+1.p                                                   |    0.106s | 21.0MiB| error | 0 |  |  |
|ALG024+1.p                                                   |    0.107s | 20.276MiB| error | 0 |  |  |
|ALG396-1.p                                                   |    0.107s | 23.776MiB| error | 0 |  |  |
|ALG361-1.p                                                   |    0.107s | 23.34MiB| error | 0 |  |  |
|ALG049+1.p                                                   |    0.107s | 21.924MiB| error | 0 |  |  |
|ALG155+1.p                                                   |    0.107s | 20.536MiB| error | 0 |  |  |
|ALG339-1.p                                                   |    0.107s | 22.528MiB| error | 0 |  |  |
|ALG183+1.p                                                   |    0.107s | 20.372MiB| error | 0 |  |  |
|ALG016+1.p                                                   |    0.108s | 20.916MiB| error | 0 |  |  |
|ALG258^1.p                                                   |    0.108s | 19.868MiB| parser | 103 |  |  |
|ALG406-1.p                                                   |    0.108s | 23.06MiB| error | 0 |  |  |
|ALG099+1.p                                                   |    0.108s | 20.94MiB| error | 0 |  |  |
|ALG221+1.p                                                   |    0.108s | 21.648MiB| error | 0 |  |  |
|ALG430-1.p                                                   |    0.109s | 20.36MiB| error | 0 |  |  |
|ALG363-1.p                                                   |    0.109s | 20.4MiB| error | 0 |  |  |
|ALG319-1.p                                                   |    0.109s | 21.136MiB| error | 0 |  |  |
|ALG400-1.p                                                   |    0.109s | 22.92MiB| error | 0 |  |  |
|ALG191+1.p                                                   |    0.109s | 20.632MiB| error | 0 |  |  |
|ALG403-1.p                                                   |    0.109s | 22.664MiB| error | 0 |  |  |
|ALG116+1.p                                                   |    0.110s | 20.612MiB| error | 0 |  |  |
|ALG345-1.p                                                   |    0.110s | 22.088MiB| error | 0 |  |  |
|ALG365-1.p                                                   |    0.111s | 24.0MiB| error | 0 |  |  |
|ALG389-1.p                                                   |    0.111s | 20.832MiB| error | 0 |  |  |
|ALG374-1.p                                                   |    0.112s | 22.844MiB| error | 0 |  |  |
|ALG395-1.p                                                   |    0.112s | 23.944MiB| error | 0 |  |  |
|ALG088+1.p                                                   |    0.112s | 20.608MiB| error | 0 |  |  |
|ALG096+1.p                                                   |    0.112s | 20.588MiB| error | 0 |  |  |
|ALG362-1.p                                                   |    0.112s | 23.88MiB| error | 0 |  |  |
|ALG227+2.p                                                   |    0.113s | 19.596MiB| parser | 103 |  |  |
|ALG048+1.p                                                   |    0.113s | 21.02MiB| error | 0 |  |  |
|ALG105+1.p                                                   |    0.113s | 20.824MiB| error | 0 |  |  |
|ALG070+1.p                                                   |    0.113s | 22.36MiB| error | 0 |  |  |
|ALG416-1.p                                                   |    0.113s | 22.388MiB| error | 0 |  |  |
|ALG174+1.p                                                   |    0.114s | 20.124MiB| error | 0 |  |  |
|ALG354-1.p                                                   |    0.114s | 22.164MiB| error | 0 |  |  |
|ALG055+1.p                                                   |    0.114s | 20.624MiB| error | 0 |  |  |
|ALG093+1.p                                                   |    0.114s | 20.216MiB| error | 0 |  |  |
|ALG418-1.p                                                   |    0.114s | 23.5MiB| error | 0 |  |  |
|ALG371-1.p                                                   |    0.115s | 22.26MiB| error | 0 |  |  |
|ALG373-1.p                                                   |    0.115s | 22.156MiB| error | 0 |  |  |
|ALG166+1.p                                                   |    0.115s | 21.06MiB| error | 0 |  |  |
|ALG119+1.p                                                   |    0.115s | 20.588MiB| error | 0 |  |  |
|ALG011-1.p                                                   |    0.117s | 20.432MiB| error | 0 |  |  |
|ALG419-1.p                                                   |    0.117s | 25.128MiB| error | 0 |  |  |
|ALG405-1.p                                                   |    0.117s | 21.28MiB| error | 0 |  |  |
|ALG215+2.p                                                   |    0.118s | 19.936MiB| parser | 103 |  |  |
|ALG364-1.p                                                   |    0.118s | 27.428MiB| error | 0 |  |  |
|ALG059+1.p                                                   |    0.118s | 20.704MiB| error | 0 |  |  |
|ALG370-1.p                                                   |    0.119s | 23.32MiB| error | 0 |  |  |
|ALG372-1.p                                                   |    0.119s | 24.092MiB| error | 0 |  |  |
|ALG212-10.p                                                  |    0.120s | 19.64MiB| parser | 103 |  |  |
|ALG434-1.p                                                   |    0.120s | 22.484MiB| error | 0 |  |  |
|ALG367-1.p                                                   |    0.121s | 24.024MiB| error | 0 |  |  |
|ALG269^4.p                                                   |    0.121s | 19.824MiB| parser | 103 |  |  |
|ALG234+1.p                                                   |    0.121s | 21.128MiB| error | 0 |  |  |
|ALG062+1.p                                                   |    0.121s | 20.656MiB| error | 0 |  |  |
|ALG423-1.p                                                   |    0.122s | 23.948MiB| error | 0 |  |  |
|ALG075+1.p                                                   |    0.122s | 20.648MiB| error | 0 |  |  |
|ALG145+1.p                                                   |    0.123s | 20.356MiB| error | 0 |  |  |
|ALG026+1.p                                                   |    0.123s | 22.372MiB| error | 0 |  |  |
|ALG425-1.p                                                   |    0.123s | 24.352MiB| error | 0 |  |  |
|ALG352-1.p                                                   |    0.124s | 23.972MiB| error | 0 |  |  |
|ALG342-1.p                                                   |    0.124s | 22.692MiB| error | 0 |  |  |
|ALG417-1.p                                                   |    0.124s | 22.712MiB| error | 0 |  |  |
|ALG438-1.p                                                   |    0.125s | 22.224MiB| error | 0 |  |  |
|ALG344-1.p                                                   |    0.125s | 23.9MiB| error | 0 |  |  |
|ALG366-1.p                                                   |    0.125s | 25.784MiB| error | 0 |  |  |
|ALG018+1.p                                                   |    0.126s | 20.624MiB| error | 0 |  |  |
|ALG086+1.p                                                   |    0.126s | 20.24MiB| error | 0 |  |  |
|ALG185+1.p                                                   |    0.126s | 20.4MiB| error | 0 |  |  |
|ALG180+1.p                                                   |    0.127s | 20.216MiB| error | 0 |  |  |
|ALG051+1.p                                                   |    0.128s | 21.404MiB| error | 0 |  |  |
|ALG386-1.p                                                   |    0.128s | 23.348MiB| error | 0 |  |  |
|ALG115+1.p                                                   |    0.132s | 20.564MiB| error | 0 |  |  |
|ALG140+1.p                                                   |    0.133s | 20.296MiB| error | 0 |  |  |
|ALG382-1.p                                                   |    0.134s | 25.104MiB| error | 0 |  |  |
|ALG391-1.p                                                   |    0.134s | 20.888MiB| error | 0 |  |  |
|ALG378-1.p                                                   |    0.137s | 23.076MiB| error | 0 |  |  |
|ALG198+1.p                                                   |    0.142s | 19.772MiB| error | 0 |  |  |
|ALG376-1.p                                                   |    0.142s | 27.432MiB| error | 0 |  |  |
|ALG433-1.p                                                   |    0.142s | 23.784MiB| error | 0 |  |  |
|ALG413-1.p                                                   |    0.142s | 24.856MiB| error | 0 |  |  |
|ALG412-1.p                                                   |    0.143s | 24.516MiB| error | 0 |  |  |
|ALG356-1.p                                                   |    0.143s | 25.552MiB| error | 0 |  |  |
|ALG427-1.p                                                   |    0.144s | 24.42MiB| error | 0 |  |  |
|ALG435-1.p                                                   |    0.144s | 23.984MiB| error | 0 |  |  |
|ALG385-1.p                                                   |    0.147s | 25.42MiB| error | 0 |  |  |
|ALG053+1.p                                                   |    0.149s | 21.28MiB| error | 0 |  |  |
|ALG379-1.p                                                   |    0.149s | 24.992MiB| error | 0 |  |  |
|ALG401-1.p                                                   |    0.150s | 25.908MiB| error | 0 |  |  |
|ALG383-1.p                                                   |    0.152s | 25.424MiB| error | 0 |  |  |
|ALG375-1.p                                                   |    0.155s | 27.548MiB| error | 0 |  |  |
|ALG426-1.p                                                   |    0.156s | 24.14MiB| error | 0 |  |  |
|ALG424-1.p                                                   |    0.156s | 24.976MiB| error | 0 |  |  |
|ALG346-1.p                                                   |    0.158s | 26.376MiB| error | 0 |  |  |
|ALG357-1.p                                                   |    0.158s | 23.56MiB| error | 0 |  |  |
|ALG421-1.p                                                   |    0.163s | 24.288MiB| error | 0 |  |  |
|ALG377-1.p                                                   |    0.164s | 23.192MiB| error | 0 |  |  |
|ALG388-1.p                                                   |    0.166s | 26.58MiB| error | 0 |  |  |
|ALG398-1.p                                                   |    0.166s | 24.756MiB| error | 0 |  |  |
|ALG347-1.p                                                   |    0.171s | 25.856MiB| error | 0 |  |  |
|ALG402-1.p                                                   |    0.177s | 24.932MiB| error | 0 |  |  |
|ALG428-1.p                                                   |    0.177s | 24.184MiB| error | 0 |  |  |
|ALG415-1.p                                                   |    0.179s | 24.828MiB| error | 0 |  |  |
|ALG380-1.p                                                   |    0.179s | 28.432MiB| error | 0 |  |  |
|ALG429-1.p                                                   |    0.180s | 26.504MiB| error | 0 |  |  |
|ALG197+1.p                                                   |    0.181s | 22.82MiB| error | 0 |  |  |
|ALG195+1.p                                                   |    0.183s | 22.776MiB| error | 0 |  |  |
|ALG359-1.p                                                   |    0.185s | 24.912MiB| error | 0 |  |  |
|ALG027+1.p                                                   |    0.185s | 24.592MiB| error | 0 |  |  |
|ALG335-1.p                                                   |    0.186s | 28.824MiB| error | 0 |  |  |
|ALG387-1.p                                                   |    0.186s | 23.78MiB| error | 0 |  |  |
|ALG360-1.p                                                   |    0.190s | 26.356MiB| error | 0 |  |  |
|ALG355-1.p                                                   |    0.194s | 36.284MiB| error | 0 |  |  |
|ALG337-1.p                                                   |    0.204s | 33.444MiB| error | 0 |  |  |
|ALG408-1.p                                                   |    0.208s | 29.168MiB| error | 0 |  |  |
|ALG279^5.p                                                   |    0.212s | 39.584MiB| error | 0 |  |  |
|ALG336-1.p                                                   |    0.215s | 29.34MiB| error | 0 |  |  |
|ALG034+1.p                                                   |    0.227s | 21.884MiB| error | 0 |  |  |
|ALG338-1.p                                                   |    0.236s | 28.724MiB| error | 0 |  |  |
|ALG196+1.p                                                   |    0.245s | 26.692MiB| error | 0 |  |  |
|ALG035+1.p                                                   |    0.472s | 22.708MiB| error | 0 |  |  |
|ALG003-1.p                                                   |    1.303s | 240.0MiB| error | 0 |  |  |
|ALG285^5.p                                                   |   20.013s | 39.416MiB| timeout | 0 |  |  |
|ALG069+1.p                                                   |   20.022s | 114.0MiB| timeout | 0 |  |  |
|ALG008-10.p                                                  |   20.049s | 188.0MiB| timeout | 0 |  |  |
|ALG288^5.p                                                   |   20.053s | 87.116MiB| timeout | 0 |  |  |
|ALG302-1.p                                                   |   20.056s | 122.0MiB| timeout | 0 |  |  |
|ALG293^5.p                                                   |   20.057s | 137.0MiB| timeout | 0 |  |  |
|ALG299-1.p                                                   |   20.059s | 242.0MiB| timeout | 0 |  |  |
|ALG294^5.p                                                   |   20.060s | 160.0MiB| timeout | 0 |  |  |
|ALG300-1.p                                                   |   20.068s | 122.0MiB| timeout | 0 |  |  |
|ALG289^5.p                                                   |   20.071s | 42.344MiB| timeout | 0 |  |  |
|ALG298^5.p                                                   |   20.081s | 42.56MiB| timeout | 0 |  |  |
|ALG441-10.p                                                  |   20.084s | 488.0MiB| timeout | 0 |  |  |
|ALG282^5.p                                                   |   20.085s | 948.0MiB| timeout | 0 |  |  |
|ALG284^5.p                                                   |   20.092s | 54.316MiB| timeout | 0 |  |  |
|ALG290^5.p                                                   |   20.094s | 101.0MiB| timeout | 0 |  |  |
|ALG292^5.p                                                   |   20.108s | 38.656MiB| timeout | 0 |  |  |
|ALG276^5.p                                                   |   20.115s | 1072.0MiB| timeout | 0 |  |  |
|ALG301-1.p                                                   |   20.119s | 113.0MiB| timeout | 0 |  |  |
|ALG008-1.p                                                   |   20.122s | 922.0MiB| timeout | 0 |  |  |
|ALG236-1.p                                                   |   20.133s | 848.0MiB| timeout | 0 |  |  |
|ALG272^5.p                                                   |   20.133s | 315.0MiB| timeout | 0 |  |  |
|ALG291^5.p                                                   |   20.136s | 895.0MiB| timeout | 0 |  |  |
|ALG273^5.p                                                   |   20.137s | 1081.0MiB| timeout | 0 |  |  |
|ALG239-1.p                                                   |   20.154s | 1251.0MiB| timeout | 0 |  |  |
|ALG271^5.p                                                   |   20.161s | 1319.0MiB| timeout | 0 |  |  |
|ALG274^5.p                                                   |   20.164s | 1451.0MiB| timeout | 0 |  |  |
|ALG010-1.p                                                   |   20.167s | 1802.0MiB| timeout | 0 |  |  |
|ALG006-1.p                                                   |   20.169s | 1812.0MiB| timeout | 0 |  |  |
|ALG278^5.p                                                   |   20.187s | 2149.0MiB| timeout | 0 |  |  |
|ALG280^5.p                                                   |   20.232s | 1485.0MiB| timeout | 0 |  |  |
|ALG007-1.p                                                   |   20.250s | 2171.0MiB| timeout | 0 |  |  |
|ALG244-1.p                                                   |   20.258s | 1883.0MiB| timeout | 0 |  |  |
|ALG005-1.p                                                   |   20.259s | 2675.0MiB| timeout | 0 |  |  |
|ALG283^5.p                                                   |   20.291s | 2739.0MiB| timeout | 0 |  |  |
|ALG211+1.p                                                   |   20.312s | 2872.0MiB| timeout | 0 |  |  |
|ALG243-1.p                                                   |   20.360s | 3646.0MiB| timeout | 0 |  |  |
|ALG246-1.p                                                   |   20.371s | 3737.0MiB| timeout | 0 |  |  |
|ALG281^5.p                                                   |   20.380s | 4261.0MiB| timeout | 0 |  |  |
|ALG237-1.p                                                   |   20.387s | 3749.0MiB| timeout | 0 |  |  |
|ALG242-1.p                                                   |   20.392s | 3151.0MiB| timeout | 0 |  |  |
|ALG238-1.p                                                   |   20.394s | 3797.0MiB| timeout | 0 |  |  |
|ALG241-1.p                                                   |   20.398s | 3900.0MiB| timeout | 0 |  |  |
|ALG275^5.p                                                   |   20.399s | 3803.0MiB| timeout | 0 |  |  |
|ALG074+1.p                                                   |   20.408s | 4318.0MiB| timeout | 0 |  |  |
|ALG072+1.p                                                   |   20.437s | 4152.0MiB| timeout | 0 |  |  |
|ALG296^5.p                                                   |   20.502s | 4727.0MiB| timeout | 0 |  |  |
|ALG004-1.p                                                   |   20.559s | 5399.0MiB| timeout | 0 |  |  |
