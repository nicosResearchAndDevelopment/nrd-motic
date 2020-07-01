# `net` Binary Operators

Network related binary operators.

- Prefix: `net`
 
### NRS

Network Reference System.

```
"ipv4"
```

### Address

```context
["178.10.10.42"]
```

### Range

```context
["178.10.10.1"]-["178.10.10.255"]
```

### net:is

Is a given string a valid IPv$-Address?

###### pseudocode
```pseudocode
isA(["178.10.10.42"], "ipv4") = true
```

|   |   |   | is|  
|---|---|---|:---|
| ["178.10.10.42"]         | `is` | "ipv4"  | true  |
| ["178.10.10.**424242**"] | `is` | "ipv4"  | false |
| ["178.10.10.42"]         | `is` | "ipv6"  | false |

###### ODRL
```json
{
    "constraint": [{
               "leftOperand": {
                    "@type": "net:address",
                    "@value": ["178.10.10.42"]
               },
               "operator": "net:is",
               "rightOperand":  {
                    "@type": "net:type",
                    "@value": "ipv4"
               }
           }]
}
```

### net:inside

###### pseudocode
```pseudocode
inside(["178.10.10.42"], ["178.10.10.1", "178.10.10.255"]) = true
```
|   |   |   | is|  
|---|---|---|:---|
| ["178.10.10.42"]     | `inside` | ["178.10.10.1", "178.10.10.255"] | true  |
| ["178.10.**42**.42"] | `inside` | ["178.10.10.1", "178.10.10.255"] | false  |

### net:contains

###### pseudocode
```pseudocode
contains(["178.10.10.1", "178.10.10.256"], ["178.10.10.42"]) = true
contains(["178.10.10.1", "178.10.10.256"], ["178.10.42.42"]) = false
```
|   |   |   | is|  
|---|---|---|:---|
| ["178.10.10.1", "178.10.10.256"] | `contains` | ["178.10.10.42"]     | true   |
| ["178.10.10.1", "178.10.10.256"] | `contains` | ["178.10.**42**.42"] | false  |

---