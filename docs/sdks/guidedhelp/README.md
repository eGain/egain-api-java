# Guidedhelp
(*portal().guidedhelp()*)

## Overview

### Available Operations

* [getAllCasebasesReleases](#getallcasebasesreleases) - Get Available Casebases Releases
* [getCasebaseReleaseById](#getcasebasereleasebyid) - Get Details of a Casebase Release
* [getClusterByCasebaseReleaseId](#getclusterbycasebasereleaseid) - Get Cluster Details of a Casebase Release
* [getAllProfilesInPortal](#getallprofilesinportal) - Get All Profiles Available in Portal
* [startGHSearch](#startghsearch) - Start a Guided Help Search
* [stepGHSearch](#stepghsearch) - Perform a Step in a Guided Help Search
* [getAllCases](#getallcases) - Get All Cases of a Cluster in Release
* [getCaseById](#getcasebyid) - Get Details of a Case
* [acceptGHSolution](#acceptghsolution) - Accept Solution
* [rejectGHSolution](#rejectghsolution) - Reject Solution
* [createQuickpick](#createquickpick) - Create Quickpick
* [getAllQuickPicks](#getallquickpicks) - Get All Quickpicks for a Portal
* [restoreQuickpick](#restorequickpick) - Resume a Quickpick

## getAllCasebasesReleases

## Overview
  The Get Available Casebases Releases API retrieves all Casebase Releases associated with a portal.


### Example Usage

<!-- UsageSnippet language="java" operationID="getAllCasebasesReleases" method="get" path="/portals/{portalID}/gh/casebasereleases" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.components.LanguageQueryParameter;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetAllCasebasesReleasesRequest;
import com.egain.sdk.models.operations.GetAllCasebasesReleasesResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetAllCasebasesReleasesRequest req = GetAllCasebasesReleasesRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .language(LanguageQueryParameter.EN_US)
                .build();

        GetAllCasebasesReleasesResponse res = sdk.portal().guidedhelp().getAllCasebasesReleases()
                .request(req)
                .call();

        if (res.casebaseResult().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                                   | Type                                                                                        | Required                                                                                    | Description                                                                                 |
| ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| `request`                                                                                   | [GetAllCasebasesReleasesRequest](../../models/operations/GetAllCasebasesReleasesRequest.md) | :heavy_check_mark:                                                                          | The request object to use for the request.                                                  |

### Response

**[GetAllCasebasesReleasesResponse](../../models/operations/GetAllCasebasesReleasesResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getCasebaseReleaseById

## Overview
  The Get Details of a Casebase Release API retrieves details of Casebase Release.


### Example Usage

<!-- UsageSnippet language="java" operationID="getCasebaseReleaseById" method="get" path="/portals/{portalID}/gh/casebasereleases/{casebaseReleaseID}" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.components.LanguageQueryParameter;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetCasebaseReleaseByIdRequest;
import com.egain.sdk.models.operations.GetCasebaseReleaseByIdResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetCasebaseReleaseByIdRequest req = GetCasebaseReleaseByIdRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .casebaseReleaseID("202201000000002")
                .language(LanguageQueryParameter.EN_US)
                .build();

        GetCasebaseReleaseByIdResponse res = sdk.portal().guidedhelp().getCasebaseReleaseById()
                .request(req)
                .call();

        if (res.casebaseResult().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                                 | Type                                                                                      | Required                                                                                  | Description                                                                               |
| ----------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| `request`                                                                                 | [GetCasebaseReleaseByIdRequest](../../models/operations/GetCasebaseReleaseByIdRequest.md) | :heavy_check_mark:                                                                        | The request object to use for the request.                                                |

### Response

**[GetCasebaseReleaseByIdResponse](../../models/operations/GetCasebaseReleaseByIdResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getClusterByCasebaseReleaseId

## Overview
  The Get Cluster Details of a Casebase Release API retrieves cluster details of Casebase Release.


### Example Usage

<!-- UsageSnippet language="java" operationID="getClusterByCasebaseReleaseId" method="get" path="/portals/{portalID}/gh/casebasereleases/{casebaseReleaseID}/clusters" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.components.LanguageQueryParameter;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetClusterByCasebaseReleaseIdRequest;
import com.egain.sdk.models.operations.GetClusterByCasebaseReleaseIdResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetClusterByCasebaseReleaseIdRequest req = GetClusterByCasebaseReleaseIdRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .casebaseReleaseID("202201000000002")
                .language(LanguageQueryParameter.EN_US)
                .build();

        GetClusterByCasebaseReleaseIdResponse res = sdk.portal().guidedhelp().getClusterByCasebaseReleaseId()
                .request(req)
                .call();

        if (res.clusterResults().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                                               | Type                                                                                                    | Required                                                                                                | Description                                                                                             |
| ------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| `request`                                                                                               | [GetClusterByCasebaseReleaseIdRequest](../../models/operations/GetClusterByCasebaseReleaseIdRequest.md) | :heavy_check_mark:                                                                                      | The request object to use for the request.                                                              |

### Response

**[GetClusterByCasebaseReleaseIdResponse](../../models/operations/GetClusterByCasebaseReleaseIdResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getAllProfilesInPortal

## Overview
  The Get All Profiles Available in Portal API retrieves all Guided Help profiles associated with a portal.


### Example Usage

<!-- UsageSnippet language="java" operationID="getAllProfilesInPortal" method="get" path="/portals/{portalID}/gh/profiles" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetAllProfilesInPortalRequest;
import com.egain.sdk.models.operations.GetAllProfilesInPortalResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetAllProfilesInPortalRequest req = GetAllProfilesInPortalRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .filterCasebaseRelease("202201000000002")
                .build();

        GetAllProfilesInPortalResponse res = sdk.portal().guidedhelp().getAllProfilesInPortal()
                .request(req)
                .call();

        if (res.profileResult().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                                 | Type                                                                                      | Required                                                                                  | Description                                                                               |
| ----------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| `request`                                                                                 | [GetAllProfilesInPortalRequest](../../models/operations/GetAllProfilesInPortalRequest.md) | :heavy_check_mark:                                                                        | The request object to use for the request.                                                |

### Response

**[GetAllProfilesInPortalResponse](../../models/operations/GetAllProfilesInPortalResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## startGHSearch

## Overview
   The Start a Guided Help search can be used to start a search session in the Guided Help. 
   
   A Guided Help profile can also be specified while starting the session and it is used to filter the results of search.
   If this is not passed in the request, the default profile of the portal is used. Questions can also be answered at time of starting the search session. 
   
   A Guided Help search session can be started in following ways:
   * Launch session for a specific Casebase Release.
   * Launch session for a Casebase and specify the release type.
   * Use a Guided Help session Article and pass the required session parameters.

## Prerequisites
   * A Guided Help profile must be assigned to the user.
   * Casebase Release passed in the request must be associated with the portal passed in URI. 
   


### Example Usage

<!-- UsageSnippet language="java" operationID="startGHSearch" method="post" path="/portals/{portalID}/gh/search" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.StartGHSearchRequest;
import com.egain.sdk.models.operations.StartGHSearchResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        StartGHSearchRequest req = StartGHSearchRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .body(GHSearchRequest.builder()
                    .casebaseId("202201000000002")
                    .build())
                .language(LanguageQueryParameter.EN_US)
                .ghCustomAdditionalAttributes("question.custom.score")
                .build();

        StartGHSearchResponse res = sdk.portal().guidedhelp().startGHSearch()
                .request(req)
                .call();

        if (res.ghSearchResult().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                               | Type                                                                    | Required                                                                | Description                                                             |
| ----------------------------------------------------------------------- | ----------------------------------------------------------------------- | ----------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| `request`                                                               | [StartGHSearchRequest](../../models/operations/StartGHSearchRequest.md) | :heavy_check_mark:                                                      | The request object to use for the request.                              |

### Response

**[StartGHSearchResponse](../../models/operations/StartGHSearchResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 429                         | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## stepGHSearch

## Overview
   The Perform a Step in a Guided Help Search API can be used to answer one or more questions (perform a step) in Guided Help search.

## Prerequisites
   * A Guided Help session must be in progress before this API is invoked.


### Example Usage

<!-- UsageSnippet language="java" operationID="stepGHSearch" method="put" path="/portals/{portalID}/gh/search" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.StepGHSearchRequest;
import com.egain.sdk.models.operations.StepGHSearchResponse;
import java.lang.Exception;
import java.util.List;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        StepGHSearchRequest req = StepGHSearchRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .body(GHStepSearchRequest.builder()
                    .casebaseId("202201000000002")
                    .questions(List.of(
                        QuestionAndAnswer.builder()
                            .id("1000000322")
                            .answers(List.of(
                                Answer.builder()
                                    .id("1000000303")
                                    .build()))
                            .build(),
                        QuestionAndAnswer.builder()
                            .id("1000000327")
                            .answers(List.of(
                                Answer.builder()
                                    .id("1000000842")
                                    .build()))
                            .build(),
                        QuestionAndAnswer.builder()
                            .id("1000001006")
                            .answers(List.of(
                                Answer.builder()
                                    .id("1000001001")
                                    .build()))
                            .build()))
                    .useLiveRelease(true)
                    .build())
                .language(LanguageQueryParameter.EN_US)
                .ghCustomAdditionalAttributes("question.custom.score")
                .build();

        StepGHSearchResponse res = sdk.portal().guidedhelp().stepGHSearch()
                .request(req)
                .call();

        if (res.ghSearchResult().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                             | Type                                                                  | Required                                                              | Description                                                           |
| --------------------------------------------------------------------- | --------------------------------------------------------------------- | --------------------------------------------------------------------- | --------------------------------------------------------------------- |
| `request`                                                             | [StepGHSearchRequest](../../models/operations/StepGHSearchRequest.md) | :heavy_check_mark:                                                    | The request object to use for the request.                            |

### Response

**[StepGHSearchResponse](../../models/operations/StepGHSearchResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getAllCases

## Overview
  The Get All Cases of a Cluster in Release API retrieves all cases in cluster of a Casebase Release. A Case is a group of Questions, Articles, and control actions that work together to guide users through a series of questions and scenarios in a Guided Help session.  

## Prerequisites
  A Guided Help search session for the provided Casebase Release must be in progress before this API is invoked.


### Example Usage

<!-- UsageSnippet language="java" operationID="getAllCases" method="get" path="/portals/{portalID}/gh/casebasereleases/{casebaseReleaseID}/clusters/{clusterID}/cases" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.components.LanguageQueryParameter;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetAllCasesRequest;
import com.egain.sdk.models.operations.GetAllCasesResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetAllCasesRequest req = GetAllCasesRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .casebaseReleaseID("202201000000002")
                .clusterID("1000000402")
                .language(LanguageQueryParameter.EN_US)
                .build();

        GetAllCasesResponse res = sdk.portal().guidedhelp().getAllCases()
                .request(req)
                .call();

        if (res.caseListResults().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `request`                                                           | [GetAllCasesRequest](../../models/operations/GetAllCasesRequest.md) | :heavy_check_mark:                                                  | The request object to use for the request.                          |

### Response

**[GetAllCasesResponse](../../models/operations/GetAllCasesResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getCaseById

## Overview
  The Get Details of a Case API retrieves details of a case in a release.

## Prerequisites
  * A Guided Help search session for the provided Casebase Release must be in progress before this API is invoked.


### Example Usage

<!-- UsageSnippet language="java" operationID="getCaseById" method="get" path="/portals/{portalID}/gh/casebasereleases/{casebaseReleaseID}/cases/{caseID}" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetCaseByIdRequest;
import com.egain.sdk.models.operations.GetCaseByIdResponse;
import java.lang.Exception;
import java.util.List;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetCaseByIdRequest req = GetCaseByIdRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .casebaseReleaseID("202201000000002")
                .caseID("1000001085")
                .language(LanguageQueryParameter.EN_US)
                .caseAdditionalAttributes(List.of(
                    CaseAdditionalAttributes.ACTIONS))
                .build();

        GetCaseByIdResponse res = sdk.portal().guidedhelp().getCaseById()
                .request(req)
                .call();

        if (res.case_().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `request`                                                           | [GetCaseByIdRequest](../../models/operations/GetCaseByIdRequest.md) | :heavy_check_mark:                                                  | The request object to use for the request.                          |

### Response

**[GetCaseByIdResponse](../../models/operations/GetCaseByIdResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## acceptGHSolution

## Overview
  The Accept Solution API can be used to accept and add positive score to a solution of a Guided Help case.

## Prerequisites
   * A Guided Help search session must be started before invoking this API.


### Example Usage

<!-- UsageSnippet language="java" operationID="acceptGHSolution" method="post" path="/portals/{portalID}/gh/cases/{caseID}/accept" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.components.QuickpickRating;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.AcceptGHSolutionResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        AcceptGHSolutionResponse res = sdk.portal().guidedhelp().acceptGHSolution()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .caseID("1000001085")
                .body(QuickpickRating.builder()
                    .id("120000001201010")
                    .name("HPE Solution Article")
                    .profileId("120450120000001")
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
| `caseID`                                                                                                                        | *String*                                                                                                                        | :heavy_check_mark:                                                                                                              | The numerical ID of the Case for which details is to be fetched.                                                                | 1000001085                                                                                                                      |
| `body`                                                                                                                          | [QuickpickRating](../../models/components/QuickpickRating.md)                                                                   | :heavy_check_mark:                                                                                                              | N/A                                                                                                                             |                                                                                                                                 |

### Response

**[AcceptGHSolutionResponse](../../models/operations/AcceptGHSolutionResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## rejectGHSolution

## Overview
  The Reject Solution API can be used to reject and add negative score to a solution of a Guided Help case.

## Prerequisites
   * A Guided Help search session must be started before invoking this API.


### Example Usage

<!-- UsageSnippet language="java" operationID="rejectGHSolution" method="post" path="/portals/{portalID}/gh/cases/{caseID}/reject" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.components.QuickpickRating;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.RejectGHSolutionResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        RejectGHSolutionResponse res = sdk.portal().guidedhelp().rejectGHSolution()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .caseID("1000001085")
                .body(QuickpickRating.builder()
                    .id("123101210000102")
                    .name("HPE Solution Article")
                    .profileId("100101210000002")
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
| `caseID`                                                                                                                        | *String*                                                                                                                        | :heavy_check_mark:                                                                                                              | The numerical ID of the Case for which details is to be fetched.                                                                | 1000001085                                                                                                                      |
| `body`                                                                                                                          | [QuickpickRating](../../models/components/QuickpickRating.md)                                                                   | :heavy_check_mark:                                                                                                              | N/A                                                                                                                             |                                                                                                                                 |

### Response

**[RejectGHSolutionResponse](../../models/operations/RejectGHSolutionResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## createQuickpick

## Overview
   The Create Quickpick API can be used to create a new quickpick(bookmark) for current Guided Help search session.

  Note: If "linkToActivity" attribute is passed as true in request body then one of below must be passed in header
   * <code>XEgainTenantId</code>
   * <code>xEgainActivityId</code>
   * <code>XInteractionId</code>

## Prerequisites
   * A Guided Help search session must be in progress before this API is invoked.
   * QuickPick can only be created for a live release of a Casebase.
   


### Example Usage

<!-- UsageSnippet language="java" operationID="createQuickpick" method="post" path="/portals/{portalID}/gh/quickpicks" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.CreateQuickpickResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        CreateQuickpickResponse res = sdk.portal().guidedhelp().createQuickpick()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .language(LanguageQueryParameter.EN_US)
                .body(CreateQuickpick.builder()
                    .casebaseReleaseId("102312010000000")
                    .name("QuickPick82700245")
                    .comment("demo quickpick")
                    .linkToActivity(false)
                    .build())
                .call();

        // handle response
    }
}
```

### Parameters

| Parameter                                                                                                                                                                                                             | Type                                                                                                                                                                                                                  | Required                                                                                                                                                                                                              | Description                                                                                                                                                                                                           | Example                                                                                                                                                                                                               |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `acceptLanguage`                                                                                                                                                                                                      | [AcceptLanguage](../../models/components/AcceptLanguage.md)                                                                                                                                                           | :heavy_check_mark:                                                                                                                                                                                                    | The Language locale accepted by the client (used for locale specific fields in resource representation and in error responses).                                                                                       | en-US                                                                                                                                                                                                                 |
| `portalID`                                                                                                                                                                                                            | *String*                                                                                                                                                                                                              | :heavy_check_mark:                                                                                                                                                                                                    | The ID of the portal being accessed.<br><br>A portal ID is composed of a 2-4 letter prefix, followed by a dash and 4-15 digits.                                                                                       | PROD-1000                                                                                                                                                                                                             |
| `language`                                                                                                                                                                                                            | [Optional\<LanguageQueryParameter>](../../models/components/LanguageQueryParameter.md)                                                                                                                                | :heavy_minus_sign:                                                                                                                                                                                                    | The language that describes the details of a resource. Resources available in different languages may differ from each other.<li>If <code>lang</code> is not passed, then the portal's default language is used.</li> | en-US                                                                                                                                                                                                                 |
| `body`                                                                                                                                                                                                                | [CreateQuickpick](../../models/components/CreateQuickpick.md)                                                                                                                                                         | :heavy_check_mark:                                                                                                                                                                                                    | N/A                                                                                                                                                                                                                   |                                                                                                                                                                                                                       |

### Response

**[CreateQuickpickResponse](../../models/operations/CreateQuickpickResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getAllQuickPicks

## Overview
  The Get All Quickpicks for a Portal API retrieves details of quickpicks created for the Casebase.

## Conditions
   If "getLastSavedQuickPickForInteraction" query parameter is passed as "Yes" then one of below must be passed in request header.
   * X-ext-integration-id
   * X-egain-activity-id
   * X-ext-interaction-id
  Either casebaseID or getLastSavedQuickPickForInteraction must be passed in request.


### Example Usage

<!-- UsageSnippet language="java" operationID="getAllQuickPicks" method="get" path="/portals/{portalID}/gh/quickpicks" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.components.LanguageQueryParameter;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetAllQuickPicksRequest;
import com.egain.sdk.models.operations.GetAllQuickPicksResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetAllQuickPicksRequest req = GetAllQuickPicksRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .casebaseReleaseID("202201000000002")
                .portalID("PROD-1000")
                .language(LanguageQueryParameter.EN_US)
                .build();

        GetAllQuickPicksResponse res = sdk.portal().guidedhelp().getAllQuickPicks()
                .request(req)
                .call();

        if (res.quickpickResults().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                     | Type                                                                          | Required                                                                      | Description                                                                   |
| ----------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| `request`                                                                     | [GetAllQuickPicksRequest](../../models/operations/GetAllQuickPicksRequest.md) | :heavy_check_mark:                                                            | The request object to use for the request.                                    |

### Response

**[GetAllQuickPicksResponse](../../models/operations/GetAllQuickPicksResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## restoreQuickpick

## Overview
  The Resume a Quickpick API can be used to restore (resume) a specific quickpick.

## Prerequisites
   * A Guided Help session for the Casebase must be started before invoking this API.


### Example Usage

<!-- UsageSnippet language="java" operationID="restoreQuickpick" method="post" path="/portals/{portalID}/gh/quickpicks/{quickpickID}" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.components.LanguageQueryParameter;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.RestoreQuickpickRequest;
import com.egain.sdk.models.operations.RestoreQuickpickResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        RestoreQuickpickRequest req = RestoreQuickpickRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .quickpickID("11")
                .language(LanguageQueryParameter.EN_US)
                .ghCustomAdditionalAttributes("question.custom.score")
                .build();

        RestoreQuickpickResponse res = sdk.portal().guidedhelp().restoreQuickpick()
                .request(req)
                .call();

        if (res.ghSearchResult().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                     | Type                                                                          | Required                                                                      | Description                                                                   |
| ----------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| `request`                                                                     | [RestoreQuickpickRequest](../../models/operations/RestoreQuickpickRequest.md) | :heavy_check_mark:                                                            | The request object to use for the request.                                    |

### Response

**[RestoreQuickpickResponse](../../models/operations/RestoreQuickpickResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |