# StringAttributeValue

An attribute can have an external value such as a readable label, and an internal value such as a unique ID. This schema provides both the internal and external values for one attribute.


## Fields

| Field                                          | Type                                           | Required                                       | Description                                    |
| ---------------------------------------------- | ---------------------------------------------- | ---------------------------------------------- | ---------------------------------------------- |
| `internalValue`                                | *Optional\<String>*                            | :heavy_minus_sign:                             | The internal value of the attribute.           |
| `externalValue`                                | *Optional\<String>*                            | :heavy_minus_sign:                             | The external or public value of the attribute. |