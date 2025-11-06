# Federatedsearchevent
(*portal().federatedsearchevent()*)

## Overview

### Available Operations

* [createFederatedSearchResultEvent](#createfederatedsearchresultevent) - Event For Viewed Federated Search Result

## createFederatedSearchResultEvent

## Overview
   The Federated Search Event API creates an event for federated search results that have been viewed.
  An event for viewing an: 
  * External web page cannot be created for a portal where the value for the "Include Web Search Section in Search Results" attribute is "Yes".
  * Internal web page cannot be created for a portal where the value for the "Include Intranet Search Section in Search Results" attribute is "Yes".


### Example Usage

<!-- UsageSnippet language="java" operationID="createFederatedSearchResultEvent" method="post" path="/portals/{portalID}/search/federated/event" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.CreateFederatedSearchResultEventResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        CreateFederatedSearchResultEventResponse res = sdk.portal().federatedsearchevent().createFederatedSearchResultEvent()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .body(CreateFederatedSearchEvent.builder()
                    .q("India")
                    .resultType(ResultType.EXTERNAL)
                    .url("https://beetle.egain.com/")
                    .title("Welcome to India")
                    .languageCode(KbLanguageCode.EN_US)
                    .build())
                .call();

        // handle response
    }
}
```

### Parameters

| Parameter                                                                                                                       | Type                                                                                                                            | Required                                                                                                                        | Description                                                                                                                     | Example                                                                                                                         |
| ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| `acceptLanguage`                                                                                                                | [AcceptLanguage](../../models/components/AcceptLanguage.md)                                                                     | :heavy_check_mark:                                                                                                              | The Language locale accepted by the client (used for locale specific fields in resource representation and in error responses). | en-US                                                                                                                           |
| `portalID`                                                                                                                      | *String*                                                                                                                        | :heavy_check_mark:                                                                                                              | The ID of the portal being accessed.<br><br>A portal ID is composed of a 2-4 letter prefix, followed by a dash and 4-15 digits. | PROD-1000                                                                                                                       |
| `body`                                                                                                                          | [CreateFederatedSearchEvent](../../models/components/CreateFederatedSearchEvent.md)                                             | :heavy_check_mark:                                                                                                              | N/A                                                                                                                             |                                                                                                                                 |

### Response

**[CreateFederatedSearchResultEventResponse](../../models/operations/CreateFederatedSearchResultEventResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |