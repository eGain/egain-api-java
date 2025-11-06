# Attachments

This schema contains the definition of Attachments.


## Fields

| Field                                                                    | Type                                                                     | Required                                                                 | Description                                                              |
| ------------------------------------------------------------------------ | ------------------------------------------------------------------------ | ------------------------------------------------------------------------ | ------------------------------------------------------------------------ |
| `count`                                                                  | *Optional\<Integer>*                                                     | :heavy_minus_sign:                                                       | The number of Attachments in the list.                                   |
| `link`                                                                   | [Optional\<Link>](../../models/components/Link.md)                       | :heavy_minus_sign:                                                       | Defines the relationship between this resource and another object.       |
| `attachments`                                                            | List\<[AttachmentSummary](../../models/components/AttachmentSummary.md)> | :heavy_minus_sign:                                                       | The list of Attachments.                                                 |