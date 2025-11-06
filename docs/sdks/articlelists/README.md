# Articlelists
(*portal().articlelists()*)

## Overview

### Available Operations

* [getAllArticleLists](#getallarticlelists) - Get All Article Lists
* [getArticleListDetails](#getarticlelistdetails) - Get Article List by ID

## getAllArticleLists

## Overview
  The Get All Article Lists API returns all quick Article lists that are configured for a portal.


### Example Usage

<!-- UsageSnippet language="java" operationID="getAllArticleLists" method="get" path="/portals/{portalID}/articlelists" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.components.LanguageQueryParameter;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetAllArticleListsResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetAllArticleListsResponse res = sdk.portal().articlelists().getAllArticleLists()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .language(LanguageQueryParameter.EN_US)
                .call();

        if (res.articleListsResult().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                                                                                                                                                             | Type                                                                                                                                                                                                                  | Required                                                                                                                                                                                                              | Description                                                                                                                                                                                                           | Example                                                                                                                                                                                                               |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `acceptLanguage`                                                                                                                                                                                                      | [AcceptLanguage](../../models/components/AcceptLanguage.md)                                                                                                                                                           | :heavy_check_mark:                                                                                                                                                                                                    | The Language locale accepted by the client (used for locale specific fields in resource representation and in error responses).                                                                                       | en-US                                                                                                                                                                                                                 |
| `portalID`                                                                                                                                                                                                            | *String*                                                                                                                                                                                                              | :heavy_check_mark:                                                                                                                                                                                                    | The ID of the portal being accessed.<br><br>A portal ID is composed of a 2-4 letter prefix, followed by a dash and 4-15 digits.                                                                                       | PROD-1000                                                                                                                                                                                                             |
| `language`                                                                                                                                                                                                            | [Optional\<LanguageQueryParameter>](../../models/components/LanguageQueryParameter.md)                                                                                                                                | :heavy_minus_sign:                                                                                                                                                                                                    | The language that describes the details of a resource. Resources available in different languages may differ from each other.<li>If <code>lang</code> is not passed, then the portal's default language is used.</li> | en-US                                                                                                                                                                                                                 |

### Response

**[GetAllArticleListsResponse](../../models/operations/GetAllArticleListsResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getArticleListDetails

## Overview
  The Get Article Lists by ID API returns the details of a quick access Article list that is configured for a portal.

  Only those articles in the quick access list articles are returned and are:

  - Available for current user profile.
  - Tagged with at least one of the provided tags, if tags query parameter is provided.
  - Displayable. An Article is displayable if "Include in browse on portals" property is enable for the Article. 


### Example Usage

<!-- UsageSnippet language="java" operationID="getArticleListDetails" method="get" path="/portals/{portalID}/articlelists/{listID}" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetArticleListDetailsRequest;
import com.egain.sdk.models.operations.GetArticleListDetailsResponse;
import java.lang.Exception;
import java.util.List;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetArticleListDetailsRequest req = GetArticleListDetailsRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .listID("PROD-12345")
                .filterTopicId("PROD-1067")
                .articleResultAdditionalAttributes(List.of(
                    ArticleResultAdditionalAttributes.AVERAGE_RATING))
                .language(LanguageQueryParameter.EN_US)
                .build();

        GetArticleListDetailsResponse res = sdk.portal().articlelists().getArticleListDetails()
                .request(req)
                .call();

        if (res.articleList().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                               | Type                                                                                    | Required                                                                                | Description                                                                             |
| --------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| `request`                                                                               | [GetArticleListDetailsRequest](../../models/operations/GetArticleListDetailsRequest.md) | :heavy_check_mark:                                                                      | The request object to use for the request.                                              |

### Response

**[GetArticleListDetailsResponse](../../models/operations/GetArticleListDetailsResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |