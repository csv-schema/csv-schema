# CSV Schema
CSV Schema is a JSON vocabulary to declare a schema and validate CSV data.

[CSV Schema Specification](specification.md)

Examples(WIP)

## Quick Example

For a lot of use cases, such as importing CSV into databases, grouping by column for further analysis, or performing data visualization, we would like to validate the data and make sure it meets all our requirements.

CVS Schema provides a way to define a schema and run validation based on it.

Let's say we have following CSV document:

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

CSV Schema is under MIT license, see LICENSE file for the details.
