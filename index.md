---
layout: default
title: Overview
description: >
  A research portfolio on analog layout techniques,
  structure-aware generation, placement, and routing.
---

# Research Overview

Analog circuit netlists specify electrical connectivity, but they do not fully express the physical constraints and design intent needed for layout. Device matching, local environment, guard rings, well-domain relations, passive-device access, and routing geometry must still be handled explicitly during physical implementation.

This research investigates how selected analog layout techniques can be studied, organized, and translated into reusable concepts for automatic physical design.

<div class="callout">
<strong>Central idea.</strong>
Analog-aware metadata provides the connection between manual layout intent and automatic physical implementation.
</div>

## Research Path

The work follows a manual-to-automatic sequence:

1. **Manual layout study**  
   Selected layout techniques are examined using controlled variants of a two-stage operational amplifier.

2. **Technique–effect–metric framework**  
   Each layout technique is related to its intended physical effect, suitable evaluation method, implementation trade-off, and automation implication.

3. **Automation-oriented representation**  
   Generated analog modules preserve boundaries, placement footprints, terminals, routing access, guard-ring geometry, and domain information.

4. **Reusable module generation**  
   Primitive and row-level generators are composed into current mirrors, differential pairs, cascoded structures, Wilson-type mirrors, and MIM capacitor modules.

5. **Analog-aware placement**  
   Multistart B*-tree placement combines generic and circuit-aware initial configurations.

6. **Well-domain repair and hierarchical assembly**  
   Post-placement repair handles n-well spacing and supports compact same-domain connections.

7. **Access-aware top-level routing**  
   Guard-ring, point-to-point, and multi-terminal connections are handled by separate routing policies.

## Explore the Research

<div class="card-grid">

<div class="card">
<h3>Manual Layout Study</h3>

<p>
Controlled layout variants examine spacing, guard-ring isolation,
and matching-oriented placement.
</p>

<p>
<a href="{{ '/manual-study/' | relative_url }}">
View the manual study →
</a>
</p>

</div>

<div class="card">
<h3>Conceptual Framework</h3>

<p>
Manual observations are organized by physical effect,
evaluation metric, trade-off, and automation implication.
</p>

<p>
<a href="{{ '/framework/' | relative_url }}">
View the framework →
</a>
</p>

</div>

<div class="card">
<h3>Automation-Oriented Representation</h3>

<p>
Module geometry and selected analog-aware metadata are preserved
as explicit interfaces for placement, assembly, and routing.
</p>

<p>
<a href="{{ '/representation/' | relative_url }}">
View the representation →
</a>
</p>

</div>

<div class="card">
<h3>Generator Architecture</h3>

<p>
Reusable device and row-level structures are composed into
selected analog module families.
</p>

<p>
<a href="{{ '/generators/' | relative_url }}">
Explore the generators →
</a>
</p>

</div>

<div class="card">
<h3>Analog-Aware Placement</h3>

<p>
Multistart B*-tree placement combines generic and analog-informed seed
topologies with local search and post-placement n-well-domain repair.
</p>

<p>
<a href="{{ '/placement/' | relative_url }}">
Explore the placement method →
</a>
</p>

</div>

<div class="card">
<h3>Access-Aware Routing</h3>

<p>
Generated module interfaces are converted into effective endpoints,
access-safe routing objects, separated net classes, and incremental
top-level routing trees.
</p>

<p>
<a href="{{ '/routing/' | relative_url }}">
Explore the routing method →
</a>
</p>

</div>

<div class="card">
<h3>Full-Flow Case Study</h3>

<p>
Generated modules are placed, repaired for well-domain constraints,
assembled hierarchically, and routed at top level.
</p>

<p>
<a href="{{ '/case-study/' | relative_url }}">
View the case study →
</a>
</p>

</div>

</div>

## Public Portfolio Scope

This website presents the research motivation, methodology, selected figures, and aggregated results of the project.

Source code, raw netlists, layout databases, generated metadata, raw simulation data, and institutional project artifacts are not publicly distributed.

<div class="page-links">
Continue with
<a href="{{ '/scope-and-validation/' | relative_url }}">Scope &amp; Validation</a>
or read the
<a href="{{ '/about/' | relative_url }}">project background</a>.
</div>