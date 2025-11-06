# Userprofile
(*portal().userprofile()*)

## Overview

### Available Operations

* [getAllUserProfiles](#getalluserprofiles) - Get All User Profiles Assigned to User
* [selectUserProfile](#selectuserprofile) - Select User Profile

## getAllUserProfiles

## Overview
  The Get All User Profiles Assigned to User API retrieves all user profiles in the portal that are assigned to the user, displayed in ascending order of name.


### Example Usage

<!-- UsageSnippet language="java" operationID="getAllUserProfiles" method="get" path="/portals/{portalID}/userprofiles" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.components.LanguageQueryParameter;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetAllUserProfilesResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetAllUserProfilesResponse res = sdk.portal().userprofile().getAllUserProfiles()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .language(LanguageQueryParameter.EN_US)
                .call();

        if (res.userProfiles().isPresent()) {
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

**[GetAllUserProfilesResponse](../../models/operations/GetAllUserProfilesResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## selectUserProfile

## Overview
  The Select User Profile API allows a user to set the user profile for a portal.


### Example Usage

<!-- UsageSnippet language="java" operationID="selectUserProfile" method="put" path="/portals/{portalID}/userprofiles/{userProfileID}/select" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.SelectUserProfileResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        SelectUserProfileResponse res = sdk.portal().userprofile().selectUserProfile()
                .acceptLanguage(AcceptLanguage.EN_US)
                .portalID("PROD-1000")
                .userProfileID("PROD-3210")
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
| `userProfileID`                                                                                                                 | *String*                                                                                                                        | :heavy_check_mark:                                                                                                              | The ID of the user profile. <br/>                                                                                               | PROD-3210                                                                                                                       |

### Response

**[SelectUserProfileResponse](../../models/operations/SelectUserProfileResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |