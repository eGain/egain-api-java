# Import
(*content().import_()*)

## Overview

### Available Operations

* [createImportJob](#createimportjob) - Create Import Job
* [getValidationHooks](#getvalidationhooks) - Get validation hooks
* [createValidationHook](#createvalidationhook) - Create validation hook
* [getValidationHookVersions](#getvalidationhookversions) - Get all versions for a validation hook
* [createValidationHookVersion](#createvalidationhookversion) - Update validation hook version
* [getValidationHookVersion](#getvalidationhookversion) - Get validation hook version details
* [getImportStatus](#getimportstatus) - Get Job Status
* [createImportValidationJob](#createimportvalidationjob) - Create Validation Job
* [cancelImport](#cancelimport) - Cancel Job

## createImportJob

# Import Content

## Overview
This API initiates a bulk content import operation from Data Sources. It creates an asynchronous import job that processes content in the background, allowing you to import large volumes of content without blocking your application.

## Pre-requisties
1. Content in Data Source needs to be in this format: [Guide to Data Import Format](https://apidev.egain.com/developer-portal/guides/ingestion/data-import-format-guide/)

## How It Works
1. **Job Creation**: The API creates an import job and returns a unique job ID
2. **Content Processing**: Content is processed asynchronously in the background
3. **Status Monitoring**: Use the job ID to monitor progress via the Status API
4. **Completion**: Job completes when all content is processed or errors occur

**Note:** After a successful import, please allow for a brief delay before the content is fully available for use. The system's search service synchronizes all new and updated content every 30 minutes.

## Supported Operations
- **Import**: Add new content to the knowledge base
- **Update**: Modify existing content

## Data Source Types
- AWS S3 bucket
- Shared file path

## Best Practices
- **Scheduling**: Use scheduleTime for off-peak imports to minimize system impact. Please note that jobs can only be scheduled for a maximum of 7 days from the current date and time.
- **Monitoring**: Regularly check job status and logs for any issues
- **Error Handling**: Review failed items and retry with corrections

## Job Timing Controls
- **scheduleTime.stopDate**: Defines a specific date time to cease operations regardless of progress (e.g., "Stop exactly at 5:00 PM").


### Example Usage

<!-- UsageSnippet language="java" operationID="createImportJob" method="post" path="/import/content" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.errors.SchemasWSErrorCommon;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.CreateImportJobResponse;
import java.lang.Exception;
import java.time.OffsetDateTime;

public class Application {

    public static void main(String[] args) throws SchemasWSErrorCommon, WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        ImportContent req = ImportContent.builder()
                .dataSource(ImportContentDataSource.builder()
                    .type(ImportContentType.AWSS3_BUCKET)
                    .path("s3://mybucket/myfolder/")
                    .region("us-east-1")
                    .credentials(DataSourceCredentials.builder()
                        .accessKeyId("AKIAIOSFODNN7EXAMPLE")
                        .secretAccessKey("wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY")
                        .build())
                    .build())
                .operation(Operation.IMPORT)
                .scheduleTime(ScheduleTime.builder()
                    .date(OffsetDateTime.parse("2024-03-01T10:00:00.000Z"))
                    .stopDate(OffsetDateTime.parse("2024-03-05T10:00:00.000Z"))
                    .build())
                .build();

        CreateImportJobResponse res = sdk.content().import_().createImportJob()
                .request(req)
                .call();

        // handle response
    }
}
```

### Parameters

| Parameter                                             | Type                                                  | Required                                              | Description                                           |
| ----------------------------------------------------- | ----------------------------------------------------- | ----------------------------------------------------- | ----------------------------------------------------- |
| `request`                                             | [ImportContent](../../models/shared/ImportContent.md) | :heavy_check_mark:                                    | The request object to use for the request.            |
| `serverURL`                                           | *String*                                              | :heavy_minus_sign:                                    | An optional server URL to use.                        |

### Response

**[CreateImportJobResponse](../../models/operations/CreateImportJobResponse.md)**

### Errors

| Error Type                         | Status Code                        | Content Type                       |
| ---------------------------------- | ---------------------------------- | ---------------------------------- |
| models/errors/SchemasWSErrorCommon | 406                                | application/json                   |
| models/errors/WSErrorCommon        | 400, 401, 403, 412                 | application/json                   |
| models/errors/WSErrorCommon        | 500                                | application/json                   |
| models/errors/APIException         | 4XX, 5XX                           | \*/\*                              |

## getValidationHooks

Retrieve all validation hooks configured for the current environment. Only the current version of each hook is returned.


### Example Usage

<!-- UsageSnippet language="java" operationID="getValidationHooks" method="get" path="/import/config/hooks" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetValidationHooksResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetValidationHooksResponse res = sdk.content().import_().getValidationHooks()
                .call();

        if (res.hooks().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                            | Type                                                                 | Required                                                             | Description                                                          |
| -------------------------------------------------------------------- | -------------------------------------------------------------------- | -------------------------------------------------------------------- | -------------------------------------------------------------------- |
| `type`                                                               | [Optional\<HookTypeParam>](../../models/components/HookTypeParam.md) | :heavy_minus_sign:                                                   | Filter by hook type.                                                 |
| `serverURL`                                                          | *String*                                                             | :heavy_minus_sign:                                                   | An optional server URL to use.                                       |

### Response

**[GetValidationHooksResponse](../../models/operations/GetValidationHooksResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 401                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## createValidationHook

# Create Validation Hook

## Overview
This API allows you to create custom JavaScript-based validation hooks that execute during the bulk content ingestion process. Validation hooks enable organizations to enforce specific business rules, verify metadata compliance, and perform complex data integrity checks that go beyond standard system validation.

## Usage Requirements
To ensure successful hook creation and execution, please adhere to the following:
- **Content Encoding**: The `fileObject.file.content` property must be a **Base64 encoded** string of your JavaScript logic.
- **File Naming**: The filename should use the `.js` extension.
- **Logic Return**: Your script must return a `result` object containing a boolean `success` property.
- **Size Limit**: The encoded content must not exceed 1.5MB.

## Hook Types
- **Pre-Validation Hooks (`import_pre_validation_hook`)**: These execute **before** the standard system validation. They are ideal for enforcing custom business rules, such as ensuring all articles contain specific metadata.
- **Post-Validation Hooks (`import_post_validation_hook`)**: These execute **after** the standard system validation. They have access to the `validationResults` from the system check, allowing you to block an import based on the severity of errors found.

## Execution Environment
Hooks run in a secure, sandboxed JavaScript environment (ES5/ES6). 
- **Prohibited**: File system access (`fs`), network access (HTTP requests), and module loading (`require`).
- **Supported**: Standard JavaScript logic, `console.log()` for debugging, and a specialized `helpers` utility library for safe data access.

## Implementation Examples

### Example: Pre-Validation Logic
This example demonstrates checking that every article contains a specific metadata field.
```javascript
// Initialize result
var result = { success: true };

// Verify data exists
if (helpers.hasField(data, 'articles') && helpers.isNotEmpty(data.articles)) {
  
  var invalidArticles = [];
  
  for (var i = 0; i < data.articles.length; i++) {
    var article = data.articles[i];
    
    if (!helpers.hasField(article, 'name')) {
      invalidArticles.push({ index: i, issue: "Missing name" });
      continue;
    }

    // Custom Rule: Check if 'description' exists in metadata
    if (!helpers.hasField(article, 'metadata') || 
        !helpers.hasField(article.metadata, 'description') || 
        helpers.isEmpty(article.metadata.description)) {
          
      invalidArticles.push({ 
        name: article.name, 
        issue: "Missing required description metadata" 
      });
    }
  }
  
  if (invalidArticles.length > 0) {
    result = {
      success: false,
      error: 'Articles failed custom metadata validation',
      details: { count: invalidArticles.length, errors: invalidArticles }
    };
  }
}
result;
```

### Example: Post-Validation Logic
This example checks the standard validation results. If there are standard errors, it logs them and fails the job explicitly.
```javascript
var result = { success: true };

// Check if standard validation found errors
if (helpers.hasField(validationResults, 'errors') && validationResults.errors.length > 0) {
  
  var errorCount = validationResults.errors.length;
  console.log('Standard validation found ' + errorCount + ' errors.');

  var errorTypes = {};
  validationResults.errors.forEach(function(err) {
    var type = err.type || 'unknown';
    errorTypes[type] = (errorTypes[type] || 0) + 1;
  });

  result = {
    success: false,
    error: 'Standard validation failed with ' + errorCount + ' errors.',
    details: {
      summary: errorTypes,
      firstError: validationResults.errors[0].message
    }
  };
}
result;
```

## Further Documentation
For more detailed context on available objects (`data`, `metadata`, `helpers`) and best practices, refer to the [Validation Hook Guide](https://apidev.egain.com/developer-portal/guides/ingestion/validation-hook-guide/#example-pre-validation-logic).


### Example Usage

<!-- UsageSnippet language="java" operationID="createValidationHook" method="post" path="/import/config/hooks" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.CreateValidationHookResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        Hook req = Hook.builder()
                .type(HookType.IMPORT_POST_VALIDATION_HOOK)
                .fileObject(FileObject.builder()
                    .file(File.builder()
                        .name("check_dept_tags.js")
                        .content("dmFyIGl0ZW0gPSBjb250ZXh0Lml0ZW07")
                        .build())
                    .build())
                .build();

        CreateValidationHookResponse res = sdk.content().import_().createValidationHook()
                .request(req)
                .call();

        // handle response
    }
}
```

### Parameters

| Parameter                                  | Type                                       | Required                                   | Description                                |
| ------------------------------------------ | ------------------------------------------ | ------------------------------------------ | ------------------------------------------ |
| `request`                                  | [Hook](../../models/shared/Hook.md)        | :heavy_check_mark:                         | The request object to use for the request. |
| `serverURL`                                | *String*                                   | :heavy_minus_sign:                         | An optional server URL to use.             |

### Response

**[CreateValidationHookResponse](../../models/operations/CreateValidationHookResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 400                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getValidationHookVersions

Retrieve a history of all versions uploaded for a specific validation hook.

### Example Usage

<!-- UsageSnippet language="java" operationID="getValidationHookVersions" method="get" path="/import/config/hooks/{hookID}/version" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetValidationHookVersionsResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetValidationHookVersionsResponse res = sdk.content().import_().getValidationHookVersions()
                .hookID(923549L)
                .call();

        if (res.fileObjects().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                        | Type                             | Required                         | Description                      |
| -------------------------------- | -------------------------------- | -------------------------------- | -------------------------------- |
| `hookID`                         | *long*                           | :heavy_check_mark:               | Integer ID of the Hook resource. |
| `serverURL`                      | *String*                         | :heavy_minus_sign:               | An optional server URL to use.   |

### Response

**[GetValidationHookVersionsResponse](../../models/operations/GetValidationHookVersionsResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 404                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## createValidationHookVersion

Upload a new version of the JavaScript logic for an existing hook.

### Example Usage

<!-- UsageSnippet language="java" operationID="createValidationHookVersion" method="post" path="/import/config/hooks/{hookID}/version" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.FileObject;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.CreateValidationHookVersionResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        CreateValidationHookVersionResponse res = sdk.content().import_().createValidationHookVersion()
                .hookID(410019L)
                .body(FileObject.builder()
                    .build())
                .call();

        // handle response
    }
}
```

### Parameters

| Parameter                                           | Type                                                | Required                                            | Description                                         |
| --------------------------------------------------- | --------------------------------------------------- | --------------------------------------------------- | --------------------------------------------------- |
| `hookID`                                            | *long*                                              | :heavy_check_mark:                                  | Integer ID of the Hook resource.                    |
| `body`                                              | [FileObject](../../models/components/FileObject.md) | :heavy_check_mark:                                  | N/A                                                 |
| `serverURL`                                         | *String*                                            | :heavy_minus_sign:                                  | An optional server URL to use.                      |

### Response

**[CreateValidationHookVersionResponse](../../models/operations/CreateValidationHookVersionResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 404                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getValidationHookVersion

Get details and content URL for a specific version of a hook.

### Example Usage

<!-- UsageSnippet language="java" operationID="getValidationHookVersion" method="get" path="/import/config/hooks/{hookID}/version/{versionID}" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetValidationHookVersionResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetValidationHookVersionResponse res = sdk.content().import_().getValidationHookVersion()
                .hookID(276494L)
                .versionID(148818L)
                .call();

        if (res.fileObject().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                  | Type                                                       | Required                                                   | Description                                                |
| ---------------------------------------------------------- | ---------------------------------------------------------- | ---------------------------------------------------------- | ---------------------------------------------------------- |
| `hookID`                                                   | *long*                                                     | :heavy_check_mark:                                         | Integer ID of the Hook resource.                           |
| `versionID`                                                | *long*                                                     | :heavy_check_mark:                                         | The sequential version number of the hook (e.g., 1, 2, 3). |
| `serverURL`                                                | *String*                                                   | :heavy_minus_sign:                                         | An optional server URL to use.                             |

### Response

**[GetValidationHookVersionResponse](../../models/operations/GetValidationHookVersionResponse.md)**

### Errors

| Error Type                  | Status Code                 | Content Type                |
| --------------------------- | --------------------------- | --------------------------- |
| models/errors/WSErrorCommon | 404                         | application/json            |
| models/errors/APIException  | 4XX, 5XX                    | \*/\*                       |

## getImportStatus

# Get Import Job Status

## Overview
This API provides real-time status information for content import and validation operations. Use this endpoint to monitor job progress, check completion status, and access detailed logs and error information.

## Status Values
- **Scheduled**: Job is queued and waiting for scheduled execution time
- **In Progress**: Job is actively processing content
- **Completed**: Job finished successfully
- **Failed**: Job encountered errors and could not complete
- **Cancelled**: Job was manually cancelled by user

## Response Information
- **Current Status**: Real-time job status
- **Progress Metrics**: Items processed, total items, completion percentage
- **Log Files**: Location of detailed operation logs
- **Error Details**: Specific errors encountered during processing
- **Timing Information**: Start time, estimated completion, actual completion

## Log File Access
Log files contain detailed information about:
- Content processing steps
- Validation results
- Error details with context


### Example Usage

<!-- UsageSnippet language="java" operationID="getImportStatus" method="get" path="/import/content/{job_id}/status" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.errors.SchemasWSErrorCommon;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.GetImportStatusResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, SchemasWSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        GetImportStatusResponse res = sdk.content().import_().getImportStatus()
                .jobId("7A84B875-6F75-4C7B-B137-0632B62DB0BD")
                .call();

        if (res.importStatus().isPresent()) {
            // handle response
        }
    }
}
```

### Parameters

| Parameter                                                                                                                                                                                                                                                                                                                      | Type                                                                                                                                                                                                                                                                                                                           | Required                                                                                                                                                                                                                                                                                                                       | Description                                                                                                                                                                                                                                                                                                                    | Example                                                                                                                                                                                                                                                                                                                        |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `jobId`                                                                                                                                                                                                                                                                                                                        | *String*                                                                                                                                                                                                                                                                                                                       | :heavy_check_mark:                                                                                                                                                                                                                                                                                                             | **Job ID Parameter**<br/><br/>The unique identifier for the import or validation job. This ID was returned when the job was created via the Import or Validate API.<br/><br/>**Format:** UUID v4 (e.g., 7A84B875-6F75-4C7B-B137-0632B62DB0BD)<br/><br/>**Example Usage:**<br/>```bash<br/>GET /import/content/7A84B875-6F75-4C7B-B137-0632B62DB0BD/status<br/>```<br/> | 7A84B875-6F75-4C7B-B137-0632B62DB0BD                                                                                                                                                                                                                                                                                           |
| `serverURL`                                                                                                                                                                                                                                                                                                                    | *String*                                                                                                                                                                                                                                                                                                                       | :heavy_minus_sign:                                                                                                                                                                                                                                                                                                             | An optional server URL to use.                                                                                                                                                                                                                                                                                                 | http://localhost:8080                                                                                                                                                                                                                                                                                                          |

### Response

**[GetImportStatusResponse](../../models/operations/GetImportStatusResponse.md)**

### Errors

| Error Type                         | Status Code                        | Content Type                       |
| ---------------------------------- | ---------------------------------- | ---------------------------------- |
| models/errors/WSErrorCommon        | 400, 401, 403, 404                 | application/json                   |
| models/errors/SchemasWSErrorCommon | 406                                | application/json                   |
| models/errors/WSErrorCommon        | 500                                | application/json                   |
| models/errors/APIException         | 4XX, 5XX                           | \*/\*                              |

## createImportValidationJob

# Validate Import Content

## Overview
This API enables users to validate content structure, format, and compliance before importing it into the production knowledge base. Validation is a non-destructive operation that checks content without making any changes to your existing data.

## What Validation Checks
- **Content Structure**: Verifies required fields and data types
- **Format Compliance**: Ensures content meets platform requirements
- **Language Support**: Validates content against supported languages
- **Metadata Mapping**: Checks field mappings and transformations
- **Business Rules**: Validates against department-specific rules

## Validation Benefits
- **Risk Mitigation**: Identify issues before affecting production data
- **Quality Assurance**: Ensure content meets organizational standards
- **Cost Savings**: Avoid failed imports that waste processing time
- **Compliance**: Meet regulatory and internal content requirements

## Validation Process
1. **Content Analysis**: System analyzes content structure and format
2. **Rule Validation**: Applies business rules and validation logic
3. **Quality Assessment**: Evaluates content quality and completeness
4. **Report Generation**: Creates detailed validation report
5. **Issue Categorization**: Classifies issues by severity and type

## Common Validation Issues
- **Missing Required Fields**: Title, description, category, etc.
- **Invalid Data Types**: Incorrect field formats (dates, numbers, etc.)
- **Language Mismatches**: Content language not supported by department

## Best Practices
- **Always Validate First**: Run validation before any import operation
- **Review Reports**: Carefully examine validation results and warnings
- **Fix Issues**: Address validation errors before proceeding with import
- **Test Small Batches**: Validate with small content samples first
- **Iterate**: Use validation feedback to improve content quality


### Example Usage

<!-- UsageSnippet language="java" operationID="createImportValidationJob" method="post" path="/import/content/validate" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.errors.SchemasWSErrorCommon;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.CreateImportValidationJobResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws SchemasWSErrorCommon, WSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        ValidateImportContent req = ValidateImportContent.builder()
                .dataSource(ValidateImportContentDataSource.builder()
                    .type(ValidateImportContentType.AWSS3_BUCKET)
                    .path("s3://mybucket/myfolder/")
                    .region("us-east-1")
                    .credentials(DataSourceCredentials.builder()
                        .accessKeyId("AKIAIOSFODNN7EXAMPLE")
                        .secretAccessKey("wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY")
                        .build())
                    .build())
                .build();

        CreateImportValidationJobResponse res = sdk.content().import_().createImportValidationJob()
                .request(req)
                .call();

        // handle response
    }
}
```

### Parameters

| Parameter                                                             | Type                                                                  | Required                                                              | Description                                                           |
| --------------------------------------------------------------------- | --------------------------------------------------------------------- | --------------------------------------------------------------------- | --------------------------------------------------------------------- |
| `request`                                                             | [ValidateImportContent](../../models/shared/ValidateImportContent.md) | :heavy_check_mark:                                                    | The request object to use for the request.                            |
| `serverURL`                                                           | *String*                                                              | :heavy_minus_sign:                                                    | An optional server URL to use.                                        |

### Response

**[CreateImportValidationJobResponse](../../models/operations/CreateImportValidationJobResponse.md)**

### Errors

| Error Type                         | Status Code                        | Content Type                       |
| ---------------------------------- | ---------------------------------- | ---------------------------------- |
| models/errors/SchemasWSErrorCommon | 406                                | application/json                   |
| models/errors/WSErrorCommon        | 400, 401, 403, 412                 | application/json                   |
| models/errors/WSErrorCommon        | 500                                | application/json                   |
| models/errors/APIException         | 4XX, 5XX                           | \*/\*                              |

## cancelImport

# Cancel Import or Validation Job

## Overview
This API allows users to cancel import or validation operations that are currently in progress or scheduled for future execution. Cancellation is immediate for scheduled jobs and graceful for running jobs.

## Cancellation Behavior
- **Scheduled Jobs**: Immediate cancellation, no processing occurs
- **In Progress Jobs**: Graceful shutdown, current item completes, no new items start
- **Completed Jobs**: Cannot be cancelled (returns error)
- **Failed Jobs**: Cannot be cancelled (already stopped)

## When to Cancel
- **Content Issues**: Discover problems with source content
- **Timing Changes**: Need to reschedule for different time
- **Resource Constraints**: System resources are needed elsewhere
- **User Request**: Manual cancellation by authorized users
- **System Maintenance**: Planned maintenance windows

## Cancellation Process
1. **Request Received**: System receives cancellation request
2. **Status Check**: Verifies current job status
3. **Graceful Shutdown**: For running jobs, completes current item
4. **Resource Cleanup**: Releases allocated system resources
5. **Status Update**: Marks job as cancelled
6. **Notification**: Updates job status and logs

## Post-Cancellation
- **Job Status**: Changes to "Cancelled"
- **Partial Results**: Any completed items remain in system
- **Logs**: Cancellation reason and timing recorded
- **Resources**: System resources freed for other operations

## Best Practices
- **Monitor Jobs**: Regularly check job status to identify candidates for cancellation
- **Plan Cancellations**: Schedule cancellations during low-usage periods
- **Resource Planning**: Consider resource impact before cancelling large jobs


### Example Usage

<!-- UsageSnippet language="java" operationID="cancelImport" method="post" path="/import/content/{job_id}/cancel" -->
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.errors.SchemasWSErrorCommon;
import com.egain.sdk.models.errors.WSErrorCommon;
import com.egain.sdk.models.operations.CancelImportResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, SchemasWSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        CancelImportResponse res = sdk.content().import_().cancelImport()
                .jobId("7A84B875-6F75-4C7B-B137-0632B62DB0BD")
                .call();

        // handle response
    }
}
```

### Parameters

| Parameter                                                                                                                                                                                                                                                                                                                      | Type                                                                                                                                                                                                                                                                                                                           | Required                                                                                                                                                                                                                                                                                                                       | Description                                                                                                                                                                                                                                                                                                                    | Example                                                                                                                                                                                                                                                                                                                        |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `jobId`                                                                                                                                                                                                                                                                                                                        | *String*                                                                                                                                                                                                                                                                                                                       | :heavy_check_mark:                                                                                                                                                                                                                                                                                                             | **Job ID Parameter**<br/><br/>The unique identifier for the import or validation job. This ID was returned when the job was created via the Import or Validate API.<br/><br/>**Format:** UUID v4 (e.g., 7A84B875-6F75-4C7B-B137-0632B62DB0BD)<br/><br/>**Example Usage:**<br/>```bash<br/>GET /import/content/7A84B875-6F75-4C7B-B137-0632B62DB0BD/status<br/>```<br/> | 7A84B875-6F75-4C7B-B137-0632B62DB0BD                                                                                                                                                                                                                                                                                           |
| `serverURL`                                                                                                                                                                                                                                                                                                                    | *String*                                                                                                                                                                                                                                                                                                                       | :heavy_minus_sign:                                                                                                                                                                                                                                                                                                             | An optional server URL to use.                                                                                                                                                                                                                                                                                                 | http://localhost:8080                                                                                                                                                                                                                                                                                                          |

### Response

**[CancelImportResponse](../../models/operations/CancelImportResponse.md)**

### Errors

| Error Type                         | Status Code                        | Content Type                       |
| ---------------------------------- | ---------------------------------- | ---------------------------------- |
| models/errors/WSErrorCommon        | 401, 403, 404                      | application/json                   |
| models/errors/SchemasWSErrorCommon | 406                                | application/json                   |
| models/errors/WSErrorCommon        | 500                                | application/json                   |
| models/errors/APIException         | 4XX, 5XX                           | \*/\*                              |