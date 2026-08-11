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

<div class="framework-flow">

<div class="flow-item">
Layout-relevant structure
</div>

<div class="flow-arrow">↓</div>

<div class="flow-item">
Layout technique
</div>

<div class="flow-arrow">↓</div>

<div class="flow-item">
Targeted physical effect
</div>

<div class="flow-arrow">↓</div>

<div class="flow-item">
Evaluation metric
</div>

<div class="flow-arrow">↓</div>

<div class="flow-item">
Implementation trade-off
</div>

<div class="flow-arrow">↓</div>

<div class="flow-item">
Automation implication
</div>

</div>

## Why This Abstraction Is Needed

Two devices implemented in the same technology may require different
layout treatment because their circuit roles are different.

A differential pair, for example, is primarily a pair-matching problem.
A current mirror additionally requires compact ratio matching, gate
sharing, and local diode routing. A guard ring addresses substrate or
well-domain control rather than matching. A compensation capacitor
introduces plate-specific access and top-level routing concerns.

The circuit role and physical intent therefore need to survive beyond
the schematic or netlist.

## Technique–Effect–Metric Mapping

| Layout-relevant structure | Technique | Targeted effect | Evaluation | Automation implication |
|---|---|---|---|---|
| Differential pair | Common centroid + dummies | Gradient and edge mismatch | Statistical spread and tails | Common-centroid generator |
| Current mirror | Interdigitation + dummies | Ratio mismatch and local environment | Statistical robustness | Interdigitation generator |
| Substrate / well domain | Guard ring | Disturbance coupling and body reference | AC and transient disturbance transfer | Domain metadata and bulk routing |
| Capacitor / extended terminal | Access-aware routing | Parasitic loading and unsafe access | Endpoint and access legality | Terminal-access policy |
| Source / bulk relation | Domain-aware connection | Body effect and well feasibility | Bias and domain consistency | Source/bulk and well metadata |

<div class="callout">
<strong>Central idea.</strong>
Analog-aware metadata provides the bridge between manual layout intent
and automatic physical implementation.
</div>

## From Technique to Machine-Usable Information

The framework does not attempt to predict complete post-layout circuit
performance from geometry. Its purpose is to identify which information
must be preserved.

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

- interdigitated matched rows;
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