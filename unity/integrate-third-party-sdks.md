# Integrate third-party SDKs

## IronSource

If you are using IronSource to diplay ads to your users, you can integrate the justtrack SDK with the IronSource SDK. In that case the justtrack SDK will initialize the IronSource SDK on your behalf and pass the justtrack user id to the IronSource SDK. For each platform, supply the correct app key in the prefab and enable all ad units you need:

<figure><img src="../.gitbook/assets/Screenshot 2023-09-15 at 14.05.45.png" alt="" width="563"><figcaption></figcaption></figure>

```csharp
using JustTrack;

JustTrackSDKBehaviour.OnIronSourceInitialized(() => {
  // callback called exactly once as soon as ironSource has been initialized.
  // the callback is also called if ironSource has already been initialized,
  // so you can safely setup banners or other ads in here.

  JustTrackSDKBehaviour.IronSourceInitialized == true; // always true at this point
});
```

You can then access `JustTrack.JustTrackSDKBehaviour.IronSourceInitialized` to check if IronSource already has been initialized or use `JustTrack.JustTrackSDKBehaviour.OnIronSourceInitialized` to schedule a callback once it has been initialized (the callback is also invoked should IronSource already have been initialized, you do not need check for that yourself prior to this. The callback is always asynchrounously called on the main thread).

## Firebase

If you are using the Firebase SDK next to the justtrack SDK, you can use the justtrack SDK to send the Firebase app instance id of a user to the justtrack backend. You can then later send events from the justtrack backend to Firebase to measure events happening outside of your app or game. To send these events, the justtrack backend needs the Firebase app instance id of the user. You can send this by calling `JustTrackSDK.SetFirebaseAppInstanceId(firebaseAppInstanceId)`, but of course this requires you to add additional boilerplate code to your app or game.

Go to the Firebase section of the justtrack SDK configuration and search for the "Enable Firebase Integration" setting. Here you can enable whether the justtrack SDK should automatically perform the call to `SetFirebaseAppInstanceId` with the correct value for you on Android as well as iOS.

![](https://sdk.justtrack.io/docs/justtrack-sdk/unity/firebaseConfiguration-6406e120-fc75-4a3c-9b69-20de3103853e.png)
