# `requirement` Binary Operators

## Requirement

A very interesting and not seldom used context.

- Prefix: `req`


### `req:probe`

The `req:probe` of given environment will be translated to a refinement of given
 asset collection. This refinement is a
 [`odrl:Constraint`](https://www.w3.org/TR/odrl-model/#constraint-class) or 
 [`odrl:LogicalConstraint`](https://www.w3.org/TR/odrl-model/#constraint-logical), so
 more clever refinements could be expressed. 

```json
{
    "@context":                    [
        "http://www.nicos-rd.com/requirement/",
        {
            "req": "http://www.nicos-rd.com/requirement/",
            "xsd":     "http://www.w3.org/2001/XMLSchema#"
        }
    ],
    "@id":         "http://www.nicos-rd.com/requirements/requirement_42",
    "@type":       "req:probe",
    "req:service": {"@type": "xsd:string", "@value": "VPN" },
    "req:year":    {"@type": "xsd:gYear", "@value": 2020 },
    "req:month":   {"@type": "xsd:gMonth", "@value": 8 }
}
```

### Examples

#### User requires a special feature

The use case:

> Do services fullfill users requirements, or
> "What are all the services meeting this user's requirement of providing 'VPN'?".
>

All assets (here: services as an asset collection) will be examined if fulfilling
 having requested feature: 

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
          "refinement": [
                {
                    "leftOperand": "year",
                    "operator": "odrl:eq",
                    "rightOperand": { "@value": "2020", "@type": "xsd:gYear" }
                },
                {
                    "leftOperand": "month",
                    "operator": "odrl:eq",
                    "rightOperand": { "@value": "2020", "@type": "xsd:gMonth" }
                },
                {
                     "leftOperand": "service",
                     "operator": "odrl:eq",
                     "rightOperand": { "@value": "VPN", "@type": "xsd:string" }
                }
          ]
      },
      "action": [{
         "rdf:value": { "@id": "odrl:use" }    
      }]
   }]
}
```

---
