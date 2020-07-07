# motic.decide Operator

REMARK: this document is NOT normative and at this point a draft only. 

Please follow, to be informed on editings...


## [ODRL](./odrl.md)

## [time](./time.md)

## [geometry](./geometry.md)

## [geospatial](./geospatial.md)

## [agent](./agent.md)

Binary operators regarding `foaf:Agent` and it's sub-class `foaf:Group`.

## [organization](./organization.md)

## [environment](./environment.md)

#### [weather](./weather.md)

This is a more crisp usage of `environment`

## [net](./net.md)

## [regex](./regex.md)


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