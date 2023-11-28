# Integrate third-party SDKs

## IronSource

The SDK can automatically integrate with the IronSource SDK and forward the revenue of each ad impression to the justtrack backend. Additionally, the SDK can set the user id in the IronSource SDK to the justtrack user id or an id of your choice to enable the justtrack backend to match the ad impressions with the users during a revenue import. You can either configure the SDK to integrate with the IronSource SDK upon instantiation of your instance of the justtrack SDK or later perform an explicit call.

If you tell the SDK builder to integrate with the IronSource SDK you will not get notified about whether the integration failed or was successful. As the integration depends on the attribution, it is done asynchronously in the background, so it might not be done after the call to `build()` returns. You can enable the integration like this:

```swift
let builder = /* create the builder somehow */ JustTrackSdkBuilder(apiToken: "prod-...")
let sdk = builder
        .set(enableIronSourceIntegration: true, userIdSource: .justtrack)
        // or, if you want to provide your own user id:
        .set(enableIronSourceIntegration: true, userIdSource: .customUserId("...your user id..."))
        // other options...
        .build()
```

Each call to `set(enableIronSourceIntegration:userIdSource:)` overrides the previous settings, only the last call before instantiating the justtrack SDK applies.

Alternatively, you can trigger the integration yourself after creating an instance of the justtrack SDK. Only one method (configuring the builder or setting up the integration manually) is needed. To set up the integration manually, take a look at the following code snippet:

```swift
// kick off the integration, will eventually complete as long as attribution succeeds
sdk.integrateWithIronSource(userIdSource: .justtrack)
// or, if you want to provide your own user id:
sdk.integrateWithIronSource(customUserId: "...your user id...")
```

If you supply your own user id to IronSource (either by letting the justtrack SDK forward it or by specifying `.noUserId`), you need to provide it as a custom user id to the justtrack SDK. Otherwise, we will not be able to import the IronSource revenue later on and match ad impressions to the correct user in every case.

## Firebase

If you are using the Firebase SDK next to the justtrack SDK, you can use the justtrack SDK to send the Firebase app instance id of a user to the justtrack backend. You can then later send events from the justtrack backend to Firebase to measure events happening outside of your app. To send these events, the justtrack backend needs the Firebase app instance id of the user. You can send this by calling `sdk.set(firebaseAppInstanceId: firebaseAppInstanceId)`, but of course this requires you to add additional boilerplate code to your app.

To help you here, the justtrack SDK offers the `sdk.integrateWithFirebase()` method.
