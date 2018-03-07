# CSV Schema
CSV Schema is a JSON vocabulary to declare a schema and validate CSV data.

* [Specification](specification.md)

* Examples(WIP)

## Quick Start

For several use cases, such as importing CSV data into databases, grouping by column for analysis, or performing data visualization, we would like to validate the data and make sure it meets all our requirements before any further manipulation.

CVS Schema provides a way to define a schema and run validation based on it.

Let's assume we have following CSV document:

```
name,e-mail,zipcode,donation
Ann,ann@mail.com,06001,"1,000"
Ben,ben@home.com,85001,100
```

We can define a schema to validate it. For example, the following schema validates if "e-mail" has valid email addresses, "zipcode" is a 5-digit number including leading zero, and "donation" is greater than 100.

```
{
    "fields": [
        {
            "name": "e-mail",
            "format": "email"
        },
        {
            "name": "zipcode",
            "type": "string",
            "pattern": "\d{5}"
        },
        {
            "name": "donation",
            "type": "number",
            "groupChar": ",",
            "minimum": 100
        }
    ]
}
```

## Implementations

* [PyCSVSchema](https://github.com/crowdskout/PyCSVSchema)

## License

CSV Schema is licensed under Apache License 2.
