# Get an advertiser and test group ID

The justtrack SDK can provide you with the advertiser id of the user if the user didn't limit ad tracking. Additionally, users are divided into three test groups (1, 2, and 3) based on their advertiser id. You can retrieve that test group id from the SDK, implement a different logic for one of the test groups, and then compare the (different) performance of that group on the justtrack dashboard.

{% tabs %}
{% tab title="Java" %}
```java
Future<AdvertiserIdInfo> infoFuture = sdk.getAdvertiserIdInfo();
@NonNull AdvertiserIdInfo info = infoFuture.get();
@Nullable String advertiserId = info.getAdvertiserId();
boolean isLimitedAdTracking = info.isLimitedAdTracking();
log("My advertiser id is " + advertiserId);
log("Ad tracking is limited = " + isLimitedAdTracking);
Future<Integer> testGroupIdFuture = sdk.getTestGroupId();
@Nullable Integer testGroupId = testGroupIdFuture.get();
log("My test group id is " + testGroupId);
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
val infoFuture: Future<AdvertiserIdInfo> = sdk.advertiserId
val info: AdvertiserIdInfo = infoFuture.get()
val advertiserId: String? = info.advertiserId
val isLimitedAdTracking: Boolean = info.isLimitedAdTracking
log("My advertiser id is $advertiserId")
log("Ad tracking is limited = $isLimitedAdTracking")

val testGroupIdFuture: Future<Int> = sdk.testGroupId
val testGroupId: Int? = testGroupIdFuture.get()
log("My test group id is $testGroupId")
```
{% endtab %}
{% endtabs %}
