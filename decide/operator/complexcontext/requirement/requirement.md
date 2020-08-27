# `requirement` Binary Operators

## Requirement

A very interesting and not seldom used context.

- Prefix: `req`


### `req:probe`

The `req:probe` of given environment is the `object` and will be represented 
 as the outcome of `odrl:rightOperand` and will be compared in given `constraint`.

So, there remarkable point is, that users requirement moves to right operand!

```json
{
    "@context":                    [
        "http://www.nicos-rd.com/requirement/",
        {
            "req": "http://www.nicos-rd.com/requirement/",
            "xsd":     "http://www.w3.org/2001/XMLSchema#"
        }
    ],
    "@id":                       "https://example.com/requirement:this",
    "@type":                     "req:probe",
    "@value": ["VPN"]
}
```

### Operator `req:fullfills`

```text

```

|   |   |   | is |
|---|---|---|:---|
| "asset"   | `req:fullfills` | "req:probe"           | true  |


### Examples

#### User requires a special feature

The use case:

> Do services fullfill users requirements, or
> "What are all the services meeting this user's requirement of providing 'VPN'?".
>

All assets (here: services as an asset collection) will be examined if fulfilling having requested feature: 

```json
{
    "@context": "http://www.w3.org/ns/odrl.jsonld",
    "@type": "Offer",
    "uid": "https://example.com/policy:42",
    "profile": "https://gaia-x/odrl:profile:42",
    "permission": [{
       "assigner": "https://www.gaia-x.com/participant:nicos",
       "assignee": "https://example.com",
       "target": {
          "@type": "AssetCollection",
          "source":  "https://gaia-x.com/ecosystem/services",
          "refinement": [{
              "leftOperand": "resolution",
              "operator": "req:fullfill",
              "rightOperand": "https://example.com/requirement:this"
          }]
      },
      "action": [{
         "rdf:value": { "@id": "odrl:use" }    
      }]
   }]
}
```

---
