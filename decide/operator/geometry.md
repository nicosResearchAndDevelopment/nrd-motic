# `geometry` Binary Operators


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
