# Configure your iOS application

The justtrack SDK will call `SKAdNetwork.registerAppForAdNetworkAttribution` upon startup. This will cause the device to send a postback describing the attribution of the app after 24 to 48 hours. The SDK also registers the justtrack backend as the receiver for the copy of the postback for the advertised app (iOS 15+).

## Ad Tracking Transparency

Starting with iOS 14, the justtrack SDK can only access the IDFA if the user allowed the app to track them. By default the justtrack SDK does not request this permission and can (albeit somewhat limited) still attribute the user to the correct campaign. You can ask the user for permission to track them to improve the precision of the attribution.

The request for this permission needs to happen _before_ the justtrack SDK is initialized (as we otherwise will not get an IDFA for the user). To help you with this you can configure the justtrack SDK to request this permission on your behalf upon app start. Go to the iOS settings of the SDK, enable "Request Tracking Permission", and set a message to be displayed to your users (so they know why you need the permission):

![Enable Tracking Permission](https://sdk.justtrack.io/docs/justtrack-sdk/unity/enableTrackingPermission-9f8df63e-8f71-429a-8a01-877f52388abe.png)

If you provide a message, it will overwrite the `NSUserTrackingUsageDescription` property in your `Info.plist` file (see also [https://developer.apple.com/documentation/bundleresources/information\_property\_list/nsusertrackingusagedescription](https://developer.apple.com/documentation/bundleresources/information\_property\_list/nsusertrackingusagedescription)). If you instead set "Ignore Empty Tracking Description" the justtrack SDK assumes you already provide that setting somewhere else and does no longer warn about the missing description.

![Custom Tracking Description](https://sdk.justtrack.io/docs/justtrack-sdk/unity/customTrackingDescription-f5ed7468-f494-46e0-bb6d-1441cc00f754.png)

The justtrack SDK can also forward the answer of the user for the permission request to the Facebook Audience Network (via [https://developers.facebook.com/docs/audience-network/setting-up/platform-setup/ios/advertising-tracking-enabled/](https://developers.facebook.com/docs/audience-network/setting-up/platform-setup/ios/advertising-tracking-enabled/)). If you don't use the Facebook Audience Network (FAN), you can leave the setting at "No Integration". If you are using the Unity version of the FAN, select "Unity Integration", otherwise, select "Native Integration". In both cases a small code file will be added to your game (you need to allow injecting the code) which is used to forward the permission upon runtime.

![Configure Facebook Audience Network Integration](https://sdk.justtrack.io/docs/justtrack-sdk/unity/configureFacebookAudienceNetworkIntegration-e69d5327-6717-44ed-bcd9-82c501ac92b6.png)

If you need to integrate with additional frameworks, you can use `JustTrackSDK.OnTrackingAuthorization` to be notified about the decision of the user:

```cs
using JustTrack;

JustTrackSDK.OnTrackingAuthorization((authorized) => {
    if (authorized) {
        // access to the GAID/IDFA has been authorized
    } else {
        // access to the GAID/IDFA has been forbidden
    }
});
```
