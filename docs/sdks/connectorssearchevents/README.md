# Connectorssearchevents
(*portal().connectorssearchevents()*)

## Overview

### Available Operations

* [createSearchResultEventForConnectors](#createsearchresulteventforconnectors) - Event for Search Using Connectors
* [createViewedSearchResultsEventForConnectors](#createviewedsearchresultseventforconnectors) - Event for Viewed Search Results Using Connectors

## createSearchResultEventForConnectors

## Overview
   The Event for Search Using Connectors API creates an event to initiate a search operation for retrieving content from external sources or integrations within the portal. 
   It allows users to set up search events based on specified criteria.


### Example Usage

<!-- UsageSnippet language="java" operationID="createSearchResultEventForConnectors" method="post" path="/portals/{portalID}/search/connectors/event" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.CreateSearchResultEventForConnectorsResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        CreateSearchResultEventForConnectorsResponse res = sdk.portal().connectorssearchevents().createSearchResultEventForConnectors()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .body(CreateSearchResultEventForConnectors.builder()
                    .languageCode(KbLanguageCode.EN_US)
                    .q("India")
                    .numberOfSearchResults(10)
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
| `body`                                                                                                                          | [CreateSearchResultEventForConnectors](../../models/components/CreateSearchResultEventForConnectors.md)                         | :heavy_check_mark:                                                                                                              | N/A                                                                                                                             |                                                                                                                                 |

### Response

**[CreateSearchResultEventForConnectorsResponse](../../models/operations/CreateSearchResultEventForConnectorsResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## createViewedSearchResultsEventForConnectors

## Overview
   The Event for Viewed Search Results Using Connectors API creates an event to log when search results from external content are viewed. 
   This helps in tracking and analyzing user interactions with search results retrieved from various external sources.


### Example Usage

<!-- UsageSnippet language="java" operationID="createViewedSearchResultsEventForConnectors" method="post" path="/portals/{portalID}/view/searchresults/connectors/event" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.CreateViewedSearchResultsEventForConnectorsResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        CreateViewedSearchResultsEventForConnectorsResponse res = sdk.portal().connectorssearchevents().createViewedSearchResultsEventForConnectors()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .body(CreateViewedSearchResultEventForConnectors.builder()
                    .languageCode(KbLanguageCode.EN_US)
                    .q("India")
                    .url("https://beetle.egain.com/")
                    .title("Welcome to India")
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
| `body`                                                                                                                          | [CreateViewedSearchResultEventForConnectors](../../models/components/CreateViewedSearchResultEventForConnectors.md)             | :heavy_check_mark:                                                                                                              | N/A                                                                                                                             |                                                                                                                                 |

### Response

**[CreateViewedSearchResultsEventForConnectorsResponse](../../models/operations/CreateViewedSearchResultsEventForConnectorsResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |