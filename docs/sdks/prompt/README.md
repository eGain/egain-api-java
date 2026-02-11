# Prompt
(*prompt()*)

## Overview

APIs for AssistGPT

### Available Operations

* [getPromptTemplates](#getprompttemplates) - Get Prompt Templates
* [getPromptTemplateById](#getprompttemplatebyid) - Get Prompt Template By ID

## getPromptTemplates

Retrieve prompt templates filtered by department and language.

### Example Usage

<!-- UsageSnippet language="java" operationID="getPromptTemplates" method="get" path="/prompts/prompttemplates" -->
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

        GetPromptTemplatesRequest req = GetPromptTemplatesRequest.builder()
                .acceptLanguage(AcceptLanguage.EN_US)
                .department("Service")
                .languageCode(GetPromptTemplatesLanguageCode.EN_US)
                .name("Generate email")
                .useFor(UseFor.KNOWLEDGE)
                .build();

        GetPromptTemplatesResponse res = sdk.prompt().getPromptTemplates()
                .request(req)
                .call();

        if (res.object().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                         | Type                                                                              | Required                                                                          | Description                                                                       |
| --------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| `request`                                                                         | [GetPromptTemplatesRequest](../../models/operations/GetPromptTemplatesRequest.md) | :heavy_check_mark:                                                                | The request object to use for the request.                                        |
| `serverURL`                                                                       | *String*                                                                          | :heavy_minus_sign:                                                                | An optional server URL to use.                                                    |

### Response

**[GetPromptTemplatesResponse](../../models/operations/GetPromptTemplatesResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getPromptTemplateById

Retrieve a prompt template filtered by department and language.

### Example Usage

<!-- UsageSnippet language="java" operationID="getPromptTemplateById" method="get" path="/prompts/prompttemplates/{promptID}" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetPromptTemplateByIdLanguageCode;
import com.egain.sdk.models.operations.GetPromptTemplateByIdResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetPromptTemplateByIdResponse res = sdk.prompt().getPromptTemplateById()
                .acceptLanguage(AcceptLanguage.EN_US)
                .department("Service")
                .languageCode(GetPromptTemplateByIdLanguageCode.EN_US)
                .promptID("PROD-4582")
                .call();

        if (res.promptTemplate().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                                                                       | Type                                                                                                                            | Required                                                                                                                        | Description                                                                                                                     | Example                                                                                                                         |
| ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| `acceptLanguage`                                                                                                                | [AcceptLanguage](../../models/components/AcceptLanguage.md)                                                                     | :heavy_check_mark:                                                                                                              | The Language locale accepted by the client (used for locale specific fields in resource representation and in error responses). | en-US                                                                                                                           |
| `department`                                                                                                                    | *String*                                                                                                                        | :heavy_check_mark:                                                                                                              | Name of the department. Must be a valid department name.                                                                        | Service                                                                                                                         |
| `languageCode`                                                                                                                  | [GetPromptTemplateByIdLanguageCode](../../models/operations/GetPromptTemplateByIdLanguageCode.md)                               | :heavy_check_mark:                                                                                                              | The language used while writing the prompt templates.                                                                           | en-US                                                                                                                           |
| `promptID`                                                                                                                      | *String*                                                                                                                        | :heavy_check_mark:                                                                                                              | The promptID of the prompt template you want to fetch details for.                                                              | PROD-4582                                                                                                                       |
| `serverURL`                                                                                                                     | *String*                                                                                                                        | :heavy_minus_sign:                                                                                                              | An optional server URL to use.                                                                                                  | http://localhost:8080                                                                                                           |

### Response

**[GetPromptTemplateByIdResponse](../../models/operations/GetPromptTemplateByIdResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |