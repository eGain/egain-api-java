# Usermilestones
(*portal().usermilestones()*)

## Overview

### Available Operations

* [getUserMilestones](#getusermilestones) - Get User Milestones

## getUserMilestones

## Overview
The User Milestones API provides information about the milestones of an agent within the article workflow. 
This API helps track the progress of articles by grouping them into relevant milestones based on their current stage.

For example, an article might go through Knowledge Workflow Stages like Draft, Initial Review, Staging, Final Review and Publish. 
       Articles in the Draft and Initial Review stages are part of the "authoring milestone", while articles in the Staging and Final Review stages are part of the "staging milestone".


### Example Usage

<!-- UsageSnippet language="java" operationID="getUserMilestones" method="get" path="/portals/user/milestones" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetUserMilestonesResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetUserMilestonesResponse res = sdk.portal().usermilestones().getUserMilestones()
                .acceptLanguage(AcceptLanguage.EN_US)
                .call();

        if (res.milestones().isPresent()) {
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

**[GetUserMilestonesResponse](../../models/operations/GetUserMilestonesResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 406          | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |