# motic.decide Operator

REMARK: this document is NOT normative and at this point a draft only. 

Please follow, to be informed on editings...


## ODRL

Prefix: `odrl`. 

See also:

- [ODRL Information Model 2.2](https://www.w3.org/TR/odrl-model/)
- [IPTC RightsML Standard 2.0](https://iptc.org/std/RightsML/2.0/RightsML_2.0-specification.html)


### Constraint Operators

[constraintRelationalOperators](https://www.w3.org/TR/odrl-vocab/#constraintRelationalOperators)

#### [odrl:eq](http://www.w3.org/ns/odrl/2/eq)
Indicating that a given value equals the right operand of
 the Constraint.

#### [odrl:gt](http://www.w3.org/ns/odrl/2/gt)
Indicating that a given value is greater than the right operand of
 the Constraint.

#### [odrl:gteq](http://www.w3.org/ns/odrl/2/gteq)
Indicating that a given value is greater than or equal to the right
 operand of the Constraint.

#### [odrl:hasPart](http://www.w3.org/ns/odrl/2/hasPart)
A set-based operator indicating that a given value contains the right
 operand of the Constraint.

#### [odrl:isA](http://www.w3.org/ns/odrl/2/isA)
A set-based operator indicating that a given value is an instance of
 the right operand of the Constraint.

#### [odrl:isAllOf](http://www.w3.org/ns/odrl/2/isAllOf)
A set-based operator indicating that a given value is all of the
 right operand of the Constraint.

#### [odrl:isAnyOf](http://www.w3.org/ns/odrl/2/isAnyOf)
A set-based operator indicating that a given value is any of the
 right operand of the Constraint.

#### [odrl:isNoneOf](http://www.w3.org/ns/odrl/2/isNoneOf)
A set-based operator indicating that a given value is none of the
 right operand of the Constraint.

#### [odrl:isPartOf](http://www.w3.org/ns/odrl/2/isPartOf)
A set-based operator indicating that a given value is contained by the
 right operand of the Constraint.

#### [odrl:lt](http://www.w3.org/ns/odrl/2/lt)
Indicating that a given value is less than the right operand of the
 Constraint.

#### [odrl:lteq](http://www.w3.org/ns/odrl/2/lteq)
Indicating that a given value is less than or equal to the right
 operand of the Constraint.

#### [odrl:neq](http://www.w3.org/ns/odrl/2/neq)
Indicating that a given value is not equal to the right operand of
 the Constraint.


### Logical Constraint Operands

- [term-LogicalConstraint](https://www.w3.org/TR/odrl-vocab/#term-LogicalConstraint)
- [constraintLogicalOperands](https://www.w3.org/TR/odrl-vocab/#constraintLogicalOperands)

> This property *MUST* only be used for Logical Constraints, and the list
 of operand values *MUST* be Constraint instances.

#### [odrl:and](http://www.w3.org/ns/odrl/2/and)

The relation is satisfied when all of the Constraints are satisfied.

#### [odrl:andSequence](http://www.w3.org/ns/odrl/2/andSequence)

The relation is satisfied when each of the Constraints are satisfied in
 the order specified.

- [**odrl:andSequence**](https://www.w3.org/TR/odrl-vocab/#term-andSequence)

#### [odrl:or](http://www.w3.org/ns/odrl/2/or)

The relation is satisfied when at least one of the Constraints is
 satisfied.

- [**odrl:or**](https://www.w3.org/TR/odrl-vocab/#term-or)


#### [odrl:xone](http://www.w3.org/ns/odrl/2/xone)
"Only One": The relation is satisfied when only one, and not more, of the Constaints
 is satisfied. 

- [**odrl:xone**](https://www.w3.org/TR/odrl-vocab/#term-xone)

---

## Time

time
- [Time Ontology in OWL](https://www.w3.org/TR/owl-time/).



```
                      |      i    |        j
  Before(i, j)        |<.........>|   <--------->    After(j, i)
   Meets(i, j)        |           |<------------>    MetBy(j, i)
Overlaps(i, j)        |       <---|------------->    OverlappedBy(j, i)
  Starts(i, j)        |<----------|------------->    StartedBy(j, i)
  During(i, j)    <---|-----------|------------->    Contains(j, i)
Finishes(i, j)    <---|---------->|                  FinishedBy(j, i)
  Equals(i, j)        |<--------->|                  Equals(j, i)
```

*Thirteen elementary possible relations between time periods*.

> ["Actions and events in interval temporal logic In: Spatial and
 Temporal Reasoning. O. Stock, ed., Kluwer, Dordrecht, Netherlands,
 pp. 205-245.. J.F. Allen; G. Ferguson.1997."](http://dx.doi.org/10.1007/978-0-585-28322-7_7)

Two additional relations, `In` and `Disjoint`:

```
      In(i, j) === (Starts(i, j) || During(i, j) || Finishes(i, j))
Disjoint(i, j) === (Before(i, j) || After(i, j))
```

### time:Before

##### Example

A `time:Instant` is `before` a `time:Interval`.

###### Pseudo code
```pseudocode
Before(["2019-01-05T00:00:00Z"], ["2019-02-01T00:00:00Z", "2019-03-01T00:00:00Z"]) = true
```

###### ODRL
```json
{
    //...
    "constraint": [{
               "leftOperand": {
                    "@type": "time:Instant",
                    "hasBeginning": {
                        "@type": "time:Instant",
                        "inXSDDateTime": {
                            "@type": "xsd:dateTime",
                            "@value": "2019-01-05T00:00:00Z"
                        }
                    }
               },
               "operator": "time:Before",
               "rightOperand":  {
                    "@type": "time:Interval",
                    "hasBeginning": {
                        "@type": "time:Instant",
                        "inXSDDateTime": {
                            "@type": "xsd:dateTime",
                            "@value": "2019-02-01T00:00:00Z"
                        }
                    },
                    "hasEnd": {
                        "@type": "time:Instant",
                        "inXSDDateTime": {
                            "@type": "xsd:dateTime",
                            "@value": "2019-03-01T00:00:00Z"
                        }
                    }
               }
           }]
        //...
}
```

---

### time:After

##### Example

A `time:Instant` is `after` a `time:Interval`.

###### Pseudo code
```pseudocode
After(["2019-04-01T00:00:00Z"], ["2019-02-01T00:00:00Z", "2019-03-01T00:00:00Z"]) = true
```
| time:TemporalEntity  |   | time:TemporalEntity | is |  
|:---|---|:---|:--|
| ["2019-04-01T00:00:00Z"] | `After`     | ["2019-02-01T00:00:00Z", "2019-03-01T00:00:00Z"] | true   |

### time:Meets
### time:MetBy
### time:Overlaps
### time:OverlappedBy
### time:Starts
### time:StartedBy
### time:During
### time:Contains
### time:Finishes
### time:FinishedBy

### time:Equals

##### Example

A `time:Instant` `equal`s a `time:Instant`.

###### Pseudo code
```pseudocode
Equals(["2019-04-01T00:00:00Z"], ["2019-04-01T00:00:00Z"]) = true
```
| time:TemporalEntity  |   | time:TemporalEntity | is |  
|:---|---|:---|:--|
| [ "2019-04-01T00:00:00Z" ] | `Equals`     | [ "2019-04-01T00:00:00Z" ] | true   |

### time:In
### time:Disjoint

---


## Geometry

- Prefix: `geom`

### geom:equals

Two sets _A_ and _B_ are equal, if for every point _a_ in _A_ and every
 point _b_ in _B_, also _a_ is in _B_ and _b_ is in _A_.

- symmetric


### geom:intersects

A set _A_ intersects a set _B_, if there exists a point _p_, such that
 _p_ is in _A_ and also in _B_.

- symmetric
- opposite of disjoint


### geom:disjoint

A set _A_ is disjoint with a set _B_, if there exists no point _p_,
 such that _p_ is in _A_ and also in _B_.
 
 - symmetric
 - opposite of intersects


### geom:contains

A set _A_ contains a set _B_, if for every point _b_ in _B_, also _b_
 is in _A_.

- not symmetric
- if _A_ contains _B_ and _B_ contains _A_, _A_ and _B_ are equal.


### geom:touches

_Two sets are touching, if they intersect and their intersection only
 includes their boundaries.
 
- also the tangent vectors of both sets at the point(s) of intersection
 should point in the same direction.
- also the intersection shall have at least 1 times less dimensionality
 than the maximum dimensionality of the two sets._ (difficult to define,
 clearer definition needed)
- symmetric


### geom:overlaps

A set _A_ overlaps a set _B_, if _A_ intersects _B_ but _A_ does
 not touch _B_.
 
- _Also the intersection shall have at least the dimensionality of the
 minimum dimensionality of the two sets._
- symmetric


#### Point

##### geom:disjoint

```
    . i

        (.) j
```
|   |   |   | is|
|---|---|---|:--|
| **i**   | `disjoint` | **j**   | true  |

##### geom:equals

```
    (.) i, j

"." is point "(*)" > "(.)", so
    i[42.0, 42.0]
is in the same place as
    j[42.0, 42.0]
```

---

#### Polygon, (n > 2)

##### geom:disjoint

```
    .-----------.
    |i          |
    |           |
    |           |
    |           |
    .-----------.

                   (.)---------(.)
                    |           |
                    |           |
                    |           |
                    |          j|
                   (.)---------(.)
```
|   |   |   | is|
|---|---|---|:--|
| **i**   | `disjoint` | **j**   | true  |

---

##### geom:touches

```
    .-----------.
    |i          |
    |           |
    |           |
    |           |
    .----------(.)---------(.)
                |           |
                |           |
                |           |
                |          j|
               (.)---------(.)
```
|   |   |   | is|
|---|---|---|:--|
| **i**   | `touches` | **j**   | true  |
```

    .-----------.
    |i          |
    |          (.)---------(.)
    |           |           |
    |           |           |
    .-----------.           |
                |          j|
               (.)---------(.)
```
|   |   |   | is|
|---|---|---|:--|
| **i**   | `touches` | **j**   | true  |

---

##### geom:overlaps

```
    .-----------.
    |i          |
    |           |
    |      (.)--|------(.)
    |       |+++|       |
    .-----------.       |
            |           |
            |          j|
           (.)---------(.)
```
|   |   |   | is|
|---|---|---|:--|
| **i** | `overlaps` | **j** | true  |
| **i** | `touches`  | **j** | false |

---

##### geom:contains

```
   (.)---------(.)
    |j          |
    |   .---.   |
    |   |   |   |
    |   .---.i  |
    |           |
   (.)---------(.)
```
|   |   |   | is|
|---|---|---|:--|
| **i** | `contains` | **j** | false |
| **j** | `contains` | **i** | true  |

---

##### geom:equals

```
   (.)---------(.)
    |i          |
    |           |
    |           |
    |          j|
   (.)---------(.)
```
|   |   |   | is|
|---|---|---|:--|
| **i** | `equals` | **j** | true |

---

#### Line

##### geom:disjoint

```
                  (.)
                  /j
                 /
    .           /
     \i        /
      \       /
       \    (.)
        \
         \
          .
```
|   |   |   | is|
|---|---|---|:--|
| **i** | `disjoint` | **j** | true |

##### geom:touches

```
    .          (.)
     \i        /j
      \       /
       \     /
        \   /
         \ /
         (.)
```
|   |   |   | is|
|---|---|---|:--|
| **i** | `touches` | **j** | true |
```
            (.)
            /j
           /
    .     /
     \i  /
      \ /
      (.)
        \
         \
          .
```
|   |   |   | is|
|---|---|---|:--|
| **i** | `touches` | **j** | true |

##### geom:intersects

```
    .    (.)
     \i  /j
      \ /
       \
      / \
     /   \
   (.)    .
```
|   |   |   | is|  
|---|---|---|:--|
| **i** | `intersects` | **j** | true  |
| *but* |              |       |       |
| **i** | `touches`    | **j** | false |
| **i** | `overlaps`   | **j** | false |


##### geom:overlaps

```
    .
     \i
      \
      (.)
        \
         \
          .
           \
            \j
            (.)
```
|   |   |   | is|  |
|---|---|---|:--|:--|
| **i** | `overlaps`   | **j** | true  | The intersection is not empty!|
| *but* |              |       |       |
| **i** | `touches`    | **j** | false |


---

##### geom:contains

```
    .
     \i
      \
      (.)
        \
         \j
         (.)
           \
            \
             .
```
```

    .
     \i
      \
       \
        \
        (.)
          \
           \j
           (.)
```
|   |   |   | is|  
|---|---|---|:--|
| **i** | `contains`   | **j** | true  |
| **i** | `overlaps`   | **j** | true  |
| **i** | `intersects` | **j** | true  |
| *but* |              |       |       |
| **i** | `touches`    | **j** | false |

---

##### geom:equals

```
    .
     \i
      \
       \
        \
         \j
         (.)
```
|   |   |   | is|
|---|---|---|:--|
| **i** | `equals`   | **j** | true  |
| **i** | `overlaps` | **j** | true  |
| **i** | `contains` | **j** | true  |
| **j** | `contains` | **i** | true  |


---

## Agent

- Prefix: `foaf`

### foaf:memberOf

Any `foaf:Agent`. can be member of some other agent.


## Group

`foaf:Group` is a `foaf:Agent`.

### foaf:hasMember

The group `exA:users` contains all users of example-company "A".

The group `exA:admin` (belonging to example-company "A") has three
 members, so has group `exA:maintainer` three others:

```
(exA:users)--. 
              |--(exA:zaphod)
              |--(exA:ford)
              |--(exA:rumpelstilz)
              |--(exA:bart)
              |--(exA:donald)
              |--(exA:spongebob)
              
(exA:adminGroup)--. 
                  |--(exA:zaphod)
                  |--(exA:ford)
                  |--(exA:rumpelstilz)
             
(exA:maintainerGroup)--. 
                       |--(exA:bart)
                       |--(exA:donald)
                       |--(exA:spongebob)
```

|   |   |   | is|
|---|---|---|:--|
| "exA:adminGroup"   | `hasMember` | "exA:zaphod"           | true  |
| "exA:adminGroup"   | `hasMember` | "exA:bob"              | false |
| "exA:ford"         | `memberOf`  | "exA:adminGroup"       | true  |
| *but*              |             |                        |       |
| "exA:rumpelstilz"  | `memberOf`  | "exB:maintainerGroup"  | false |


## Organization

`org:Organization` grounded on `foaf:Organization`.

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
|---|---|---|:--|
| "urn:companyA"   | `hasSubOrganization` | "urn:companyB" | true  |
| "urn:companyA"   | `subOrganizationOf`  | "urn:companyB" | false |


### org:hasUnit

|   |   |   | is|
|---|---|---|:--|
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
|---|---|---|:--|
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


## Net

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

### net:isA

Is a given string a valid IPv$-Address?

```pseudocode
isA(["178.10.10.42"], "ipv4") = true
```

|   |   |   | is|  
|---|---|---|:--|
| ["178.10.10.42"]         | `is` | "ipv4"  | true  |
| ["178.10.10.**424242**"] | `is` | "ipv4"  | false |
| ["178.10.10.42"]         | `is` | "ipv6"  | false |

###### ODRL
```json
{
    //...
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
        //...
}
```

### net:inside

```pseudocode
inside(["178.10.10.42"], ["178.10.10.1", "178.10.10.255"]) = true
```
|   |   |   | is|  
|---|---|---|:--|
| ["178.10.10.42"]     | `inside` | ["178.10.10.1", "178.10.10.255"] | true  |
| ["178.10.**42**.42"] | `inside` | ["178.10.10.1", "178.10.10.255"] | false  |

### net:contains

```pseudocode
contains(["178.10.10.1", "178.10.10.256"], ["178.10.10.42"]) = true
contains(["178.10.10.1", "178.10.10.256"], ["178.10.42.42"]) = false
```
|   |   |   | is|  
|---|---|---|:--|
| ["178.10.10.1", "178.10.10.256"] | `contains` | ["178.10.10.42"]     | true   |
| ["178.10.10.1", "178.10.10.256"] | `contains` | ["178.10.**42**.42"] | false  |

---

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
|---|---|---|:--|
| "C8-5B-76-B5-43-2B" | `equals`     | "C8-5B-76-B5-43-2B" | true   |
| "C8-5B-76-B5-43-2B" | `startsWith` | "C8-5B-76"          | true   |
| "C8-5B-76-B5-43-2B" | `endsWith`   | "B5-43-2B"          | true   |
| "C8-5B-76-B5-43-2B" | `contains`   | "5B-76-B5"          | true   |

---