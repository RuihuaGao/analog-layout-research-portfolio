---
layout: default
title: Generator Architecture
description: >
  Reusable structure-aware analog layout generators
  built from primitives and common row-level structures.
permalink: /generators/
---

# Generator Architecture

The automatic generator is organized as a reuse hierarchy rather than
as a collection of independent layout scripts.

Complex analog modules reuse lower-level physical constructions and add
only the placement and routing policies required by their circuit
topology.

## Reuse Hierarchy

<div class="hierarchy-level" markdown="1">

### Primitive Level

**MOS device primitive**  
Local active region, multifinger gate structure, contacts, and terminal
access.

**MIM capacitor primitive**  
Passive capacitor construction with explicit plate terminals.

</div>

<div class="hierarchy-arrow">↓</div>

<div class="hierarchy-level" markdown="1">

### Basic Structure Level

- single device;
- shared-diffusion interdigitation;
- split-diffusion interdigitation;
- common-centroid array.

</div>

<div class="hierarchy-arrow">↓</div>

<div class="hierarchy-level" markdown="1">

### Compound Module Level

- simple current mirror;
- differential pair;
- cascode current mirror;
- wide-swing current mirror;
- improved-Wilson current mirror;
- Wilson current mirror;
- cascoded differential pair;
- MIM capacitor module.

</div>

## Selected Generated Modules

### MOS-Based Modules

The MOS-based generator hierarchy reuses the common device primitive
through single-device, interdigitated, common-centroid, and compound
structures.

<div class="figure-grid generator-grid">

<div class="figure-card">

<img
  src="{{ '/assets/images/generators/single.png' | relative_url }}"
  alt="Generated single NMOS module"
>

<p class="figure-caption">
<strong>Single MOS.</strong>
Reusable device construction and local terminal access.
</p>

</div>

<div class="figure-card">

<img
  src="{{ '/assets/images/generators/current-mirror.png' | relative_url }}"
  alt="Generated simple current mirror"
>

<p class="figure-caption">
<strong>Simple current mirror.</strong>
Shared-diffusion interdigitation with local diode and gate routing.
</p>

</div>

<div class="figure-card">

<img
  src="{{ '/assets/images/generators/differential-pair.png' | relative_url }}"
  alt="Generated common-centroid differential pair"
>

<p class="figure-caption">
<strong>Differential pair.</strong>
Common-centroid placement with local dummies and pair-specific access.
</p>

</div>

<div class="figure-card">

<img
  src="{{ '/assets/images/generators/cascode-mirror.png' | relative_url }}"
  alt="Generated cascode current mirror"
>

<p class="figure-caption">
<strong>Cascode current mirror.</strong>
Compound structure assembled from reusable matched rows.
</p>

</div>

<div class="figure-card">

<img
  src="{{ '/assets/images/generators/wide-swing.png' | relative_url }}"
  alt="Generated wide-swing current mirror"
>

<p class="figure-caption">
<strong>Wide-swing current mirror.</strong>
Shared and source-isolated row structures with topology-specific routing.
</p>

</div>

<div class="figure-card">

<img
  src="{{ '/assets/images/generators/cascoded-differential-pair.png' | relative_url }}"
  alt="Generated cascoded differential pair"
>

<p class="figure-caption">
<strong>Cascoded differential pair.</strong>
A common-centroid input array combined with a split-diffusion cascode row.
</p>

</div>

</div>

### Passive Module

The capacitor generator forms a separate passive branch. It uses a MIM
capacitor primitive rather than the MOS-device construction, while
exporting the same type of module interface for top-level integration.

<div class="passive-module-card">

<img
  src="{{ '/assets/images/generators/mim-capacitor.png' | relative_url }}"
  alt="Generated MIM capacitor"
>

<p class="figure-caption">
<strong>MIM capacitor.</strong>
Simplified passive module with explicit plate access for top-level integration.
</p>

</div>

## Internal Closure

Local analog structure remains inside the generated child cell.

The generator is responsible for:

- device-finger ordering;
- dummy-device insertion;
- local gate and source buses;
- diode connections;
- internal stack-node connections;
- topology-specific local routing;
- local guard-ring construction where required.

The top-level flow does not reconstruct these structures.

Instead, each module exports a compact interface describing the
information needed by downstream physical-design stages.

## Public Generator Coverage

The prototype includes selected generator families covering:

- single MOS devices;
- two- and three-branch simple current mirrors;
- differential pairs;
- two- and three-branch cascode current mirrors;
- Wilson current mirrors;
- improved-Wilson current mirrors;
- wide-swing current mirrors;
- cascoded differential pairs;
- MIM capacitors.

The generator library is a prototype rather than a complete layout
synthesis library for arbitrary analog topologies.

## Why Reuse Matters

The central implementation idea is not merely that several circuit
topologies can be drawn automatically.

The same low-level layout construction is reused across different
families. The family-specific behaviour is concentrated in:

- row organization;
- diffusion-sharing policy;
- dummy policy;
- relative placement;
- local buses;
- guard-ring policy;
- topology-specific routing.

This reduces duplicated layout logic and keeps related module families
physically consistent.

<div class="page-links">
Next:
<a href="{{ '/placement/' | relative_url }}">
analog-aware placement
</a>.
</div>