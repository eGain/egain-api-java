# Answers
(*aiservices().answers()*)

## Overview

### Available Operations

* [getBestAnswer](#getbestanswer) - Get the best answer for a user query

## getBestAnswer

The **Answers API** allows enterprises to deliver fast, accurate, and contextual responses powered by their organizational knowledge. It supports two complementary approaches:  
  - **Certified Answers**: Direct snippets retrieved from enterprise-authored content.
  - **Generative Answers**: Natural language responses synthesized by a large language model (LLM).

Every response includes supporting search results, references, and confidence scores—ensuring transparency, trust, and traceability. The API is built for secure, scalable integration across enterprise environments. <br>**This endpoint is only available for Self Service environments.**


### Example Usage

<!-- UsageSnippet language="java" operationID="getBestAnswer" method="post" path="/{portalID}/answers" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.LanguageCodeParameter;
import com.egain.sdk.models.operations.GetBestAnswerRequest;
import com.egain.sdk.models.operations.GetBestAnswerResponse;
import java.lang.Exception;
import java.util.List;
import java.util.Map;

public class Application {

    public static void main(String[] args) throws Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetBestAnswerRequest req = GetBestAnswerRequest.builder()
                .q("fair lending")
                .portalID("PROD-1000")
                .language(LanguageCodeParameter.EN_US)
                .filterUserProfileID("PROD-3210")
                .filterTags(Map.ofEntries(
                    Map.entry("PROD-1234", List.of(
                        "PROD-2000",
                        "PROD-2003")),
                    Map.entry("PROD-2005", List.of(
                        "PROD-2007"))))
                .build();

        GetBestAnswerResponse res = sdk.aiservices().answers().getBestAnswer()
                .request(req)
                .call();

        if (res.answersResponse().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                               | Type                                                                    | Required                                                                | Description                                                             |
| ----------------------------------------------------------------------- | ----------------------------------------------------------------------- | ----------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| `request`                                                               | [GetBestAnswerRequest](../../models/operations/GetBestAnswerRequest.md) | :heavy_check_mark:                                                      | The request object to use for the request.                              |
| `serverURL`                                                             | *String*                                                                | :heavy_minus_sign:                                                      | An optional server URL to use.                                          |

### Response

**[GetBestAnswerResponse](../../models/operations/GetBestAnswerResponse.md)**

### Errors

| Error Type                 | Status Code                | Content Type               |
| -------------------------- | -------------------------- | -------------------------- |
| models/errors/APIException | 4XX, 5XX                   | \*/\*                      |