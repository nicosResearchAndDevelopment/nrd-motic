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

The `geom` is heavily inspired by the _Egenhofer Topology_ for two-dimensional spacial objects and the _Simple Features_ specifications that translate well into the _GeoJSON_ specification, especially for usage on the world wide web. Besides those you may find the _Region Connection Calculus (RCC8)_, but the `geom` currently does not adopt its definitions, as it only intruduces 2 more edge cases which are rarely useful and difficult to compute.

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

This provides a way to categorize the relation between two sets, by simply denoting a DE-9IM notation for the requirements of that relation. All geometric operators/predicates below will have a notation in this form to allow a better understanding of the requirements.

## Spatial Properties

### `geom:coordinates`

The `geom:coordinates` contains all relevant information about a geometries structure. It consists of a deep array tree with numbers as leafes. The structure is determined by the type of the geometry. All leaf numbers are part of a position array, containing at least two position coordinates, representing __x__ and __y__ on a coordinate plain. E.g. for a point (the smallest geometry type) this means the `geom:coordinates` member is an array with two points (or more).

```json
{
    "@type": "geom:Point",
    "geom:coordinates": [0, 0]
}
```

### `geom:geometries`

The `geom:geometries` identifies all members, that are part of a geometry collection. Those members themselfes must be one of the seven sub types of `geom:Geometry`.

```json
{
    "@type": "geom:GeometryCollection",
    "geom:geometries": [{
        "@type": "geom:Point",
        "geom:coordinates": [0, 0]
    }, {
        "@type": "geom:LineString",
        "geom:coordinates": [
            [1, 0],
            [0, 1]
        ]
    }]
}
```

### `geom:reference`

> The coordinate reference system for all GeoJSON coordinates is a geographic coordinate reference system, using the World Geodetic System 1984 (WGS 84) datum, with longitude and latitude units of decimal degrees. This is equivalent to the coordinate reference system identified by the Open Geospatial Consortium (OGC) URN urn:ogc:def:crs:OGC::CRS84. An OPTIONAL third-position element SHALL be the height in meters above or below the WGS 84 reference ellipsoid.  In the absence of elevation values, applications sensitive to height or depth SHOULD interpret positions as being at local ground or sea level.

Note that the `geom:reference` is just a hint for interpreters, whether particular geometries are comparable. A conversion between reference systems is not inherintly provided. Also all calculations are based on two-dimensional geometries. That means map projections from a sphere cannot assume a logical connection between for example 180° east and 180° west. In cases that polygons span over it, they should be split into appropriate multi polygons along the edges.

```json
{
    "@type": "geom:Point",
    "geom:coordinates": [51.9500023, 7.4840148],
    "geom:reference": { 
        "@id": "http://dbpedia.org/page/Geographic_coordinate_system" 
    }
}
```

A use case would be to define a custom reference system relative to the layout of a building, with the origin on one corner and the coordinates represented in meters. All geometric properties still apply, as long as it is a two-dimensional euclidean topology.

## [Spatial Objects](https://tools.ietf.org/html/rfc7946#section-3)

### `geom:Geometry`

- isAbstract: `true`
- see: [geo:Geometry](https://tools.ietf.org/html/rfc7946#section-3.1)

> A Geometry object represents points, curves, and surfaces in coordinate space. Every Geometry object is a GeoJSON object no matter where it occurs in a GeoJSON text.
>
> - The value of a Geometry object's "type" member MUST be one of the seven geometry types (see Section 1.4).
> - A GeoJSON Geometry object of any type other than __GeometryCollection__ has a member with the name _coordinates_. The value of the _coordinates_ member is an array. The structure of the elements in this array is determined by the type of geometry. GeoJSON processors MAY interpret Geometry objects with empty _coordinates_ arrays as null objects.

### `geom:Point`

- subClassOf: `geom:Geometry`
- see: [geo:Point](https://tools.ietf.org/html/rfc7946#section-3.1.2)

> For type __Point__, the _coordinates_ member is a single position.

```
*
one point
```

```json
{
    "@type": "geom:Point",
    "geom:coordinates": [0, 0]
}
```

### `geom:MultiPoint`

- subClassOf: `geom:Geometry`
- see: [geo:MultiPoint](https://tools.ietf.org/html/rfc7946#section-3.1.3)

> For type __MultiPoint__, the _coordinates_ member is an array of positions.

```
    *


*
two points
```

```json
{
    "@type": "geom:MultiPoint",
    "geom:coordinates": [
        [0, 0],
        [1, 2]
    ]
}
```

### `geom:LineString`

- subClassOf: `geom:Geometry`
- see: [geo:LineString](https://tools.ietf.org/html/rfc7946#section-3.1.4)

> For type __LineString__, the _coordinates_ member is an array of two or more positions.

```
.---.
|
.
one line around a corner
```

```json
{
    "@type": "geom:LineString",
    "geom:coordinates": [
        [0, 0],
        [0, 1],
        [1, 1]
    ]
}
```

### `geom:MultiLineString`

- subClassOf: `geom:Geometry`
- see: [geo:MultiLineString](https://tools.ietf.org/html/rfc7946#section-3.1.5)

> For type __MultiLineString__, the _coordinates_ member is an array of LineString coordinate arrays.

```
    .---.
    |
.---*
|
.
two lines as a stair
```

```json
{
    "@type": "geom:MultiLineString",
    "geom:coordinates": [[
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

### `geom:Polygon`

- subClassOf: `geom:Geometry`
- see: [geo:Polygon](https://tools.ietf.org/html/rfc7946#section-3.1.6)

> To specify a constraint specific to Polygons, it is useful to introduce the concept of a linear ring:
> 
> - A linear ring is a closed LineString with four or more positions. The first and last positions are equivalent, and they MUST contain identical values; their representation SHOULD also be identical.
> - A linear ring is the boundary of a surface or the boundary of a hole in a surface.
> - A linear ring MUST follow the right-hand rule with respect to the area it bounds, i.e., exterior rings are counterclockwise, and holes are clockwise.

> Though a linear ring is not explicitly represented as a GeoJSON geometry type, it leads to a canonical formulation of the Polygon geometry type definition as follows:
> 
> - For type __Polygon__, the _coordinates_ member MUST be an array of linear ring coordinate arrays.
> - For Polygons with more than one of these rings, the first MUST be the exterior ring, and any others MUST be interior rings. The exterior ring bounds the surface, and the interior rings (if present) bound holes within the surface.

```
.-------.
| .---. |  
| |   | |
| .---. |
.-------.
square with hole
```

```json
{
    "@type": "geom:Polygon",
    "geom:coordinates": [[
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

### `geom:MultiPolygon`

- subClassOf: `geom:Geometry`
- see: [geo:MultiPolygon](https://tools.ietf.org/html/rfc7946#section-3.1.7)

> For type __MultiPolygon__, the _coordinates_ member is an array of Polygon coordinate arrays.

```
     .----.
     |    |
.----.----.
|    |
.----.
two squares corner on corner
```

```json
{
    "@type": "geom:MultiPolygon",
    "geom:coordinates": [[[
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

### `geom:GeometryCollection`

- subClassOf: `geom:Geometry`
- see: [geo:GeometryCollection](https://tools.ietf.org/html/rfc7946#section-3.1.8)

> A GeoJSON object with type __GeometryCollection__ is a Geometry object. A GeometryCollection has a member with the name _geometries_. The value of _geometries_ is an array.  Each element of this array is a GeoJSON Geometry object.  It is possible for this array to be empty.

> Unlike the other geometry types described above, a GeometryCollection can be a heterogeneous composition of smaller Geometry objects. For example, a Geometry object in the shape of a lowercase roman "i" can be composed of one point and one LineString.
> 
> GeometryCollections have a different syntax from single type Geometry objects (Point, LineString, and Polygon) and homogeneously typed multipart Geometry objects (MultiPoint, MultiLineString, and MultiPolygon) but have no different semantics.  Although a GeometryCollection object has no _coordinates_ member, it does have coordinates: the coordinates of all its parts belong to the collection.  The _geometries_ member of a GeometryCollection describes the parts of this composition.  Implementations SHOULD NOT apply any additional semantics to the _geometries_ array.
> 
> To maximize interoperability, implementations SHOULD avoid nested GeometryCollections.  Furthermore, GeometryCollections composed of a single part or a number of parts of a single type SHOULD be avoided when that single part or a single object of multipart type (MultiPoint, MultiLineString, or MultiPolygon) could be used instead.

```
.----.
     |
*    .
point under a corner line
```

```json
{
    "@type": "geom:GeometryCollection",
    "geometries": [{
        "@type": "geom:Point",
        "geom:coordinates": [0, 0]
    }, {
        "@type": "geom:LineString",
        "geom:coordinates": [
            [0, 1],
            [1, 1],
            [1, 0]
        ]
    }]
}
```

## [Spatial Predicates](https://en.wikipedia.org/wiki/DE-9IM#Spatial_predicates)

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

#### odrl:Constraint - geom:intersects

```json
{
    "@context": {
        "odrl": "http://www.w3.org/ns/odrl.jsonld",
        "geom": "https://github.com/nicosResearchAndDevelopment/nrd-motic/blob/master/decide/operator/geometry.md"
    },
    "odrl:constraints": [{
        "odrl:leftOperand": {
            "@type": "geom:LineString",
            "geom:coordinates": [
                [0, 0],
                [1, 1]
            ]
        },
        "odrl:operator": "geom:intersects",
        "odrl:rightOperand": {
            "@type": "geom:LineString",
            "geom:coordinates": [
                [0, 1],
                [1, 0]
            ]
        }
    }]
}
```

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
    - reverse of _inside_, _within_
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
    - reverse of _coveredBy_

### `geom:coveredBy`

- __Prerequisite:__ `[ T*F **F *** ]` _OR_ `[ *TF **F *** ]` _OR_ `[ **F T*F *** ]` _OR_ `[ **F *TF *** ]`
- __Describtion:__ A geometry __A__ is _coveredBy_ a geometry __B__, if:
    - at least one point __a__ in __A__ is also in __B__
    - and there exists no point in __A__, which is in the exterior of __B__.
- __Properties:__ 
    - reflexive
    - transitive
    - reverse of _covers_

### `geom:inside`, `geom:within`

- __Prerequisite:__ `[ T*F **F *** ]`
- __Describtion:__ A geometry __A__ is _inside_ a geometry __B__, if:
    - for every point __a__ in __A__, __a__ is not in the exterior of __B__
    - and at least one point __c__ in __A__ is also in the interior of __B__.
- __Properties:__ 
    - reflexive
    - transitive
    - reverse of _contains_

#### odrl:Constraint - geom:inside

```json
{
    "@context": {
        "odrl": "http://www.w3.org/ns/odrl.jsonld",
        "geom": "https://github.com/nicosResearchAndDevelopment/nrd-motic/blob/master/decide/operator/geometry.md"
    },
    "odrl:constraints": [{
        "odrl:leftOperand": {
            "@type": "geom:Point",
            "geom:coordinates": [51.9500023, 7.4840148],
            "geom:reference": { "@id": "http://dbpedia.org/page/Geographic_coordinate_system" }
        },
        "odrl:operator": "geom:inside",
        "odrl:rightOperandReference": { "@id": "http://dbpedia.org/page/Germany" }
    }]
}
```

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
