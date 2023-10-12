---
author: Colin Gross
title: Adding Data to Bravo
date: 2023-10-12
---

# Serving eQTL Data

## Flask Endpoint

Routing is done via decorator
```py
endpoint definition
```

## Auth required

Indicating if session is requied via decorator

```py
endpoint with auth decorator
```

## Validate parameters
The marshmallow package handls arg validation

```py
Arg validation
```

## Query Mongo

```py
    pipeline = [
        {'$match': {'gene_name': gene_name}},
        {'$lookup': {'from': "eqtl_susie",
                     'localField': "gene_id",
                     'foreignField': "phenotype_id",
                     'as': "eqtls"}},
        {'$project': {'_id': False, 'eqtls._id': False}}
    ]
```

## Handle data absent

Avoid 500 errors

```py
    cursor = current_app.mmongo.db.genes.aggregate(pipeline)
    cursor.limit = 1
    answer = next(cursor, None)
    if answer is None:
        return []
    else:
        return answer['eqtls']
```
