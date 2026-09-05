---
layout: default
title: Scope and Validation
description: >
  Public scope, validation boundary, and implementation availability.
permalink: /scope-and-validation/
---

# Scope and Validation

## Demonstrated Scope

The project addresses selected analog layout structures in an open-source SKY130-based environment. The implemented prototype covers:

- manual analysis of selected layout techniques;
- reusable generation of selected MOS and MIM capacitor structures;
- analog-aware multistart B*-tree placement;
- post-placement n-well spacing repair;
- hierarchical top-level assembly;
- guard-ring and bulk-domain routing;
- point-to-point signal routing;
- multi-terminal signal routing;
- flow-level interface and consistency checks.

The automatic flow is demonstrated using a representative two-stage operational-amplifier case. It is not presented as a general synthesis system for arbitrary analog circuits or process technologies.

## Validation Boundary

The reported validation concerns consistency between the main stages of the prototype:

- generated module interfaces;
- placement orientation and geometry;
- well-domain spacing and bridgeability;
- hierarchical cell assembly;
- guard-ring routing;
- point-to-point routing;
- multi-terminal routing.

These checks establish that the implemented stages exchange and use the expected information. They do not replace final physical and electrical verification.

Final DRC, LVS, extraction, and post-layout simulation must be reported separately when they have been performed in the corresponding verification environment.

## Simplified Evaluation Models

The manual guard-ring and layout-aware Monte Carlo studies use comparative models.

The guard-ring study is interpreted as a controlled body-domain sensitivity comparison rather than as calibrated substrate extraction. The matching-oriented study uses behavioural perturbations to compare residual gradient and edge-environment sensitivity. It is not a process-calibrated prediction of fabricated mismatch.

The corresponding numerical results should therefore be read as comparative evidence within the evaluated setup.

## Public Availability

This website is a public research portfolio derived from the master’s thesis.

It presents selected figures, summarized results, methodological
explanations, and technical documentation that describe the research and
the demonstrated prototype flow.

The underlying implementation source code is maintained separately.
University project files, including raw circuit netlists, simulation
testbenches, and simulation code, are also not distributed through this
public repository.