# Forward in-app purchases

If the user performs an in-app product or subscription purchase, you can forward the purchase via the justtrack SDK to our backend. By default, this is already enabled and every purchase of the user will be forwarded automatically. You can enable or disable the integration by calling `SetAutomaticInAppPurchaseTracking` or by configuring it in the justtrack settings.

```cs
JustTrackSDK.SetAutomaticInAppPurchaseTracking(forwardPurchasesAutomatically);
```

When enabled, all purchases will be reported automatically to the justtrack backend and, if you have correctly configured purchase validation, show up as revenue for your app.

You have to [configure a service account](https://docs.justtrack.io/product-features/cost-and-revenue-aggregation/in-app-purchase-revenue/google-play-store) and [a shared secret](https://docs.justtrack.io/product-features/cost-and-revenue-aggregation/in-app-purchase-revenue/apple-app-store) to correctly validate purchases.
