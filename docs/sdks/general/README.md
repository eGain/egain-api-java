# General
(*portal().general()*)

## Overview

### Available Operations

* [getAllPortals](#getallportals) - Get All Portals
* [getMyPortals](#getmyportals) - Get All Portals Accessible To User
* [getPortalDetailsById](#getportaldetailsbyid) - Get Portal Details By ID
* [getTagCategoriesForInterestForPortal](#gettagcategoriesforinterestforportal) - Get Tag Categories for Interest for a Portal

## getAllPortals

## Overview
  The Get All Portals API returns all portals in a partition or department.


### Example Usage

<!-- UsageSnippet language="java" operationID="getAllPortals" method="get" path="/portals" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.components.LanguageQueryParameter;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetAllPortalsRequest;
import com.egain.sdk.models.operations.GetAllPortalsResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetAllPortalsRequest req = GetAllPortalsRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .departmentID("999")
                .language(LanguageQueryParameter.EN_US)
                .build();

        GetAllPortalsResponse res = sdk.portal().general().getAllPortals()
                .request(req)
                .call();

        if (res.portalResult().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                               | Type                                                                    | Required                                                                | Description                                                             |
| ----------------------------------------------------------------------- | ----------------------------------------------------------------------- | ----------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| `request`                                                               | [GetAllPortalsRequest](../../models/operations/GetAllPortalsRequest.md) | :heavy_check_mark:                                                      | The request object to use for the request.                              |

### Response

**[GetAllPortalsResponse](../../models/operations/GetAllPortalsResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getMyPortals

## Overview
  The Get All Portals Accessible to User API allows a user to fetch all portals accessible to user across all department.
  * If no access tags are specified for a portal, then any user can access the portal.
  * If access tags are specified for a portal, users with a user profile that allows access have access to the portal. For users with multiple user profiles, the user profile that allows access does not need to be the active user profile.
  * All the global users(partition) cannot be assigned user profiles; their access is limited to portals without access restrictions.
  * The only articles returned are associated to an Article type when the parameter, “Include in browse on portals” is set to "Yes".
  * When the "shortUrlTemplate" query parameter is provided, the API filters accessible portals according to the specified language and template name. Portal Short URL specific to to the "shortUrlTemplate" query parameter value is returned in the response.
  * When there is no short URL available for a specific language, the API returns a portal object with an empty "shortURL" field.


### Example Usage

<!-- UsageSnippet language="java" operationID="getMyPortals" method="get" path="/myportals" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.components.MandatoryLanguageQueryParameter;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetMyPortalsRequest;
import com.egain.sdk.models.operations.GetMyPortalsResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetMyPortalsRequest req = GetMyPortalsRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .language(MandatoryLanguageQueryParameter.EN_US)
                .department("service")
                .filterText("master")
                .shortUrlTemplate("silver")
                .build();

        GetMyPortalsResponse res = sdk.portal().general().getMyPortals()
                .request(req)
                .call();

        if (res.allAccessiblePortals().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                             | Type                                                                  | Required                                                              | Description                                                           |
| --------------------------------------------------------------------- | --------------------------------------------------------------------- | --------------------------------------------------------------------- | --------------------------------------------------------------------- |
| `request`                                                             | [GetMyPortalsRequest](../../models/operations/GetMyPortalsRequest.md) | :heavy_check_mark:                                                    | The request object to use for the request.                            |

### Response

**[GetMyPortalsResponse](../../models/operations/GetMyPortalsResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getPortalDetailsById

## Overview
  The Get Portal Details By ID API allows a user to fetch details of a portal by the ID.


### Example Usage

<!-- UsageSnippet language="java" operationID="getPortalDetailsById" method="get" path="/portals/{portalID}" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.components.LanguageQueryParameter;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetPortalDetailsByIdResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetPortalDetailsByIdResponse res = sdk.portal().general().getPortalDetailsById()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .language(LanguageQueryParameter.EN_US)
                .call();

        if (res.portal().isPresent()) {
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

**[GetPortalDetailsByIdResponse](../../models/operations/GetPortalDetailsByIdResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getTagCategoriesForInterestForPortal

## Overview
  The Get Tag Categories for Interest for a Portal API retrieves the Tag Categories for Interest configured for a portal.
  * Tag Categories are ordered in order of their addition in the "Tag Categories for Interest" in the Portal configuration.
  * Tags are ordered as per their order defined in their Tag Category.
  * Tag Groups are sorted by their name, in ascending order.


### Example Usage

<!-- UsageSnippet language="java" operationID="getTagCategoriesForInterestForPortal" method="get" path="/portals/{portalID}/tagcategoriesforinterest" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.components.MandatoryLanguageQueryParameter;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetTagCategoriesForInterestForPortalResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetTagCategoriesForInterestForPortalResponse res = sdk.portal().general().getTagCategoriesForInterestForPortal()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .language(MandatoryLanguageQueryParameter.EN_US)
                .call();

        if (res.tagCategoriesForInterest().isPresent()) {
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
| `language`                                                                                                                       | [MandatoryLanguageQueryParameter](../../models/components/MandatoryLanguageQueryParameter.md)                                    | :heavy_check_mark:                                                                                                               | The language used for fetching the details of a resource. Resources available in different languages may differ from each other. | en-US                                                                                                                            |

### Response

**[GetTagCategoriesForInterestForPortalResponse](../../models/operations/GetTagCategoriesForInterestForPortalResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |