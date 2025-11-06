# Userdetails
(*portal().userdetails()*)

## Overview

### Available Operations

* [getUserDetails](#getuserdetails) - Get User Details

## getUserDetails

## Overview
  The Get User Details API retrieves details of a knowledge user.


### Example Usage

<!-- UsageSnippet language="java" operationID="getUserDetails" method="get" path="/portals/user" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetUserDetailsResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetUserDetailsResponse res = sdk.portal().userdetails().getUserDetails()
                .acceptLanguage(AcceptLanguage.EN_US)
                .call();

        if (res.userDetails().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                                                                       | Type                                                                                                                            | Required                                                                                                                        | Description                                                                                                                     | Example                                                                                                                         |
| ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| `acceptLanguage`                                                                                                                | [AcceptLanguage](../../models/components/AcceptLanguage.md)                                                                     | :heavy_check_mark:                                                                                                              | The Language locale accepted by the client (used for locale specific fields in resource representation and in error responses). | en-US                                                                                                                           |

### Response

**[GetUserDetailsResponse](../../models/operations/GetUserDetailsResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 406          | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |