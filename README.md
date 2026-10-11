# VerifierProbe

Investigating whether α,β-CROWN's safety certificates hold under the model's actual float32 execution.

VerifierProbe is a research toolbox for studying the relationship between a neural network verifier's safety certificates, its intermediate computations, and the model's actual floating-point behavior.

## Objectives

1. **Adversarial probing:** Search for inputs inside a certified region `X` that α,β-CROWN certifies as safe but that the model misclassifies under float32 execution.
2. **Intermediate-state introspection:** Capture relevant internal verification computations in a structured, JSON-serializable trace.
3. **Reproducible investigation:** Record configurations, random seeds, verification outcomes, and the evidence supporting each result.

If no violation is found within the configured search budget, the tool reports the closest observed near-miss and explains the evidence.

## Selected verifier

[α,β-CROWN](https://github.com/Verified-Intelligence/alpha-beta-CROWN), with its bound-propagation engine [auto_LiRPA](https://github.com/Verified-Intelligence/auto_LiRPA).

The project investigates its bound-propagation and branch-and-bound verification process.

**Target model:** a small LeNet-style CNN trained on MNIST.

## Background

_TODO: sound and complete for α,β-CROWN; the numerical gap between verification and float32 execution; what Jia & Rinard demonstrated and what remains open._

## Required interface

```python
from verifier_probe import Attacker, Prober

atk = Attacker(model, verifier, config)
adv_x, atk_logs = atk.run(X)

prober = Prober(verifier)
trace = prober.dump(model, x)
```

The implementation also provides a verifier adapter with a `certify(model, X)` method and access to the relevant internal state.

## Research direction

The initial goal is to understand how floating-point numerical error can affect neural network verification, using Jia and Rinard's *Exploiting Verified Neural Networks via Floating Point Numerical Error* (2021) as a starting point.

The specific research hypothesis and contribution remain open pending investigation of the paper, α,β-CROWN's implementation, and related work.

## Install

_TODO_

## Trace schema

_TODO: every field documented with meaning, shape, and units._

## Results

_TODO: honest report of whether a genuine soundness violation was found, and why._

## Development status

- [ ] Understand the reference paper and attack mechanism
- [ ] Set up α,β-CROWN and reproduce a baseline verification run
- [ ] Implement the verifier adapter
- [ ] Define and implement the intermediate-state trace
- [ ] Develop and evaluate the numerical-error-guided attack
- [ ] Test safe and unsafe toy properties
- [ ] Investigate and document the research contribution
- [ ] Validate reproducibility and document limitations

## Research integrity

A verifier result alone is not evidence of a floating-point soundness violation. Any claimed violation must include the verification outcome, the concrete input, and an independent evaluation of the model's actual behavior.

The model's behavior is evaluated under a fixed, recorded execution setting (dtype, device, PyTorch version, determinism flags). A counterexample is stored as its exact float32 tensor, not a printed or rounded value, and its membership in `X` is checked in that same precision.

Near-misses are reported as near-misses, not as successful attacks.

## References

- K. Jia and M. Rinard. *Exploiting Verified Neural Networks via Floating Point Numerical Error.* SAS 2021. [arXiv:2003.03021](https://arxiv.org/abs/2003.03021)
- α,β-CROWN: [github.com/Verified-Intelligence/alpha-beta-CROWN](https://github.com/Verified-Intelligence/alpha-beta-CROWN)
