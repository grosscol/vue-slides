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

## Encapsulate Data Visualization in a Vue Component.
Table with information about each eQTL

```html
<template>
<div class="child-component">
  <div ref="eqtltable" class="table-sm"></div>
</div>
</template>

<script>
import Tabulator from 'tabulator-tables'
export default {
 name: "EqtlSusieTable"
}
</script>
```

## Description in plain HTML as a Component
Used `pandoc` to convert Peter's readme to HTML.

```html
<template>
 <h5 id="cis-eqtlsusie">Cis-eQTL/SuSiE</h5>
  <p>SuSiE fine-mapping results (using SuSiE [3]...</p>
  <p>By default the SNP posterior inclusion probabilities....</p>

  <h6 id="column-descriptions">Column descriptions:</h6>
  <ul>
    <li><strong>variant_id:</strong> genetic variant, in format ...</li>
    <li><strong>pip:</strong> SuSiE PIP (the probability the ...</li>
  </ul>
</template>
<script>
export default {
  name: "EqtlTableDescription"
}
</script>
```

## Compose Components.
```html
<div class="col-md-5">
    <EqtlSusieTable/>
</div>
<div class="col-md-5">
    <EqtlCondTable/>
</div>
<div class="col-md-11">
  <EqtlTableDescription/>
</div>
```



