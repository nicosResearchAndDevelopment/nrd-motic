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

The topological dimension of _points_ is always __0__, of _lines_ __1__ and of geometries _with an area_ __2__. The dimension of _empty sets_ would be denoted __-1__, but there is a simpler and more often used boolean notation, which replaces all -1 with __F__ (false), all other numbers with __T__ (true) and all values that are irrelevant for a particular consideration are denoted with __*__ (any). Also the letters are flattened out to one line. With this we would get the following for a, b, c and d:

- DE9IM(a, b) = `[ TTT TTT TTT ]`
- DE9IM(a, c) = `[ FFT FFT TTT ]`
- DE9IM(b, c) = `[ FFT FTT TTT ]`
- DE9IM(b, d) = `[ TTT FTT FFT ]`
- ...

This provides a way to categorize the relation between two sets, by simply denoting a DE-9IM notation for the requirements of that relation. All following geometric operators will have a notation
in this form to allow a better understanding of the requirements.

## Spatial Operators

### `geom:equals`

- __Prerequisite:__ `[ T*F **F FF* ]`
- __Describtion:__ Two geometries __A__ and __B__ are _equal_, if:
    - for every point __a__ in __A__, __a__ is also in __B__
    - and for every point __b__ in __B__, __b__ is also in __A__.
- __Properties:__
    - reflexive
    - symmetric
    - transitive
    - If __A__ _contains_ __B__ and __A__ is _inside_ __B__, __A__ must be _equal_ to __B__.

### `geom:disjoint`

- __Prerequisite:__ `[ FF* FF* *** ]`
- __Describtion:__ Two geometries __A__ and __B__ are _disjoint_, if:
    - for every point __a__ in __A__, __a__ is not in __B__.
- __Properties:__ 
    - anti reflexive
    - symmetric
    - opposite of _intersects_


### `geom:intersects`

- __Prerequisite:__ `[ T** *** *** ]` _OR_ `[ *T* *** *** ]` _OR_ `[ *** T** *** ]` _OR_ `[ *** *T* *** ]`
- __Describtion:__ Two geometries __A__ and __B__ _intersect_, if:
    - their exists at least one point __a__ in __A__, that is also in __B__.
- __Properties:__
    - reflexive
    - symmetric
    - opposite of _disjoint_

### `geom:touches`, `geom:meets`

- __Prerequisite:__ `[ FT* *** *** ]` _OR_ `[ F** T** *** ]` _OR_ `[ F** *T* *** ]`
- __Describtion:__ Two geometries __A__ and __B__ are _touching_, if:
    - for every point __a__ in the interior of __A__, __a__ is not in the interior of __B__
    - and at least one point __c__ on the boundary of __A__ is on the boundary of __B__.
- __Properties:__ 
    - symmetric

### `geom:contains`

- __Prerequisite:__ `[ T** *** FF* ]`
- __Describtion:__ A geometry __A__ _contains_ a geometry __B__, if:
    - for every point __b__ in __B__, __b__ is also in __A__
    - and at least one point __c__ in the interior of __B__ is also in the interior of __A__.
- __Properties:__ 
    - transitive
    - If __A__ _equals_ __B__, __A__ must _contain_ __B__ and __B__ must _contain_ __A__.

### `geom:overlaps`

- __Prerequisite:__ `dim(A) == dim(B)` _AND_ _ONE OF_:
    - `dim(A) == 1` _AND_ `[ 1*T *** T** ]`
    - `dim(A) != 1` _AND_ `[ T*T *** T** ]`
- __Describtion:__ The geometries __A__ and __B__ are _overlapping_, if:
    - __A__ _intersects_ __B__
    - and the _dimension_ of the _intersection_ is the same as the _dimension_ of __A__ and __B__.
- __Properties:__ 
    - symmetric
    - If __A__ _intersects_ __B__, but does not _touch_ __B__ nor _equals_ __B__, __A__ and __B__ must _overlap_.

### `geom:covers`

- __Prerequisite:__ `[ T** *** FF* ]` _OR_ `[ *T* *** FF* ]` _OR_ `[ *** T** FF* ]` _OR_ `[ *** *T* FF* ]`
- __Describtion:__ A geometry __A__ _covers_ a geometry __B__, if:
    - at least one point __b__ in __B__ is also in __A__
    - and there exists no point in __B__, which is in the exterior of __A__.
- __Properties:__ 
    - reflexive
    - transitive
    - opposite of _coveredBy_

### `geom:coveredBy`

- __Prerequisite:__ `[ T*F **F *** ]` _OR_ `[ *TF **F *** ]` _OR_ `[ **F T*F *** ]` _OR_ `[ **F *TF *** ]`
- __Describtion:__ A geometry __A__ is _coveredBy_ a geometry __B__, if:
    - at least one point __a__ in __A__ is also in __B__
    - and there exists no point in __A__, which is in the exterior of __B__.
