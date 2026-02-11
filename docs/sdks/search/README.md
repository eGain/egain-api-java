# Search
(*portal().search()*)

## Overview

### Available Operations

* [aiSearch](#aisearch) - Hybrid Search

## aiSearch

The Search API is a hybrid search service that combines semantic understanding with keyword precision to deliver fast, contextual, and relevant results from your enterprise knowledge base. It enables secure, role-aware access to articles, FAQs, and documentation across customer, agent, and employee interfaces. Each query returns a ranked list of results with snippets, metadata, and relevance scores.


### Example Usage

<!-- UsageSnippet language="java" operationID="aiSearch" method="get" path="/{portalID}/search" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.LanguageCodeParameter;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.AiSearchRequest;
import com.egain.sdk.models.operations.AiSearchResponse;
import java.lang.Exception;
import java.util.List;
import java.util.Map;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        AiSearchRequest req = AiSearchRequest.builder()
                .q("What is a loan?")
                .portalID("PROD-1000")
                .language(LanguageCodeParameter.EN_US)
                .filterUserProfileID("PROD-1030")
                .filterTags(Map.ofEntries(
                    Map.entry("PROD-1234", List.of(
                        "PROD-2000",
                        "PROD-2003")),
                    Map.entry("PROD-2005", List.of(
                        "PROD-2007"))))
                .build();

        AiSearchResponse res = sdk.portal().search().aiSearch()
                .request(req)
                .call();

        if (res.aiSearchResponse().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                     | Type                                                          | Required                                                      | Description                                                   |
| ------------------------------------------------------------- | ------------------------------------------------------------- | ------------------------------------------------------------- | ------------------------------------------------------------- |
| `request`                                                     | [AiSearchRequest](../../models/operations/AiSearchRequest.md) | :heavy_check_mark:                                            | The request object to use for the request.                    |

### Response

**[AiSearchResponse](../../models/operations/AiSearchResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |