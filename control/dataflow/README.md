# motic.decide Dataflow Control

## Validation

### Validation of Type

```
{
  "@context": "http://www.w3.org/ns/odrl.jsonld",
  "@type": "fua-d:Dataflow",
  "uid": "http://example.com/policy:42",
  "profile": "http://example.com/odrl:profile:09",
  "flowrule": [{
	  "@type": "fua-d:DataflowRule",
      "typeValidation": [{
		  "@id": "1/types/xsd/boolean",
		  "leftOperand": "value",
		  "operator": "fua-d:isType",
		  "rightOperand": "xsd:boolean"
		  //"action": "fua-d:typeTransformBooleanToInteger"
      }]
    }]
}
```

### Validation of Value


```
{
  "@context": "http://www.w3.org/ns/odrl.jsonld",
  "@type": "fua-d:Dataflow",
  "uid": "http://example.com/policy:43",
  "profile": "http://example.com/odrl:profile:09",
  "flowrule": [{
	  "@type": "fua-d:DataflowRule",
	  "valueValidation": [{
		  "@id": "1/values/boolean/true",
		  "leftOperand": "value",
		  "operator": "fua-d:veq", // value equals
		  "rightOperand": true,
		  "unit": "xsd:boolean"
	  }],
	  
    }]
}
```


## Transformation

### Transformation of Type

### Transformation of Value


---

