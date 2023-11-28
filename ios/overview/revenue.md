# Revenue

The justtrack SDK can track advertisement and in-app purchase revenue for you. In-app purchase revenue is always tracked automatically, advertising revenue can be tracked automatically from the IronSource SDK right now (see [Third-Party Integrations](broken-reference)). If you are using another SDK to deliver ads for your users, you can track the impressions manually.

## Forwarding in-app purchases

If the user performs an in-app product or subscription purchase, you can forward the purchase via the justtrack SDK to our backend. By default, this is already enabled and every purchase of the user will be forwarded automatically. You can enable or disable the integration by calling `set(automaticInAppPurchaseTracking:)` when creating the SDK instance or on the SDK:

```swift
let builder = /* create the builder somehow */ JustTrackSdkBuilder(apiToken: "prod-...")
let sdk = builder
        // configure the SDK when creating an instance:
        .set(automaticInAppPurchaseTracking: forwardPurchasesAutomatically)
        // other options...
        .build()
// configure an already existing SDK instance:
sdk.set(automaticInAppPurchaseTracking: forwardPurchasesAutomatically)
```

When enabled, all purchases will be reported automatically to the justtrack backend and, if you have correctly configured purchase validation, show up as revenue for your app.

You have to [configure a shared secret](https://docs.justtrack.io/product-features/cost-and-revenue-aggregation/in-app-purchase-revenue/apple-app-store) to correctly validate purchases.

## Forwarding Ad Impressions

The justtrack SDK can forward data about ad impressions to the justtrack backend to track the ad revenue of your app. If you are using the IronSource integration, this will already happen automatically for IronSource ad impressions. For other ad network integrations (AdMob for example), you have to provide the ad impression events yourself by calling `forwardAdImpression`:

```swift
sdk.forwardAdImpression(adFormat: adFormat, adSdkName: adSdkName, adNetwork: adNetwork, placement: placement, abTesting: abTesting, segmentName: segmentName, instanceName: instanceName, revenue: revenue)
```

You need to provide the following parameters to such a call:

* adFormat: The format of the ad.
* adSdkName: The name of the SDK which provided the event, for example "ironsource" or "admob".
* adNetwork: The name of the ad network which provided the ad. May be null.
* placement: The placement of the ad. May be null.
* abTesting: The test group of the user if we are A/B testing. May be null.
* segmentName: The name of the segment of the ad. May be null.
* instanceName: The name of the instance of the ad. May be null.
* revenue: The amount of revenue this ad generated. May be zero but must not be negative.

Capitalization of the parameter values is important (i.e., "Ironsource" would not work as ad SDK name). Upon success, `forwardAdImpression` returns true, if a parameter was invalid, false is returned.
