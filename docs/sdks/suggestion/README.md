# Suggestion
(*portal().suggestion()*)

## Overview

### Available Operations

* [makeSuggestion](#makesuggestion) - Make a Suggestion
* [modifySuggestions](#modifysuggestions) - Modify Suggestion
* [searchSuggestion](#searchsuggestion) - Get Suggestion by Status
* [getSuggestion](#getsuggestion) - Get Suggestion by ID
* [deleteSuggestion](#deletesuggestion) - Delete a Suggestion
* [getRelatedArticlesForSuggestion](#getrelatedarticlesforsuggestion) - Get Related Articles for Suggestion
* [getSuggestionComments](#getsuggestioncomments) - Get Suggestion Comments
* [getSuggestionAttachments](#getsuggestionattachments) - Get Suggestion Attachments
* [getSuggestionAttachmentById](#getsuggestionattachmentbyid) - Get Suggestion Attachment by ID

## makeSuggestion

## Overview
  The Make a Suggestion API allows users to create an Article Suggestion from within a knowledge portal.

## Prerequisites
  * Enable the setting "Manage a Suggestion" for the portal specified in the URL.
  * If the user is a Customer, enable the setting "Allow Customer Access" for the portal.
  * If you want to add an attachment to a Suggestion, first call the Generate Signed URL to Upload API to add an attachment using the provided returned altID in attachment request.


### Example Usage

<!-- UsageSnippet language="java" operationID="makeSuggestion" method="post" path="/portals/{portalID}/suggestions" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.MakeSuggestionResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        MakeSuggestionResponse res = sdk.portal().suggestion().makeSuggestion()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .body(CreateSuggestion.builder()
                    .name("Proposed Store Phone Number")
                    .content("You should update your website with the new phone number.")
                    .language(CreateSuggestionLanguage.builder()
                        .code(CreateSuggestionCode.EN_US)
                        .build())
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
| `body`                                                                                                                          | [Optional\<CreateSuggestion>](../../models/components/CreateSuggestion.md)                                                      | :heavy_minus_sign:                                                                                                              | N/A                                                                                                                             |                                                                                                                                 |

### Response

**[MakeSuggestionResponse](../../models/operations/MakeSuggestionResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## modifySuggestions

## Overview
  The Modify Suggestion API allows authenticated users to modify their own Suggestion.

## Prerequisites
  * Enable the setting "Manage a Suggestion" for the portal specified in the URL.
  * If the user is a Customer, enable the setting "Allow Customer Access" for the portal.
  * The "Suggestion ID" specified in the request body must exist and belong to the user.
  * The status of this Suggestion as returned by the Get Suggestion by ID API must be "pending".
  * At least one of the optional request body attributes must be provided.


### Example Usage

<!-- UsageSnippet language="java" operationID="modifySuggestions" method="put" path="/portals/{portalID}/suggestions" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.components.ModifySuggestion;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.ModifySuggestionsResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        ModifySuggestionsResponse res = sdk.portal().suggestion().modifySuggestions()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .body(ModifySuggestion.builder()
                    .id("PROD-5722")
                    .modifiedDate("2024-07-31T14:10:19Z")
                    .name("Improving Telecommunication Services")
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
| `body`                                                                                                                          | [Optional\<ModifySuggestion>](../../models/components/ModifySuggestion.md)                                                      | :heavy_minus_sign:                                                                                                              | N/A                                                                                                                             |                                                                                                                                 |

### Response

**[ModifySuggestionsResponse](../../models/operations/ModifySuggestionsResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## searchSuggestion

## Overview
  The Get Suggestion by Status API allows authenticated users to retrieve their own suggestions based on Suggestion status.
  
## Prerequisites
  * Enable the setting "My Suggestions" for the portal specified in the URL.
  * If the user is a customer, enable the setting "Allow Customer Access" for the portal.


### Example Usage

<!-- UsageSnippet language="java" operationID="searchSuggestion" method="get" path="/portals/{portalID}/suggestions" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.*;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        SearchSuggestionRequest req = SearchSuggestionRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .filterStatus(FilterStatus.SUGGESTED)
                .build();

        SearchSuggestionResponse res = sdk.portal().suggestion().searchSuggestion()
                .request(req)
                .call();

        if (res.suggestions().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                     | Type                                                                          | Required                                                                      | Description                                                                   |
| ----------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| `request`                                                                     | [SearchSuggestionRequest](../../models/operations/SearchSuggestionRequest.md) | :heavy_check_mark:                                                            | The request object to use for the request.                                    |

### Response

**[SearchSuggestionResponse](../../models/operations/SearchSuggestionResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getSuggestion

## Overview
  The Get Suggestion by ID API allows authenticated users to retrieve their own suggestions.

## Prerequisites
  * Enable the setting "My Suggestions" for the portal specified in the URL.
  * If the user is a customer, enable the setting "Allow Customer Access" for the portal.
  * The "Suggestion for the ID" specified in the URL must belong to the user.
 


### Example Usage

<!-- UsageSnippet language="java" operationID="getSuggestion" method="get" path="/portals/{portalID}/suggestions/{suggestionID}" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.components.SuggestionAdditionalAttributes;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetSuggestionRequest;
import com.egain.sdk.models.operations.GetSuggestionResponse;
import java.lang.Exception;
import java.util.List;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetSuggestionRequest req = GetSuggestionRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .suggestionID("PROD-11829")
                .suggestionAdditionalAttributes(List.of(
                    SuggestionAdditionalAttributes.CONTENT))
                .customAdditionalAttributes("country_name")
                .build();

        GetSuggestionResponse res = sdk.portal().suggestion().getSuggestion()
                .request(req)
                .call();

        if (res.suggestion().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                               | Type                                                                    | Required                                                                | Description                                                             |
| ----------------------------------------------------------------------- | ----------------------------------------------------------------------- | ----------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| `request`                                                               | [GetSuggestionRequest](../../models/operations/GetSuggestionRequest.md) | :heavy_check_mark:                                                      | The request object to use for the request.                              |

### Response

**[GetSuggestionResponse](../../models/operations/GetSuggestionResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## deleteSuggestion

## Overview
  The Delete a Suggestion API allows authenticated users to delete their own Suggestion.

## Prerequisites
  * Enable the setting "Manage a Suggestion" for the portal specified in the URL.
  * If the user is a customer, enable the setting "Allow Customer Access" for the portal.
  * The Suggestion must belong to the user.
  * The status of the Suggestion must not be "approved".


### Example Usage

<!-- UsageSnippet language="java" operationID="deleteSuggestion" method="delete" path="/portals/{portalID}/suggestions/{suggestionID}" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.DeleteSuggestionResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        DeleteSuggestionResponse res = sdk.portal().suggestion().deleteSuggestion()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .suggestionID("PROD-11829")
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
| `suggestionID`                                                                                                                  | *String*                                                                                                                        | :heavy_check_mark:                                                                                                              | The ID of the Suggestion.<br><br>A Suggestion ID is composed of a 2-4 letter prefix, followed by a dash and 4-15 digits.        | PROD-11829                                                                                                                      |

### Response

**[DeleteSuggestionResponse](../../models/operations/DeleteSuggestionResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 401, 403, 404, 406          | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getRelatedArticlesForSuggestion

## Overview
  The Get Related Articles for Suggestion API allows authenticated users to retrieve related articles for a Suggestion.

## Prerequisites
  * Enable the setting "My Suggestions" for the portal specified in the URL.
  * If the user is a customer, enable the setting "Allow Customer Access" for the portal.
  * The Suggestion specified in the URL must belong to the user.


### Example Usage

<!-- UsageSnippet language="java" operationID="getRelatedArticlesForSuggestion" method="get" path="/portals/{portalID}/suggestions/{suggestionID}/relatedArticles" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.components.ArticleResultAdditionalAttributes;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetRelatedArticlesForSuggestionResponse;
import java.lang.Exception;
import java.util.List;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetRelatedArticlesForSuggestionResponse res = sdk.portal().suggestion().getRelatedArticlesForSuggestion()
                .acceptLanguage(AcceptLanguage.EN_US)
                .articleResultAdditionalAttributes(List.of(
                    ArticleResultAdditionalAttributes.AVERAGE_RATING))
                .portalID("PROD-1000")
                .suggestionID("PROD-11829")
                .call();

        if (res.feedbackArticleForSuggestion().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Type                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Required                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Example                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `acceptLanguage`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | [AcceptLanguage](../../models/components/AcceptLanguage.md)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | :heavy_check_mark:                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | The Language locale accepted by the client (used for locale specific fields in resource representation and in error responses).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | en-US                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| `articleResultAdditionalAttributes`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | List\<[ArticleResultAdditionalAttributes](../../models/components/ArticleResultAdditionalAttributes.md)>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | :heavy_minus_sign:                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | The attributes of an Article to be returned *in addition to* the default list of attributes, listed below. Multiple additional attributes can be specified using a comma-separated list. Passing 'all' will return all attributes.<br/><br/>#### Default Attributes<br/>These Article attributes are always returned:<br/><br/>\| Name \| Description <br/>\| ---- \| -----------<br/>\| id \| The ID of the Article.<br/>\| name  \| The name of the Article.<br/>\| articleType \| The Article Type and its attributes.<br/>\| createdBy \| The ID, first name, middle name and last name of the user that created the Article.<br/>\| createdDate \| The date that the Article was created.<br/>\| hasAttachments \| True: The Article has one or more attachments.<br>False: The Article does not have any attachments.<br/>\| languageCode \| The language code of the Article language. <br/>\| modifiedBy \| The ID, first name, middle name and last name of the user that last modified the Article.<br/>\| modifiedDate \| The date that the Article was last modified on.<br/>\| link \| The link object, used to retrieve the details of the Article.<br/>\| versionId \| The ID of the Article version that is returned.<br/> |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `portalID`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | *String*                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | :heavy_check_mark:                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | The ID of the portal being accessed.<br><br>A portal ID is composed of a 2-4 letter prefix, followed by a dash and 4-15 digits.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | PROD-1000                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| `suggestionID`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | *String*                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | :heavy_check_mark:                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | The ID of the Suggestion.<br><br>A Suggestion ID is composed of a 2-4 letter prefix, followed by a dash and 4-15 digits.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | PROD-11829                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |

### Response

**[GetRelatedArticlesForSuggestionResponse](../../models/operations/GetRelatedArticlesForSuggestionResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getSuggestionComments

## Overview
  The Get Suggestion Comments API allows authenticated users to retrieve all comments for a Suggestion.

## Prerequisites
  * Enable the setting "My Suggestions" for the portal specified in the URL. 
  * If the user is a customer, enable the setting "Allow Customer Access" for the portal.
  * The Suggestion specified in the URL must belong to the user.


### Example Usage

<!-- UsageSnippet language="java" operationID="getSuggestionComments" method="get" path="/portals/{portalID}/suggestions/{suggestionID}/comments" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetSuggestionCommentsResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetSuggestionCommentsResponse res = sdk.portal().suggestion().getSuggestionComments()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .suggestionID("PROD-11829")
                .call();

        if (res.comments().isPresent()) {
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
| `suggestionID`                                                                                                                  | *String*                                                                                                                        | :heavy_check_mark:                                                                                                              | The ID of the Suggestion.<br><br>A Suggestion ID is composed of a 2-4 letter prefix, followed by a dash and 4-15 digits.        | PROD-11829                                                                                                                      |

### Response

**[GetSuggestionCommentsResponse](../../models/operations/GetSuggestionCommentsResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getSuggestionAttachments

## Overview
  The Get Suggestion Attachments API allows authenticated users to retrieve attachments for a Suggestion.

## Prerequisites
  * Enable the setting "My Suggestions" for the portal specified in the URL
  * If the user is a customer, enable the setting "Allow Customer Access" for the portal.
  * The Suggestion specified in the URL must belong to the user.


### Example Usage

<!-- UsageSnippet language="java" operationID="getSuggestionAttachments" method="get" path="/portals/{portalID}/suggestions/{suggestionID}/attachments" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetSuggestionAttachmentsResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetSuggestionAttachmentsResponse res = sdk.portal().suggestion().getSuggestionAttachments()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .suggestionID("PROD-11829")
                .call();

        if (res.attachments().isPresent()) {
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
| `suggestionID`                                                                                                                  | *String*                                                                                                                        | :heavy_check_mark:                                                                                                              | The ID of the Suggestion.<br><br>A Suggestion ID is composed of a 2-4 letter prefix, followed by a dash and 4-15 digits.        | PROD-11829                                                                                                                      |

### Response

**[GetSuggestionAttachmentsResponse](../../models/operations/GetSuggestionAttachmentsResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getSuggestionAttachmentById

## Overview
  The Get Suggestion Attachment by ID API allows authenticated users to get the details of an attachment that belongs to their own Suggestion. It also allows the download of attachment content. 

## Prerequisites
  * Enable the setting "My Suggestions" for the portal specified in the URL
  * If the user is a customer, enable the setting "Allow Customer Access" for the portal.
  * The Suggestion specified in the URL must belong to the user.


### Example Usage

<!-- UsageSnippet language="java" operationID="getSuggestionAttachmentById" method="get" path="/portals/{portalID}/suggestions/attachments/{attachmentID}" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.components.AttachmentAdditionalAttributes;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetSuggestionAttachmentByIdResponse;
import java.lang.Exception;
import java.util.List;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetSuggestionAttachmentByIdResponse res = sdk.portal().suggestion().getSuggestionAttachmentById()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .attachmentID("PROD-1000")
                .attachmentAdditionalAttributes(List.of(
                    AttachmentAdditionalAttributes.CONTENT_URL))
                .call();

        if (res.suggestionAttachment().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Type                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Required                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | Example                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `acceptLanguage`                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | [AcceptLanguage](../../models/components/AcceptLanguage.md)                                                                                                                                                                                                                                                                                                                                                                                                                        | :heavy_check_mark:                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | The Language locale accepted by the client (used for locale specific fields in resource representation and in error responses).                                                                                                                                                                                                                                                                                                                                                    | en-US                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| `portalID`                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | *String*                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | :heavy_check_mark:                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | The ID of the portal being accessed.<br><br>A portal ID is composed of a 2-4 letter prefix, followed by a dash and 4-15 digits.                                                                                                                                                                                                                                                                                                                                                    | PROD-1000                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `attachmentID`                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | *String*                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | :heavy_check_mark:                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | The ID of the attachment.<br><br>An attachment ID is composed of a 2-4 letter prefix, followed by a dash and 4-15 digits.                                                                                                                                                                                                                                                                                                                                                          | PROD-1000                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `attachmentAdditionalAttributes`                                                                                                                                                                                                                                                                                                                                                                                                                                                   | List\<[AttachmentAdditionalAttributes](../../models/components/AttachmentAdditionalAttributes.md)>                                                                                                                                                                                                                                                                                                                                                                                 | :heavy_minus_sign:                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | The attributes of an attachment to be returned, along with the default attachment details.<br/><br/>\| Attribute    \| Description                        \|<br/>\|--------------\|------------------------------------\|<br/>\| id         \| Unique identifier for the attachment \|<br/>\| fileName   \| Name of the file                   \|<br/>\| contentType\| Content type of the file              \|<br/>\| size       \| Size of the file in bytes          \|<br/>\| link       \| Link to the attachment             \| <br/> |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |

### Response

**[GetSuggestionAttachmentByIdResponse](../../models/operations/GetSuggestionAttachmentByIdResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |