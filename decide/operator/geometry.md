# `geom` Binary Operators

## Geometry

- [RFC 7946 - The GeoJSON Format](https://tools.ietf.org/html/rfc7946)
- [W3C Geospatial Ontologies](https://www.w3.org/2005/Incubator/geo/XGR-geo-ont/)
- [W3C Geospatial Vocabulary](https://www.w3.org/2005/Incubator/geo/XGR-geo/)
- [Topologische Beziehungen in GeoInformationssystemen](http://digbib.ubka.uni-karlsruhe.de/volltexte/documents/764669)
- [GeoSPARQL - A Geographic Query Language for RDF Data](https://www.ogc.org/standards/geosparql)
- [Dimensionally Extended nine-Intersection Model (DE-9IM)](https://en.wikipedia.org/wiki/DE-9IM)
- [Topologische Beziehungen](http://www.gitta.info/SpatialQueries/de/html/TopoBasedOps_learningObject1.html)
- [Simple Feature Access](https://www.ogc.org/standards/sfa)

The `geom` is heavily inspired by the _Egenhofer Topology_ for two-dimensional spacial objects and the _Simple Features_ specifications that translate well into the _GeoJSON_ specification, especially for usage on the world wide web. Besides those you may find the _Region Connection Calculus (RCC8)_, although the `geom` currently does not adopt its definitions, as it only intruduces 2 more edge cases which are rarely useful.

### DE-9IM

The _Dimensionally Extended nine-Intersection Model_ is a notation standard for describing spatial relations. The relation between two sets is reduced to statements about the interior, the exterior and the boundary of the sets.

```
  Exterior
    .----------.
    |          |
    | Interior |
    |          |
    .-Boundary-.
```

As the dimension of the set varies, the boundary might not be all the same, e.g. a 1 dimensional line has its boundary at its two ends. For the _DE-9IM_ you lock at the intersection of the interior, boundary and exterior of the two sets and denote their dimension, which gives you 9 values.

```
  .------.  .-----.
  |  a   |  |  c  |
  |  .---+--.-----.
  .--+---.   .----|
     |    b  |  d |
     .------------.
```

| DE9IM(a, b) | Interior(b) | Boundary(b) | Exterior(b) |
|---|---|---|---|
| __Interior(a)__ | 2 | 1 | 2 |
| __Boundary(a)__ | 1 | 0 | 1 |
| __Exterior(a)__ | 2 | 1 | 2 |

The dimension of empty sets would be denoted -1, but there is a simpler and more often used boolean notation, which replaces all -1 with __F__ (false), all other numbers with __T__ (true) and all values that are irrelevant for a particular consideration are denoted with __*__ (any). Also the letters are flattened out to one line. With this we would get the following for a, b, c and d:

- DE9IM(a, b) = [ T T T T T T T T T ]
- DE9IM(a, c) = [ F F T F F T T T T ]
- DE9IM(b, c) = [ F F T F T T T T T ]
- DE9IM(b, d) = [ T T T F T T F F T ]
- ...

This provides a way to categorize the relation between two sets, by simply denoting a DE-9IM notation for the requirements of that relation. All following geometric operators will have a notation
in this form to allow a better understanding of the requirements.

## Spatial Operators

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
|---|---|---|:---|
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
|---|---|---|:---|
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
|---|---|---|:---|
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
|---|---|---|:---|
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
|---|---|---|:---|
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
|---|---|---|:---|
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
|---|---|---|:---|
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
|---|---|---|:---|
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
|---|---|---|:---|
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
|---|---|---|:---|
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
|---|---|---|:---|
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
|---|---|---|:---|:---|
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
|---|---|---|:---|
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
|---|---|---|:---|
| **i** | `equals`   | **j** | true  |
| **i** | `overlaps` | **j** | true  |
| **i** | `contains` | **j** | true  |
| **j** | `contains` | **i** | true  |


---

## Spatial Objects

### Point
### MultiPoint
### LineString
### MultiLineString
### Polygon
### MultiPolygon
### GeometryCollection