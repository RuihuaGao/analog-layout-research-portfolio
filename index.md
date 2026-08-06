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

## Main Areas

<div class="card-grid">

<div class="card">
<h3>Manual Analysis</h3>
<p>
Controlled layout variants are used to study spacing,
guard-ring isolation, matching-oriented placement,
dummy devices, and routing access.
</p>
</div>

<div class="card">
<h3>Layout Representation</h3>
<p>
Generated modules retain both geometric information
and analog design intent for later placement,
routing, and hierarchical assembly.
</p>
</div>

<div class="card">
<h3>Automatic Generation</h3>
<p>
Reusable primitives and layout structures are composed
into selected analog generator families.
</p>
</div>

<div class="card">
<h3>Physical Integration</h3>
<p>
Placement, well-domain repair, hierarchical assembly,
and access-aware routing form one prototype flow.
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