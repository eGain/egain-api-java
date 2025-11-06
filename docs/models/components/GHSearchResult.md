# GHSearchResult

Success


## Fields

| Field                                                                  | Type                                                                   | Required                                                               | Description                                                            |
| ---------------------------------------------------------------------- | ---------------------------------------------------------------------- | ---------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| `casebase`                                                             | [Optional\<Casebase>](../../models/components/Casebase.md)             | :heavy_minus_sign:                                                     | N/A                                                                    |
| `actionSearch`                                                         | List\<[ActionSearch](../../models/components/ActionSearch.md)>         | :heavy_minus_sign:                                                     | actions in the search                                                  |
| `answeredQuestion`                                                     | List\<[AnsweredQuestion](../../models/components/AnsweredQuestion.md)> | :heavy_minus_sign:                                                     | questions answered in the search                                       |
| `caseSearch`                                                           | List\<[CaseSearch](../../models/components/CaseSearch.md)>             | :heavy_minus_sign:                                                     | cases in the search                                                    |
| `dynamicSearch`                                                        | List\<[DynamicSearch](../../models/components/DynamicSearch.md)>       | :heavy_minus_sign:                                                     | dynamic cases in the search                                            |
| `unansweredQuestion`                                                   | List\<[Question](../../models/components/Question.md)>                 | :heavy_minus_sign:                                                     | unanswered questions in the search                                     |
| `startupQuestion`                                                      | List\<[Question](../../models/components/Question.md)>                 | :heavy_minus_sign:                                                     | startup questions in the search                                        |
| `targetClusters`                                                       | List\<[ClusterId](../../models/components/ClusterId.md)>               | :heavy_minus_sign:                                                     | active clusters in the search                                          |