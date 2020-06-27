# `time` Binary Operators

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
}
```

#### fno
```turtle
gbx:timeBeforeExecution
    a                    fno:Execution ;
	# TODO: is rhis the right way to express the parameters?
	# time, i
    gbx:timeLeftOperand  [ fno:type          time:TemporalEntity ;
                           time:hasBeginning "2020-06-24T15:35:19.42Z" ;
                           time:hasEnd       "2020-06-24T15:35:19.42Z" ; ] ;
    fno:executes         gbx:timeBeforeFunction ;
	#time, j
    gbx:timeRightOperand [ fno:type          time:TemporalEntity ;
                           time:hasBeginning "2020-06-24T15:35:19.43Z" ;
                           time:hasEnd       "2020-06-24T15:35:19.43Z" ; ] ;
.
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
| :--- | --- | :--- | :--- |
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
|:---|---|:---|:---|
| [ "2019-04-01T00:00:00Z" ] | `Equals`     | [ "2019-04-01T00:00:00Z" ] | true   |

### time:In
### time:Disjoint

---