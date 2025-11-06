# Topic
(*portal().topic()*)

## Overview

### Available Operations

* [getTopicBreadcrumbForArticle](#gettopicbreadcrumbforarticle) - Get Topic Breadcrumb for Article
* [getchildtopics](#getchildtopics) - Get Immediate Child Topics
* [getancestortopics](#getancestortopics) - Get All Ancestor Topics
* [getalltopics](#getalltopics) - Get All Topics

## getTopicBreadcrumbForArticle

## Overview
  * Use this API to retrieve the topic breadcrumb for an article in a portal. A breadcrumb shows the hierarchical path from the root topic to the article's primary topic.


### Example Usage

<!-- UsageSnippet language="java" operationID="getTopicBreadcrumbForArticle" method="get" path="/portals/{portalID}/articles/{articleID}/breadcrumb" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.components.MandatoryLanguageQueryParameter;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetTopicBreadcrumbForArticleResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetTopicBreadcrumbForArticleResponse res = sdk.portal().topic().getTopicBreadcrumbForArticle()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .articleID("PROD-2996")
                .language(MandatoryLanguageQueryParameter.EN_US)
                .call();

        if (res.topicBreadcrumb().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                                                                        | Type                                                                                                                             | Required                                                                                                                         | Description                                                                                                                      | Example                                                                                                                          |
| -------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| `acceptLanguage`                                                                                                                 | [AcceptLanguage](../../models/components/AcceptLanguage.md)                                                                      | :heavy_check_mark:                                                                                                               | The Language locale accepted by the client (used for locale specific fields in resource representation and in error responses).  | en-US                                                                                                                            |
| `portalID`                                                                                                                       | *String*                                                                                                                         | :heavy_check_mark:                                                                                                               | The ID of the portal being accessed.<br><br>A portal ID is composed of a 2-4 letter prefix, followed by a dash and 4-15 digits.  | PROD-1000                                                                                                                        |
| `articleID`                                                                                                                      | *String*                                                                                                                         | :heavy_check_mark:                                                                                                               | The ID of the Article.<br><br>An Article ID is composed of a 2-4 letter prefix followed by a dash and 4-15 digits.               | PROD-2996                                                                                                                        |
| `language`                                                                                                                       | [MandatoryLanguageQueryParameter](../../models/components/MandatoryLanguageQueryParameter.md)                                    | :heavy_check_mark:                                                                                                               | The language used for fetching the details of a resource. Resources available in different languages may differ from each other. | en-US                                                                                                                            |

### Response

**[GetTopicBreadcrumbForArticleResponse](../../models/operations/GetTopicBreadcrumbForArticleResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getchildtopics

## Overview
  The Get Immediate Child Topics API retrieves details about a topic and it's immediate child topics. The <code>$level</code> attribute determines how deep the topic hierarchy should go, or how many child topics of a topic are returned in the response.


### Example Usage

<!-- UsageSnippet language="java" operationID="getchildtopics" method="get" path="/portals/{portalID}/topics/{topicID}" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetchildtopicsRequest;
import com.egain.sdk.models.operations.GetchildtopicsResponse;
import java.lang.Exception;
import java.util.List;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetchildtopicsRequest req = GetchildtopicsRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .topicID("PROD-1069")
                .searchProfileId("959500000204621")
                .level(-1L)
                .language(LanguageQueryParameter.EN_US)
                .topicAdditionalAttributes(List.of(
                    TopicAdditionalAttributes.MODIFIED_BY_USER_NAME))
                .customAdditionalAttributes("country_name")
                .build();

        GetchildtopicsResponse res = sdk.portal().topic().getchildtopics()
                .request(req)
                .call();

        if (res.topicTreeResult().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                 | Type                                                                      | Required                                                                  | Description                                                               |
| ------------------------------------------------------------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| `request`                                                                 | [GetchildtopicsRequest](../../models/operations/GetchildtopicsRequest.md) | :heavy_check_mark:                                                        | The request object to use for the request.                                |

### Response

**[GetchildtopicsResponse](../../models/operations/GetchildtopicsResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getancestortopics

## Overview
  The Get All Ancestor Topics API retrieves the hierarchy from the root topic down to the given topic.


### Example Usage

<!-- UsageSnippet language="java" operationID="getancestortopics" method="get" path="/portals/{portalID}/topics/{topicID}/parents" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetancestortopicsRequest;
import com.egain.sdk.models.operations.GetancestortopicsResponse;
import java.lang.Exception;
import java.util.List;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetancestortopicsRequest req = GetancestortopicsRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .topicID("PROD-1069")
                .language(LanguageQueryParameter.EN_US)
                .topicAdditionalAttributes(List.of(
                    TopicAdditionalAttributes.MODIFIED_BY_USER_NAME))
                .customAdditionalAttributes("country_name")
                .build();

        GetancestortopicsResponse res = sdk.portal().topic().getancestortopics()
                .request(req)
                .call();

        if (res.topicResult().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                       | Type                                                                            | Required                                                                        | Description                                                                     |
| ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| `request`                                                                       | [GetancestortopicsRequest](../../models/operations/GetancestortopicsRequest.md) | :heavy_check_mark:                                                              | The request object to use for the request.                                      |

### Response

**[GetancestortopicsResponse](../../models/operations/GetancestortopicsResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getalltopics

## Overview
  The Get All Topics API retrieves the topic tree for a portal. The <code>$level</code> attribute determines how deep the topic hierarchy should go, or how many child topics of a topic are returned in the response.


### Example Usage

<!-- UsageSnippet language="java" operationID="getalltopics" method="get" path="/portals/{portalID}/topics" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetalltopicsRequest;
import com.egain.sdk.models.operations.GetalltopicsResponse;
import java.lang.Exception;
import java.util.List;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetalltopicsRequest req = GetalltopicsRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .level(-1L)
                .language(LanguageQueryParameter.EN_US)
                .topicAdditionalAttributes(List.of(
                    TopicAdditionalAttributes.MODIFIED_BY_USER_NAME))
                .customAdditionalAttributes("country_name")
                .build();

        GetalltopicsResponse res = sdk.portal().topic().getalltopics()
                .request(req)
                .call();

        if (res.topicTreeResult().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                             | Type                                                                  | Required                                                              | Description                                                           |
| --------------------------------------------------------------------- | --------------------------------------------------------------------- | --------------------------------------------------------------------- | --------------------------------------------------------------------- |
| `request`                                                             | [GetalltopicsRequest](../../models/operations/GetalltopicsRequest.md) | :heavy_check_mark:                                                    | The request object to use for the request.                            |

### Response

**[GetalltopicsResponse](../../models/operations/GetalltopicsResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |