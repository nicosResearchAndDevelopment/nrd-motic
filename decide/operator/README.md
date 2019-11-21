# motic.decide Operator

REMARK: this document is NOT normative and at this point a draft only. 

Please follow, to be informed on editings...


## ODRL

See also:

- [ODRL Information Model 2.2](https://www.w3.org/TR/odrl-model/)
- [IPTC RightsML Standard 2.0](https://iptc.org/std/RightsML/2.0/RightsML_2.0-specification.html)


### Constraint Operators

[constraintRelationalOperators](https://www.w3.org/TR/odrl-vocab/#constraintRelationalOperators)

#### [eq](http://www.w3.org/ns/odrl/2/eq)
Indicating that a given value equals the right operand of
 the Constraint.

#### [gt](http://www.w3.org/ns/odrl/2/gt)
Indicating that a given value is greater than the right operand of
 the Constraint.

#### [gteq](http://www.w3.org/ns/odrl/2/gteq)
Indicating that a given value is greater than or equal to the right
 operand of the Constraint.

#### [hasPart](http://www.w3.org/ns/odrl/2/hasPart)
A set-based operator indicating that a given value contains the right
 operand of the Constraint.

#### [isA](http://www.w3.org/ns/odrl/2/isA)
A set-based operator indicating that a given value is an instance of
 the right operand of the Constraint.

#### [isAllOf](http://www.w3.org/ns/odrl/2/isAllOf)
A set-based operator indicating that a given value is all of the
 right operand of the Constraint.

#### [isAnyOf](http://www.w3.org/ns/odrl/2/isAnyOf)
A set-based operator indicating that a given value is any of the
 right operand of the Constraint.

#### [isNoneOf](http://www.w3.org/ns/odrl/2/isNoneOf)
A set-based operator indicating that a given value is none of the
 right operand of the Constraint.

#### [isPartOf](http://www.w3.org/ns/odrl/2/isPartOf)
A set-based operator indicating that a given value is contained by the
 right operand of the Constraint.

#### [lt](http://www.w3.org/ns/odrl/2/lt)
Indicating that a given value is less than the right operand of the
 Constraint.

#### [lteq](http://www.w3.org/ns/odrl/2/lteq)
Indicating that a given value is less than or equal to the right
 operand of the Constraint.

#### [neq](http://www.w3.org/ns/odrl/2/neq)
Indicating that a given value is not equal to the right operand of
 the Constraint.


### Logical Constraint Operands

- [term-LogicalConstraint](https://www.w3.org/TR/odrl-vocab/#term-LogicalConstraint)
- [constraintLogicalOperands](https://www.w3.org/TR/odrl-vocab/#constraintLogicalOperands)

> This property *MUST* only be used for Logical Constraints, and the list
 of operand values *MUST* be Constraint instances.

#### [and](http://www.w3.org/ns/odrl/2/and)
The relation is satisfied when all of the Constraints are satisfied.

#### [andSequence](http://www.w3.org/ns/odrl/2/andSequence)

[**andSequence**](https://www.w3.org/TR/odrl-vocab/#term-andSequence)

The relation is satisfied when each of the Constraints are satisfied in
 the order specified.

#### [or](http://www.w3.org/ns/odrl/2/or)

[**or**](https://www.w3.org/TR/odrl-vocab/#term-or)

The relation is satisfied when at least one of the Constraints is
 satisfied.

#### [xone](http://www.w3.org/ns/odrl/2/xone)
"Only One": [**xone**](https://www.w3.org/TR/odrl-vocab/#term-xone)

The relation is satisfied when only one, and not more, of the Constaints
 is satisfied.

---

## Time

See: [Time Ontology in OWL](https://www.w3.org/TR/owl-time/).

### Before
### After
### Meets
### MetBy
### Overlaps
### OverlappedBy
### Starts
### StartedBy
### During
### Contains
### Finishes
### FinishedBy
### Equals
### _In_
### _Disjoint_

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

---


## Geometry

### equals

Two sets _A_ and _B_ are equal, if for every point _a_ in _A_ and every
 point _b_ in _B_, also _a_ is in _B_ and _b_ is in _A_.

- symmetric


### intersects

A set _A_ intersects a set _B_, if there exists a point _p_, such that
 _p_ is in _A_ and also in _B_.

- symmetric
- opposite of disjoint


### disjoint

A set _A_ is disjoint with a set _B_, if there exists no point _p_,
 such that _p_ is in _A_ and also in _B_.
 
 - symmetric
 - opposite of intersects


### contains

A set _A_ contains a set _B_, if for every point _b_ in _B_, also _b_
 is in _A_.

- not symmetric
- if _A_ contains _B_ and _B_ contains _A_, _A_ and _B_ are equal.


### touches

_Two sets are touching, if they intersect and their intersection only
 includes their boundaries.
 
- also the tangent vectors of both sets at the point(s) of intersection
 should point in the same direction.
- also the intersection shall have at least 1 times less dimensionality
 than the maximum dimensionality of the two sets._ (difficult to define,
 clearer definition needed)
- symmetric


### overlaps

A set _A_ overlaps a set _B_, if _A_ intersects _B_ but _A_ does
 not touch _B_.
 
- _Also the intersection shall have at least the dimensionality of the
 minimum dimensionality of the two sets._
- symmetric


#### Point

##### disjoint

```
    . i

        (.) j
```
|   |   |   | is|
|---|---|---|:--|
| **i**   | `disjoint` | **j**   | true  |

##### equals

```
    (.) i, j

"." is point "(*)" > "(.)", so
    i[42.0, 42.0]
is in the same place as
    j[42.0, 42.0]
```

---

#### Polygon, (n > 2)

##### disjoint

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

##### touches

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

##### overlaps

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

##### contains

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

##### equals

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

#### line

##### disjoint

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

##### touches

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

##### intersects

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


##### overlaps

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

##### contains

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

##### equals

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

### memberOf

Any `foaf:Agent`. can be member of some other agent.


## Group

`foaf:Group` is a `foaf:Agent`.

### hasMember

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
| "exA:rumpelstilz"  | `memberOf`  | "exB:maintainerGroup"  | false |


### Organization

`org:Organization` grounded on `foaf:Organization`.

- [vocab-org](https://www.w3.org/TR/vocab-org/)
- [term_Organization](http://xmlns.com/foaf/spec/#term_Organization)

### hasSubOrganization

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


### hasUnit

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

### hasSite

|   |   |   | is|  
|---|---|---|:--|
| "urn:companyA"   | `hasSite` | "urn:atlantis"   | true  |
| "urn:companyA"   | `hasSite` | "urn:wonderland" | false |
| "urn:companyB"   | `hasSite` | "urn:atlantis"   | true  |
| "urn:nirvana"    | `siteOf`  | "urn:companyA"   | true  |
| "urn:wonderland" | `siteOf`  | "urn:companyA"   | false |

### hasPrimarySite

```
```

### hasRegisteredSite

```
```

---