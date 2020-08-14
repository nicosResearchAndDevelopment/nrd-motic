# Smelting Furnace as a motic.decide Complex Context


- Prefix: `smf`


## smelting furnace `probe 1`

```json
{
    "@context":                    [
        "http://www.nicos-rd.com/smeltingfurnace/",
        {
            "smf": "http://www.nicos-rd.com/smeltingfurnace/",
            "xsd":     "http://www.w3.org/2001/XMLSchema#"
        }
    ],
    "@id": "http://www.nicos-rd.com/smeltingfurnace/probe1",
    "@type":                       "smf:probe",
    "smf:temperature":         {
        "@type":  "xsd:decimal",
        "@value": 1200,
        "odrl:unit": "celsius"
    },
    "smf:burnerFireOn":         {
        "@type":  "xsd:boolean",
        "@value": true
    },
    "smf:serviceDoorOpen":         {
      "rdfs:comment": "",
      "@type":  "xsd:boolean",
      "@value": false
    }
}
```

### operator `smf:mode`

We want to detect, if furnace is up and running:

|   |   |   | is |
|---|---|---|:---|
| "smf:probe1"   | `smf:mode` | ["smf:active"](./examples/smfActive_RightOperand_1.json) | true  |

### Examples

##### Enforcement and Right Operand

- [constraint](./examples/active_1.json)
- [rightOperand "active"](./examples/smfActive_RightOperand_1.json)

---
