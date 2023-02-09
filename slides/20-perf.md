---
author: Colin Gross
title: Freeze 10 Pipeline
date: 2023-02-09
---

# Montitoring 

- SLURM tools
- Nextflow trace & reports
- GCP dashboards and gcloud tool

## SLURM: tools squeue
Useful for seeing processes as they are running

```txt
JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
   33   highcpu nf-varia grosscol  R    7:44:37      1 dpclust-compute-1-2
   34   highcpu nf-varia grosscol  R    7:17:47      1 dpclust-compute-1-2
   35   highcpu nf-varia grosscol  R    5:52:26      1 dpclust-compute-1-1
   36   highcpu nf-varia grosscol  R    5:35:26      1 dpclust-compute-1-1
   37   highcpu nf-varia grosscol  R    2:14:02      1 dpclust-compute-1-1
   38   highcpu nf-varia grosscol  R      10:52      1 dpclust-compute-1-1
```

## SLURM: tools sacct
Useful for looking at completed processes

```sh
sacct -o JobName%26,State%15
```
```txt
nf-variants_by_sample_(10)         FAILED
nf-variants_by_sample_(12)      COMPLETED
nf-variants_by_sample_(17)  OUT_OF_MEMORY
nf-variants_by_sample_(13)        RUNNING
```

## GCP Dashboard

![mem_use_24](https://user-images.githubusercontent.com/1520508/217684334-07c306ab-b0cb-4ed2-ae17-c12c7f202e07.png)

![cpu_use_24h](https://user-images.githubusercontent.com/1520508/217684343-67220dc5-064d-4b59-a268-f66008392e17.png)

## Nextflow trace & report
**Post run** breakdown of process cpu and memory use.

- Report is all in one html visual report. 
- The trace contains the underlying data in csv.

- [initial coverage run](first_cov_run.html)
- [tuned vcf run](tuned_vcf_run.html)



