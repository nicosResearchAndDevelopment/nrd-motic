# `organization` Binary Operators

- Prefix: `org`


A `org:Organization` grounded on `foaf:Organization`.

- Prefix: `org`
- [vocab-org](https://www.w3.org/TR/vocab-org/)
- [term_Organization](http://xmlns.com/foaf/spec/#term_Organization)


### org:hasSubOrganization

```
(companyA)--.
            |
            .--(companyB)
            |
            .--(companyC)
            |
            .--[finance]
```
|   |   |   | is|
|---|---|---|:---|
| "urn:companyA"   | `hasSubOrganization` | "urn:companyB" | true  |
| "urn:companyA"   | `subOrganizationOf`  | "urn:companyB" | false |


### org:hasUnit

|   |   |   | is|
|---|---|---|:---|
| "urn:companyA"  | `hasUnit` | "urn:finance" | true  |
| "urn:finance"   | `unitOf` | "urn:companyA" | true |
| "urn:finance"   | `unitOf` | "urn:companyB" | false |

```

               {nirvana}
             /
            /
           /
(companyA)
           \
            \
             \
               {atlantis}
             /
            /
           /
(companyB)
           \
            \
             \
               {wonderland}
               
```

### org:hasSite

|   |   |   | is|  
|---|---|---|:---|
| "urn:companyA"   | `hasSite` | "urn:atlantis"   | true  |
| "urn:companyA"   | `hasSite` | "urn:wonderland" | false |
| "urn:companyB"   | `hasSite` | "urn:atlantis"   | true  |
| "urn:nirvana"    | `siteOf`  | "urn:companyA"   | true  |
| "urn:wonderland" | `siteOf`  | "urn:companyA"   | false |


### org:hasPrimarySite

```
```

### org:hasRegisteredSite

```
```

---