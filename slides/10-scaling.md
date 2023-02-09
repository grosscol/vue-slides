---
author: Colin Gross
title: Freeze 10 Pipeline
date: 2023-02-09
---

# Available ajustments
- SLURM
- Nextflow
- GCP

## Knobs: SLURM partitions
- node quantity
- node type
```tf
  partitions = [
    { name                 = "highcpu"
      machine_type         = "n1-highcpu-8"
      static_node_count    = 0
      max_node_count       = 40 },
    { name                 = "highmem"
      machine_type         = "n2-highmem-4"
      static_node_count    = 0
      max_node_count       = 80 },
```

## Knobs: Nextflow
- Limit much memory and cpu
- Which queue of nodes to run in

```nf
	withLabel: highmem {
		queue = "highmem"
		memory = "16 GB"
		cpus = 2
	}

	withLabel: highcpu {
		queue = "highcpu"
		memory = "0.75 GB"
		cpus = 1
	}
```

## Knobs: GCP instances.
Machine  vCPUs  Memory
```txt
n1-standard-4  4  vCPUs   15   GiB
n1-highmem-8   8  vCPUs   52   GiB
n1-highcpu-16  16 vCPUs   14.4 GiB
c2d-highcpu-8  8  vCPUs   16   GiB
```

