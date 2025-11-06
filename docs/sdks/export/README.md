# Export
(*portal().export()*)

## Overview

### Available Operations

* [exportContent](#exportcontent) - Export Knowledge
* [exportStatus](#exportstatus) - Get Export Job Status

## exportContent

## Overview
   The Content Export API initiates a bulk export of the Knowledge Hub to a client-provided Amazon S3 bucket or SFTP server path.
   It returns a URL with a Job ID to enable tracking the status of this asynchronous operation.  
   Each export job can send multiple JSON files, depending on the total number of items to process. 
   More than one bulk export can take place, as needed, one per portal.

## Permission
  * Only a client application can invoke this API.   


### Example Usage

<!-- UsageSnippet language="java" operationID="exportContent" method="post" path="/content/export" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.ExportContentResponse;
import java.lang.Exception;
import java.util.List;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        KnowledgeExport req = KnowledgeExport.builder()
                .portalID("PROD-1000")
                .language(KnowledgeExportLanguage.builder()
                    .code(KnowledgeExportCode.EN_US)
                    .build())
                .resourceTypes(List.of(
                    ResourceType.ARTICLES))
                .dataDestination(DataDestination.builder()
                    .destinationType(DestinationType.AWSS3_BUCKET)
                    .path("s3://amzn-s3-demo-bucket/mydeptfolder")
                    .region("us-west-2")
                    .credentials(KnowledgeExportCredentials.builder()
                        .accessKey("s3-access-user")
                        .secretKey("s3-access-secret")
                        .build())
                    .build())
                .build();

        ExportContentResponse res = sdk.portal().export().exportContent()
                .request(req)
                .call();

        // handle response
    }
}
```

### Parameters

| Parameter                                                 | Type                                                      | Required                                                  | Description                                               |
| --------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------- |
| `request`                                                 | [KnowledgeExport](../../models/shared/KnowledgeExport.md) | :heavy_check_mark:                                        | The request object to use for the request.                |

### Response

**[ExportContentResponse](../../models/operations/ExportContentResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401                    | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## exportStatus

## Overview
   The Content Export Status API provides real-time status information to monitor job progress and check completion status.

## Status Values
- **Pending**: Job is pending start of processing
- **In Progress**: Job is actively processing content        
- **Completed**: Job finished successfully
- **Failed**: Job encountered errors and could not complete

## Response Information
- **Current Status**: Real-time job status
- **Progress Metrics**: Items processed, total items
- **Error Details**: Specific errors encountered during processing
- **Timing Information**: Start time, estimated completion, actual completion           

## Permission
  * Only a client application can invoke this API.  


### Example Usage

<!-- UsageSnippet language="java" operationID="exportStatus" method="get" path="/content/export/{jobID}/status" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.ExportStatusResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        ExportStatusResponse res = sdk.portal().export().exportStatus()
                .jobID("7A84B875-6F75-4C7B-B137-0632B62DB0BD")
                .call();

        if (res.exportStatus().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                                       | Type                                                                                            | Required                                                                                        | Description                                                                                     | Example                                                                                         |
| ----------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| `jobID`                                                                                         | *String*                                                                                        | :heavy_check_mark:                                                                              | **Example Usage:**<br/>```bash<br/>GET /content/export/7A84B875-6F75-4C7B-B137-0632B62DB0BD/status<br/>```<br/> | 7A84B875-6F75-4C7B-B137-0632B62DB0BD                                                            |

### Response

**[ExportStatusResponse](../../models/operations/ExportStatusResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404          | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |