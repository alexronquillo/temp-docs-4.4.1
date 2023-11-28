# Get the justtrack user ID

Each user is assigned a unique user ID by the justtrack SDK which you can access inside your app. You can use this ID to recognize returning users in your app again. Should you already assign a unique ID to your users, you can also link your ID to the justtrack user ID.

The justtrack SDK provides you with a unique ID for each user. You can retrieve a `AsyncFuture` resolving to this ID by using the `getUserId` method on the justtrack SDK:

{% tabs %}
{% tab title="Java" %}
<pre class="language-java"><code class="lang-java">AsyncFuture&#x3C;String> userIdFuture = sdk.getUserId();
String userId = userIdFuture.get();
<strong>log("My user id is " + userId);
</strong></code></pre>
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
val userIdFuture = sdk.userId
val userId = userIdFuture.await()
log("My user id is $userId")
```
{% endtab %}
{% endtabs %}

To wait for the returned `AsyncFuture` to resolve to a value, you need to call `get` on it. This will block the current thread until the future resolves or throws an exception.

**You can not call `get` on the main (UI) thread!** This is a safeguard for you as a `AsyncFuture` you get back from the SDK might actually depend on work the main/UI thread is expected to do. Blocking that thread could then deadlock your app. If you call `get` on the main thread, an exception will be thrown. You can either call the method on another thread or use `registerPromiseCallback` to schedule a callback when the future resolves:

{% tabs %}
{% tab title="Java" %}
```java
AsyncFuture<AttributionResponse> responseFuture = sdk.attributeUser();
responseFuture.registerPromiseCallback(new Promise<AttributionResponse, Exception>() {
    @Override
    public void resolve(AttributionResponse response) {
        UUID userId = response.getUserId();
        log("My user id is " + userId);
    }

    @Override
    public void reject(@NonNull Exception exception) {
        log("An error occurred", exception);
    }
});
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
val responseFuture = sdk.attribution
responseFuture.registerPromiseCallback(object : Promise<AttributionResponse> {
    override fun resolve(response: AttributionResponse) {
        val userId = response.userId.toString()
        log("My user id is $userId")
    }
    override fun reject(throwable: Throwable) {
        log("An error occurred", exception)
    }
})
```
{% endtab %}
{% endtabs %}

If you are using Kotlin coroutines, you can use `await` instead to suspend your current coroutine until the `AsyncFuture` resolves to a result like this:

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
myScope.launch {
    try {
        val responseFuture = sdk.attribution.await()
        val userId = responseFuture.userId.toString()
        log("My user id is $userId")      
    } catch (exception: Exception) {
        log("An error occurred", exception)
    }
}
```
{% endtab %}
{% endtabs %}

If the attribution can not be performed because the user is offline, it will eventually fail. The SDK will then wait for a new internet connection and retry the attribution. An attribution obtained after the returned `AsyncFuture` resolved to a failure value will not update the future. You have to subscribe to updates of the attribution using `registerAttributionListener()` to receive the attribution.

## Provide your own User Id

If you already have a mechanism to assign a unique id to each user, you can share this information with the justtrack SDK. This then allows the backend of the justtrack SDK to associate events received from third parties via that user id with the correct user on justtrack side. You can provide as an id multiple times, the justtrack SDK takes care about sending it to the justtrack backend as needed.

You can supply the own user id when constructing the SDK as well as sending it later once your app is already running for some time. Should your own user id change for some reason, you have to supply the new value to the justtrack SDK again:

{% tabs %}
{% tab title="Java" %}
```java
String ownUserId = /* get your own user id from your app */;
SdkBuilder builder = /* create the builder somehow */ new JustTrackSdkBuilder(this, BuildConfig.APP_KEY);
JustTrackSdk sdk = builder
        .setCustomUserId(ownUserId)
        // other options...
        .build();

ownUserId = /* new user id for some reason */;
sdk.setCustomUserId(ownUserId);
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
var ownUserId: String = /* get your own user id from your app */
val builder: SdkBuilder = /* create the builder somehow */ JustTrackSdkBuilder(this, BuildConfig.APP_KEY)
val sdk: JustTrackSdk = builder
    .setCustomUserId(ownUserId)
    // other options...
    .build()

ownUserId = /* new user id for some reason */
sdk.setCustomUserId(ownUserId)
```
{% endtab %}
{% endtabs %}

A custom user id must be shorter than 4096 characters and only consist of printable ASCII characters (U+0020 to U+007E).
