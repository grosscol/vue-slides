---
author: Colin Gross
title: Adding Data to Bravo
date: 2023-10-12
---

# Displaying eQTL Data

Considered questions that users want to answer
- How many eQTLs?
- How strong is the signal in the evidence?
- How was the analysis done?

## Encapsulate in a Vue Component.

```vue
Template section for eqtl
```

## Manage Different Data in Child Components.

```vue
SuSiE table
Conditional table
```

## Description in plain HTML

Used `pandoc` to convert Peter's readme to HTML.

```html
 <h5 id="cis-eqtlsusie">Cis-eQTL/SuSiE</h5>
  <p>SuSiE fine-mapping results (using SuSiE [3]...</p>
  <p>By default the SNP posterior inclusion probabilities....</p>

  <h6 id="column-descriptions">Column descriptions:</h6>
  <ul>
    <li><strong>variant_id:</strong> genetic variant, in format ...</li>
    <li><strong>pip:</strong> SuSiE PIP (the probability the ...</li>
  </ul>
```
