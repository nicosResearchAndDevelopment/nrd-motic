# `event` Binary Operators

## Event

A very interesting and not seldom used context.

- Prefix: `event`

- seeAlso: [https://schema.org/Event](https://schema.org/Event)
- seeAlso: [https://schema.org/eventSchedule](https://schema.org/eventSchedule)

### `event:probe`

The `event:probe` of given environment is the subject and will be represented 
 as the outcome of `odrl:leftOperand` and will be compared in given `constraint`.

```json
{
    "@context":                    [
        "http://www.nicos-rd.com/event/",
        {
            "event": "http://www.nicos-rd.com/event/",
            "xsd":     "http://www.w3.org/2001/XMLSchema#"
        }
    ],
    "@type":                       "event:probe",
    "dateTimeStamp": {
      "@type": "xsd:dateTimeStamp",
      "@value": "2020-07-07T16-42-00"
    }
}
```

### Operator `event:inside`

```text

```

|   |   |   | is |
|---|---|---|:---|
| "event:probe"   | `event:inside` | "example:birthday"           | true  |


### Examples

#### Exhibition

The use case:
> Provide permission to a data consumer to use the data
> during the exhibition.

Time period:

```text
27.04.2020, 10:00 - 14.05.2020, 17:00
```

##### Enforcement and Right Operand

[enforced 'event:probe'](./examples/idsa_virtual_expo_probe.json)

[rightOperand 'exhibition'](./examples/idsa_virtual_expo_2020_rightOperand.json)

---

#### Accident

The use case:
> If an accident occurred, provide permission to read the
> geographic location.

better:

The use case:
> Provide permission to read the geographic location if status
> of accident is detected.

>>> TODO: what s ment: the damaged status (of the car) OR the accident itself as an event?

Assuming, we can detect it by observing cars airbag, status is present:
```text
probe.airbagOpenedAt time:Before now
```

or

```text
probe.airbagIsOpened equals true
```
---

#### Barbershop

The use case:
> Provide customer permission to shop if it's opened.

It is open at:

```text
Monday:    09:00 - 12:00 and 13:00 - 18:00 
Tuesday:   09:00 - 12:00 and 13:00 - 18:00
Wednesday: 09:00 - 12:00 and 13:00 - 18:00
Thursday:  09:00 - 12:00 and 13:00 - 20:00
Friday:    09:00 - 12:00 and 13:00 - 18:00
Saturday:  09:00 - 13:00
```

| [time:dayOfWeek](https://www.w3.org/TR/owl-time/#time:DayOfWeek)  | [xsd:time](https://www.w3.org/TR/xmlschema-2/#time) `from` | [xsd:time](https://www.w3.org/TR/xmlschema-2/#time) `to` |
|---|---|---|
| [time:Monday](https://www.w3.org/TR/owl-time/#time:Monday)    |||
|                | 09:00 | 12:00 |
|                | 13:00 | 18:00 |
| [time:Tuesday](https://www.w3.org/TR/owl-time/#time:Tuesday)    |||
|                | 09:00 | 12:00 |
|                | 13:00 | 18:00 |
| [time:Wednesday](https://www.w3.org/TR/owl-time/#time:Wednesday)    |||
|                | 09:00 | 12:00 |
|                | 13:00 | 18:00 |
| [time:Thursday](https://www.w3.org/TR/owl-time/#time:Thursday)    |||
|                | 09:00 | 12:00 |
|                | 13:00 | **20:00** |
| [time:Friday](https://www.w3.org/TR/owl-time/#time:Friday)    |||
|                | 09:00 | 12:00 |
|                | 13:00 | 18:00 |
| [time:Saturday](https://www.w3.org/TR/owl-time/#time:Saturday)    |||
|                | 09:00 | 13:00 |
|   |   |   |

Remarks:
- this table above results in a complex rightOperand.
- probe

...but not at

```text
24.12.
01.01.
```
| [xsd:gMonthDay]($)  |||
|---|---|---|
| "--12-24" | christmas  |
| "--01-01" | new year    |
|||

So, handling a probe out this scope means, to handle
 [`xsd:time`](https://www.w3.org/TR/xmlschema-2/#time) and
 [`xsd:date`](https://www.w3.org/TR/xmlschema-2/#date), also.
 
Probe:

```json
{
    "@context":                    [
        "http://www.nicos-rd.com/event/",
        {
            "event": "http://www.nicos-rd.com/event/",
            "xsd":     "http://www.w3.org/2001/XMLSchema#"
        }
    ],
    "@type":                       "event:probe",
    "dateTimeStamp": {
      "@type": "xsd:dateTimeStamp",
      "@value": "2020-07-07T16-42-00"
    }
}
``` 

|   |   |   | is |
|---|---|---|:---|
| "event:probe"   | `event:inside` | "shop:open"           | true  |
 

Probe:

```json
{
    "@context":                    [
        "http://www.nicos-rd.com/event/",
        {
            "event": "http://www.nicos-rd.com/event/",
            "xsd":     "http://www.w3.org/2001/XMLSchema#"
        }
    ],
    "@type":                       "event:probe",
    "dateTimeStamp": {
      "@type": "xsd:dateTimeStamp",
      "@value": "2020-12-24T10-00-00"
    }
}
``` 

|   |   |   | is |
|---|---|---|:---|
| "event:probe"   | `event:inside` | "shop:open"           | false  |


---
