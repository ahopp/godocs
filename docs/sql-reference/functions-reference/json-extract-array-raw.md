---
layout: default
title: JSON_EXTRACT_ARRAY_RAW
description: Reference material for JSON_EXTRACT_ARRAY_RAW function
parent: SQL functions
---

## JSON_EXTRACT_ARRAY_RAW

Returns a string representation of a JSON array pointed by the supplied JSON pointer. The returned string represents a Firebolt array with elements that are string representations of the scalars or objects contained in the JSON array under the specified key, if the key exists. If the key does not exist, the function returns `NULL`.

This function is useful when working with heterogeneously typed arrays and arrays containing JSON objects in which case each object will be further processed by functions such as [TRANSFORM](/transform.md).

For more information on manipulating JSON data sets, please refer to [JSON functions](./json-functions.md).

The example below uses our [JSON Common Example](./json-functions.md#json-common-example)

##### Syntax
{: .no_toc}

```sql
​​JSON_EXTRACT_ARRAY_RAW(<json>, '<json_pointer_expression>')
```

| Parameter                   | Type           | Description                                               |
| :--------------------------- | :-------------- | :--------------------------------------------------------- |
| `<json>`                    | TEXT           | The JSON document from which the array is to be extracted |
| `<json_pointer_expression>` | Literal string | A JSON pointer to the location of the array in the JSON   |

##### Example
{: .no_toc}

```sql
SELECT
    JSON_EXTRACT_ARRAY_RAW(<json_common_example>, '/value/events')
```

**Returns**: `["{\"EventId\":547,\"EventProperties\":{\"UserName\":\"John Doe\",\"Successful\":true}}","{\"EventId\":548,\"EventProperties\":{\"ProductID\":\"xy123\",\"items\":2}}"]`