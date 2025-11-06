# Article
(*portal().article()*)

## Overview

### Available Operations

* [getArticleById](#getarticlebyid) - Get Article by ID
* [getArticleByIdWithEditions](#getarticlebyidwitheditions) - Get Article By ID with Editions
* [getArticleEditionDetails](#getarticleeditiondetails) - Get Article Edition Details
* [addToReply](#addtoreply) - Add Article to Reply
* [addAsReference](#addasreference) - Add as Reference
* [getArticlesInTopic](#getarticlesintopic) - Get Articles in Topic
* [getArticleAttachmentById](#getarticleattachmentbyid) - Get Article Attachment By ID
* [getAttachmentByIdInPortal](#getattachmentbyidinportal) - Get Article Attachment in Portal
* [getRelatedArticles](#getrelatedarticles) - Get Related Articles
* [getAnnouncementArticles](#getannouncementarticles) - Get Announcement Articles
* [getArticleRatings](#getarticleratings) - Get Article Ratings
* [rateArticle](#ratearticle) - Rate an Article
* [getPendingComplianceArticles](#getpendingcompliancearticles) - Get Pending Article Compliances
* [getAcknowledgedComplianceArticles](#getacknowledgedcompliancearticles) - Get Acknowledged Article Compliances
* [complyArticle](#complyarticle) - Comply With an Article
* [getMySubscription](#getmysubscription) - My Subscription
* [subscribeArticle](#subscribearticle) - Subscribe to an Article
* [unsubscribeArticle](#unsubscribearticle) - Unsubscribe to an Article
* [getArticlePermissionsById](#getarticlepermissionsbyid) - Get Article Permissions By ID
* [getArticlePersonalization](#getarticlepersonalization) - Get Article Personalization Details

## getArticleById

## Overview
  * The Get Article by ID API allows a user to retrieve an Article using its ID.
    * Additional Article attributes and contextual views can be specified in the query parameters.

  * This API returns structured authoring attributes of Issue, Environment, Cause and Confidence Level when the following conditions are met:
    * The "Allow Structured Authoring" setting is enabled at the partition/department level through the Administrative Console.
    * The "Use Structured Authoring" flag is set on the article type.

## Prerequisites
  * Agents without a user profile and customers in a portal without a default user profile only have access to articles that:
    * Do not contain any access tags.
    * Do not contain any publish views.
    * Contain publish views without any associated tags.
  * Agents with a user profile and customers in a portal with a default user profile have access to articles that:
    * Do not contain any access tags.
    * Do not contain any publish views.
    * Contain publish views without any associated tags.
    * Contain access tags that are also in the assigned user profiles.
    * Contain publish views with associated tags that are also in the assigned user profiles.
  * Agents with the following assigned actions can view updates to articles currently being processed in workflows:
    * View Author Portal – Allows agents to view updates to articles at any stage in a workflow.
    * View Staging Portal – Allows agents to view updates to articles in the Staging stage or a subsequent stage in a workflow.


### Example Usage

<!-- UsageSnippet language="java" operationID="getArticleById" method="get" path="/portals/{portalID}/articles/{articleID}" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetArticleByIdRequest;
import com.egain.sdk.models.operations.GetArticleByIdResponse;
import java.lang.Exception;
import java.util.List;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetArticleByIdRequest req = GetArticleByIdRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .articleID("PROD-2996")
                .language(LanguageQueryParameter.EN_US)
                .articleAdditionalAttributes(List.of(
                    ArticleAdditionalAttributes.AVERAGE_RATING))
                .customAdditionalAttributes("country_name")
                .publishViewId("PROD-3203")
                .workflowMilestone(WorkflowMilestone.PUBLISH)
                .build();

        GetArticleByIdResponse res = sdk.portal().article().getArticleById()
                .request(req)
                .call();

        if (res.article().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                 | Type                                                                      | Required                                                                  | Description                                                               |
| ------------------------------------------------------------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| `request`                                                                 | [GetArticleByIdRequest](../../models/operations/GetArticleByIdRequest.md) | :heavy_check_mark:                                                        | The request object to use for the request.                                |

### Response

**[GetArticleByIdResponse](../../models/operations/GetArticleByIdResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getArticleByIdWithEditions

## Overview
  * This API allows a user to retrieve an article with all its editions.
  * If there are no editions for the article, the response will contain the base content of the article.


### Example Usage

<!-- UsageSnippet language="java" operationID="getArticleByIdWithEditions" method="get" path="/articles/{articleID}/witheditions" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.components.MandatoryLanguageQueryParameter;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetArticleByIdWithEditionsResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetArticleByIdWithEditionsResponse res = sdk.portal().article().getArticleByIdWithEditions()
                .acceptLanguage(AcceptLanguage.EN_US)
                .articleID("PROD-2996")
                .language(MandatoryLanguageQueryParameter.EN_US)
                .call();

        if (res.articleWithEditions().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                                                                        | Type                                                                                                                             | Required                                                                                                                         | Description                                                                                                                      | Example                                                                                                                          |
| -------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| `acceptLanguage`                                                                                                                 | [AcceptLanguage](../../models/components/AcceptLanguage.md)                                                                      | :heavy_check_mark:                                                                                                               | The Language locale accepted by the client (used for locale specific fields in resource representation and in error responses).  | en-US                                                                                                                            |
| `articleID`                                                                                                                      | *String*                                                                                                                         | :heavy_check_mark:                                                                                                               | The ID of the Article.<br><br>An Article ID is composed of a 2-4 letter prefix followed by a dash and 4-15 digits.               | PROD-2996                                                                                                                        |
| `language`                                                                                                                       | [MandatoryLanguageQueryParameter](../../models/components/MandatoryLanguageQueryParameter.md)                                    | :heavy_check_mark:                                                                                                               | The language used for fetching the details of a resource. Resources available in different languages may differ from each other. | en-US                                                                                                                            |

### Response

**[GetArticleByIdWithEditionsResponse](../../models/operations/GetArticleByIdWithEditionsResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getArticleEditionDetails

## Overview
  * This API allows a user to retrieve an article with all its editions.


### Example Usage

<!-- UsageSnippet language="java" operationID="getArticleEditionDetails" method="get" path="/articles/{articleID}/editions/{publishViewId}" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.components.MandatoryLanguageQueryParameter;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetArticleEditionDetailsResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetArticleEditionDetailsResponse res = sdk.portal().article().getArticleEditionDetails()
                .acceptLanguage(AcceptLanguage.EN_US)
                .articleID("PROD-2996")
                .publishViewId("959500000204621")
                .language(MandatoryLanguageQueryParameter.EN_US)
                .call();

        if (res.editionWithContent().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                                                                        | Type                                                                                                                             | Required                                                                                                                         | Description                                                                                                                      | Example                                                                                                                          |
| -------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| `acceptLanguage`                                                                                                                 | [AcceptLanguage](../../models/components/AcceptLanguage.md)                                                                      | :heavy_check_mark:                                                                                                               | The Language locale accepted by the client (used for locale specific fields in resource representation and in error responses).  | en-US                                                                                                                            |
| `articleID`                                                                                                                      | *String*                                                                                                                         | :heavy_check_mark:                                                                                                               | The ID of the Article. Both numeric and alternate ID formats are supported.<br><br>Valid numerical IDs are 15-19 digits long.    |                                                                                                                                  |
| `publishViewId`                                                                                                                  | *String*                                                                                                                         | :heavy_check_mark:                                                                                                               | Publish View Id of the article on which operation is performed.                                                                  | 959500000204621                                                                                                                  |
| `language`                                                                                                                       | [MandatoryLanguageQueryParameter](../../models/components/MandatoryLanguageQueryParameter.md)                                    | :heavy_check_mark:                                                                                                               | The language used for fetching the details of a resource. Resources available in different languages may differ from each other. | en-US                                                                                                                            |

### Response

**[GetArticleEditionDetailsResponse](../../models/operations/GetArticleEditionDetailsResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## addToReply

## Overview
The Add Article to Reply API captures events for articles used in a reply for a digital channel activity.

Note:  Either the <code>x-ext-activity-id</code> or<br><code>x-ext-integration-id</code> and <code>x-ext-interaction-id</code> header must be provided.

## Permissions
  * Only Agents can invoke this API.


### Example Usage

<!-- UsageSnippet language="java" operationID="AddToReply" method="put" path="/portals/{portalID}/articles/{articleID}/addtoreply" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.AddToReplyResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        AddToReplyResponse res = sdk.portal().article().addToReply()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .articleID("PROD-2996")
                .body(ArticleActivityLink.builder()
                    .versionId("PROD-12416")
                    .language(ArticleActivityLinkLanguage.builder()
                        .code(ArticleActivityLinkCode.EN_US)
                        .build())
                    .editionId("PROD-13015")
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
| `articleID`                                                                                                                     | *String*                                                                                                                        | :heavy_check_mark:                                                                                                              | The ID of the Article.<br><br>An Article ID is composed of a 2-4 letter prefix followed by a dash and 4-15 digits.              | PROD-2996                                                                                                                       |
| `body`                                                                                                                          | [ArticleActivityLink](../../models/components/ArticleActivityLink.md)                                                           | :heavy_check_mark:                                                                                                              | N/A                                                                                                                             | {<br/>"editionId": "PROD-13015",<br/>"versionId": "PROD-12416",<br/>"language": {<br/>"code": "en-US"<br/>}<br/>}               |

### Response

**[AddToReplyResponse](../../models/operations/AddToReplyResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## addAsReference

## Overview
The Add as Reference API captures events for articles that are referenced by agents replying inside of a digital channel activity.

Note: Either the <code>x-ext-activity-id</code> or<br><code>x-ext-integration-id</code> and <code>x-ext-interaction-id</code> header must be provided.

## Permissions
  * Only Agents can invoke this API.


### Example Usage

<!-- UsageSnippet language="java" operationID="AddAsReference" method="put" path="/portals/{portalID}/articles/{articleID}/addasreference" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.AddAsReferenceResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        AddAsReferenceResponse res = sdk.portal().article().addAsReference()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .articleID("PROD-2996")
                .body(ArticleActivityLink.builder()
                    .versionId("PROD-12416")
                    .language(ArticleActivityLinkLanguage.builder()
                        .code(ArticleActivityLinkCode.EN_US)
                        .build())
                    .editionId("PROD-13015")
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
| `articleID`                                                                                                                     | *String*                                                                                                                        | :heavy_check_mark:                                                                                                              | The ID of the Article.<br><br>An Article ID is composed of a 2-4 letter prefix followed by a dash and 4-15 digits.              | PROD-2996                                                                                                                       |
| `body`                                                                                                                          | [ArticleActivityLink](../../models/components/ArticleActivityLink.md)                                                           | :heavy_check_mark:                                                                                                              | N/A                                                                                                                             | {<br/>"editionId": "PROD-13015",<br/>"versionId": "PROD-12416",<br/>"language": {<br/>"code": "en-US"<br/>}<br/>}               |

### Response

**[AddAsReferenceResponse](../../models/operations/AddAsReferenceResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getArticlesInTopic

## Overview
The Get Articles in Topic API allows a user to retrieve the browsable articles in a topic.

## Prerequisites
  * Set the Article type’s parameter “Include in browse on portals” to "Yes" for an article to be returned by this API.
  * Agents without a user profile and customers in a portal without a default user profile only have access to articles that:
    * Do not contain any access tags.
    * Do not contain any publish views.
    * Contain published views without any associated tags.
  * Agents with a user profile and customers in a portal with a default user profile have access to articles that:
    * Do not contain any access tags.
    * Do not contain any publish views.
    * Contain publish views without any associated tags.
    * Contain access tags that are also in the assigned user profiles.
    * Contain publish views with associated tags that are also in the assigned user profiles.
  
## Permissions  
  * Agents with the following assigned actions can view updates to articles currently being processed in workflows:
    * View Author Portal – Allows agents to view updates to articles at any stage in a workflow.
    * View Staging Portal – Allows agents to view updates to articles in the Staging stage or a subsequent stage in a workflow.


### Example Usage

<!-- UsageSnippet language="java" operationID="getArticlesInTopic" method="get" path="/portals/{portalID}/articles" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetArticlesInTopicRequest;
import com.egain.sdk.models.operations.GetArticlesInTopicResponse;
import java.lang.Exception;
import java.util.List;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetArticlesInTopicRequest req = GetArticlesInTopicRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .filterTopicId("PROD-1065")
                .searchProfileId("959500000204621")
                .articleResultAdditionalAttributes(List.of(
                    ArticleResultAdditionalAttributes.AVERAGE_RATING))
                .filterTags("PROD-5381")
                .workflowMilestone(WorkflowMilestone.PUBLISH)
                .language(LanguageQueryParameter.EN_US)
                .sort(ArticleSort.NAME)
                .order(ArticleSortOrder.ASC)
                .build();

        GetArticlesInTopicResponse res = sdk.portal().article().getArticlesInTopic()
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
| `request`                                                                         | [GetArticlesInTopicRequest](../../models/operations/GetArticlesInTopicRequest.md) | :heavy_check_mark:                                                                | The request object to use for the request.                                        |

### Response

**[GetArticlesInTopicResponse](../../models/operations/GetArticlesInTopicResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getArticleAttachmentById

## Overview
  This API allows one article attachment identified by an attachment ID to be retrieved.


### Example Usage

<!-- UsageSnippet language="java" operationID="getArticleAttachmentById" method="get" path="/articles/attachments/{attachmentID}" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetArticleAttachmentByIdResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetArticleAttachmentByIdResponse res = sdk.portal().article().getArticleAttachmentById()
                .acceptLanguage(AcceptLanguage.EN_US)
                .attachmentID("PROD-1000")
                .call();

        if (res.attachmentContentResult().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                                                                       | Type                                                                                                                            | Required                                                                                                                        | Description                                                                                                                     | Example                                                                                                                         |
| ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| `acceptLanguage`                                                                                                                | [AcceptLanguage](../../models/components/AcceptLanguage.md)                                                                     | :heavy_check_mark:                                                                                                              | The Language locale accepted by the client (used for locale specific fields in resource representation and in error responses). | en-US                                                                                                                           |
| `attachmentID`                                                                                                                  | *String*                                                                                                                        | :heavy_check_mark:                                                                                                              | The ID of the attachment.<br><br>An attachment ID is composed of a 2-4 letter prefix, followed by a dash and 4-15 digits.       | PROD-1000                                                                                                                       |

### Response

**[GetArticleAttachmentByIdResponse](../../models/operations/GetArticleAttachmentByIdResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getAttachmentByIdInPortal

## Overview
The Get Article Attachment API retrieves an attachment associated to an article by calling the attachment ID for a specified portal ID.


### Example Usage

<!-- UsageSnippet language="java" operationID="getAttachmentByIdInPortal" method="get" path="/portals/{portalID}/articles/attachments/{attachmentID}" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.components.LanguageQueryParameter;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetAttachmentByIdInPortalResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetAttachmentByIdInPortalResponse res = sdk.portal().article().getAttachmentByIdInPortal()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .attachmentID("PROD-1000")
                .language(LanguageQueryParameter.EN_US)
                .call();

        if (res.attachmentContentResult().isPresent()) {
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
| `attachmentID`                                                                                                                                                                                                        | *String*                                                                                                                                                                                                              | :heavy_check_mark:                                                                                                                                                                                                    | The ID of the attachment.<br><br>An attachment ID is composed of a 2-4 letter prefix, followed by a dash and 4-15 digits.                                                                                             | PROD-1000                                                                                                                                                                                                             |
| `language`                                                                                                                                                                                                            | [Optional\<LanguageQueryParameter>](../../models/components/LanguageQueryParameter.md)                                                                                                                                | :heavy_minus_sign:                                                                                                                                                                                                    | The language that describes the details of a resource. Resources available in different languages may differ from each other.<li>If <code>lang</code> is not passed, then the portal's default language is used.</li> | en-US                                                                                                                                                                                                                 |

### Response

**[GetAttachmentByIdInPortalResponse](../../models/operations/GetAttachmentByIdInPortalResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getRelatedArticles

## Overview
The Get Related Articles API retrieves all related articles associated to a given article.

## Prerequisites
  * Set the Article type’s parameter “Include in browse on portals” to "Yes" for an article to be returned by this API.
  * Agents without a user profile and customers in a portal without a default user profile only have access to articles that:
    * Do not contain any access tags.
    * Do not contain any publish views.
    * Contain publish views without any associated tags.
  * Agents with a user profile and customers in a portal with a default user profile have access to articles that:
    * Do not contain any access tags.
    * Do not contain any publish views.
    * Contain publish views without any associated tags.
    * Contain access tags that are also in the assigned user profiles.
    * Contain publish views with associated tags that are also in the assigned user profiles.

## Permissions 
  * Agents with the following assigned actions can view updates to articles currently being processed in workflows:
    * View Author Portal – Allows agents to view updates to articles at any stage in a workflow.
    * View Staging Portal – Allows agents to view updates to articles in the Staging stage or a subsequent stage in a workflow.


### Example Usage

<!-- UsageSnippet language="java" operationID="getRelatedArticles" method="get" path="/portals/{portalID}/articles/{articleID}/related" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetRelatedArticlesRequest;
import com.egain.sdk.models.operations.GetRelatedArticlesResponse;
import java.lang.Exception;
import java.util.List;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetRelatedArticlesRequest req = GetRelatedArticlesRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .articleID("PROD-2996")
                .filterTags("PROD-5381")
                .articleResultAdditionalAttributes(List.of(
                    ArticleResultAdditionalAttributes.AVERAGE_RATING))
                .workflowMilestone(WorkflowMilestone.PUBLISH)
                .language(LanguageQueryParameter.EN_US)
                .build();

        GetRelatedArticlesResponse res = sdk.portal().article().getRelatedArticles()
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
| `request`                                                                         | [GetRelatedArticlesRequest](../../models/operations/GetRelatedArticlesRequest.md) | :heavy_check_mark:                                                                | The request object to use for the request.                                        |

### Response

**[GetRelatedArticlesResponse](../../models/operations/GetRelatedArticlesResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getAnnouncementArticles

## Overview
The Get Announcement Articles API returns a portal's announcement articles. Only displayable announcement articles in the portal are returned. 

## Prerequisites
  * For an article to display or be returned, set the Article type’s parameter, “Include in browse on portals,” to "Yes".
  * Agents without a user profile and customers in a portal without a default user profile only have access to articles that:
    * Do not contain any access tags.
    * Do not contain any publish views.
    * Contain publish views without any associated tags.
  * Agents with a user profile and customers in a portal with a default user profile have access to articles that:
    * Do not contain any access tags.
    * Do not contain any publish views.
    * Contain publish views without any associated tags.
    * Contain access tags that are also in the assigned user profiles.
    * Contain publish views with associated tags that are also in the assigned user profiles.

## Permissions 
  * Agents with the following assigned actions can view updates to articles currently being processed in workflows:
    * View Author Portal – Allows agents to view updates to articles at any stage in a workflow.
    * View Staging Portal – Allows agents to view updates to articles in the Staging stage or a subsequent stage in a workflow.
    


### Example Usage

<!-- UsageSnippet language="java" operationID="getAnnouncementArticles" method="get" path="/portals/{portalID}/articles/announcements" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetAnnouncementArticlesRequest;
import com.egain.sdk.models.operations.GetAnnouncementArticlesResponse;
import java.lang.Exception;
import java.util.List;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetAnnouncementArticlesRequest req = GetAnnouncementArticlesRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .filterTags("PROD-5381")
                .articleResultAdditionalAttributes(List.of(
                    ArticleResultAdditionalAttributes.AVERAGE_RATING))
                .workflowMilestone(WorkflowMilestone.PUBLISH)
                .language(LanguageQueryParameter.EN_US)
                .build();

        GetAnnouncementArticlesResponse res = sdk.portal().article().getAnnouncementArticles()
                .request(req)
                .call();

        if (res.articleResults().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                                   | Type                                                                                        | Required                                                                                    | Description                                                                                 |
| ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| `request`                                                                                   | [GetAnnouncementArticlesRequest](../../models/operations/GetAnnouncementArticlesRequest.md) | :heavy_check_mark:                                                                          | The request object to use for the request.                                                  |

### Response

**[GetAnnouncementArticlesResponse](../../models/operations/GetAnnouncementArticlesResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getArticleRatings

## Overview
The Get Article Ratings API returns ratings set for an Article. These ratings help you to assess the quality, helpfulness, or relevance of an article. 


### Example Usage

<!-- UsageSnippet language="java" operationID="getArticleRatings" method="get" path="/portals/{portalID}/articles/{articleID}/ratings" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.components.LanguageQueryParameter;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetArticleRatingsRequest;
import com.egain.sdk.models.operations.GetArticleRatingsResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetArticleRatingsRequest req = GetArticleRatingsRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .articleID("PROD-2996")
                .language(LanguageQueryParameter.EN_US)
                .build();

        GetArticleRatingsResponse res = sdk.portal().article().getArticleRatings()
                .request(req)
                .call();

        if (res.articleRatingsResponse().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                       | Type                                                                            | Required                                                                        | Description                                                                     |
| ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| `request`                                                                       | [GetArticleRatingsRequest](../../models/operations/GetArticleRatingsRequest.md) | :heavy_check_mark:                                                              | The request object to use for the request.                                      |

### Response

**[GetArticleRatingsResponse](../../models/operations/GetArticleRatingsResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## rateArticle

## Overview
The Rate an Article API allows a user to set a rating for an article. These ratings allow you to assess the quality, helpfulness, or relevance of an article. 


### Example Usage

<!-- UsageSnippet language="java" operationID="rateArticle" method="put" path="/portals/{portalID}/articles/{articleID}/ratings" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.components.LanguageQueryParameter;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.RateArticleRequest;
import com.egain.sdk.models.operations.RateArticleResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        RateArticleRequest req = RateArticleRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .articleID("PROD-2996")
                .score(10)
                .language(LanguageQueryParameter.EN_US)
                .comments("Accepted")
                .build();

        RateArticleResponse res = sdk.portal().article().rateArticle()
                .request(req)
                .call();

        // handle response
    }
}
```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `request`                                                           | [RateArticleRequest](../../models/operations/RateArticleRequest.md) | :heavy_check_mark:                                                  | The request object to use for the request.                          |

### Response

**[RateArticleResponse](../../models/operations/RateArticleResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getPendingComplianceArticles

## Overview
The Get Pending Article Compliances API retrieves all compliance-enabled articles in a portal that need to be read by the current user. Results are sorted in ascending order of the compliance due date.

## Prerequisites
* The Article compliances that are returned must be:
    * Available for the current user profile.
    * Displayable. An article is displayable when the "Include in browse on portals" property is enabled for the article.


### Example Usage

<!-- UsageSnippet language="java" operationID="getPendingComplianceArticles" method="get" path="/portals/{portalID}/articles/compliance/pending" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetPendingComplianceArticlesRequest;
import com.egain.sdk.models.operations.GetPendingComplianceArticlesResponse;
import java.lang.Exception;
import java.util.List;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetPendingComplianceArticlesRequest req = GetPendingComplianceArticlesRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .complianceArticleResultAdditionalAttributes(List.of(
                    ComplianceArticleResultAdditionalAttributes.AVERAGE_RATING))
                .language(LanguageQueryParameter.EN_US)
                .build();

        GetPendingComplianceArticlesResponse res = sdk.portal().article().getPendingComplianceArticles()
                .request(req)
                .call();

        if (res.complianceArticleResults().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                                             | Type                                                                                                  | Required                                                                                              | Description                                                                                           |
| ----------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| `request`                                                                                             | [GetPendingComplianceArticlesRequest](../../models/operations/GetPendingComplianceArticlesRequest.md) | :heavy_check_mark:                                                                                    | The request object to use for the request.                                                            |

### Response

**[GetPendingComplianceArticlesResponse](../../models/operations/GetPendingComplianceArticlesResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getAcknowledgedComplianceArticles

## Overview
The Get Acknowledged Article Compliances API retrieves all compliance-enabled articles in a portal that have been read by the current user in the last 60 days. Results are sorted in descending order of the acknowledgement date.

## Prerequisites
* The Article compliances that are returned must be:
    * Available for the current user profile.
    * Displayable. An article is displayable when the "Include in browse on portals" property is enabled for the article.
    * Acknowledged within the last 60 days.
    * Only the latest version of a republished compliance article will be shown.
    * Results will be sorted in descending order of acknowledgment date.


### Example Usage

<!-- UsageSnippet language="java" operationID="getAcknowledgedComplianceArticles" method="get" path="/portals/{portalID}/articles/compliance/acknowledged" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetAcknowledgedComplianceArticlesRequest;
import com.egain.sdk.models.operations.GetAcknowledgedComplianceArticlesResponse;
import java.lang.Exception;
import java.util.List;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetAcknowledgedComplianceArticlesRequest req = GetAcknowledgedComplianceArticlesRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .complianceArticleResultAdditionalAttributes(List.of(
                    ComplianceArticleResultAdditionalAttributes.AVERAGE_RATING))
                .language(LanguageQueryParameter.EN_US)
                .build();

        GetAcknowledgedComplianceArticlesResponse res = sdk.portal().article().getAcknowledgedComplianceArticles()
                .request(req)
                .call();

        if (res.complianceArticleResults().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                                                       | Type                                                                                                            | Required                                                                                                        | Description                                                                                                     |
| --------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| `request`                                                                                                       | [GetAcknowledgedComplianceArticlesRequest](../../models/operations/GetAcknowledgedComplianceArticlesRequest.md) | :heavy_check_mark:                                                                                              | The request object to use for the request.                                                                      |

### Response

**[GetAcknowledgedComplianceArticlesResponse](../../models/operations/GetAcknowledgedComplianceArticlesResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## complyArticle

## Overview
The Comply with an Article API allows the user to comply with an article by passing the Article's ID, which marks it as read by the user.            

## Prerequisites
  * The user must be an agent and:
    * Be available in the portal.
    * Be available for the current user profile.
    * Have the Article's compliance policy enabled.
  * If the Article has Access Tags, then it must be available for the agent's current user profile.
  * If the Article has Publish Views, then at least one edition of the Article must be available for the agent's current user profile.


### Example Usage

<!-- UsageSnippet language="java" operationID="complyArticle" method="put" path="/portals/{portalID}/articles/{articleID}/comply" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.ComplyArticleResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        ComplyArticleResponse res = sdk.portal().article().complyArticle()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .articleID("PROD-2996")
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
| `articleID`                                                                                                                     | *String*                                                                                                                        | :heavy_check_mark:                                                                                                              | The ID of the Article.<br><br>An Article ID is composed of a 2-4 letter prefix followed by a dash and 4-15 digits.              | PROD-2996                                                                                                                       |

### Response

**[ComplyArticleResponse](../../models/operations/ComplyArticleResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getMySubscription

## Overview
The My Subscription API allows authenticated users and agents to retrieve the list of articles to which they are subscribed.


### Example Usage

<!-- UsageSnippet language="java" operationID="getMySubscription" method="get" path="/portals/{portalID}/articles/subscribed" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetMySubscriptionRequest;
import com.egain.sdk.models.operations.GetMySubscriptionResponse;
import java.lang.Exception;
import java.util.List;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetMySubscriptionRequest req = GetMySubscriptionRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .workflowMilestone(WorkflowMilestone.PUBLISH)
                .articleResultAdditionalAttributes(List.of(
                    ArticleResultAdditionalAttributes.AVERAGE_RATING))
                .build();

        GetMySubscriptionResponse res = sdk.portal().article().getMySubscription()
                .request(req)
                .call();

        if (res.articleResults().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                       | Type                                                                            | Required                                                                        | Description                                                                     |
| ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| `request`                                                                       | [GetMySubscriptionRequest](../../models/operations/GetMySubscriptionRequest.md) | :heavy_check_mark:                                                              | The request object to use for the request.                                      |

### Response

**[GetMySubscriptionResponse](../../models/operations/GetMySubscriptionResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## subscribeArticle

## Overview
The Subscribe to an Article API allows eGain users, authenticated customers and agents to subscribe and receive notifications about changes to an Article.

## Prerequisites
  * Notifications are sent only if the following conditions are met:
    * The Article content has been modified since the last published version.
    * The attachment list has been modified since the last published version.
    * The author has checked the "Include Article in new and modified Article list" option while publishing the Article.
  * For the Subscribe to an Article API to execute successfully:
    * The Article must be in the portal.
    * The user must have provided an email address.

## Permissions
  * Agent Permissions: The following permissions are required if the user is an agent:
    * If the Article has Access Tags:
      * The Article must be available for the agent's current user profile.
    * If the Article has Publish Views:
      * At least one edition of the Article must be available for the agent's current user profile.
    * If the Article has filters and the "tags query parameter" is provided:
      * The Article filters must match the provided tags or tag groups.
  * Customer Permissions: The following permissions are required if the user is a customer:
    * If the Article has Access Tags:
      * The portal must have a default user profile
      * The Article must be available for the portal's default user profile.
    * If the Article has Publish Views:
      * The portal must have a default user profile
      * At least one edition must be available for the portal's default user profile.
    * If the Article has filters and the "tags query parameter" is provided, then the Article filters must match the provided tags or tag groups.


### Example Usage

<!-- UsageSnippet language="java" operationID="subscribeArticle" method="put" path="/portals/{portalID}/articles/{articleID}/subscribe" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.SubscribeArticleResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        SubscribeArticleResponse res = sdk.portal().article().subscribeArticle()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .articleID("PROD-2996")
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
| `articleID`                                                                                                                     | *String*                                                                                                                        | :heavy_check_mark:                                                                                                              | The ID of the Article.<br><br>An Article ID is composed of a 2-4 letter prefix followed by a dash and 4-15 digits.              | PROD-2996                                                                                                                       |

### Response

**[SubscribeArticleResponse](../../models/operations/SubscribeArticleResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## unsubscribeArticle

## Overview
The Unsubscribe to an Article API allows authenticated users and agents to unsubscribe to an Article for which they were earlier subscribed to receive change notifications.


### Example Usage

<!-- UsageSnippet language="java" operationID="unsubscribeArticle" method="put" path="/portals/{portalID}/articles/{articleID}/unsubscribe" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.UnsubscribeArticleResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        UnsubscribeArticleResponse res = sdk.portal().article().unsubscribeArticle()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .articleID("PROD-2996")
                .call();

        // handle response
    }
}
```

### Parameters

| Parameter                                                                                                                                                                                                                                                   | Type                                                                                                                                                                                                                                                        | Required                                                                                                                                                                                                                                                    | Description                                                                                                                                                                                                                                                 | Example                                                                                                                                                                                                                                                     |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `acceptLanguage`                                                                                                                                                                                                                                            | [AcceptLanguage](../../models/components/AcceptLanguage.md)                                                                                                                                                                                                 | :heavy_check_mark:                                                                                                                                                                                                                                          | The Language locale accepted by the client (used for locale specific fields in resource representation and in error responses).                                                                                                                             | en-US                                                                                                                                                                                                                                                       |
| `portalID`                                                                                                                                                                                                                                                  | *String*                                                                                                                                                                                                                                                    | :heavy_check_mark:                                                                                                                                                                                                                                          | The ID of the portal being accessed.<br><br>A portal ID is composed of a 2-4 letter prefix, followed by a dash and 4-15 digits.                                                                                                                             | PROD-1000                                                                                                                                                                                                                                                   |
| `articleID`                                                                                                                                                                                                                                                 | *String*                                                                                                                                                                                                                                                    | :heavy_check_mark:                                                                                                                                                                                                                                          | The ID of the Article.<br><br>An Article ID is composed of a 2-4 letter prefix followed by a dash and 4-15 digits.                                                                                                                                          | PROD-2996                                                                                                                                                                                                                                                   |
| `unsubscriptionToken`                                                                                                                                                                                                                                       | *Optional\<String>*                                                                                                                                                                                                                                         | :heavy_minus_sign:                                                                                                                                                                                                                                          | An encrypted token that contains information about "object/userId/userType/userProfileId". This is used to unsubscribe the user from Article change notifications sent via email, without necessitating that the user be logged into the eGain application. |                                                                                                                                                                                                                                                             |

### Response

**[UnsubscribeArticleResponse](../../models/operations/UnsubscribeArticleResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getArticlePermissionsById

## Overview
  * The Get Article Permission by ID permits a user to retrieve permissions associated to an article.


### Example Usage

<!-- UsageSnippet language="java" operationID="getArticlePermissionsById" method="get" path="/portals/{portalID}/articles/{articleID}/permissions" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetArticlePermissionsByIdResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetArticlePermissionsByIdResponse res = sdk.portal().article().getArticlePermissionsById()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .articleID("PROD-2996")
                .call();

        if (res.articlePermissionsResult().isPresent()) {
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
| `articleID`                                                                                                                     | *String*                                                                                                                        | :heavy_check_mark:                                                                                                              | The ID of the Article.<br><br>An Article ID is composed of a 2-4 letter prefix followed by a dash and 4-15 digits.              | PROD-2996                                                                                                                       |

### Response

**[GetArticlePermissionsByIdResponse](../../models/operations/GetArticlePermissionsByIdResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getArticlePersonalization

## Overview
  The Get Article Personalization Details API allows agents to retrieve the personalization tag details of an Article.


### Example Usage

<!-- UsageSnippet language="java" operationID="getArticlePersonalization" method="get" path="/portals/{portalID}/articles/{articleID}/personalization" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.components.LanguageQueryParameter;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetArticlePersonalizationRequest;
import com.egain.sdk.models.operations.GetArticlePersonalizationResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetArticlePersonalizationRequest req = GetArticlePersonalizationRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .articleID("PROD-2996")
                .language(LanguageQueryParameter.EN_US)
                .build();

        GetArticlePersonalizationResponse res = sdk.portal().article().getArticlePersonalization()
                .request(req)
                .call();

        if (res.personalization().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                                       | Type                                                                                            | Required                                                                                        | Description                                                                                     |
| ----------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| `request`                                                                                       | [GetArticlePersonalizationRequest](../../models/operations/GetArticlePersonalizationRequest.md) | :heavy_check_mark:                                                                              | The request object to use for the request.                                                      |

### Response

**[GetArticlePersonalizationResponse](../../models/operations/GetArticlePersonalizationResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |