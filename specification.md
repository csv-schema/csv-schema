# CSV Schema

**Version:** 0.0.2 (2018-03-08)  
**Author(s):** Guangyang Li  
**License:** [Apache License 2](https://www.apache.org/licenses/LICENSE-2.0)  
**Status:** Work in Progress

## Table of Contents

* [1. Introduction](#1-introduction)
* [2. Terminology](#2-terminology)
* [3. Overview](#3-overview)
  + [3.1 Meta-Schema](#31-meta-schema)
  + [3.2 Keyword Relations](#32-keyword-relations)
* [4. Validator and Transformer Keywords](#4-validator-and-transformer-keywords)
  + [4.1 fields](#41-fields)
    - [4.1.1 Keywords for Any Type](#411-keywords-for-any-type)
      * [4.1.1.1 name](#4111-name)
      * [4.1.1.2 type](#4112-type)
      * [4.1.1.3 enum](#4113-enum)
      * [4.1.1.4 nullable](#4114-nullable)
      * [4.1.1.5 required](#4115-required)
    - [4.1.2 Keywords for String Type](#412-keywords-for-string-type)
      * [4.1.2.1 format](#4121-format)
      * [4.1.2.2 datetimePattern](#4122-datetimepattern)
      * [4.1.2.3 pattern](#4123-pattern)
      * [4.1.2.4 maxLength](#4124-maxlength)
      * [4.1.2.5 minLength](#4125-minlength)
    - [4.1.3 Keywords for Boolean Type](#413-keywords-for-boolean-type)
      * [4.1.3.1 falseValues](#4131-falsevalues)
      * [4.1.3.2 trueValues](#4132-truevalues)
    - [4.1.4 Keywords for Number and Integer Type](#414-keywords-for-number-and-integer-type)
      * [4.1.4.1 groupChar](#4141-groupchar)
      * [4.1.4.2 maximum](#4142-maximum)
      * [4.1.4.3 exclusiveMaximum](#4143-exclusivemaximum)
      * [4.1.4.4 minimum](#4144-minimum)
      * [4.1.4.5 exclusiveMinimum](#4145-exclusiveminimum)
      * [4.1.4.6 multipleOf](#4146-multipleof)
  + [4.2 missingValues](#42-missingvalues)
  + [4.3 exactFields](#43-exactfields)
  + [4.4 dependencies](#44-dependencies)
  + [4.5 patternFields](#45-patternfields)
  + [4.6 additionalFields](#46-additionalfields)
* [5. definitions and $ref](#5-definitions-and-ref)
* [6. Annotator Keywords](#6-annotator-keywords)
  + [6.1 title and description](#61-title-and-description)
  + [6.2 examples](#62-examples)

## 1. Introduction

CSV Schema is a JSON vocabulary to declare a schema and validate CSV data. The schema requires a CSV document satisfying certain criteria, such as data structure, types, format and relations. This specification defines a set of JSON keywords representing these criteria, in order to validate CSV data.

## 2. Terminology

The key words MUST, MUST NOT, REQUIRED, SHALL, SHALL NOT, SHOULD, SHOULD NOT, RECOMMENDED, MAY, and OPTIONAL in this document are to be interpreted as described in [RFC 2119](https://tools.ietf.org/html/rfc2119).

The terms "CSV" in this document are to be interpreted as defined in [RFC 4180, section 2](https://tools.ietf.org/html/rfc4180#section-2).

The terms "JSON", "object", "array", "number", "string", "boolean", "true", "false", and "null" in this document are to be interpreted as defined in [RFC 7159](https://tools.ietf.org/html/rfc7159).

## 3. Overview

A CSV Schema MUST be an JSON object.

CSV Schema defines a set of JSON object properties, which is called schema keywords, for validation purpose.

There are three types of keywords: validator which validates if a value or field satisfies the schema, transformer which pre-processes the value or schema for further validation with other validator keywords, and annotator which performs annotation about CSV data.

Validator keyword includes "fields", "exactFields", "dependencies", "patternFields" and "additionalFields", and "type", "format", "pattern", "maxLength", "minLength", "multipleOf", "maximum", "exclusiveMaximum", "minimum", "exclusiveMinimum", "enum", "required", "nullable" under field schema. 

Transformer keyword includes "missingValues", "definitions", and "groupChar", "trueValues", "falseValues", "$ref" under field schema. 

Annotator keyword includes "title", "description" and "examples".

A CSV Schema MAY contain properties which are not schema keywords. Unknown keywords SHOULD be ignored.

### 3.1 Meta-Schema

The schema that describes a CSV Schema is called meta-schema.

The meta-schema for this version of CSV Schema is [schema.json](schema.json).

### 3.2 Keyword Relations

Implementation SHOULD process validator, transformer and descriptor keywords in a certain order.

Transformer SHOULD be processed before validator keywords at the same level. For example keyword "missingValues" is processed before "fields" if it exists; "groupChar" is processed before "maximum" under the same field schema if it exists.

## 4. Validator and Transformer Keywords

### 4.1 fields

The value of "fields" MUST be an array. This array SHOULD have one or more than one field schema.

Each field schema MUST be a JSON object, which has a set of properties defining a field.

#### 4.1.1 Keywords for Any Type

##### 4.1.1.1 name

A field schema MUST contain a name property. The value SHOULD correspond to the name of field in the CSV data and it SHOULD be case sensitive.

There MAY be multiple field schema having same name, only the first one will to be applied to the correspond field and others SHOULD be ignored, unless "exactFields" keyword has boolean value true.

##### 4.1.1.2 type

The value of this keyword MUST be a string. The string value MUST be one of four types `{"string", "number", "integer", "boolean"}`.

The default value is `"string"`. 

##### 4.1.1.3 enum

The value of this keyword MUST be an array. This array SHOULD have one or more elements. These elements in the array MIGHT be of null or any value in the type defined in "type" keyword in this field schema. These elements SHOULD be unique.

A value is valid if this value is equal to one of the elements in this keyword's array value.

##### 4.1.1.4 nullable

The value of this keyword MUST be a boolean, which indicates if null is allowed in this field.

If this keyword has boolean value false, any value is valid. If it has boolean value true, a value validates successfully only if it is not null.

##### 4.1.1.5 required

The value of this keyword MUST be a boolean.

If this keyword has boolean value false, any field is valid. If it has boolean value true, a field validates successfully against this keyword only if it the correspond field exists in CSV data.

#### 4.1.2 Keywords for String Type

#### 4.1.2.1 format

The value of this keyword MUST be a string, which represents a format of string value. The string value MUST be one of four types `{"email", "uri", "uuid", "ipv4", "ipv6", "hostname", "datetime"}`.

If keyword "format" has string value "email", a value validates successfully only if it is a valid email address as defined in [RFC 5322, section 3.4.1](https://tools.ietf.org/html/rfc5322#section-3.4.1).

If keyword "format" has string value "uri", a value validates successfully only if it is a valid URI as defined in [RFC 1034, section 3.1](https://tools.ietf.org/html/rfc1034#section-3.1).

If keyword "format" has string value "uuid", a value validates successfully only if it is a valid version 4 UUID as defined in [RFC 4122, section 4.4](https://tools.ietf.org/html/rfc4122.html#section-4.4).

If keyword "format" has string value "ipv4", a value validates successfully only if it is a valid IPv4 address as defined in [RFC 2673, section 3.2](https://tools.ietf.org/html/rfc2673#section-3.2).

If keyword "format" has string value "ipv6", a value validates successfully only if it is a valid IPv6 address as defined in [RFC 4291, section 2.2](https://tools.ietf.org/html/rfc4291#section-2.2).

If keyword "format" has string value "hostname", a value validates successfully only if it is a valid hostname as defined in [RFC 1034, section 3.1](https://tools.ietf.org/html/rfc1034#section-3.1).

If keyword "format" has string value "datetime", a value validates successfully only if it is a valid datetime in ISO 8601 format of YYYY-MM-DDThh:mm:ssZ in UTC time.

#### 4.1.2.2 datetimePattern

The value of this keyword MUST be a valid date-time pattern in string. If this keyword is specified, ketword "format" MUST have a string value "datetime".

The pattern MUST follow the syntax of [C strptime](https://www.gnu.org/software/libc/manual/html_node/Low_002dLevel-Time-String-Parsing.html). 

A string value is valid if it matches the date-time pattern.

The default value is `"%Y-%m-%dT%H:%M:%S.%f%z"`.

#### 4.1.2.3 pattern

The value of this keyword MUST be a string. This string SHOULD be a valid regular expression.

A string value is valid if the regular expression matches the value successfully.

If keyword "format" is specified, this keyword SHOULD be ignored.

#### 4.1.2.4 maxLength

The value of this keyword MUST be a non-negative integer.

A string value is valid if its length is less than, or equal to, the value of this keyword.

#### 4.1.2.5 minLength

The value of this keyword MUST be a non-negative integer.

A string value is valid if its length is greater than, or equal to, the value of this keyword.

#### 4.1.3 Keywords for Boolean Type

##### 4.1.3.1 falseValues

The value of this keyword MUST be an array. This array SHOULD have one or more elements. These elements in the array MIGHT be of any value, including null.

If a boolean value is equal to one of the elements in this keyword's array value, it will be converted to boolean value false for further validation.

The default value is `["false", "False", "FALSE", "0"]`.

##### 4.1.3.2 trueValues

The value of this keyword MUST be an array. This array SHOULD have one or more elements. These elements in the array MIGHT be of any value, including null.

If a boolean value is equal to one of the elements in this keyword's array value, it will be converted to boolean value true for further validation.

The default value is `["true", "True", "TRUE", "1"]`.

#### 4.1.4 Keywords for Number and Integer Type

##### 4.1.4.1 groupChar

Keyword "groupChar" is a transformer keyword.

The value of this keyword MUST be a string, which is used to group digits within the number. The string will be ignored when convert value into number or integer type.

##### 4.1.4.2 maximum

The value of this keyword MUST be a number, which represents the inclusive upper limit of this field.

A number of integer value is valid if it is less than, or equal to, the value of this keyword.

##### 4.1.4.3 exclusiveMaximum

The value of this keyword MUST be a boolean, which indicates if to the value of "maximum" keyword to exclusive upper limit of this number type field, instead of inclusive upper limit.

If keyword "maximum" is not specified, is keyword will be ignored.

The default valus is `false`.

##### 4.1.4.4 minimum

The value of this keyword MUST be a number, which represents the inclusive lower limit of this field.

A number of integer value is valid if it is greater than, or equal to, the value of this keyword.

##### 4.1.4.5 exclusiveMinimum

The value of this keyword MUST be a boolean, which indicates if to the value of "minimum" keyword to exclusive lower limit of this number type field, instead of inclusive lower limit.

If keyword "minimum" is not specified, is keyword will be ignored.

The default valus is `false`.

##### 4.1.4.6 multipleOf

The value of this keyword MUST be a number, strictly greater than 0.

A number of integer value is valid if it is multiple of "multipleOf".

### 4.2 missingValues

The value of this keyword MUST be an array of strings, which represents the strings that need to be treated as null value in the whole CSV data.

Implementation SHOULD process the keyword "missingValues" before keyword "fields", therefore the type of elements in "missingValues" SHOULD only be string, instead of value of "type" in "fields".

The default value is `['']`. 

### 4.3 exactFields

The value of this keyword MUST be a boolean, which indicates if every field schema defined under "fields" matches correspond field in CSV data by order exactly.

If this keyword has boolean value false, any CSV data is valid. If this keyword has boolean value true, the CSV fields validate successfully only if every field name defined in keyword "name" under "fields" matches correspond field in CSV data by order exactly.

The default valus is `false`.

### 4.4 dependencies

The value of this keyword MUST be a JSON object.

The dependencies key is a field name. 

The dependencies value MUST be an array. Each element in the array MUST be a string, and MUST be unique.

If the dependencies key exists in CSV data field names, each element in the dependencies value must exists in CSV data field names.

### 4.5 patternFields

Keyword "patternFields" provides a way to match multiple fields in CSV data with single field schema.

The value of this keyword MUST be a JSON object. For each item of the object, the item key MUST be a valid regex for matching field names, the item value MUST be a valid field schema.

The keyword "name" will be ignored in the field schema if it exists.

If a field is defined in keyword "fields" and is matched with any regex in keyword "patternFields", the field schema in keyword "patternFields" will be ignored for that field.

If the regex is not matching any field, the corresponding field schema will not affect the validation result. 

Example schema:

```
{
    "patternFields": {
        ".*_name$": {
            "type": "string",
            "maxLength": 30
        }
    }
}
```

In this example, it defines a string type field schema with regex ".*_name$". This field schema will be applied to all field names ending with "_name".

### 4.6 additionalFields

The value of this keyword MUST be a boolean, which indicates whether allowing any field in CSV data that is not defined in this schema.

If this keyword has boolean value true, any CSV data is valid. If this keyword has boolean value false, the CSV fields validate successfully only if everyone of them is defined in keyword "fields", or can be matched through keyword "patternFields".

The default valus is `true`.

## 5. definitions and \$ref

Keyword "definitions" is a descriptor keyword which provides a way to re-use field schema with schema authors.

The value of this keyword MUST be a JSON object. For each item of the object, the item key represents the author name, the item value MUST be a valid field schema.

The keyword "name" will be ignored in the field schema if it exists.

To refer the field schema in "definitions", add keyword "$ref" with corresponding author name in the field schema referencing the defined field schema. All other keywords excluding "$ref" and "name" in that field schema are ignored.

Keyword "$ref" can be used under keyword "fields" and keyword "patternFields". It SHOULD NOT be used in keyword "definitions" and nested definitions SHOULD NOT be allowed.

Example schema:

```
{
    "fields": [
        {
            "name": "column",
            "$ref": "email_field"
        }
    ],
    "definitions": {
        "email_field": {
            "type": "string",
            "format": "email"
        }
    }
}
```

In this example, it defines a string type field schema with author name "email_field", which is refercened by field schema "column".

## 6. Annotator Keywords

### 6.1 title and description

Keyword "title" and "description" can be added to root schema, field schema or definitions. The value of these two keywords MUST be a string.

These two keywords provide human readable information about the CSV document or field. "title" is usually a short text or label. "description" usually contains explanation about the document or field in detail.

### 6.2 examples

Keyword "examples" is a keyword under field schema, which provides one or more than one possible values of the field.

The value of keyword "examples" MUST be an array.
