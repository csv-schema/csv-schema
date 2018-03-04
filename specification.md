# CSV Schema

Version: 0.0.2 (2018-03-08)

Author(s): Guangyang Li

## 1. Introduction

CSV Schema is a JSON vocabulary to declare a schema and validate CSV data.

A CSV Schema MUST be an JSON object. 


## 2. Terminology

The key words MUST, MUST NOT, REQUIRED, SHALL, SHALL NOT, SHOULD, SHOULD NOT, RECOMMENDED, MAY, and OPTIONAL in this document are to be interpreted as described in [RFC 2119](https://tools.ietf.org/html/rfc2119).

The terms "CSV" in this document are to be interpreted as defined in [RFC 4180](https://tools.ietf.org/html/rfc4180).

The terms "JSON", "object", "array", "number", "string", "boolean", "true", "false", and "null" in this document are to be interpreted as defined in [RFC 7159](https://tools.ietf.org/html/rfc7159).

## 3. Meta-Schema

The schema that describes a CSV Schema is called meta-schema. 

## 4. Validation Keywords

CSV Schema defines a set of JSON object properties, which is called schema keywords, for validation purpose.

A keyword MIGHT be a validator which validates if a value or field satisfies the schema, or a descriptor which pre-process the value for further validation with other keywords.

A CSV Schema MAY contain properties which are not schema keywords. Unknown keywords SHOULD be ignored. 

### 4.1 fields

The value of "fields" MUST be an array. This array SHOULD have one or more than one field schema.

Each field schema MUST be a JSON object, which has a set of properties defining a field.

#### 4.1.1 Keywords for Any Type

##### 4.1.1.1 name

A field schema MUST contain a name property. The value SHOULD correspond to the name of field in the CSV data and it SHOULD be case sensitive.

There MAY be multiple field schema having same name, only the first one will to be applied to the correspond field and others SHOULD be ignored, unless "exactFields" keyword has boolean value true.

##### 4.1.1.2 type

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

See [5. Format](5. Format) for all possible values of format.

#### 4.1.2.2 pattern

The value of this keyword MUST be a string. This string SHOULD be a valid regular expression.

A string value is valid if the regular expression matches the value successfully.

If keyword "format" is specified, this keyword SHOULD be ignored.

#### 4.1.2.3 maxLength

The value of this keyword MUST be a non-negative integer.

A string value is valid if its length is less than, or equal to, the value of this keyword.

#### 4.1.2.4 minLength

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

The implementation SHOULD process the keyword "missingValues" before keyword "fields", therefore the type of elements in "missingValues" SHOULD only be string, instead of value of "type" in "fields".

The default value is `['']`.

### 4.3 definitions 

### 4.4 exactFields 

### 4.5 dependencies 

### 4.6 patternFields 

### 4.7 additionalFields 

## 5. Format

## 6. Keyword Independence

## 