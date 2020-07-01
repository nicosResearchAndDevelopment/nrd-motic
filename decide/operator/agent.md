# `agent` Binary Operators

## Agent

- Prefix: `foaf`

### foaf:memberOf

Any `foaf:Agent`. can be member of some other `foaf:Agent`.

## Group

`foaf:Group` is a `foaf:Agent`.

### foaf:hasMember

The group `exA:users` contains all users of example-company "A".

The group `exA:admin` (belonging to example-company "A") has three
 members, so has group `exA:maintainer` three others:

##### Example
```text
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
|---|---|---|:---|
| "exA:adminGroup"   | `hasMember` | "exA:zaphod"           | true  |
| "exA:adminGroup"   | `hasMember` | "exA:bob"              | false |
| "exA:ford"         | `memberOf`  | "exA:adminGroup"       | true  |
| *but*              |             |                        |       |
| "exA:rumpelstilz"  | `memberOf`  | "exB:maintainerGroup"  | false |


---