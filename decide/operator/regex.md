# `regex` Binary Operators

- Prefix: `regex`


## Regular Expression

- prefix : regex

### regex:match

##### NLP
```text
Is a given string a valid Email-Address?
```

##### Pseudo code
```text
match("GAIAboX@nicos-ag.com", "^[A-Za-z0-9.!#$%&'*+\\-/=?^_`{|}~]+@[A-Za-z0-9.\\-]+\\.[A-Za-z]+$") = true
```

##### ODRL

```json
{
    "constraint": [{
               "@id": "{{root}}/constraints/is_mail", 
               "leftOperand": {
                    "@type": "xsd:string",
                    "@value": "GAIAboX@nicos-ag.com"
               },
               "operator": "regex:match",
               "rightOperand":  {
                    "@type": "xsd:string",
                    "@value": "^[A-Za-z0-9.!#$%&'*+\\-/=?^_`{|}~]+@[A-Za-z0-9.\\-]+\\.[A-Za-z]+$"
               }
           }]
}
```

---