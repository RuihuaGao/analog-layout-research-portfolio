---
layout: default
title: Conceptual Framework
description: >
  A technique-effect-metric-trade-off framework
  connecting manual analog layout knowledge with automation.
permalink: /framework/
---

# Conceptual Framework

The manual experiments show that analog layout techniques are not
independent drawing rules. Each technique responds to a particular
layout situation, targets a physical effect, requires an appropriate
evaluation method, and introduces implementation trade-offs.

The conceptual framework organizes this reasoning before it is
translated into automation.

<div class="research-flow" aria-label="Conceptual framework flow">

  <div class="research-flow-step">
    <strong>Layout-Relevant Structure</strong>
    <span>Circuit Role and Physical Context</span>
  </div>

  <div class="research-flow-arrow" aria-hidden="true">→</div>

  <div class="research-flow-step">
    <strong>Layout Technique</strong>
    <span>Matching, Isolation, or Access Strategy</span>
  </div>

  <div class="research-flow-arrow" aria-hidden="true">→</div>

  <div class="research-flow-step">
    <strong>Targeted Physical Effect</strong>
    <span>Mismatch, Coupling, or Parasitics</span>
  </div>

  <div class="research-flow-arrow" aria-hidden="true">→</div>

  <div class="research-flow-step">
    <strong>Evaluation Metric</strong>
    <span>Effect-Specific Measurement</span>
  </div>

  <div class="research-flow-arrow" aria-hidden="true">→</div>

  <div class="research-flow-step">
    <strong>Implementation Trade-Off</strong>
    <span>Area, Complexity, and Routability</span>
  </div>

  <div class="research-flow-arrow" aria-hidden="true">→</div>

  <div class="research-flow-step">
    <strong>Automation Implication</strong>
    <span>Generator or Downstream Policy</span>
  </div>

</div>

## Why This Abstraction Is Needed

Two devices implemented in the same technology may require different
layout treatment because their circuit roles are different.

A differential pair, for example, is primarily a pair-matching and
symmetry problem. A current mirror additionally requires compact ratio
matching, gate sharing, and local diode routing. A guard ring addresses
substrate/well coupling rather than matching. A compensation
capacitor introduces plate-specific access and top-level routing concerns.

The circuit role and physical intent therefore need to survive beyond
the schematic or netlist.

## Technique–Effect–Metric–Trade-Off Mapping

The studied layout techniques can be summarized by the physical effect
they target, the metric used to evaluate that effect, the associated
implementation trade-off, and the corresponding automation implication.

| Technique | Targeted physical effect | Evaluation metric | Main trade-off | Automation implication |
|---|---|---|---|---|
| Guard ring | Substrate and well-domain disturbance isolation | AC and transient disturbance transfer | Area | Represent guard-ring domain geometry and extended routing access |
| Interdigitation | Averaging of spatial process gradient and local-environment variation | Layout-aware Monte Carlo | Routing complexity | Use interdigitated generator patterns for matched current mirrors |
| Common centroid | Cancellation of spatial gradient mismatch through two-dimensional symmetry | Layout-aware Monte Carlo | Area and routing complexity | Use common-centroid generator patterns for differential pairs |
| Dummy devices | Reduction of edge-environment imbalance in matched arrays | Layout-aware Monte Carlo | Area and dummy routing | Attach dummy-device policies to matched generator families |
| Source / bulk planning | Body-effect control and substrate / well reference consistency | Bias and domain consistency | Body-domain connection constraints | Export source/bulk metadata and guard-ring relations |
| Access-aware routing | Avoidance of unnecessary detours and unsafe internal access | Route compactness and endpoint legality | Possible routing asymmetry | Separate boundary access, point-to-point routing, and multi-terminal routing |

<div class="callout">
<strong>Framework outcome.</strong>
The technique–effect–metric–trade-off mapping determines which layout
information should be preserved as generator metadata and which decisions
remain with placement, well-domain handling, or routing.
</div>

## From Technique to Machine-Usable Information

The framework identifies the layout information that must be preserved for later physical-design decisions. It is not intended as a predictive post-layout performance model.

Examples include:

- matching relations;
- local dummy policy;
- module role;
- source and bulk relation;
- guard-ring geometry;
- n-well domain membership;
- capacitor plate identity;
- routing-access class.

This information can then be exported by generated modules and used by
placement, hierarchical assembly, and routing.

## Composition of Layout Techniques

Analog structures rarely rely on only one technique.

A differential pair may combine:

- common-centroid placement;
- dummy devices;
- local source and drain buses;
- guard-ring isolation;
- explicit top-level access.

A cascoded current mirror may combine:

- both shared- and split-diffusion interdigitation matched rows;
- isolated stack nodes;
- topology-specific local routing;
- module-level guard-ring and bulk policies.

This compositional view motivates the reusable generator hierarchy
implemented in the automatic part of the work.

<div class="page-links">
Next:
<a href="{{ '/representation/' | relative_url }}">
automation-oriented representation
</a>.
</div>