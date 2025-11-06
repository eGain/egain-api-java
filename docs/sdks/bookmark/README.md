# Bookmark
(*portal().bookmark()*)

## Overview

### Available Operations

* [addbookmark](#addbookmark) - Add a Bookmark
* [getbookmark](#getbookmark) - Get All Bookmarks for a Portal
* [deletebookmark](#deletebookmark) - Delete a Bookmark

## addbookmark

## Overview
  * The Add a Bookmark API adds a bookmark from a portal.


### Example Usage

<!-- UsageSnippet language="java" operationID="addbookmark" method="post" path="/portals/{portalID}/bookmarks" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.components.CreateBookmark;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.AddbookmarkResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        AddbookmarkResponse res = sdk.portal().bookmark().addbookmark()
                .portalID("PROD-1000")
                .acceptLanguage(AcceptLanguage.EN_US)
                .body(CreateBookmark.builder()
                    .resourceId("PROD-9274782")
                    .bookmarkName("DemoBookmark")
                    .resourceType(2)
                    .build())
                .call();

        // handle response
    }
}
```

### Parameters

| Parameter                                                                                                                       | Type                                                                                                                            | Required                                                                                                                        | Description                                                                                                                     | Example                                                                                                                         |
| ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| `portalID`                                                                                                                      | *String*                                                                                                                        | :heavy_check_mark:                                                                                                              | The ID of the portal being accessed.<br><br>A portal ID is composed of a 2-4 letter prefix, followed by a dash and 4-15 digits. | PROD-1000                                                                                                                       |
| `acceptLanguage`                                                                                                                | [AcceptLanguage](../../models/components/AcceptLanguage.md)                                                                     | :heavy_check_mark:                                                                                                              | The Language locale accepted by the client (used for locale specific fields in resource representation and in error responses). | en-US                                                                                                                           |
| `body`                                                                                                                          | [CreateBookmark](../../models/components/CreateBookmark.md)                                                                     | :heavy_check_mark:                                                                                                              | N/A                                                                                                                             |                                                                                                                                 |

### Response

**[AddbookmarkResponse](../../models/operations/AddbookmarkResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getbookmark

## Overview
  The Get All Bookmarks for a Portal API returns all bookmarks for a portal. Only the topic bookmarks that are available in the scope of the portal are returned.

## Permissions
  * The user must have the __View Author Portal__ action to access the authoring view.
  * The user must have the __View Staging Portal__ action to access the staging view.


### Example Usage

<!-- UsageSnippet language="java" operationID="getbookmark" method="get" path="/portals/{portalID}/bookmarks" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetbookmarkResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetbookmarkResponse res = sdk.portal().bookmark().getbookmark()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .call();

        if (res.bookmarkResult().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                                                                       | Type                                                                                                                            | Required                                                                                                                        | Description                                                                                                                     | Example                                                                                                                         |
| ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| `acceptLanguage`                                                                                                                | [AcceptLanguage](../../models/components/AcceptLanguage.md)                                                                     | :heavy_check_mark:                                                                                                              | The Language locale accepted by the client (used for locale specific fields in resource representation and in error responses). | en-US                                                                                                                           |
| `portalID`                                                                                                                      | *String*                                                                                                                        | :heavy_check_mark:                                                                                                              | The ID of the portal being accessed.<br><br>A portal ID is composed of a 2-4 letter prefix, followed by a dash and 4-15 digits. | PROD-1000                                                                                                                       |

### Response

**[GetbookmarkResponse](../../models/operations/GetbookmarkResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## deletebookmark

## Overview
  The Delete a Bookmark API deletes a bookmark from a portal.


### Example Usage

<!-- UsageSnippet language="java" operationID="deletebookmark" method="delete" path="/portals/{portalID}/bookmarks/{bookmarkID}" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.DeletebookmarkResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        DeletebookmarkResponse res = sdk.portal().bookmark().deletebookmark()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .bookmarkID("4759")
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
| `bookmarkID`                                                                                                                    | *String*                                                                                                                        | :heavy_check_mark:                                                                                                              | The ID of the bookmark.                                                                                                         | 4759                                                                                                                            |

### Response

**[DeletebookmarkResponse](../../models/operations/DeletebookmarkResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |