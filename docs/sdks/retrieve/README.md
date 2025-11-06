# Retrieve
(*aiservices().retrieve()*)

## Overview

### Available Operations

* [retrieveChunks](#retrievechunks) - Retrieve Chunks

## retrieveChunks

The Retrieve API enables enterprises to directly access relevant content chunks from their organizational knowledge sources. It is designed for scenarios where developers want granular control over retrieved information, such as powering custom search, analytics, or retrieval-augmented generation (RAG) pipelines. 

The Retrieve API's response will either be a Certified Answer or a list of chunks.
- **Certified Answers Response**: This response is observed when there's a matching certified answer to the user's query.
- **Chunk List Response**: This response is observed, if there is no matched Certified Answer.

Responses for both chunks and certified answers include relevance scores and metadata. By leveraging the Retrieve API, organizations can build tailored experiences while retaining confidence in the source material.


### Example Usage

<!-- UsageSnippet language="java" operationID="retrieveChunks" method="post" path="/{portalID}/retrieve" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.operations.RetrieveChunksRequest;
import com.egain.sdk.models.operations.RetrieveChunksResponse;
import java.lang.Exception;
import java.util.List;
import java.util.Map;

public class Application {

    public static void main(String[] args) throws Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        RetrieveChunksRequest req = RetrieveChunksRequest.builder()
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
                .body(RetrieveRequest.builder()
                    .channel(RetrieveRequestChannel.builder()
                        .name("Eight Bank Website")
                        .build())
                    .build())
                .build();

        RetrieveChunksResponse res = sdk.aiservices().retrieve().retrieveChunks()
                .request(req)
                .call();

        if (res.retrieveResponse().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                 | Type                                                                      | Required                                                                  | Description                                                               |
| ------------------------------------------------------------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| `request`                                                                 | [RetrieveChunksRequest](../../models/operations/RetrieveChunksRequest.md) | :heavy_check_mark:                                                        | The request object to use for the request.                                |
| `serverURL`                                                               | *String*                                                                  | :heavy_minus_sign:                                                        | An optional server URL to use.                                            |

### Response

**[RetrieveChunksResponse](../../models/operations/RetrieveChunksResponse.md)**

### Errors

| Error Type                 | Status Code                | Content Type               |
| -------------------------- | -------------------------- | -------------------------- |
| models/errors/APIException | 4XX, 5XX                   | \*/\*                      |