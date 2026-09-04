---
layout: default
title: Analog-Aware Placement
description: >
  Multistart B*-tree placement combining generic packing,
  analog-aware seed topologies, local search, and
  post-placement n-well-domain repair.
permalink: /placement/
---

# Analog-Aware Placement

The generated analog modules are treated as hierarchical placement
blocks rather than being flattened into individual devices.

Each block enters placement with an effective placement footprint and
selected structural metadata. The placement stage then combines a
generic B*-tree floorplanning representation with several initialization
strategies that reflect analog structure, device polarity, circuit role,
and well-domain relations.

The objective is not to encode one fixed op-amp floorplan. Instead,
multiple candidate organizations are generated and optimized, allowing
generic compactness and analog-aware preferences to compete within the
same search framework.

## B*-Tree Representation

A B*-tree represents the relative placement of rectangular or
footprint-based modules using parent–child relations.

In the adopted convention:

- the **left child** is placed to the **right of its parent**;
- the **right child** starts from the parent's horizontal position and is
  packed **above the existing contour**.

For a parent module **P**:

- `P.left = A` means that module **A** begins at the right boundary of
  **P**;
- `P.right = B` means that module **B** is placed at the same starting
  x-coordinate as **P**, with its vertical position determined by the
  current contour.

The tree therefore defines horizontal placement relations, while contour
packing determines the lowest legal vertical position that avoids overlap.

This representation provides a compact combinatorial search space while
allowing the actual module dimensions and placement footprints to remain
unchanged.

![B*-tree representation and contour-packed floorplan]({{ '/assets/images/placement/bstar-tree-relations.png' | relative_url }})

<p class="figure-caption">
B*-tree representation and the corresponding contour-packed floorplan.
Left-child relations initialize right-of-parent placement, while
right-child relations initialize above-parent placement; the contour
determines the final vertical coordinate during packing.
</p>

## Placement Footprints

The placer operates on the effective placement geometry exported into the
placement problem rather than reconstructing the internal transistor
layout.

The effective footprint can account for:

- the generated module boundary;
- module-specific placement margins;
- additional spacing required around selected module classes;
- non-rectangular placement descriptions represented through a compact
  placement abstraction.

This separation allows local analog layout structure to remain inside the
generated child cell while the top-level placer works only with the
geometry required for packing and spacing.

## Multistart Strategy

A single initial B*-tree can strongly influence the result of a local
search. The placement stage therefore evaluates multiple initial
topologies.

The implementation contains both generic baseline seeds and
analog-informed seeds.

### Generic Seeds

Generic seeds provide topology-independent starting points, including:

- a simple left-chain organization;
- global area-descending ordering;
- a balanced area-descending tree;
- row-aware area-descending placement;
- stage-aware balanced variants.

These seeds provide baseline solutions and reduce dependence on any one
analog-specific assumption.

### Same-Well Seeds

When modules belong to the same n-well domain, two explicit orientation
seeds are available.

**Same-well horizontal**

Modules belonging to the same n-well cluster are initialized as a
left-chain. Under the adopted B*-tree convention, this produces a
row-like horizontal organization that is suitable for a local horizontal
n-well bridge.

**Same-well vertical**

The same cluster is initialized as a right-chain, producing a vertical
stack-like candidate suitable for a vertical n-well connection.

These are initialization strategies rather than universal placement
rules. They provide the optimizer with physically meaningful candidate
topologies while the objective function determines whether the resulting
placement remains competitive.

For the same-well-oriented and analog-row seeds, the corresponding
same-nwell clusters are preserved during local search so that a candidate
is not destroyed by moving only part of the cluster.

### Analog-Row-Compact Seed

The analog-row-compact seed is a circuit-aware initialization intended for
small two-stage op-amp structures when the required module roles have been
recognized.

It uses structure and polarity information to construct an initial
organization around roles such as:

- the differential-pair core;
- current-mirror or bias structures;
- source-connected PMOS modules;
- second-stage or side modules;
- the compensation capacitor.

The purpose of this seed is to provide a compact, routing-relevant analog
starting point rather than to impose a hard template.

If the expected analog-role pattern is not recognized, the implementation
falls back to the generic row-stack seed instead of forcing the
circuit-specific organization.

![Representative placement seed modes]({{ '/assets/images/placement/placement-seeds.png' | relative_url }})

<p class="figure-caption">
Representative initialization strategies ranging from generic
area-descending organization to row-aware, same-nwell, and
circuit-role-aware placement seeds.
</p>

**Abbreviations:** CCM = current-mirror-related module, e.g. cascode current mirror;
DP = differential-pair module;
PM0/PM1 = same-nwell PMOS modules;
OUT = output-related module;
CAP = compensation capacitor;
REF = remaining side/reference module.

## Local Search

Each seed is improved using a best-improvement hybrid hill-climbing
search.

At every iteration, two neighborhood classes are evaluated.

### Subtree Detach–Reattach

A non-root B*-tree subtree can be detached and reattached to another
available parent position as either a left or right child.

This changes the relative placement topology while preserving all modules
inside the moved subtree.

### Node-Label Swap

Two B*-tree nodes can exchange their associated module labels while the
tree topology itself remains unchanged.

This allows the search to test a different assignment of physical modules
to the same relative placement structure.

For each iteration, the search evaluates the available candidates from
both move classes and applies the move with the lowest improved objective
cost.

