# motic.decide Operator

REMARK: this document is NOT normative and at this point a draft only. 

Please follow, to be informed on editings...


## [ODRL](./odrl.md)

## [time](./time.md)

## [geometry](./geometry.md)

## [agent](./agent.md)

Binary operators regarding `foaf:Agent` and it's sub-class `foaf:Group`.

## [organization](./organization.md)


## Regular Expression

- prefix : regex

### regex:match

Is a given string a valid Email-Address?

```pseudocode
match("GAIAboX@nicos-ag.com", "^[A-Za-z0-9.!#$%&'*+\\-/=?^_`{|}~]+@[A-Za-z0-9.\\-]+\\.[A-Za-z]+$") = true
```

###### ODRL
```json
{
    //...
    "constraint": [{
               "leftOperand": {
                    "@type": "xsd:string:address",
                    "@value": "GAIAboX@nicos-ag.com"
               },
               "operator": "regex:match",
               "rightOperand":  {
                    "@type": "xsd:string",
                    "@value": "^[A-Za-z0-9.!#$%&'*+\\-/=?^_`{|}~]+@[A-Za-z0-9.\\-]+\\.[A-Za-z]+$"
               }
           }]
        //...
}
```

---

## Id

Operators, specially designed for handling identificator related problems.

### IRS

Identificator Reference System.

```draft enum
"mac"
"opcua:NodeId"
"mail-address"
"uri"
"url"
"ipv4"
"ipv6"
```

### id:equals
### id:startsWith
### id:endsWith
### id:contains
### id:regex

### Hardware Address

#### MAC-Address

- [`MAC-Address`, wiki, en](https://en.wikipedia.org/wiki/MAC_address)

```pseudocode
id:equals("C8-5B-76-B5-43-2B", "C8-5B-76-B5-43-2B") = true
id:startsWith("C8-5B-76-B5-43-2B", "C8-5B-76") = true
id:endsWith("C8-5B-76-B5-43-2B", "B5-43-2B") = true
id:contains("C8-5B-76-B5-43-2B", "5B-76-B5") = true
```
|   |   |   | is|  
|---|---|---|:---|
| "C8-5B-76-B5-43-2B" | `equals`     | "C8-5B-76-B5-43-2B" | true   |
| "C8-5B-76-B5-43-2B" | `startsWith` | "C8-5B-76"          | true   |
| "C8-5B-76-B5-43-2B" | `endsWith`   | "B5-43-2B"          | true   |
| "C8-5B-76-B5-43-2B" | `contains`   | "5B-76-B5"          | true   |

---