# `validation` Binary Operators

## REMARK

- DRAFT only, don't watch out for the semantic and/or correct URIs
- very buggy so far, not understoo totally...

## Requirement

A very interesting and not seldom used context.

- Prefix: `val`


### `val:probe`


```json
{
    "@context":                    [
        "http://www.nicos-rd.com/requirement/",
        {
            "req": "http://www.nicos-rd.com/requirement/",
            "xsd":     "http://www.w3.org/2001/XMLSchema#"
        }
    ],
    "@id":         "https://www.nicos-rd.com/validation/validation_42",
    "@type":       "val:DAT_sub",
    "@value": 		"skiaki"
}
```

### Examples

#### User requires a special feature

The use case:

> 

```json
{
    "@context": "http://www.w3.org/ns/odrl.jsonld",
    "@type": "val:Validation",
    "uid": "http://example.com/policy:6161",
    "profile": "http://example.com/odrl:profile:10",
    "validation": [{
       "target": "https://www.nicos-rd.com/validation/validation_42",
       "assigner": "http://example.com/org:616",
       "action": [
	   	{
          "rdf:value": { "@id": "val:accept" },
          "refinement": [{
             "leftOperand": "@value",
             "operator": "val:DAT_eq",
             "rightOperand": { "@value": "skiaki", "@type": "val:DAT" },
             "unit": "http://dbpedia.org/resource/DAT_sub"
          },
		  {
          "rdf:value": { "@id": "val:decline" },
          "refinement": [{
             "leftOperand": "@value",
             "operator": "val:DAT_not",
             "rightOperand": { "@value": "DAT", "@type": "val:DAT" },
             "unit": "http://dbpedia.org/resource/DAT_sub"
		 }
		  ]
      }]
   }]
}
```

---