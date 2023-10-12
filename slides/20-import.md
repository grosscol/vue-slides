---
author: Colin Gross
title: Adding Data to Bravo
date: 2023-10-12
---

# Import 
Store data in MongoDB that backs the API

- Python using pymongo
- Mongo utilities

## Python Option
Previous imports were done via python. E.g. loading gene data

```py
    for gene in read_gencode(gencode_file, ['gene']):
        gene_id = gene['gene_id']
        if gene_id in canonical_transcripts:
            gene['canonical_transcripts'] = canonical_transcripts[gene_id]
        if gene_id in omim_annotations:
            gene['omim_accession'] = omim_annotations[gene_id][0]
            gene['omim_description'] = omim_annotations[gene_id][1]
        if gene_id in genenames:
            gene['gene_name'] = genenames[gene_id][0]
            gene['full_gene_name'] = genenames[gene_id][1]
            gene['other_names'] = genenames[gene_id][2]
        mongo.db.genes.insert_one(gene)
    mongo.db.genes.create_indexes([pymongo.operations.IndexModel(key) for key in ['gene_id', 'gene_name', 'other_names', 'xstart', 'xstop']])
```

## Mongo Import Tool Option

Supported formats:

- mongo's bson format
- tabular data import

## Tabular Formating consideration
Type specification can be added to and parsed from column headers.

```txt
phenotype_id.string()   variant_id.string()     pip.double()    af.double()     cs_id.int32()   tissue.string()
ENSG00000151240 chr10_483204_G_A        0.05126923      0.83288044      1       T_cell
ENSG00000151240 chr10_499255_G_C        0.06409895      0.8342391       1       T_cell

```

## Mongo Import Command

- Independent of Bravo API app
- Pretty fast
- Can read from stdin (e.g pipe from s3)
- Simple for relatively flat data

```sh
mongoimport \
  --host="localhost" --db="bravo" \
  --type="tsv" --columnsHaveTypes  --headerline \
  --parseGrace=stop --collection=eqtl_cond \
  --file=result/all.cond.tsv 
```
