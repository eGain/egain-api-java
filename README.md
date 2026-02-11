# egain-api-java

Developer-friendly & type-safe Java SDK specifically catered to leverage eGain Java API.


<br />

<!-- Start Summary [summary] -->
## Summary

Knowledge Portal Manager APIs: 
### License
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
* [egain-api-java](#egain-api-java)
  * [SDK Installation](#sdk-installation)
  * [SDK Example Usage](#sdk-example-usage)
  * [Asynchronous Support](#asynchronous-support)
  * [Authentication](#authentication)
  * [Available Resources and Operations](#available-resources-and-operations)
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
implementation 'com.egain.sdk:egain-api:0.2.0'
```

Maven:
```xml
<dependency>
    <groupId>com.egain.sdk</groupId>
    <artifactId>egain-api</artifactId>
    <version>0.2.0</version>
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
                .q("What is a loan?")
                .portalID("PROD-1000")
                .language(RequiredLanguageCode.EN_US)
                .filterUserProfileID("PROD-1030")
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
                    .eventId("6154589f-b43f-4471-b2c7-92c6c888a664")
                    .clientSessionId("6154589f-b43f-4471-b2c7-92c6c888a643")
                    .sessionId("6154589f-b43f-4471-b2c7-92c6c888a689")
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
import com.egain.sdk.models.components.ExecutePrompt;
import com.egain.sdk.models.components.LanguageCodeRequestBody;
import com.egain.sdk.models.operations.async.ExecutePromptResponse;
import java.util.concurrent.CompletableFuture;

public class Application {

    public static void main(String[] args) {

        AsyncEgain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build()
            .async();

        CompletableFuture<ExecutePromptResponse> resFut = sdk.aiservices().prompt().executePrompt()
                .promptId("<id>")
                .body(ExecutePrompt.builder()
                    .department("Service")
                    .languageCode(LanguageCodeRequestBody.EN_US)
                    .clientSessionId("123e4567-e89b-12d3-a456-426614174000")
                    .build())
                .call();

        resFut.thenAccept(res -> {
            if (res.executePromptResponse().isPresent()) {
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
<!-- End Authentication [security] -->

<!-- Start Available Resources and Operations [operations] -->
## Available Resources and Operations

<details open>
<summary>Available methods</summary>

#### [aiservices().answers()](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/answers/README.md)

* [getBestAnswer](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/answers/README.md#getbestanswer) - Generate an Answer

#### [aiservices().prompt()](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/aiservicesprompt/README.md)

* [executePrompt](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/aiservicesprompt/README.md#executeprompt) - Execute a predefined prompt

#### [aiservices().retrieve()](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/retrieve/README.md)

* [retrieveChunks](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/retrieve/README.md#retrievechunks) - Retrieve Chunks

#### [content().import_()](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/import/README.md)

* [createImportJob](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/import/README.md#createimportjob) - Create Import Job
* [getImportStatus](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/import/README.md#getimportstatus) - Get Job Status
* [createImportValidationJob](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/import/README.md#createimportvalidationjob) - Create Validation Job
* [cancelImport](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/import/README.md#cancelimport) - Cancel Job

#### [portal().article()](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/article/README.md)

* [getArticleById](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/article/README.md#getarticlebyid) - Get Article by ID
* [getArticleByIdWithEditions](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/article/README.md#getarticlebyidwitheditions) - Get Article By ID with Editions
* [getArticleEditionDetails](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/article/README.md#getarticleeditiondetails) - Get Article Edition Details
* [addToReply](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/article/README.md#addtoreply) - Add Article to Reply
* [addAsReference](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/article/README.md#addasreference) - Add as Reference
* [getArticlesInTopic](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/article/README.md#getarticlesintopic) - Get Articles in Topic
* [getArticleAttachmentById](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/article/README.md#getarticleattachmentbyid) - Get Article Attachment By ID
* [getAttachmentByIdInPortal](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/article/README.md#getattachmentbyidinportal) - Get Article Attachment in Portal
* [getRelatedArticles](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/article/README.md#getrelatedarticles) - Get Related Articles
* [getAnnouncementArticles](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/article/README.md#getannouncementarticles) - Get Announcement Articles
* [getArticleRatings](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/article/README.md#getarticleratings) - Get Article Ratings
* [rateArticle](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/article/README.md#ratearticle) - Rate an Article
* [getPendingComplianceArticles](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/article/README.md#getpendingcompliancearticles) - Get Pending Article Compliances
* [getAcknowledgedComplianceArticles](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/article/README.md#getacknowledgedcompliancearticles) - Get Acknowledged Article Compliances
* [complyArticle](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/article/README.md#complyarticle) - Comply With an Article
* [getMySubscription](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/article/README.md#getmysubscription) - My Subscription
* [subscribeArticle](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/article/README.md#subscribearticle) - Subscribe to an Article
* [unsubscribeArticle](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/article/README.md#unsubscribearticle) - Unsubscribe to an Article
* [getArticlePermissionsById](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/article/README.md#getarticlepermissionsbyid) - Get Article Permissions By ID
* [getArticlePersonalization](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/article/README.md#getarticlepersonalization) - Get Article Personalization Details

#### [portal().articlelists()](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/articlelists/README.md)

* [getAllArticleLists](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/articlelists/README.md#getallarticlelists) - Get All Article Lists
* [getArticleListDetails](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/articlelists/README.md#getarticlelistdetails) - Get Article List by ID

#### [portal().attachment()](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/attachment/README.md)

* [createSignedURL](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/attachment/README.md#createsignedurl) - Generate Signed URL to Upload API

#### [portal().bookmark()](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/bookmark/README.md)

* [addbookmark](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/bookmark/README.md#addbookmark) - Add a Bookmark
* [getbookmark](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/bookmark/README.md#getbookmark) - Get All Bookmarks for a Portal
* [deletebookmark](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/bookmark/README.md#deletebookmark) - Delete a Bookmark

#### [portal().connectorssearchevents()](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/connectorssearchevents/README.md)

* [createSearchResultEventForConnectors](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/connectorssearchevents/README.md#createsearchresulteventforconnectors) - Event for Search Using Connectors
* [createViewedSearchResultsEventForConnectors](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/connectorssearchevents/README.md#createviewedsearchresultseventforconnectors) - Event for Viewed Search Results Using Connectors

#### [portal().export()](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/export/README.md)

* [exportContent](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/export/README.md#exportcontent) - Export Knowledge
* [exportStatus](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/export/README.md#exportstatus) - Get Export Job Status

#### [portal().federatedsearchevent()](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/federatedsearchevent/README.md)

* [createFederatedSearchResultEvent](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/federatedsearchevent/README.md#createfederatedsearchresultevent) - Event For Viewed Federated Search Result

#### [portal().general()](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/general/README.md)

* [getAllPortals](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/general/README.md#getallportals) - Get All Portals
* [getMyPortals](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/general/README.md#getmyportals) - Get All Portals Accessible To User
* [getPortalDetailsById](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/general/README.md#getportaldetailsbyid) - Get Portal Details By ID
* [getTagCategoriesForInterestForPortal](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/general/README.md#gettagcategoriesforinterestforportal) - Get Tag Categories for Interest for a Portal

#### [portal().guidedhelp()](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/guidedhelp/README.md)

* [getAllCasebasesReleases](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/guidedhelp/README.md#getallcasebasesreleases) - Get Available Casebases Releases
* [getCasebaseReleaseById](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/guidedhelp/README.md#getcasebasereleasebyid) - Get Details of a Casebase Release
* [getClusterByCasebaseReleaseId](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/guidedhelp/README.md#getclusterbycasebasereleaseid) - Get Cluster Details of a Casebase Release
* [getAllProfilesInPortal](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/guidedhelp/README.md#getallprofilesinportal) - Get All Profiles Available in Portal
* [startGHSearch](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/guidedhelp/README.md#startghsearch) - Start a Guided Help Search
* [stepGHSearch](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/guidedhelp/README.md#stepghsearch) - Perform a Step in a Guided Help Search
* [getAllCases](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/guidedhelp/README.md#getallcases) - Get All Cases of a Cluster in Release
* [getCaseById](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/guidedhelp/README.md#getcasebyid) - Get Details of a Case
* [acceptGHSolution](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/guidedhelp/README.md#acceptghsolution) - Accept Solution
* [rejectGHSolution](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/guidedhelp/README.md#rejectghsolution) - Reject Solution
* [createQuickpick](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/guidedhelp/README.md#createquickpick) - Create Quickpick
* [getAllQuickPicks](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/guidedhelp/README.md#getallquickpicks) - Get All Quickpicks for a Portal
* [restoreQuickpick](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/guidedhelp/README.md#restorequickpick) - Resume a Quickpick

#### [portal().populararticles()](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/populararticles/README.md)

* [getpopulararticles](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/populararticles/README.md#getpopulararticles) - Get Popular Articles

#### [portal().search()](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/search/README.md)

* [aiSearch](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/search/README.md#aisearch) - Hybrid Search

#### [portal().suggestion()](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/suggestion/README.md)

* [makeSuggestion](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/suggestion/README.md#makesuggestion) - Make a Suggestion
* [modifySuggestions](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/suggestion/README.md#modifysuggestions) - Modify Suggestion
* [searchSuggestion](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/suggestion/README.md#searchsuggestion) - Get Suggestion by Status
* [getSuggestion](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/suggestion/README.md#getsuggestion) - Get Suggestion by ID
* [deleteSuggestion](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/suggestion/README.md#deletesuggestion) - Delete a Suggestion
* [getRelatedArticlesForSuggestion](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/suggestion/README.md#getrelatedarticlesforsuggestion) - Get Related Articles for Suggestion
* [getSuggestionComments](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/suggestion/README.md#getsuggestioncomments) - Get Suggestion Comments
* [getSuggestionAttachments](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/suggestion/README.md#getsuggestionattachments) - Get Suggestion Attachments
* [getSuggestionAttachmentById](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/suggestion/README.md#getsuggestionattachmentbyid) - Get Suggestion Attachment by ID

#### [portal().topic()](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/topic/README.md)

* [getTopicBreadcrumbForArticle](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/topic/README.md#gettopicbreadcrumbforarticle) - Get Topic Breadcrumb for Article
* [getchildtopics](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/topic/README.md#getchildtopics) - Get Immediate Child Topics
* [getancestortopics](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/topic/README.md#getancestortopics) - Get All Ancestor Topics
* [getalltopics](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/topic/README.md#getalltopics) - Get All Topics

#### [portal().userdetails()](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/userdetails/README.md)

* [getUserDetails](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/userdetails/README.md#getuserdetails) - Get User Details

#### [portal().usermilestones()](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/usermilestones/README.md)

* [getUserMilestones](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/usermilestones/README.md#getusermilestones) - Get User Milestones

#### [portal().userprofile()](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/userprofile/README.md)

* [getAllUserProfiles](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/userprofile/README.md#getalluserprofiles) - Get All User Profiles Assigned to User
* [selectUserProfile](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/userprofile/README.md#selectuserprofile) - Select User Profile

### [prompt()](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/prompt/README.md)

* [getPromptTemplates](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/prompt/README.md#getprompttemplates) - Get Prompt Templates
* [getPromptTemplateById](https://github.com/eGain/egain-api-java/blob/master/docs/sdks/prompt/README.md#getprompttemplatebyid) - Get Prompt Template By ID

</details>
<!-- End Available Resources and Operations [operations] -->

<!-- Start Error Handling [errors] -->
## Error Handling

Handling errors in this SDK should largely match your expectations. All operations return a response object or raise an exception.


[`EgainException`](https://github.com/eGain/egain-api-java/blob/master/src/main/java/models/errors/EgainException.java) is the base class for all HTTP error responses. It has the following properties:

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
import com.egain.sdk.models.components.ExecutePrompt;
import com.egain.sdk.models.components.LanguageCodeRequestBody;
import com.egain.sdk.models.errors.BadRequestException;
import com.egain.sdk.models.errors.EgainException;
import com.egain.sdk.models.operations.ExecutePromptResponse;
import java.io.UncheckedIOException;
import java.lang.Exception;
import java.lang.Long;
import java.lang.String;
import java.util.Optional;

public class Application {

    public static void main(String[] args) throws BadRequestException, Exception {

        Egain sdk = Egain.builder()
                .accessToken(System.getenv().getOrDefault("ACCESS_TOKEN", ""))
            .build();
        try {

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
            if (ex instanceof BadRequestException) {
                var e = (BadRequestException) ex;
                // Check error data fields
                e.data().ifPresent(payload -> {
                      Optional<Long> code = payload.code();
                      Optional<String> developerMessage = payload.developerMessage();
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
* [`EgainException`](https://github.com/eGain/egain-api-java/blob/master/src/main/java/models/errors/EgainException.java): The base class for HTTP error responses.
  * [`com.egain.sdk.models.errors.WSErrorCommon`](https://github.com/eGain/egain-api-java/blob/master/src/main/java/models/errors/com.egain.sdk.models.errors.WSErrorCommon.java): Bad Request. *

<details><summary>Less common errors (8)</summary>

<br />

**Network errors:**
* `java.io.IOException` (always wrapped by `java.io.UncheckedIOException`). Commonly encountered subclasses of
`IOException` include `java.net.ConnectException`, `java.net.SocketTimeoutException`, `EOFException` (there are
many more subclasses in the JDK platform).

**Inherit from [`EgainException`](https://github.com/eGain/egain-api-java/blob/master/src/main/java/models/errors/EgainException.java)**:
* [`com.egain.sdk.models.errors.SchemasWSErrorCommon`](https://github.com/eGain/egain-api-java/blob/master/src/main/java/models/errors/com.egain.sdk.models.errors.SchemasWSErrorCommon.java): Not acceptable. Applicable to 4 of 76 methods.*
* [`com.egain.sdk.models.errors.BadRequestException`](https://github.com/eGain/egain-api-java/blob/master/src/main/java/models/errors/com.egain.sdk.models.errors.BadRequestException.java): Bad Request. Status code `400`. Applicable to 1 of 76 methods.*


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
import com.egain.sdk.models.components.ExecutePrompt;
import com.egain.sdk.models.components.LanguageCodeRequestBody;
import com.egain.sdk.models.errors.BadRequestException;
import com.egain.sdk.models.operations.ExecutePromptResponse;
import java.lang.Exception;

public class Application {

    public static void main(String[] args) throws BadRequestException, Exception {

        Egain sdk = Egain.builder()
                .serverURL("https://api.aidev.egain.cloud/knowledge/portalmgr/v4")
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

### Override Server URL Per-Operation

The server URL can also be overridden on a per-operation basis, provided a server list was specified for the operation. For example:
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
                .serverURL("https://api.aidev.egain.cloud/core/aiservices/v4")
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
