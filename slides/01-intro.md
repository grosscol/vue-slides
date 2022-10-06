---
author: Colin Gross
title: Freeze 10 Pipeline
date: 2022-09-30
---

#
<h3>BRAVO Data Prep</h3>

Repo of tools, workflows, and hacks.

[github.com/statgen/bravo\_data\_prep](https://github.com/statgen/bravo_data_prep)

```
workspace/data_prep/
├── hacks/
├── README.md
├── tools/
└── workflows/
```

## Tools
Purpose built applications for processing steps.

- python tools
    - python3
    - requires pysam 

- cpp tools
    - cget for dependencies
    - builds htslib

## Workflows
Nextflow to orchestrate processing of VCF and CRAM data.

- coverage
- process\_vcf
- sequences

## Hacks

- Clearly dilineated in a directory.
- One off scripts and experimenting.
- May be useful in future development.
- Provide some history otherwise excluded from git.
- Described by `hacks/readme.md`.

```
data_prep/hacks/
├── 198_samples.tsv
├── 198_samples.txt
├── make_198_sample_links.sh
├── make_198_samples_tsv.sh
├── readme.md
```


# 
<h3>Inputs</h3>
- VCFs with DP & GQ fields.
- VCFs with genotypes only. 
- VCFs with sites only.
- CRAM files
- samples\_list.txt
- samples\_list.txt (not the same as above)
- samples\_list.tsv (also not the same)

# 
<h3>Outputs</h3>
- Files to be loaded into MongoDB
    - QC\_Metrics
    - VCFs
- Files to be accessed at runtime
    - CRAMs
    - Coverage depth summaries

## Basis Data
Produced by process\_vcf workflow
```
/bravo/data/basis/
├── qc_metrics
│   └── metrics.json.gz
├── reference
│   ├── canonical_transcripts.tsv.gz
│   ├── gencode.v38.annotation.gtf.gz
│   ├── hgcn_genenames.tsv.gz
│   └── omim_ensembl_refs.tsv.gz
└── vcfs
    ├── chr11.bravo.vcf.gz
    └── chr12.bravo.vcf.gz
```

## Runtime Data
Produced by coverage and sequences workflows
```
/bravo/data/runtime
├── cache
├── coverage
│   ├── bin_0.25
│   │   ├── chr11.bin_0.25.tsv.gz
│   │   ├── chr11.bin_0.25.tsv.gz.tbi
│   │   ├── chr12.bin_0.25.tsv.gz
│   │   └── chr12.bin_0.25.tsv.gz.tbi
│   ├── bin_0.50 ...
│   ├── bin_0.75 ...
│   ├── bin_1.00 ...
│   └── full ...
├── crams
│   ├── sequences ...
│   ├── variant_map.tsv.gz
│   └── variant_map.tsv.gz.tbi
└── reference
    ├── hs38DH.fa
    └── hs38DH.fa.fai
```

