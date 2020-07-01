# `organization` Binary Operators

- Prefix: `org`

A `org:Organization` grounded on `foaf:Organization`.

- [vocab-org](https://www.w3.org/TR/vocab-org/)
- [foaf spec: term_Organization](http://xmlns.com/foaf/spec/#term_Organization)


### org:hasSubOrganization

##### Example
```text
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


### org:hasSite

##### Example
```text

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