---
layout: default
title: JSON functions
description: Reference for SQL functions for working with JSON available in Firebolt.
parent: SQL functions
no_toc: true
nav_exclude: true
---

# JSON functions
{: .no_toc}

This page describes the functions used for JSON manipulation using ​Firebolt​. You can use JSON functions to extract values and objects from a JSON document.

* Topic ToC
{:toc}

## Function reference conventions

This reference uses the following conventions to represent function syntax.

### JSON pointer parameters

This reference uses the placeholder `json_pointer_expression` to indicate where you should use a JSON pointer. A JSON pointer is a way to access specific elements in a JSON document. For a formal specification, see [RFC6901](https://tools.ietf.org/html/rfc6901).

A JSON pointer starts with a forward slash (`/`), which denotes the root of the JSON document. This is followed by a sequence of property (key) names or zero-based ordinal numbers separated by slashes. You can specify property names or use ordinal numbers to specify the _n_th property or the _n_th element of an array.

The tilde (`~`) and forward slash (`/`) characters have special meanings and need to be escaped according to the guidelines below:

* To specify a literal tilde (`~`), use `~0`
* To specify a literal slash (`/`), use `~1`

For example, consider the JSON document below.

```javascript
{
    "key": 123,
    "key~with~tilde": 2,
    "key/with/slash": 3,
    "value": {
      "dyid": 987,
      "keywords" : ["insanely","fast","analytics"]
    }
}
```

With this JSON document, the JSON pointer expressions below evaluate to the results shown.

| Pointer             | Result                                                                                                                                                                                                                                                                                                    | Notes                                                                                                                                |
| :------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------ |
| `/`                 | `{` <br>`   "key": 123,` <br>`   "key~with~tilde": 2,` <br>`   "key/with/slash": 3,` <br>`   "value": {` <br>`      "dyid": 987,` <br>`      "keywords" : ["insanely","fast","analytics"]` <br>`   }` | The whole document                                                                                                                   |
| `/key`              | 123                                                                                                                                                                                                                                                                                                       |                                                                                                                                      |
| `/key~with~tilde`   | 2                                                                                                                                                                                                                                                                                                         | Indicates the value associated with the `key~with~tilde` property name.                                                              |
| `/key/with/slash`   | 3                                                                                                                                                                                                                                                                                                         | Indicates the value associated with the `key/with/slash` property name.                                                              |
| `/0`                | 123                                                                                                                                                                                                                                                                                                       | Uses an ordinal to indicate the value associated with the `key` property name. The `key` property is in the first 0-based position.  |
| `/value/keywords/2` | analytics                                                                                                                                                                                                                                                                                                 | Indicates the element "analytics", which is in the third 0-based position of the array value associated with they keywords property. |

### Supported type parameters

Some functions accept a *type parameter* shown as `expected_type`. This parameter is given as a literal string corresponding to supported Firebolt SQL data types to specify the expected type indicated by the JSON pointer parameter. The type parameter does not accept all SQL types because the JSON type system has fewer types than SQL.

The following values are supported for this argument:

* `INT` - used for integers as well as JSON boolean.
* `DOUBLE` - used for real numbers. It will also work with integers. For performance reasons, favor using `INT` when the values in the JSON document are known integers.
* `TEXT` - used for strings.
* `ARRAY(<type>)` - indicates an array where `<type>` is one of `INT`, `DOUBLE`, or `TEXT`.

The following data types are _not supported_: `DATE`, `DATETIME`, `FLOAT` (for real numbers, use `DOUBLE`).

### JSON common example

Usage examples in this reference are based on the JSON document below, which is referenced using the `<json_common_example>` placeholder.

```javascript
{
    "key": 123,
    "value": {
      "dyid": 987,
      "uid": "987654",
      "keywords" : ["insanely","fast","analytics"],
      "tagIdToHits": {
        "map": {
          "1737729": 32,
          "1775582": 35
        }
      },
      "events":[
        {
            "EventId": 547,
            "EventProperties" :
            {
                "UserName":"John Doe",
                "Successful": true
            }
        },
        {
            "EventId": 548,
            "EventProperties" :
            {
                "ProductID":"xy123",
                "items": 2
            }
        }
    ]
    }
}
```