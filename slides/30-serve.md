---
author: Colin Gross
title: Adding Data to Bravo
date: 2023-10-12
---

# Serving eQTL Data
Emit data from Bravo API

## Flask Endpoint
Put eqtl endpoints in a blueprint and routing is done via decorator
```py
# bravo_api/blueprints/eqtl.py
bp = Blueprint('eqtl', __name__)

@bp.route('/eqtl/susie', methods=['GET'])
def get_susie(args):
    result = susie(args['gene'])
    return make_response(jsonify(result))
```

## Validate parameters
The marshmallow package handles arg validation

```py
# bravo_api/blueprints/eqtl.py
eqtl_argmap = {
    'gene': fields.Str(
      required=True, validate=lambda x: len(x) > 0,
       error_messages={'validator_failed': 'Value must be a non-empty string.'})
}
```
```py
# bravo_api/blueprints/eqtl.py
@bp.route('/eqtl/susie', methods=['GET'])
@parser.use_args(eqtl_argmap, location='query')
def get_susie(args):
    result = susie(args['gene'])
    return make_response(jsonify(result))
```

## Query Mongo

```py
# bravo_api/blueprints/eqtl.py
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

Avoid 500 errors by avoiding `None` and returning expected type.

```py
# bravo_api/blueprints/eqtl.py
    cursor = current_app.mmongo.db.genes.aggregate(pipeline)
    cursor.limit = 1
    answer = next(cursor, None)
    if answer is None:
        return []
    else:
        return answer['eqtls']
```

## Auth required

Indicating authed session is requied for all routes in the blueprint

```py
# bravo_api/__init__.py
from bravo_api.blueprints.eqtl import eqtl

def create_app(test_config=None):

    # Protect data endpoint blueprints
    eqtl.bp.before_request(auth_routes.agreement_required)

    # Register and set prefix for blueprint's routes
    app.register_blueprint(eqtl.bp, url_prefix='/ui')
```
