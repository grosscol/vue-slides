---
author: Colin Gross
title: Freeze 10 Pipeline
date: 2022-09-30
---

#
<h3>Parallel When Possible</h3>
Refactored depth count aggregation of coverage workflow to use successive rounds of concurrent reads.

## Fill pipes
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

## Read from pipes
- [mlr](https://github.com/johnkerl/miller) is a tool for tabular data.
- Here it's concatenating the depth counts from `chr\tpos\tdepth` files.
- `mlr` handles multiple files increasing throughput.

```sh
# Aggregate depths from depth file chunks
mlr -N --tsv 'nest' --ivar ";" -f 3 \${PIPES[@]} |\
  sort --numeric-sort --key=2 |\
  bgzip >> ${result_file}
```
