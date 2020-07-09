# `event` Binary Operators

## Event

A very interesting and not seldom used context.

- Prefix: `event`


### event `probe`

The `event:probe` of given environment is the subject and will be represented 
 as the outcome of `odrl:leftOperand` and will be compared in given `contraint`.

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


### Example

#### exhibition

The use case:
> Provide permission to a data consumer to use the data
> during the exhibition.

Duration:
```text
27.04.2020 - 14.05.2020
```
---

#### accident

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
#### barbershop

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
...but not at
```text
24.12.2020
01.01.2021
```

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
