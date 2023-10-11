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

## Handle data absent
Avoid 500 errors
```py
Return nothing if nothing found.
```
