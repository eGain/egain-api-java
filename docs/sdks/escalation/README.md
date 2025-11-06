# Escalation
(*portal().escalation()*)

## Overview

### Available Operations

* [startCustomerEscalation](#startcustomerescalation) - Start Customer Escalation
* [searchPriorToEscalation](#searchpriortoescalation) - Search Prior To Customer Escalation
* [completeCustomerEscalation](#completecustomerescalation) - Complete Customer Escalation
* [avertCustomerEscalation](#avertcustomerescalation) - Avert Customer Escalation

## startCustomerEscalation

## Overview
  The Start Escalation API is called to initiate an escalation and it must be called before any other escalation API. 
  An escalation occurs when a customer is referred to a higher level of support, such as chat or email, to address their issue. 
  This process is initiated when the initial support resources are insufficient by themselves. 

  Customer object is optional for authenticated customers. Values provided in request takes precedence over the values provided during customer registration.


### Example Usage

<!-- UsageSnippet language="java" operationID="startCustomerEscalation" method="post" path="/portals/{portalID}/escalate" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.StartCustomerEscalationResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        StartCustomerEscalationResponse res = sdk.portal().escalation().startCustomerEscalation()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .language(MandatoryLanguageQueryParameter.EN_US)
                .body(StartEscalationRequest.builder()
                    .subject("Issue with Product X")
                    .body("I am experiencing an issue with Product X. Here are the details...\n")
                    .channel(ChannelEnum.EMAIL)
                    .url("https://developerv3api.egain.cloud/system/templates/selfservice/oasis/help/customer/locale/en-us/portal/PROD-1002/escalate/search?q=help")
                    .build())
                .call();

        // handle response
    }
}
```

### Parameters

| Parameter                                                                                                                        | Type                                                                                                                             | Required                                                                                                                         | Description                                                                                                                      | Example                                                                                                                          |
| -------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| `acceptLanguage`                                                                                                                 | [AcceptLanguage](../../models/components/AcceptLanguage.md)                                                                      | :heavy_check_mark:                                                                                                               | The Language locale accepted by the client (used for locale specific fields in resource representation and in error responses).  | en-US                                                                                                                            |
| `portalID`                                                                                                                       | *String*                                                                                                                         | :heavy_check_mark:                                                                                                               | The ID of the portal being accessed.<br><br>A portal ID is composed of a 2-4 letter prefix, followed by a dash and 4-15 digits.  | PROD-1000                                                                                                                        |
| `language`                                                                                                                       | [MandatoryLanguageQueryParameter](../../models/components/MandatoryLanguageQueryParameter.md)                                    | :heavy_check_mark:                                                                                                               | The language used for fetching the details of a resource. Resources available in different languages may differ from each other. | en-US                                                                                                                            |
| `body`                                                                                                                           | [StartEscalationRequest](../../models/components/StartEscalationRequest.md)                                                      | :heavy_check_mark:                                                                                                               | N/A                                                                                                                              |                                                                                                                                  |

### Response

**[StartCustomerEscalationResponse](../../models/operations/StartCustomerEscalationResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 406          | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## searchPriorToEscalation

## Overview
  The Search Prior to Customer Escalation API performs search on the subject and description parameters, but filters out any articles that the customer has viewed in the current session.


### Example Usage

<!-- UsageSnippet language="java" operationID="searchPriorToEscalation" method="get" path="/portals/{portalID}/escalate/{escalationId}/search" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.components.OptionalArticleAttributes;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.SearchPriorToEscalationRequest;
import com.egain.sdk.models.operations.SearchPriorToEscalationResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        SearchPriorToEscalationRequest req = SearchPriorToEscalationRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1002")
                .escalationId("6e1309ae-aef5-468a-9ff7-8c1f82a7c831")
                .filterArticlesInTopicTree("PROD-1066")
                .additionalAttributes(OptionalArticleAttributes.ARTICLE_MACRO)
                .maxResults(5)
                .build();

        SearchPriorToEscalationResponse res = sdk.portal().escalation().searchPriorToEscalation()
                .request(req)
                .call();

        if (res.articleSearchResults().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                                   | Type                                                                                        | Required                                                                                    | Description                                                                                 |
| ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| `request`                                                                                   | [SearchPriorToEscalationRequest](../../models/operations/SearchPriorToEscalationRequest.md) | :heavy_check_mark:                                                                          | The request object to use for the request.                                                  |

### Response

**[SearchPriorToEscalationResponse](../../models/operations/SearchPriorToEscalationResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 406          | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## completeCustomerEscalation

## Overview
  The Complete Customer Escalation API allows customer to complete escalation and send an email to a customer service agent on behalf of the customer.

  After invoking the Complete Escalation or Avert Escalation APIs, the escalation ID associated with the escalation becomes invalid. 
  **Important:** Further attempts to use or reference the same escalation ID results in an error or failure.


### Example Usage

<!-- UsageSnippet language="java" operationID="completeCustomerEscalation" method="put" path="/portals/{portalID}/escalate/{escalationId}/complete" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.CompleteCustomerEscalationResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        CompleteCustomerEscalationResponse res = sdk.portal().escalation().completeCustomerEscalation()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .escalationId("5d2b0881-9ef2-4c96-a4c2-b530f1ac306f")
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
| `escalationId`                                                                                                                  | *String*                                                                                                                        | :heavy_check_mark:                                                                                                              | The ID (uuid) of the customer escalation.<br/>                                                                                  |                                                                                                                                 |

### Response

**[CompleteCustomerEscalationResponse](../../models/operations/CompleteCustomerEscalationResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 406          | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## avertCustomerEscalation

## Overview
  The Avert Customer Escalation API notifies the server that escalation to a live agent has been averted. Usually this means that the customer found the information they needed.

  After invoking the Complete Escalation or Avert Escalation APIs, the escalation ID associated with the escalation becomes invalid. 
  **Important:** Further attempts to use or reference the same escalation ID results in an error or failure.


### Example Usage

<!-- UsageSnippet language="java" operationID="avertCustomerEscalation" method="put" path="/portals/{portalID}/escalate/{escalationId}/avert" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.AvertCustomerEscalationResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        AvertCustomerEscalationResponse res = sdk.portal().escalation().avertCustomerEscalation()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .escalationId("858789ed-672c-43dc-af37-e6a46d534ffe")
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
| `escalationId`                                                                                                                  | *String*                                                                                                                        | :heavy_check_mark:                                                                                                              | The ID (uuid) of the customer escalation.<br/>                                                                                  |                                                                                                                                 |

### Response

**[AvertCustomerEscalationResponse](../../models/operations/AvertCustomerEscalationResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 406          | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |