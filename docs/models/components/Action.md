# Action


## Fields

| Field                                                            | Type                                                             | Required                                                         | Description                                                      |
| ---------------------------------------------------------------- | ---------------------------------------------------------------- | ---------------------------------------------------------------- | ---------------------------------------------------------------- |
| `id`                                                             | *Optional\<String>*                                              | :heavy_minus_sign:                                               | ID of the action                                                 |
| `title`                                                          | *Optional\<String>*                                              | :heavy_minus_sign:                                               | Name of the action                                               |
| `type`                                                           | *Optional\<String>*                                              | :heavy_minus_sign:                                               | type of the action                                               |
| `shortName`                                                      | *Optional\<String>*                                              | :heavy_minus_sign:                                               | short name of the action                                         |
| `rejectCount`                                                    | *Optional\<Long>*                                                | :heavy_minus_sign:                                               | number of times action was rejected.                             |
| `acceptCount`                                                    | *Optional\<Long>*                                                | :heavy_minus_sign:                                               | number of times action was accepted.                             |
| `metadata`                                                       | List\<[Metadata](../../models/components/Metadata.md)>           | :heavy_minus_sign:                                               | Metadata of action                                               |
| `articleType`                                                    | [Optional\<ArticleType>](../../models/components/ArticleType.md) | :heavy_minus_sign:                                               | The type of the Article and its attributes.                      |