---
layout: default
title: Manual Layout Study
description: >
  Controlled manual layout experiments used to identify
  physical effects, evaluation metrics, and automation implications.
permalink: /manual-study/
---

# Manual Layout Study

The automatic layout work was preceded by a manual study of selected
analog layout techniques using a two-stage operational amplifier.

The purpose of the study was not to identify one universally optimal
layout style. Instead, it was used to determine which physical effects
different layout techniques target, how those effects should be evaluated,
and which information should later be preserved by an automatic flow.

## Reference Layout Situations

The reference amplifier contains several recurring analog layout
situations:

- a matching-critical differential pair;
- current-mirror structures;
- output-connected devices;
- a Miller compensation capacitor;
- source and bulk domains;
- substrate and n-well isolation structures.

These situations later become either generated module families or
explicit physical-design metadata.

## Controlled Layout Variants

Four manual variants were used to separate the main layout decisions.

<div class="figure-grid manual-variant-grid">

<div class="figure-card">

<img
  class="manual-layout-image layout-a"
  src="{{ '/assets/images/manual/layout-a.png' | relative_url }}"
  alt="Manual layout variant A"
>

<p class="figure-caption">
<strong>Variant A.</strong>
Clustered baseline without guard rings.
Layout boundary: 36.7 µm × 29.6 µm.
</p>

</div>

<div class="figure-card">

<img
  class="manual-layout-image layout-b"
  src="{{ '/assets/images/manual/layout-b.png' | relative_url }}"
  alt="Manual layout variant B"
>

<p class="figure-caption">
<strong>Variant B.</strong>
Baseline with module-level guard-ring structures.
Layout boundary: 37.9 µm × 29.6 µm.
</p>

</div>

<div class="figure-card">

<img
  class="manual-layout-image layout-c"
  src="{{ '/assets/images/manual/layout-c.png' | relative_url }}"
  alt="Manual layout variant C"
>

<p class="figure-caption">
<strong>Variant C.</strong>
Matching-oriented variant using interdigitation.
Layout boundary: 42.9 µm × 29.6 µm.
</p>

</div>

<div class="figure-card">

<img
  class="manual-layout-image layout-d"
  src="{{ '/assets/images/manual/layout-d.png' | relative_url }}"
  alt="Manual layout variant D"
>

<p class="figure-caption">
<strong>Variant D.</strong>
Matching-oriented variant with a common-centroid differential pair.
Layout boundary: 42.9 µm × 33.2 µm.
</p>

</div>

</div>

The comparisons are organized as controlled changes:

- **A → B:** guard-ring insertion;
- **B → C / D:** matching-oriented placement.

The schematic topology remains unchanged while the selected physical
layout decision is varied.

## Spacing Sensitivity

A spacing sweep was used to test whether increasing the physical
separation between selected layout groups strongly changes the amplifier
response in the evaluated setup.

| Metric | Pre-layout | 1.0 s₀ | 3.0 s₀ | Observation |
|---|---:|---:|---:|---|
| GBW | 9.751 MHz | 9.574 MHz | 9.574 MHz | Sweep remains essentially flat |
| Phase margin | 70.609° | 70.092° | 70.095° | Negligible change |
| Positive slew rate | 27.042 V/µs | 26.133 V/µs | 26.120 V/µs | Small monotonic change |
| Selected critical parasitics | — | baseline | < 0.5% change | Spacing is not dominant here |

<div class="callout">
<strong>Interpretation.</strong>
For this circuit and extraction setup, the evaluated spacing range
has only a limited influence on the selected performance metrics.
This is a useful negative result: spacing should not automatically
be treated as the dominant layout variable when stronger physical
effects are present.
</div>

## Guard-Ring Isolation

Guard-ring insertion targets substrate and well-domain disturbance
rather than device matching. The two manual layouts are therefore
compared using a controlled body-domain disturbance model.

![AC disturbance-transfer comparison with and without guard rings]({{ '/assets/images/manual/guard-ring-ac.png' | relative_url }})

<p class="figure-caption">
AC disturbance-transfer comparison. The guarded variant exhibits a
smaller output response under the adopted body-domain sensitivity model.
</p>

![Transient disturbance comparison with and without guard rings]({{ '/assets/images/manual/guard-ring-transient.png' | relative_url }})

<p class="figure-caption">
Transient comparison under a pulsed aggressor. The main disturbance
appears around pulse transitions, while the guarded case shows a smaller
response.
</p>

The model uses normalized disturbance-transfer factors to distinguish
the guarded and unguarded body domains.

<div class="callout">
<strong>Model boundary.</strong>
This experiment is a comparative body-domain sensitivity study.
It is not a calibrated substrate-extraction model and the numerical
attenuation should not be interpreted as a silicon-accurate prediction
of guard-ring isolation.
</div>

## Matching-Oriented Evaluation

Matching-oriented layout techniques are evaluated statistically because
their intended benefit appears primarily in mismatch sensitivity,
distribution spread, and tail behaviour rather than as a large shift
of the nominal operating point.

![ECDF of absolute input offset for clustered, interdigitated, and common-centroid layouts]({{ '/assets/images/manual/offset-ecdf.png' | relative_url }})

<p class="figure-caption">
Empirical cumulative distribution of absolute input offset for the
three matching-layout variants.
</p>

| Metric | Clustered B | Interdigitated C | Common centroid D |
|---|---:|---:|---:|
| std(\|Voffset\|) | 2.67 mV | 2.45 mV | 2.44 mV |
| p90(\|Voffset\|) | 7.64 mV | 7.23 mV | 7.27 mV |
| p10(CMRR) | 62.57 dB | 63.02 dB | 63.03 dB |
| p10(PSRR) | 61.77 dB | 62.16 dB | 62.17 dB |

The mean values remain close. The clearer difference appears in spread
and distribution tails. Variants C and D also remain close under the
adopted simplified model.

The Monte Carlo model is comparative rather than process-calibrated.
Behavioural threshold-voltage perturbations represent residual random,
gradient, and edge-environment sensitivity.

## From Manual Study to Automation

The manual study provides the basis for later automation decisions.

| Manual observation | Automation implication |
|---|---|
| Current mirrors require compact ratio matching and local gate sharing | Shared-diffusion interdigitation |
| Differential pairs benefit from two-dimensional symmetry | Common-centroid generation |
| Dummy devices define the local edge environment | Generator-level dummy policy |
| Guard rings are extended physical domains | Explicit guard-ring geometry and separate routing |
| PMOS well relations constrain legal placement | Well-domain metadata and post-placement repair |
| Buses and capacitor plates provide extended routing access | Explicit access geometry and access-aware routing |

The important transition is therefore not from one manual layout
directly to one generated layout. The manual study first identifies
the physical purpose of each technique; the resulting knowledge is then
abstracted into reusable policies for generation, placement, and routing.

<div class="page-links">
Next:
<a href="{{ '/framework/' | relative_url }}">
the conceptual framework
</a>.
</div>