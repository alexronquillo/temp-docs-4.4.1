# Forward ad impressions

The justtrack SDK can forward data about ad impressions to the justtrack backend to track the ad revenue of your app. If you are using the IronSource integration, this will already happen automatically for IronSource ad impressions. For other ad network integrations (AdMob for example), you have to provide the ad impression events yourself by calling `ForwardAdImpression`:

```cs
JustTrackSDK.ForwardAdImpression(adFormat, adSdkName, adNetwork, placement, abTesting, segmentName, instanceName, revenue);
```

You need to provide the following parameters to such a call:

* adFormat: The format of the ad.
* adSdkName: The name of the SDK which provided the event, for example "ironsource" or "admob".
* adNetwork: The name of the ad network which provided the ad. May be null.
* placement: The placement of the ad. May be null.
* abTesting: The test group of the user if we are A/B testing. May be null.
* segmentName: The name of the segment of the ad. May be null.
* instanceName: The name of the instance of the ad. May be null.
* revenue: The amount of revenue this ad generated together with a currency. May be zero but must not be negative.

Capitalization of the parameter values is important (i.e., "Ironsource" would not work as ad SDK name). Upon success, `ForwardAdImpression` returns true, if a parameter was invalid, false is returned.
