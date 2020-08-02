# motic.data

## Dataflow Control

### Validation

#### Validation of Type


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
		  "rightOperand": "xsd:Boolean"
		  //"action": "fua-d:typeTransformBooleanToInteger"
      }],
	  "valueValidation": [{
		  "@id": "1/values/boolean/true",
		  "leftOperand": "value",
		  "operator": "fua-d:veq", // value equals
		  "rightOperand": true,
		  "unit": "xsd:Boolean"
	  }],
	  
    }]
}
```

#### Validation of Value

### Transformation

#### Transformation of Type

#### Transformation of Value

---