The process stops when no evaluated move improves the current solution or
when the configured iteration limit is reached.

Generic module rotation is not used as a neighborhood move in the
reported implementation. Generated modules retain their allowed
orientation rather than being freely rotated during B*-tree search.

## Placement Objective

Placement quality is evaluated using a weighted objective rather than
area alone.

The implemented cost model combines several classes of consideration:

| Objective class | Physical purpose |
|---|---|
| Compactness and aspect ratio | Limit wasted area and excessively elongated top-level layouts |
| Structural proximity | Keep strongly related circuit structures reasonably close |
| Vertical and relative ordering | Preserve useful PMOS/NMOS, stack, and stage relations |
| Region preference | Encourage modules toward appropriate PMOS, NMOS, or stage-local regions |
| Alignment | Encourage useful edge, stack, and current-mirror alignment |
| Internal and floating whitespace | Penalize unnecessary empty regions |
| Well-domain spacing | Discourage insufficient spacing between different n-well domains |
| Same-well-domain alignment | Encourage same-nwell modules to form regular, bridgeable local domains |
| Supply-related vertical preference | Keep source/bulk rail-related modules near useful stage-local positions |

These terms are soft preferences rather than a complete set of hard
analog constraints.

The multistart search therefore evaluates several physically different
initial organizations under the same objective and retains the
lowest-cost result.

## Best Seed Selection

Every selected seed mode is independently packed and locally optimized.

The final B*-tree result is chosen as the trial with the lowest final
objective cost.

This makes the seed modes complementary:

- generic seeds provide topology-independent baselines;
- same-well seeds emphasize local n-well organization;
- analog-row-compact introduces circuit-role-aware structure;
- stage-aware seeds provide alternative stage-level organizations.

A seed is therefore a starting hypothesis, not the final placement rule.

## Post-Placement Well-Domain Repair

The B*-tree objective contains well-related preferences, but a weighted
soft cost does not by itself guarantee that every different-nwell spacing
requirement is satisfied.

A separate post-placement repair stage is therefore applied after the
B*-tree search.

![Post-placement well-domain repair and same-nwell compaction]({{ '/assets/images/placement/well-domain-repair.png' | relative_url }})

<p class="figure-caption">
Post-placement well-domain handling. Different-nwell spacing violations
are removed through local repair, while an optional same-nwell cohesion
step can compact domain members to support a short, bridgeable local
n-well connection.
</p>

The repair stage evaluates the placed module geometry as a proxy for the
corresponding n-well-domain extent and locally shifts modules or domain
member groups to reduce remaining **different-nwell spacing violations**.

Candidate repairs are selected conservatively so that legalization does
not unnecessarily destroy an otherwise compact placement.

After different-nwell spacing has been repaired, an optional same-nwell
cohesion pass can move members of a shared n-well domain closer together.

This second pass supports:

- compact same-nwell islands;
- reduced dead space between same-domain modules;
- local n-well bridgeability;
- preservation of useful stage-local proximity where possible.

The resulting sequence is therefore:

**multistart B*-tree search → different-nwell spacing repair → optional
same-nwell-domain compaction**

rather than attempting to encode all n-well behavior as fixed module
keepout boxes before placement.

## Why Well Repair Is Separate

Inflating every PMOS module with a large fixed n-well keepout would make
different-domain spacing easy to enforce, but it would also separate
modules that intentionally belong to the same n-well domain.

That would work against the desired behavior for same-domain PMOS
structures, which may need to remain close enough to form one compact
physical well island or to be connected by a short local n-well bridge.

The adopted flow therefore distinguishes:

- **different-nwell domains**, which require legal spacing;
- **same-nwell domains**, which may benefit from compact alignment and
  bridgeability.

This distinction is carried from the module representation into placement
and the subsequent repair stage.

## Placement Flow

The complete placement sequence is summarized below.

<div class="placement-flow">

  <div class="placement-flow-step">
    <strong>Effective Footprints</strong>
    <span>&amp; Analog-Aware Metadata</span>
  </div>

  <div class="placement-flow-arrow">→</div>

  <div class="placement-flow-step">
    <strong>Multiple B*-Tree Seeds</strong>
    <span>Generic and Analog-Aware</span>
  </div>

  <div class="placement-flow-arrow">→</div>

  <div class="placement-flow-step">
    <strong>Contour Packing</strong>
    <span>Initial Legal Placement</span>
  </div>

  <div class="placement-flow-arrow">→</div>

  <div class="placement-flow-step">
    <strong>Local Search</strong>
    <span>Best-Improvement</span>
  </div>

  <div class="placement-flow-arrow">→</div>

  <div class="placement-flow-step">
    <strong>Best Seed Selection</strong>
    <span>Lowest Final Cost</span>
  </div>

  <div class="placement-flow-arrow">→</div>

  <div class="placement-flow-step">
    <strong>Different-Nwell Repair</strong>
    <span>Spacing Legalization</span>
  </div>

  <div class="placement-flow-arrow">→</div>

  <div class="placement-flow-step">
    <strong>Same-Nwell Compaction</strong>
    <span>Optional Cohesion</span>
  </div>

</div>

The repaired placement is then passed to hierarchical assembly, where
placement-dependent well connections and top-level routing can be added.

<div class="page-links">
Next:
<a href="{{ '/routing/' | relative_url }}">
access-aware top-level routing
</a>.
</div>