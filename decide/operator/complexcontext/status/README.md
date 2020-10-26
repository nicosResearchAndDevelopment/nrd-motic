# `status` Binary Operators

## Status

A very interesting and not seldom used context.

- Prefix: `status`



### `status:probe`

The `status:probe` of given environment is the subject and will be represented 
 as the outcome of `odrl:leftOperand` and will be compared in given `constraint`.

```json
{
    "@context":                    [
        "http://www.nicos-rd.com/status/",
        {
            "status": "http://www.nicos-rd.com/status/",
            "xsd":     "http://www.w3.org/2001/XMLSchema#",
			"car":     "https://www.bmw.com/car/"
        }
    ],
	"@id": "car:m3/abc123/status",
    "@type":                       "car:status",
	"car:issuedAt": {
		"@type": "xsd:xsdDateTimeStamp",
		"@value": "2020-08-11T11:22:33.42Z"
	}
    "car:airBagOpened": {
      "@type": "xsd:boolean",
      "@value": "true"
    }
}
```

### Operator `status:fullfills`

```text

```

### Examples

#### Accident

The use case:

> Make a notification if given status 'accident' is reached

	|   |   |   | is |
|---|---|---|:---|
| "car:status"   | `stutus:fullfills` | "definition:accident"           | true  |


```json
{
	"leftOperand": "car:status",
	"operator": "definition:fullfills",
	"rightOperand": "definition:accident"
}
```

```json
{
  "@context": "http://www.w3.org/ns/odrl.jsonld",
  "@type": "Status",
  "uid": "http://example.com/statuspolicy:42",
  "profile": "http://www.nicos-rd.com/odrl:profile:42",
  "obligation": [{
    "assigner": "https://www.bmw.com/car/m3/abc123/",
    "target": {
      "@type": "Asset",
      "source":  "https://www.bmw.com/car/m3/abc123/status",
      "refinement": [{
		  "@type": "Constraint",
		  "leftOperand": "airBagOpened",
		  "operator": "eq",
		  "rightOperand": { "@value": true, "@type": "xsd:boolean" }
      }]
    },
    "action": "notify"
  }]
}

```

---