- __Properties:__ 
    - reflexive
    - transitive
    - opposite of _covers_

### `geom:inside`, `geom:within`

- __Prerequisite:__ `[ T*F **F *** ]`
- __Describtion:__ A geometry __A__ is _inside_ a geometry __B__, if:
    - for every point __a__ in __A__, __a__ is not in the exterior of __B__
    - and at least one point __c__ in __A__ is also in the interior of __B__.
- __Properties:__ 
    - reflexive
    - transitive
    - opposite of _contains_

### `geom:crosses`

- __Prerequisite:__ _ONE OF_:
    - `dim(A) < dim(B)` _AND_ `[ T*T *** *** ]`
    - `dim(A) > dim(B)` _AND_ `[ T** *** T** ]`
    - `dim(A) == 1 || dim(B) == 1` _AND_ `[ 0** *** *** ]`
- __Describtion:__ A geometry __A__ _crosses_ a geometry __B__, if:
    - __A__ intersects __B__
    - and the _dimension_ of the _intersection_ is less than the maximum _dimension_ of __A__ and __B__.
- __Properties:__ 
    - symmetric

## Spatial Objects

### `geom:Point`

```json
{
    "@type": "geom:Point",
    "geom:coords": [0, 0]
}
```

```
*
one point
```

### `geom:MultiPoint`

```json
{
    "@type": "geom:MultiPoint",
    "geom:coords": [
        [0, 0],
        [1, 2]
    ]
}
```

```
    *


*
two points
```

### `geom:LineString`

```json
{
    "@type": "geom:LineString",
    "geom:coords": [
        [0, 0],
        [0, 1],
        [1, 1]
    ]
}
```

```
.---.
|
.
one line around a corner
```

### `geom:MultiLineString`

```json
{
    "@type": "geom:MultiLineString",
    "geom:coords": [[
        [0, 0],
        [0, 1],
        [1, 1]
    ], [
        [1, 1],
        [1, 2],
        [2, 2]
    ]]
}
```

```
    .---.
    |
.---*
|
.
two lines as a stair
```

### `geom:Polygon`

```json
{
    "@type": "geom:Polygon",
    "geom:coords": [[
        [0, 0],
        [1, 0],
        [1, 1],
        [0, 1],
        [0, 0]
    ], [
        [0.2, 0.2],
        [0.8, 0.2],
        [0.8, 0.8],
        [0.2, 0.8],
        [0.2, 0.2]
    ]]
}
```

```
.-------.
| .---. |  
| |   | |
| .---. |
.-------.
square with hole
```

### `geom:MultiPolygon`

```json
{
    "@type": "geom:MultiPolygon",
    "geom:coords": [[[
        [0, 0],
        [1, 0],
        [1, 1],
        [0, 1],
        [0, 0]
    ]], [[
        [1, 1],
        [2, 1],
        [2, 2],
        [1, 2],
        [1, 1]
    ]]]
}
```

```
     .----.
     |    |
.----.----.
|    |
.----.
two squares corner on corner
```

### `geom:GeometryCollection`

```json
{
    "@type": "geom:GeometryCollection",
    "geometries": [{
        "@type": "geom:Point",
        "geom:coords": [0, 0]
    }, {
        "@type": "geom:LineString",
        "geom:coords": [
            [0, 1],
            [1, 1],
            [1, 0]
        ]
    }]
}
```

```
.----.
     |
*    .
point under a corner line
```

## Examples

### Point

#### geom:disjoint

```
    . i

        (.) j
```
|   |   |   | is|
|---|---|---|:---|
| **i**   | `disjoint` | **j**   | true  |

#### geom:equals

```
    (.) i, j

"." is point "(*)" > "(.)", so
    i[42.0, 42.0]
is in the same place as
    j[42.0, 42.0]
```

---

### Polygon, (n > 2)

#### geom:disjoint

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

#### geom:touches

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

#### geom:overlaps

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

#### geom:contains

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

#### geom:equals

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

### Line

#### geom:disjoint

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

#### geom:touches

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

#### geom:intersects

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


#### geom:overlaps

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

#### geom:contains

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
| **i** | `intersects` | **j** | true  |
| *but* |              |       |       |
| **i** | `touches`    | **j** | false |
| **i** | `overlaps`   | **j** | false  |

---

#### geom:equals

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
| **i** | `contains` | **j** | true  |
| **j** | `contains` | **i** | true  |
| *but* |              |       |       |
| **i** | `overlaps` | **j** | false  |


---
