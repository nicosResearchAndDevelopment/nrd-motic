# `weather` Binary Operators

## Weather

A very interesting and not seldom used context.

- Prefix: `weather`


### weather `probe1`

```json
{
    "@context":                    [
        "http://www.nicos-rd.com/weather/",
        {
            "weather": "http://www.nicos-rd.com/weather/",
            "xsd":     "http://www.w3.org/2001/XMLSchema#"
        }
    ],
    "@id": "http://www.nicos-rd.com/weather#probe1",
    "@type":                       "smf:probe",
    "weather:timestamp":         {
        "@type":  "xsd:dateTimestamp",
        "@value": "2020-07-07T16:27:42.42+02:00"
    },
    "weather:temperature":         {
        "@type":  "xsd:decimal",
        "@value": 22
    },
    "weather:humidity":            {
        "@type":  "xsd:decimal",
        "@value": 88
    },
    "weather:atmosphericpressure": {
        "@type":  "xsd:decimal",
        "@value": 1042.42
    }

}
```

### operator `weather:is`

>>> TODO: OR (following [environment](../environment/environment.md) / [event](../event/event.md)) `weather:inside`?

```text

```

|   |   |   | is |
|---|---|---|:---|
| "weather:probe"   | `weather:is` | "example:nice"           | true  |


### Examples

#### Two Organizations and Nice Weather

Two organizations (`exA` and `exB` had defined what "weather is nice" means (`niceWeatherRightOperand_1.json`)) and
 handle it in a working constraint `weather_is_nice_1.json` by putting in *current weather*... 

##### Enforcement and Right Operand

- [enforce weather:probe1](./examples/weather_is_nice_1.json)
- [rightOperand "nice" = 'exA_to_exB/niceWeatherRightOperand_1'](./examples/exA_to_exB/niceWeatherRightOperand_1.json)

---
