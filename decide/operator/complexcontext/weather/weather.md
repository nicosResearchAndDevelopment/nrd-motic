# `weather` Binary Operators

## Weather

A very interesting and not seldom used context.

- Prefix: `weather`


### weather `probe`

```json
{
    "@context":                    [
        "http://www.nicos-rd.com/weather/",
        {
            "weather": "http://www.nicos-rd.com/weather/",
            "xsd":     "http://www.w3.org/2001/XMLSchema#"
        }
    ],
    "@type":                       "weather:probe",
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

### weather:is

>>> TODO: OR (following [environment](../environment/environment.md) / [event](../event/event.md)) `weather:inside`?

```text

```

|   |   |   | is |
|---|---|---|:---|
| "weather:probe"   | `weather:is` | "example:nice"           | true  |


### Example

Two organizations (`exA` and `exB` had defined what "weather is nice" means (`niceWeatherRightOperand_1.json`)) and
 handle it in a working constraint `weather_is_nice_1.json` by putting in *current weather*... 

- seeAlso: [exA_to_exB/niceWeatherRightOperand_1](https://github.com/nicosResearchAndDevelopment/nrd-motic/blob/master/decide/policy/custom/exA_to_exB/niceWeatherRightOperand_1.json)
- seeAlso: [weather_is_nice_1](https://github.com/nicosResearchAndDevelopment/nrd-motic/blob/master/decide/policy/rule/constraint/weather_is_nice_1.json)

---
