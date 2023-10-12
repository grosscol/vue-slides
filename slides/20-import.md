---
author: Colin Gross
title: Adding Data to Bravo
date: 2023-10-12
---

# Import 

- Python using pymongo
- Mongo utilities

## Python Option
Previous imports were done via python

```py
Python import code
```

## Mongo Import Tool

Supported formats:

- mongo's bson format
- tabular data import

## Formating consideration

```txt
Example processed data with mongo headers
```

## Mongo Import Command

- Independent of Bravo API app
- Pretty fast
- Can read from stdin (e.g pipe from s3)

```sh
mongoimport \
  --host="localhost" --db="bravo" \
  --type="tsv" --columnsHaveTypes  --headerline \
  --parseGrace=stop --collection=eqtl_cond \
  --file=result/all.cond.tsv 
```
