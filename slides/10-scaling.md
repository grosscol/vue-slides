---
author: Colin Gross
title: Freeze 10 Pipeline
date: 2022-09-30
---

#
<h3>Scaling</h3>
Add more nodes when needed until available queue is running or maximum nodes provisioned.

## Nextflow Config
Nextflow config handles limit of jobs to enqueue at a time.

```
executor {
  $slurm {
    queueSize = 1000
    jobName = { "bravo_coverage" }
  }
```

## SLURM Config
Slurm on GCP handles limit of nodes to spin up.

```
partitions = [
  { name                 = "highcpu"
    machine_type         = "n1-highcpu-8"
    static_node_count    = 0
    max_node_count       = 25
  },
  { name                 = "highmem"
    machine_type         = "n2-highmem-4"
    static_node_count    = 0
    max_node_count       = 52
  }
```
