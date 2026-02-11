# AiservicesPrompt
(*aiservices().prompt()*)

## Overview

### Available Operations

* [executePrompt](#executeprompt) - Execute a predefined prompt

## executePrompt

Execute a published and active prompt template from the AI console.

### Example Usage

<!-- UsageSnippet language="java" operationID="executePrompt" method="post" path="/promptmanager/execute/prompt/{promptId}" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.ExecutePrompt;
import com.egain.sdk.models.components.LanguageCodeRequestBody;
import com.egain.sdk.models.errors.BadRequestException;
import com.egain.sdk.models.operations.ExecutePromptResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws BadRequestException, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        ExecutePromptResponse res = sdk.aiservices().prompt().executePrompt()
                .promptId("<id>")
                .body(ExecutePrompt.builder()
                    .department("Service")
                    .languageCode(LanguageCodeRequestBody.EN_US)
                    .clientSessionId("123e4567-e89b-12d3-a456-426614174000")
                    .build())
                .call();

        if (res.executePromptResponse().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                                        | Type                                                                                             | Required                                                                                         | Description                                                                                      |
| ------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------ |
| `promptId`                                                                                       | *String*                                                                                         | :heavy_check_mark:                                                                               | ID of the prompt template from the AI console. Only published and active prompt IDs are allowed. |
| `body`                                                                                           | [Optional\<ExecutePrompt>](../../models/components/ExecutePrompt.md)                             | :heavy_minus_sign:                                                                               | N/A                                                                                              |
| `serverURL`                                                                                      | *String*                                                                                         | :heavy_minus_sign:                                                                               | An optional server URL to use.                                                                   |

### Response

**[ExecutePromptResponse](../../models/operations/ExecutePromptResponse.md)**

### Errors

| Error Type                        | Status Code                       | Content Type                      |
| --------------------------------- | --------------------------------- | --------------------------------- |
| models/errors/BadRequestException | 400                               | application/json                  |
| models/errors/APIException        | 4XX, 5XX                          | \*/\*                             |