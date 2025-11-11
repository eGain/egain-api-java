# openapi

Developer-friendly & type-safe Java SDK specifically catered to leverage *openapi* API.


<br /><br />
> [!IMPORTANT]
> This SDK is not yet ready for production use. To complete setup please follow the steps outlined in your [workspace](https://app.speakeasy.com/org/egain-corporation/portalmgr). Delete this section before > publishing to a package manager.

<!-- Start Summary [summary] -->
## Summary

Knowledge Portal Manager APIs: ### License
  The following licenses are required to use the Knowledge Access APIs:
  * If the user is an agent, then the *Knowledge + AI* license is required.
  * If the user is a customer, the *Self-Service* and *Advanced Self-Service* licenses must be available.

### Tiers

| Tier	|Tier Name|	Named Users |	Description
| ---------- | ---------- | ---------- | ----------------------------
| Tier 1 |  Starter |	Up to 10|	Designed for small-scale implementations or pilot environments
| Tier 2 |	Growth	| Up to 1000|	Suitable for mid-scale deployments requiring moderate scalability
| Tier 3 |	Enterprise	| Greater than 1000|	Supports large-scale environments with extended configuration options

### API Resource Limits
The following Resources have predefined limits for specific access attributes for Starter, Growth and Enterprise use.

| Resource | Limits | Starter | Growth | Enterprise
| ---------------- | ---------------------------- | ---------- | ---------- | ----------
| Article Reference |Number of attachments used in any article | 25 | 50 |50
|  |Number of custom attributes in an article | 10 | 25| 50 
|  |Number of publish views used in an article version | 20 | 20 | 20
| Topic Reference |User-defined topics in a department| 1000| 5000 | 50000
|  |Depth of topics  | 5 | 20 | 20
|  |Topics at any level | 500 | 2500 | 2500
|  |Number of custom attributes in a topic | 10 | 10 | 10
| Portal Reference | Tag categories in a portal | 15 | 15 | 15
|  |Topics to be included in a portal | 100 | 500 | 5000 
|  |Number of articles to display in announcements | 10 | 25 | 25
|  |Usage links and link groups setup for a portal | 5 | 10 | 25
<!-- End Summary [summary] -->

<!-- Start Table of Contents [toc] -->
## Table of Contents
<!-- $toc-max-depth=2 -->
* [openapi](#openapi)
  * [SDK Installation](#sdk-installation)
  * [SDK Example Usage](#sdk-example-usage)
  * [Asynchronous Support](#asynchronous-support)
  * [Authentication](#authentication)
  * [Available Resources and Operations](#available-resources-and-operations)
  * [File uploads](#file-uploads)
  * [Error Handling](#error-handling)
  * [Server Selection](#server-selection)
  * [Custom HTTP Client](#custom-http-client)
  * [Debugging](#debugging)
* [Development](#development)
  * [Maturity](#maturity)
  * [Contributions](#contributions)

<!-- End Table of Contents [toc] -->

<!-- Start SDK Installation [installation] -->
## SDK Installation

### Getting started

JDK 11 or later is required.

The samples below show how a published SDK artifact is used:

Gradle:
```groovy
implementation 'com.egain.sdk:egain-api:0.1.4'
```

Maven:
```xml
<dependency>
    <groupId>com.egain.sdk</groupId>
    <artifactId>egain-api</artifactId>
    <version>0.1.4</version>
</dependency>
```

### How to build
After cloning the git repository to your file system you can build the SDK artifact from source to the `build` directory by running `./gradlew build` on *nix systems or `gradlew.bat` on Windows systems.

If you wish to build from source and publish the SDK artifact to your local Maven repository (on your filesystem) then use the following command (after cloning the git repo locally):

On *nix:
```bash
./gradlew publishToMavenLocal -Pskip.signing
```
On Windows:
```bash
gradlew.bat publishToMavenLocal -Pskip.signing
```
<!-- End SDK Installation [installation] -->

<!-- Start SDK Example Usage [usage] -->
## SDK Example Usage

### Example

```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.operations.RetrieveChunksRequest;
import com.egain.sdk.models.operations.RetrieveChunksResponse;
import java.lang.Exception;
import java.util.List;
import java.util.Map;

public class Application {

    public static void main(String[] args) throws Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        RetrieveChunksRequest req = RetrieveChunksRequest.builder()
                .q("fair lending")
                .portalID("PROD-1000")
                .language(LanguageCodeParameter.EN_US)
                .filterUserProfileID("PROD-3210")
                .filterTags(Map.ofEntries(
                    Map.entry("PROD-1234", List.of(
                        "PROD-2000",
                        "PROD-2003")),
                    Map.entry("PROD-2005", List.of(
                        "PROD-2007"))))
                .body(RetrieveRequest.builder()
                    .channel(RetrieveRequestChannel.builder()
                        .name("Eight Bank Website")
                        .build())
                    .build())
                .build();

        RetrieveChunksResponse res = sdk.aiservices().retrieve().retrieveChunks()
                .request(req)
                .call();

        if (res.retrieveResponse().isPresent()) {
            // handle response
        }
    }
}
```
#### Asynchronous Call
An asynchronous SDK client is also available that returns a [`CompletableFuture<T>`][comp-fut]. See [Asynchronous Support](#asynchronous-support) for more details on async benefits and reactive library integration.
```java
package hello.world;

import com.egain.sdk.AsyncEgain;
import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.operations.RetrieveChunksRequest;
import com.egain.sdk.models.operations.async.RetrieveChunksResponse;
import java.util.List;
import java.util.Map;
import java.util.concurrent.CompletableFuture;

public class Application {

    public static void main(String[] args) {

        AsyncEgain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build()
            .async();

        RetrieveChunksRequest req = RetrieveChunksRequest.builder()
                .q("fair lending")
                .portalID("PROD-1000")
                .language(LanguageCodeParameter.EN_US)
                .filterUserProfileID("PROD-3210")
                .filterTags(Map.ofEntries(
                    Map.entry("PROD-1234", List.of(
                        "PROD-2000",
                        "PROD-2003")),
                    Map.entry("PROD-2005", List.of(
                        "PROD-2007"))))
                .body(RetrieveRequest.builder()
                    .channel(RetrieveRequestChannel.builder()
                        .name("Eight Bank Website")
                        .build())
                    .build())
                .build();

        CompletableFuture<RetrieveChunksResponse> resFut = sdk.aiservices().retrieve().retrieveChunks()
                .request(req)
                .call();

        resFut.thenAccept(res -> {
            if (res.retrieveResponse().isPresent()) {
            // handle response
            }
        });
    }
}
```

[comp-fut]: https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CompletableFuture.html
<!-- End SDK Example Usage [usage] -->

<!-- Start Asynchronous Support [async-support] -->
## Asynchronous Support

The SDK provides comprehensive asynchronous support using Java's [`CompletableFuture<T>`][comp-fut] and [Reactive Streams `Publisher<T>`][reactive-streams] APIs. This design makes no assumptions about your choice of reactive toolkit, allowing seamless integration with any reactive library.

<details>
<summary>Why Use Async?</summary>

Asynchronous operations provide several key benefits:

- **Non-blocking I/O**: Your threads stay free for other work while operations are in flight
- **Better resource utilization**: Handle more concurrent operations with fewer threads
- **Improved scalability**: Build highly responsive applications that can handle thousands of concurrent requests
- **Reactive integration**: Works seamlessly with reactive streams and backpressure handling

</details>

<details>
<summary>Reactive Library Integration</summary>

The SDK returns [Reactive Streams `Publisher<T>`][reactive-streams] instances for operations dealing with streams involving multiple I/O interactions. We use Reactive Streams instead of JDK Flow API to provide broader compatibility with the reactive ecosystem, as most reactive libraries natively support Reactive Streams.

**Why Reactive Streams over JDK Flow?**
- **Broader ecosystem compatibility**: Most reactive libraries (Project Reactor, RxJava, Akka Streams, etc.) natively support Reactive Streams
- **Industry standard**: Reactive Streams is the de facto standard for reactive programming in Java
- **Better interoperability**: Seamless integration without additional adapters for most use cases

**Integration with Popular Libraries:**
- **Project Reactor**: Use `Flux.from(publisher)` to convert to Reactor types
- **RxJava**: Use `Flowable.fromPublisher(publisher)` for RxJava integration
- **Akka Streams**: Use `Source.fromPublisher(publisher)` for Akka Streams integration
- **Vert.x**: Use `ReadStream.fromPublisher(vertx, publisher)` for Vert.x reactive streams
- **Mutiny**: Use `Multi.createFrom().publisher(publisher)` for Quarkus Mutiny integration

**For JDK Flow API Integration:**
If you need JDK Flow API compatibility (e.g., for Quarkus/Mutiny 2), you can use adapters:
```java
// Convert Reactive Streams Publisher to Flow Publisher
Flow.Publisher<T> flowPublisher = FlowAdapters.toFlowPublisher(reactiveStreamsPublisher);

// Convert Flow Publisher to Reactive Streams Publisher
Publisher<T> reactiveStreamsPublisher = FlowAdapters.toPublisher(flowPublisher);
```

For standard single-response operations, the SDK returns `CompletableFuture<T>` for straightforward async execution.

</details>

<details>
<summary>Supported Operations</summary>

Async support is available for:

- **[Server-sent Events](#server-sent-event-streaming)**: Stream real-time events with Reactive Streams `Publisher<T>`
- **[JSONL Streaming](#jsonl-streaming)**: Process streaming JSON lines asynchronously
- **[Pagination](#pagination)**: Iterate through paginated results using `callAsPublisher()` and `callAsPublisherUnwrapped()`
- **[File Uploads](#file-uploads)**: Upload files asynchronously with progress tracking
- **[File Downloads](#file-downloads)**: Download files asynchronously with streaming support
- **[Standard Operations](#example)**: All regular API calls return `CompletableFuture<T>` for async execution

</details>

[comp-fut]: https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CompletableFuture.html
[reactive-streams]: https://www.reactive-streams.org/
<!-- End Asynchronous Support [async-support] -->

<!-- Start Authentication [security] -->
## Authentication

### Per-Client Security Schemes

This SDK supports the following security scheme globally:

| Name          | Type | Scheme      |
| ------------- | ---- | ----------- |
| `accessToken` | http | HTTP Bearer |

To authenticate with the API the `accessToken` parameter must be set when initializing the SDK client instance. For example:
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.operations.RetrieveChunksRequest;
import com.egain.sdk.models.operations.RetrieveChunksResponse;
import java.lang.Exception;
import java.util.List;
import java.util.Map;

public class Application {

    public static void main(String[] args) throws Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        RetrieveChunksRequest req = RetrieveChunksRequest.builder()
                .q("fair lending")
                .portalID("PROD-1000")
                .language(LanguageCodeParameter.EN_US)
                .filterUserProfileID("PROD-3210")
                .filterTags(Map.ofEntries(
                    Map.entry("PROD-1234", List.of(
                        "PROD-2000",
                        "PROD-2003")),
                    Map.entry("PROD-2005", List.of(
                        "PROD-2007"))))
                .body(RetrieveRequest.builder()
                    .channel(RetrieveRequestChannel.builder()
                        .name("Eight Bank Website")
                        .build())
                    .build())
                .build();

        RetrieveChunksResponse res = sdk.aiservices().retrieve().retrieveChunks()
                .request(req)
                .call();

        if (res.retrieveResponse().isPresent()) {
            // handle response
        }
    }
}
```
<!-- End Authentication [security] -->

<!-- Start Available Resources and Operations [operations] -->
## Available Resources and Operations

<details open>
<summary>Available methods</summary>

#### [aiservices().answers()](docs/sdks/answers/README.md)

* [getBestAnswer](docs/sdks/answers/README.md#getbestanswer) - Get the best answer for a user query

#### [aiservices().retrieve()](docs/sdks/retrieve/README.md)

* [retrieveChunks](docs/sdks/retrieve/README.md#retrievechunks) - Retrieve Chunks

#### [content().import_()](docs/sdks/import/README.md)

* [createImportJob](docs/sdks/import/README.md#createimportjob) - Import content from external sources by creating an import job
* [getImportStatus](docs/sdks/import/README.md#getimportstatus) - Get the current status of an import or validation job
* [createImportValidationJob](docs/sdks/import/README.md#createimportvalidationjob) - Validate content structure and format before import by creating an import validation job
* [cancelImport](docs/sdks/import/README.md#cancelimport) - Cancel an import or validation job

#### [portal().article()](docs/sdks/article/README.md)

* [getArticleById](docs/sdks/article/README.md#getarticlebyid) - Get Article by ID
* [getArticleByIdWithEditions](docs/sdks/article/README.md#getarticlebyidwitheditions) - Get Article By ID with Editions
* [getArticleEditionDetails](docs/sdks/article/README.md#getarticleeditiondetails) - Get Article Edition Details
* [addToReply](docs/sdks/article/README.md#addtoreply) - Add Article to Reply
* [addAsReference](docs/sdks/article/README.md#addasreference) - Add as Reference
* [getArticlesInTopic](docs/sdks/article/README.md#getarticlesintopic) - Get Articles in Topic
* [getArticleAttachmentById](docs/sdks/article/README.md#getarticleattachmentbyid) - Get Article Attachment By ID
* [getAttachmentByIdInPortal](docs/sdks/article/README.md#getattachmentbyidinportal) - Get Article Attachment in Portal
* [getRelatedArticles](docs/sdks/article/README.md#getrelatedarticles) - Get Related Articles
* [getAnnouncementArticles](docs/sdks/article/README.md#getannouncementarticles) - Get Announcement Articles
* [getArticleRatings](docs/sdks/article/README.md#getarticleratings) - Get Article Ratings
* [rateArticle](docs/sdks/article/README.md#ratearticle) - Rate an Article
* [getPendingComplianceArticles](docs/sdks/article/README.md#getpendingcompliancearticles) - Get Pending Article Compliances
* [getAcknowledgedComplianceArticles](docs/sdks/article/README.md#getacknowledgedcompliancearticles) - Get Acknowledged Article Compliances
* [complyArticle](docs/sdks/article/README.md#complyarticle) - Comply With an Article
* [getMySubscription](docs/sdks/article/README.md#getmysubscription) - My Subscription
* [subscribeArticle](docs/sdks/article/README.md#subscribearticle) - Subscribe to an Article
* [unsubscribeArticle](docs/sdks/article/README.md#unsubscribearticle) - Unsubscribe to an Article
* [getArticlePermissionsById](docs/sdks/article/README.md#getarticlepermissionsbyid) - Get Article Permissions By ID
* [getArticlePersonalization](docs/sdks/article/README.md#getarticlepersonalization) - Get Article Personalization Details

#### [portal().articlelists()](docs/sdks/articlelists/README.md)

* [getAllArticleLists](docs/sdks/articlelists/README.md#getallarticlelists) - Get All Article Lists
* [getArticleListDetails](docs/sdks/articlelists/README.md#getarticlelistdetails) - Get Article List by ID

#### [portal().attachment()](docs/sdks/attachment/README.md)

* [createSignedURL](docs/sdks/attachment/README.md#createsignedurl) - Generate Signed URL to Upload API
* [uploadAttachment](docs/sdks/attachment/README.md#uploadattachment) - Upload Attachment

#### [portal().bookmark()](docs/sdks/bookmark/README.md)

* [addbookmark](docs/sdks/bookmark/README.md#addbookmark) - Add a Bookmark
* [getbookmark](docs/sdks/bookmark/README.md#getbookmark) - Get All Bookmarks for a Portal
* [deletebookmark](docs/sdks/bookmark/README.md#deletebookmark) - Delete a Bookmark

#### [portal().connectorssearchevents()](docs/sdks/connectorssearchevents/README.md)

* [createSearchResultEventForConnectors](docs/sdks/connectorssearchevents/README.md#createsearchresulteventforconnectors) - Event for Search Using Connectors
* [createViewedSearchResultsEventForConnectors](docs/sdks/connectorssearchevents/README.md#createviewedsearchresultseventforconnectors) - Event for Viewed Search Results Using Connectors

#### [portal().escalation()](docs/sdks/escalation/README.md)

* [startCustomerEscalation](docs/sdks/escalation/README.md#startcustomerescalation) - Start Customer Escalation
* [searchPriorToEscalation](docs/sdks/escalation/README.md#searchpriortoescalation) - Search Prior To Customer Escalation
* [completeCustomerEscalation](docs/sdks/escalation/README.md#completecustomerescalation) - Complete Customer Escalation
* [avertCustomerEscalation](docs/sdks/escalation/README.md#avertcustomerescalation) - Avert Customer Escalation

#### [portal().export()](docs/sdks/export/README.md)

* [exportContent](docs/sdks/export/README.md#exportcontent) - Export Knowledge
* [exportStatus](docs/sdks/export/README.md#exportstatus) - Get Export Job Status

#### [portal().federatedsearchevent()](docs/sdks/federatedsearchevent/README.md)

* [createFederatedSearchResultEvent](docs/sdks/federatedsearchevent/README.md#createfederatedsearchresultevent) - Event For Viewed Federated Search Result

#### [portal().general()](docs/sdks/general/README.md)

* [getAllPortals](docs/sdks/general/README.md#getallportals) - Get All Portals
* [getMyPortals](docs/sdks/general/README.md#getmyportals) - Get All Portals Accessible To User
* [getPortalDetailsById](docs/sdks/general/README.md#getportaldetailsbyid) - Get Portal Details By ID
* [getTagCategoriesForInterestForPortal](docs/sdks/general/README.md#gettagcategoriesforinterestforportal) - Get Tag Categories for Interest for a Portal

#### [portal().guidedhelp()](docs/sdks/guidedhelp/README.md)

* [getAllCasebasesReleases](docs/sdks/guidedhelp/README.md#getallcasebasesreleases) - Get Available Casebases Releases
* [getCasebaseReleaseById](docs/sdks/guidedhelp/README.md#getcasebasereleasebyid) - Get Details of a Casebase Release
* [getClusterByCasebaseReleaseId](docs/sdks/guidedhelp/README.md#getclusterbycasebasereleaseid) - Get Cluster Details of a Casebase Release
* [getAllProfilesInPortal](docs/sdks/guidedhelp/README.md#getallprofilesinportal) - Get All Profiles Available in Portal
* [startGHSearch](docs/sdks/guidedhelp/README.md#startghsearch) - Start a Guided Help Search
* [stepGHSearch](docs/sdks/guidedhelp/README.md#stepghsearch) - Perform a Step in a Guided Help Search
* [getAllCases](docs/sdks/guidedhelp/README.md#getallcases) - Get All Cases of a Cluster in Release
* [getCaseById](docs/sdks/guidedhelp/README.md#getcasebyid) - Get Details of a Case
* [acceptGHSolution](docs/sdks/guidedhelp/README.md#acceptghsolution) - Accept Solution
* [rejectGHSolution](docs/sdks/guidedhelp/README.md#rejectghsolution) - Reject Solution
* [createQuickpick](docs/sdks/guidedhelp/README.md#createquickpick) - Create Quickpick
* [getAllQuickPicks](docs/sdks/guidedhelp/README.md#getallquickpicks) - Get All Quickpicks for a Portal
* [restoreQuickpick](docs/sdks/guidedhelp/README.md#restorequickpick) - Resume a Quickpick

#### [portal().populararticles()](docs/sdks/populararticles/README.md)

* [getpopulararticles](docs/sdks/populararticles/README.md#getpopulararticles) - Get Popular Articles

#### [portal().search()](docs/sdks/search/README.md)

* [aiSearch](docs/sdks/search/README.md#aisearch) - Get the best search results for a user query

#### [portal().suggestion()](docs/sdks/suggestion/README.md)

* [makeSuggestion](docs/sdks/suggestion/README.md#makesuggestion) - Make a Suggestion
* [modifySuggestions](docs/sdks/suggestion/README.md#modifysuggestions) - Modify Suggestion
* [searchSuggestion](docs/sdks/suggestion/README.md#searchsuggestion) - Get Suggestion by Status
* [getSuggestion](docs/sdks/suggestion/README.md#getsuggestion) - Get Suggestion by ID
* [deleteSuggestion](docs/sdks/suggestion/README.md#deletesuggestion) - Delete a Suggestion
* [getRelatedArticlesForSuggestion](docs/sdks/suggestion/README.md#getrelatedarticlesforsuggestion) - Get Related Articles for Suggestion
* [getSuggestionComments](docs/sdks/suggestion/README.md#getsuggestioncomments) - Get Suggestion Comments
* [getSuggestionAttachments](docs/sdks/suggestion/README.md#getsuggestionattachments) - Get Suggestion Attachments
* [getSuggestionAttachmentById](docs/sdks/suggestion/README.md#getsuggestionattachmentbyid) - Get Suggestion Attachment by ID

#### [portal().topic()](docs/sdks/topic/README.md)

* [getTopicBreadcrumbForArticle](docs/sdks/topic/README.md#gettopicbreadcrumbforarticle) - Get Topic Breadcrumb for Article
* [getchildtopics](docs/sdks/topic/README.md#getchildtopics) - Get Immediate Child Topics
* [getancestortopics](docs/sdks/topic/README.md#getancestortopics) - Get All Ancestor Topics
* [getalltopics](docs/sdks/topic/README.md#getalltopics) - Get All Topics

#### [portal().userdetails()](docs/sdks/userdetails/README.md)

* [getUserDetails](docs/sdks/userdetails/README.md#getuserdetails) - Get User Details

#### [portal().usermilestones()](docs/sdks/usermilestones/README.md)

* [getUserMilestones](docs/sdks/usermilestones/README.md#getusermilestones) - Get User Milestones

#### [portal().userprofile()](docs/sdks/userprofile/README.md)

* [getAllUserProfiles](docs/sdks/userprofile/README.md#getalluserprofiles) - Get All User Profiles Assigned to User
* [selectUserProfile](docs/sdks/userprofile/README.md#selectuserprofile) - Select User Profile

</details>
<!-- End Available Resources and Operations [operations] -->

<!-- Start File uploads [file-upload] -->
## File uploads

Certain SDK methods accept file objects as part of a request body or multi-part request. It is possible and typically recommended to upload files as a stream rather than reading the entire contents into memory. This avoids excessive memory consumption and potentially crashing with out-of-memory errors when working with very large files.

The SDK provides a [`Blob`](src/main/java/com/egain/sdk/utils/Blob.java) utility class for efficient file handling. It supports various input sources including file paths, streams, strings, and byte arrays, while providing memory-efficient streaming and reactive processing.

```java
// Recommended for large files - streams data efficiently
Blob fileBlob = Blob.from(Paths.get("large-document.pdf"));

// For in-memory data
Blob textBlob = Blob.from("Hello, World!");
Blob dataBlob = Blob.from(myByteArray);
```

> [!TIP]
> For comprehensive documentation including all factory methods, consumption patterns, and advanced usage examples, see the [Blob Utility Documentation](docs/utils/Blob.md).

The following example demonstrates how to attach a file to a request:
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
<!-- End File uploads [file-upload] -->

<!-- Start Error Handling [errors] -->
## Error Handling

Handling errors in this SDK should largely match your expectations. All operations return a response object or raise an exception.


[`EgainException`](./src/main/java/models/errors/EgainException.java) is the base class for all HTTP error responses. It has the following properties:

| Method           | Type                        | Description                                                              |
| ---------------- | --------------------------- | ------------------------------------------------------------------------ |
| `message()`      | `String`                    | Error message                                                            |
| `code()`         | `int`                       | HTTP response status code eg `404`                                       |
| `headers`        | `Map<String, List<String>>` | HTTP response headers                                                    |
| `body()`         | `byte[]`                    | HTTP body as a byte array. Can be empty array if no body is returned.    |
| `bodyAsString()` | `String`                    | HTTP body as a UTF-8 string. Can be empty string if no body is returned. |
| `rawResponse()`  | `HttpResponse<?>`           | Raw HTTP response (body already read and not available for re-read)      |

### Example
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.errors.*;
import com.egain.sdk.models.operations.CreateImportJobResponse;
import java.io.UncheckedIOException;
import java.lang.Exception;
import java.lang.String;
import java.time.OffsetDateTime;
import java.util.Optional;

public class Application {

    public static void main(String[] args) throws WSErrorCommon, SchemasWSErrorCommon, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();
        try {

            ImportContent req = ImportContent.builder()
                    .dataSource(ImportContentDataSource.builder()
                        .type(ImportContentType.AWSS3_BUCKET)
                        .path("s3://mybucket/myfolder/")
                        .region("us-east-1")
                        .credentials(ImportContentCredentials.builder()
                            .build())
                        .build())
                    .operation(Operation.IMPORT)
                    .scheduleTime(ScheduleTime.builder()
                        .date(OffsetDateTime.parse("2024-03-01T10:00:00.000Z"))
                        .build())
                    .build();

            CreateImportJobResponse res = sdk.content().import_().createImportJob()
                    .request(req)
                    .call();

            // handle response
        } catch (EgainException ex) { // all SDK exceptions inherit from EgainException

            // ex.ToString() provides a detailed error message including
            // HTTP status code, headers, and error payload (if any)
            System.out.println(ex);

            // Base exception fields
            var rawResponse = ex.rawResponse();
            var headers = ex.headers();
            var contentType = headers.first("Content-Type");
            int statusCode = ex.code();
            Optional<byte[]> responseBody = ex.body();

            // different error subclasses may be thrown 
            // depending on the service call
            if (ex instanceof WSErrorCommon) {
                var e = (WSErrorCommon) ex;
                // Check error data fields
                e.data().ifPresent(payload -> {
                      String code = payload.code();
                      String developerMessage = payload.developerMessage();
                      // ...
                });
            }

            // An underlying cause may be provided. If the error payload 
            // cannot be deserialized then the deserialization exception 
            // will be set as the cause.
            if (ex.getCause() != null) {
                var cause = ex.getCause();
            }
        } catch (UncheckedIOException ex) {
            // handle IO error (connection, timeout, etc)
        }    }
}
```

### Error Classes
**Primary errors:**
* [`EgainException`](./src/main/java/models/errors/EgainException.java): The base class for HTTP error responses.
  * [`com.egain.sdk.models.errors.WSErrorCommon`](./src/main/java/models/errors/com.egain.sdk.models.errors.WSErrorCommon.java): Bad Request. *

<details><summary>Less common errors (7)</summary>

<br />

**Network errors:**
* `java.io.IOException` (always wrapped by `java.io.UncheckedIOException`). Commonly encountered subclasses of
`IOException` include `java.net.ConnectException`, `java.net.SocketTimeoutException`, `EOFException` (there are
many more subclasses in the JDK platform).

**Inherit from [`EgainException`](./src/main/java/models/errors/EgainException.java)**:
* [`com.egain.sdk.models.errors.SchemasWSErrorCommon`](./src/main/java/models/errors/com.egain.sdk.models.errors.SchemasWSErrorCommon.java): Preconditions failed. Status code `412`. Applicable to 2 of 78 methods.*


</details>

\* Check [the method documentation](#available-resources-and-operations) to see if the error is applicable.
<!-- End Error Handling [errors] -->

<!-- Start Server Selection [server] -->
## Server Selection

### Override Server URL Per-Client

The default server can be overridden globally using the `.serverURL(String serverUrl)` builder method when initializing the SDK client instance. For example:
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.operations.RetrieveChunksRequest;
import com.egain.sdk.models.operations.RetrieveChunksResponse;
import java.lang.Exception;
import java.util.List;
import java.util.Map;

public class Application {

    public static void main(String[] args) throws Exception {

        Egain sdk = Egain.builder()
                .serverURL("https://api.aidev.egain.cloud/knowledge/portalmgr/v4")
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        RetrieveChunksRequest req = RetrieveChunksRequest.builder()
                .q("fair lending")
                .portalID("PROD-1000")
                .language(LanguageCodeParameter.EN_US)
                .filterUserProfileID("PROD-3210")
                .filterTags(Map.ofEntries(
                    Map.entry("PROD-1234", List.of(
                        "PROD-2000",
                        "PROD-2003")),
                    Map.entry("PROD-2005", List.of(
                        "PROD-2007"))))
                .body(RetrieveRequest.builder()
                    .channel(RetrieveRequestChannel.builder()
                        .name("Eight Bank Website")
                        .build())
                    .build())
                .build();

        RetrieveChunksResponse res = sdk.aiservices().retrieve().retrieveChunks()
                .request(req)
                .call();

        if (res.retrieveResponse().isPresent()) {
            // handle response
        }
    }
}
```

### Override Server URL Per-Operation

The server URL can also be overridden on a per-operation basis, provided a server list was specified for the operation. For example:
```java
package hello.world;

import com.egain.sdk.Egain;
import com.egain.sdk.models.components.*;
import com.egain.sdk.models.operations.RetrieveChunksRequest;
import com.egain.sdk.models.operations.RetrieveChunksResponse;
import java.lang.Exception;
import java.util.List;
import java.util.Map;

public class Application {

    public static void main(String[] args) throws Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();

        RetrieveChunksRequest req = RetrieveChunksRequest.builder()
                .q("fair lending")
                .portalID("PROD-1000")
                .language(LanguageCodeParameter.EN_US)
                .filterUserProfileID("PROD-3210")
                .filterTags(Map.ofEntries(
                    Map.entry("PROD-1234", List.of(
                        "PROD-2000",
                        "PROD-2003")),
                    Map.entry("PROD-2005", List.of(
                        "PROD-2007"))))
                .body(RetrieveRequest.builder()
                    .channel(RetrieveRequestChannel.builder()
                        .name("Eight Bank Website")
                        .build())
                    .build())
                .build();

        RetrieveChunksResponse res = sdk.aiservices().retrieve().retrieveChunks()
                .request(req)
                .serverURL("https://api.aidev.egain.cloud/core/aiservices/v4")
                .call();

        if (res.retrieveResponse().isPresent()) {
            // handle response
        }
    }
}
```
<!-- End Server Selection [server] -->

<!-- Start Custom HTTP Client [http-client] -->
## Custom HTTP Client

The Java SDK makes API calls using an `HTTPClient` that wraps the native
[HttpClient](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/HttpClient.html). This
client provides the ability to attach hooks around the request lifecycle that can be used to modify the request or handle
errors and response.

The `HTTPClient` interface allows you to either use the default `SpeakeasyHTTPClient` that comes with the SDK,
or provide your own custom implementation with customized configuration such as custom executors, SSL context,
connection pools, and other HTTP client settings.

The interface provides synchronous (`send`) methods and asynchronous (`sendAsync`) methods. The `sendAsync` method
is used to power the async SDK methods and returns a `CompletableFuture<HttpResponse<Blob>>` for non-blocking operations.

The following example shows how to add a custom header and handle errors:

```java
import com.egain.sdk.Egain;
import com.egain.sdk.utils.HTTPClient;
import com.egain.sdk.utils.SpeakeasyHTTPClient;
import com.egain.sdk.utils.Utils;

import java.io.IOException;
import java.net.URISyntaxException;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.io.InputStream;
import java.time.Duration;

public class Application {
    public static void main(String[] args) {
        // Create a custom HTTP client with hooks
        HTTPClient httpClient = new HTTPClient() {
            private final HTTPClient defaultClient = new SpeakeasyHTTPClient();
            
            @Override
            public HttpResponse<InputStream> send(HttpRequest request) throws IOException, URISyntaxException, InterruptedException {
                // Add custom header and timeout using Utils.copy()
                HttpRequest modifiedRequest = Utils.copy(request)
                    .header("x-custom-header", "custom value")
                    .timeout(Duration.ofSeconds(30))
                    .build();
                    
                try {
                    HttpResponse<InputStream> response = defaultClient.send(modifiedRequest);
                    // Log successful response
                    System.out.println("Request successful: " + response.statusCode());
                    return response;
                } catch (Exception error) {
                    // Log error
                    System.err.println("Request failed: " + error.getMessage());
                    throw error;
                }
            }
        };

        Egain sdk = Egain.builder()
            .client(httpClient)
            .build();
    }
}
```

<details>
<summary>Custom HTTP Client Configuration</summary>

You can also provide a completely custom HTTP client with your own configuration:

```java
import com.egain.sdk.Egain;
import com.egain.sdk.utils.HTTPClient;
import com.egain.sdk.utils.Blob;
import com.egain.sdk.utils.ResponseWithBody;

import java.io.IOException;
import java.net.URISyntaxException;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.io.InputStream;
import java.time.Duration;
import java.util.concurrent.Executors;
import java.util.concurrent.CompletableFuture;

public class Application {
    public static void main(String[] args) {
        // Custom HTTP client with custom configuration
        HTTPClient customHttpClient = new HTTPClient() {
            private final HttpClient client = HttpClient.newBuilder()
                .executor(Executors.newFixedThreadPool(10))
                .connectTimeout(Duration.ofSeconds(30))
                // .sslContext(customSslContext) // Add custom SSL context if needed
                .build();

            @Override
            public HttpResponse<InputStream> send(HttpRequest request) throws IOException, URISyntaxException, InterruptedException {
                return client.send(request, HttpResponse.BodyHandlers.ofInputStream());
            }

            @Override
            public CompletableFuture<HttpResponse<Blob>> sendAsync(HttpRequest request) {
                // Convert response to HttpResponse<Blob> for async operations
                return client.sendAsync(request, HttpResponse.BodyHandlers.ofPublisher())
                    .thenApply(resp -> new ResponseWithBody<>(resp, Blob::from));
            }
        };

        Egain sdk = Egain.builder()
            .client(customHttpClient)
            .build();
    }
}
```

</details>

You can also enable debug logging on the default `SpeakeasyHTTPClient`:

```java
import com.egain.sdk.Egain;
import com.egain.sdk.utils.SpeakeasyHTTPClient;

public class Application {
    public static void main(String[] args) {
        SpeakeasyHTTPClient httpClient = new SpeakeasyHTTPClient();
        httpClient.enableDebugLogging(true);

        Egain sdk = Egain.builder()
            .client(httpClient)
            .build();
    }
}
```
<!-- End Custom HTTP Client [http-client] -->

<!-- Start Debugging [debug] -->
## Debugging

### Debug
You can setup your SDK to emit debug logs for SDK requests and responses.

For request and response logging (especially json bodies), call `enableHTTPDebugLogging(boolean)` on the SDK builder like so:
```java
SDK.builder()
    .enableHTTPDebugLogging(true)
    .build();
```
Example output:
```
Sending request: http://localhost:35123/bearer#global GET
Request headers: {Accept=[application/json], Authorization=[******], Client-Level-Header=[added by client], Idempotency-Key=[some-key], x-speakeasy-user-agent=[speakeasy-sdk/java 0.0.1 internal 0.1.0 org.openapis.openapi]}
Received response: (GET http://localhost:35123/bearer#global) 200
Response headers: {access-control-allow-credentials=[true], access-control-allow-origin=[*], connection=[keep-alive], content-length=[50], content-type=[application/json], date=[Wed, 09 Apr 2025 01:43:29 GMT], server=[gunicorn/19.9.0]}
Response body:
{
  "authenticated": true, 
  "token": "global"
}
```
__WARNING__: This should only used for temporary debugging purposes. Leaving this option on in a production system could expose credentials/secrets in logs. <i>Authorization</i> headers are redacted by default and there is the ability to specify redacted header names via `SpeakeasyHTTPClient.setRedactedHeaders`.

__NOTE__: This is a convenience method that calls `HTTPClient.enableDebugLogging()`. The `SpeakeasyHTTPClient` honors this setting. If you are using a custom HTTP client, it is up to the custom client to honor this setting.

Another option is to set the System property `-Djdk.httpclient.HttpClient.log=all`. However, this second option does not log bodies.
<!-- End Debugging [debug] -->

<!-- Placeholder for Future Speakeasy SDK Sections -->

# Development

## Maturity

This SDK is in beta, and there may be breaking changes between versions without a major version update. Therefore, we recommend pinning usage
to a specific package version. This way, you can install the same version each time without breaking changes unless you are intentionally
looking for the latest version.

## Contributions

While we value open-source contributions to this SDK, this library is generated programmatically. Any manual changes added to internal files will be overwritten on the next generation. 
We look forward to hearing your feedback. Feel free to open a PR or an issue with a proof of concept and we'll do our best to include it in a future release. 
