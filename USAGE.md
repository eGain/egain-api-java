<!-- Start SDK Example Usage [usage] -->
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
<!-- End SDK Example Usage [usage] -->