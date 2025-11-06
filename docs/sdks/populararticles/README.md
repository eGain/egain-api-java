# Populararticles
(*portal().populararticles()*)

## Overview

### Available Operations

* [getpopulararticles](#getpopulararticles) - Get Popular Articles

## getpopulararticles

## Overview
  The Popular Articles API allows a user to retrieve popular articles. 
## Prerequisites 
    * Only displayable articles are returned. An Article is displayable when "Include in browse on portals" property is enable for the Article. 
## Permissions
  * Agent permissions: The following permissions are required if user is an Agent:
    * If Article has Access Tags, Article must be available for agent's current user profile.
    * If Article has Publish Views, at least one edition of Article must be available for agent's current user profile.
    * If Article has filters and tags query parameter provided, Article filters must match provided tags or tag groups.
  * Customer permissions: The following permissions are required if user is a Customer:              
      * If Article has Access tags: 
       * Portal must have default user profile, and;
       * Article must be available for portal's default user profile.
    * If Article has Publish Views:
      * Portal must have default user profile, and;
      * At least one edition must be available for portal's default user profile.
    *  If Article has filters and tags query parameter provided, Article filters must match provided tags or tag groups.


### Example Usage

<!-- UsageSnippet language="java" operationID="getpopulararticles" method="get" path="/portals/{portalID}/populararticles" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetpopulararticlesRequest;
import com.egain.sdk.models.operations.GetpopulararticlesResponse;
import java.lang.Exception;
import java.util.List;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetpopulararticlesRequest req = GetpopulararticlesRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .filterTopicId("PROD-1067")
                .language(LanguageQueryParameter.EN_US)
                .filterTags("PROD-5381")
                .articleResultAdditionalAttributes(List.of(
                    ArticleResultAdditionalAttributes.AVERAGE_RATING))
                .build();

        GetpopulararticlesResponse res = sdk.portal().populararticles().getpopulararticles()
                .request(req)
                .call();

        if (res.articleResults().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                         | Type                                                                              | Required                                                                          | Description                                                                       |
| --------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| `request`                                                                         | [GetpopulararticlesRequest](../../models/operations/GetpopulararticlesRequest.md) | :heavy_check_mark:                                                                | The request object to use for the request.                                        |

### Response

**[GetpopulararticlesResponse](../../models/operations/GetpopulararticlesResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |