# Revenue

The justtrack SDK can track advertisement and in-app purchase revenue for you. In-app purchase revenue is always tracked automatically, advertising revenue can be tracked automatically from the IronSource SDK right now (see [Third-Party Integrations](broken-reference)). If you are using another SDK to deliver ads for your users, you can track the impressions manually.

## Forwarding in-app purchases

If the user performs an in-app product or subscription purchase, you can forward the purchase via the justtrack SDK to our backend. By default, this is already enabled and every purchase of the user will be forwarded automatically. You can enable or disable the integration by calling `setAutomaticInAppPurchaseTracking` when creating the SDK instance or on the SDK:

{% tabs %}
{% tab title="Java" %}
```java
SdkBuilder builder = /* create the builder somehow */ new JustTrackSdkBuilder(this, BuildConfig.APP_KEY);
JustTrackSdk sdk = builder
        // configure the SDK when creating an instance:
        .setAutomaticInAppPurchaseTracking(forwardPurchasesAutomatically)
        // other options...
        .build();
// configure an already existing SDK instance:
sdk.setAutomaticInAppPurchaseTracking(forwardPurchasesAutomatically);
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
val builder: SdkBuilder = /* create the builder somehow */ JustTrackSdkBuilder(this, BuildConfig.APP_KEY)
val sdk: JustTrackSdk = builder
    // configure the SDK when creating an instance:
    .setAutomaticInAppPurchaseTracking(forwardPurchasesAutomatically)
    // other options...
    .build()
// configure an already existing SDK instance:
sdk.setAutomaticInAppPurchaseTracking(forwardPurchasesAutomatically)
```
{% endtab %}
{% endtabs %}

When enabled, all purchases will be reported automatically to the justtrack backend and, if you have correctly configured purchase validation, show up as revenue for your app.

You have to [configure a service account](https://docs.justtrack.io/product-features/cost-and-revenue-aggregation/in-app-purchase-revenue/google-play-store) to correctly validate purchases.

## Forwarding Ad Impressions

The justtrack SDK can forward data about advertisement impressions to the justtrack backend to track the advertisement revenue of your app. For IronSource see the setup guide for combining justtrack SDK with IronSource SDK first, [here](broken-reference).

Right now, we automatically keep track of ad views for the main ad providers like Unity, Applovin, IronSource, Adjoe, Adcolony, and Chartboost. If your ad provider isn't in this list, you can still send us ad view info on your own by using the "forwardAdImpression" function:

{% tabs %}
{% tab title="Java" %}
```java
JustTrackSdk sdk = JustTrack.getInstance();
sdk.forwardAdImpression(adFormat, adSdkName, adNetwork, placement, abTesting, segmentName, instanceName, revenue, currency);
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
val sdk: JustTrackSdk = JustTrack.getInstance()
sdk.forwardAdImpression(adFormat, adSdkName, adNetwork, placement, abTesting, segmentName, instanceName, revenue, currency)
```
{% endtab %}
{% endtabs %}

You need to provide the following parameters to such a call:

* **adFormat**: The format of the ad.
* **adSdkName**: The name of the SDK which provided the event, for example "ironsource" or "admob".
* **adNetwork**: The name of the ad network which provided the ad. May be null.
* **placement**: The placement of the ad. May be null.
* **abTesting**: The test group of the user if we are A/B testing. May be null.
* **segmentName**: The name of the segment of the ad. May be null.
* **instanceName**: The name of the instance of the ad. May be null.
* **revenue**: The amount of revenue this ad generated. May be zero but must not be negative.
* **currency**: The currency of the revenue amount. Should be a three letter uppercase ISO 4217 string.

Capitalization of the parameter values is important (i.e., "Ironsource" would not work as ad SDK name). Upon success, `forwardAdImpression` returns true, if a parameter was invalid, false is returned.

