# Get an attribution

The justtrack SDK determines on each start (if necessary and not cached) the identity and attribution of a user. This includes information about the ad the user clicked on (if any), to which campaign that ad belongs as well as the network and channel of that campaign. The following example shows you you can access the campaign used to acquire the current user: After initializing the SDK you can simply call `getAttribution` to retrieve an attribution containing your justtrack user id as well as information about the source of the user.

{% tabs %}
{% tab title="Java" %}
```java
Future<AttributionResponse> response = sdk.getAttribution();
String campaignName = response.get().getCampaign().getName();
log("The campaign name is " + campaignName);
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
val response = sdk.attribution
val campaignName = response.get().campaign.name
log("The campaign name is $campaignName")
```
{% endtab %}
{% endtabs %}

If you are only using the justtrack dashboard to keep track of your install sources, you don't need to call `getAttribution` yourself.

To wait for the returned `Future` to resolve to a value, you need to call `get` on it. This will block the current thread until the future resolves or throws an exception.
