---
author: Colin Gross
title: Freeze 10 Pipeline
date: 2022-09-30
---

#
<h3>Performance</h3>
Refactored depth count aggregation of coverage workflow to use successive rounds of concurrent reads.

## Aggregate Depths
Aggregation depth counts needs to read all the depth files and concatenate rows.
Individual depth files that look like:
```txt
chr11 52230001  20
chr11 52230002  14
chr11 52230003  22
```
to single concatenated depth file like:
```txt
chr11 52230001  20;18;29;18
chr11 52230002  14;21;24;24 
chr11 52230003  22;21;22;12
```

## Parallel Reading with Pipes
Convenient method of reading four or five files at a time.
```sh
# Create pipes for tabix reading
PIPES=()
PIPE_COUNT=0

for INFILE in \${INFILES[@]}
do
  PIPE_NAME=pipe_\${PIPE_COUNT}
  mkfifo \${PIPE_NAME}
  PIPES+=( \${PIPE_NAME} )

  # Start reading depth file into pipe
  (tabix \${INFILE} ${chromosome}:\${POS}-\${END} > \${PIPE_NAME}) &

  let "PIPE_COUNT = PIPE_COUNT + 1"
done
```

## Aggregate multiple files
- [mlr](https://github.com/johnkerl/miller) is a tool for tabular data.
- Here it's concatenating the depth counts from `chr\tpos\tdepth` files.
- `mlr` handles multiple files increasing throughput.
- This approach can aggregate prev aggregated files.

```sh
# Aggregate depths from depth file chunks
mlr -N --tsv 'nest' --ivar ";" -f 3 \${PIPES[@]} |\
  sort --numeric-sort --key=2 |\
  bgzip >> ${result_file}
```

## Repeat Aggregation Until Done.
Nextflow's DSL 1.0 doesn't support looping.

This does **not** work:
```
process aggregate_depths_loop {
  input:
  tuple file(depth_files) from agg_channel

  output:
  tuple file("${chrom}_${task.index}.tsv.gz") into agg_channel

  when: aggregation_complete != true
```

## Unrolled loop
- Handles limited quantity of input files
- Makes workflow script longer & repetative

```
process aggregate_depths_rnd_1 {
  input:  file(depth_files) from pileups
  output: file("${chrom}_rnd_1_${task.index}.tsv.gz") into agg_rnd_1

process aggregate_depths_rnd_2 {
  input:  file(depth_files) from agg_rnd_1
  output: file("${chrom}_rnd_1_${task.index}.tsv.gz") into agg_rnd_2

process aggregate_depths_rnd_3 {
  input:  file(depth_files) from agg_rnd_2
  output: file("${chrom}_rnd_1_${task.index}.tsv.gz") into agg_rnd_3
```
