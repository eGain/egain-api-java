# Attachment
(*portal().attachment()*)

## Overview

### Available Operations

* [createSignedURL](#createsignedurl) - Generate Signed URL to Upload API
* [uploadAttachment](#uploadattachment) - Upload Attachment

## createSignedURL

## Overview
   The Generate Signed URL to Upload API produces a signature that is used to upload an attachment. 


### Example Usage

<!-- UsageSnippet language="java" operationID="createSignedURL" method="post" path="/attachment/preupload" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.components.AttachmentUpload;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.CreateSignedURLResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        CreateSignedURLResponse res = sdk.portal().attachment().createSignedURL()
                .acceptLanguage(AcceptLanguage.EN_US)
                .body(AttachmentUpload.builder()
                    .name("article.png")
                    .size(11889L)
                    .build())
                .call();

        if (res.attachment().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                                                                       | Type                                                                                                                            | Required                                                                                                                        | Description                                                                                                                     | Example                                                                                                                         |
| ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| `acceptLanguage`                                                                                                                | [AcceptLanguage](../../models/components/AcceptLanguage.md)                                                                     | :heavy_check_mark:                                                                                                              | The Language locale accepted by the client (used for locale specific fields in resource representation and in error responses). | en-US                                                                                                                           |
| `body`                                                                                                                          | [AttachmentUpload](../../models/components/AttachmentUpload.md)                                                                 | :heavy_check_mark:                                                                                                              | N/A                                                                                                                             |                                                                                                                                 |

### Response

**[CreateSignedURLResponse](../../models/operations/CreateSignedURLResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## uploadAttachment

## Overview
   The Upload Attachment API uses the signed URL produced by the Generate Signed URL to Upload API to upload an attachment. 
   The Make a Suggestion API uses this API to upload attachments.


### Example Usage

<!-- UsageSnippet language="java" operationID="uploadAttachment" method="post" path="/attachment/upload" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.AcceptLanguage;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.UploadAttachmentResponse;
import com.egain.sdk.utils.Blob;
import java.lang.Exception;
import java.nio.file.Paths;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        UploadAttachmentResponse res = sdk.portal().attachment().uploadAttachment()
                .acceptLanguage(AcceptLanguage.EN_US)
                .signature("<value>")
                .body(Blob.from(Paths.get("example.file")))
                .call();

    }
}
```

### Parameters

| Parameter                                                                                                                       | Type                                                                                                                            | Required                                                                                                                        | Description                                                                                                                     | Example                                                                                                                         |
| ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| `acceptLanguage`                                                                                                                | [AcceptLanguage](../../models/components/AcceptLanguage.md)                                                                     | :heavy_check_mark:                                                                                                              | The Language locale accepted by the client (used for locale specific fields in resource representation and in error responses). | en-US                                                                                                                           |
| `signature`                                                                                                                     | *String*                                                                                                                        | :heavy_check_mark:                                                                                                              | Signature data to upload attachment.                                                                                            |                                                                                                                                 |
| `body`                                                                                                                          | [Blob](../../../src/main/java/com/egain/sdk/utils/Blob.java)                                                                    | :heavy_check_mark:                                                                                                              | N/A                                                                                                                             |                                                                                                                                 |

### Response

**[UploadAttachmentResponse](../../models/operations/UploadAttachmentResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400, 401, 403, 404, 406     | application/json            |
| models/errors/WSErrorCommon | 500                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |