# Integrate third-party SDKs

## IronSource

The SDK can automatically integrate with the IronSource SDK and publish UserEvents for each ad impression. Additionally, the SDK can set the user id in the IronSource SDK to the justtrack user id or an id of your choice to enable the justtrack backend to match the ad impressions with the users during a revenue import. You can either configure the SDK to integrate with the IronSource SDK upon instantiation of your instance of the justtrack SDK or later perform an explicit call.

The integration uses reflection to locate and setup the IronSource SDK. It was tested with version `7.0.3.1` of `com.ironsource.sdk:mediationsdk`. It should be compatible with version 6 as well as 7 of the IronSource SDK though.

If you tell the SDK builder to integrate with the IronSource SDK you will not get notified about whether the integration failed or was successful. As the integration depends on the attribution, it is done asynchronously in the background, so it might not be done after the call to `build()` returns. You can enable the integration like this:

{% tabs %}
{% tab title="Java" %}
```java
SdkBuilder builder = /* create the builder somehow */ new JustTrackSdkBuilder(this, BuildConfig.APP_KEY);
JustTrackSdk sdk = builder
        .setEnableIronSourceIntegration(true, UserIdSource.JustTrack)
        // or, if you want to provide your own user id:
        .setEnableIronSourceIntegration(true, "...your user id...")
        // other options...
        .build();
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
val builder: SdkBuilder = /* create the builder somehow */ JustTrackSdkBuilder(this, BuildConfig.APP_KEY)
val sdk: JustTrackSdk = builder
    .setEnableIronSourceIntegration(true, UserIdSource.JustTrack)
    // or, if you want to provide your own user id:
    .setEnableIronSourceIntegration(true, "...your user id...")
    // other options...
    .build()
```
{% endtab %}
{% endtabs %}

Each call to `setEnableIronSourceIntegration` overrides the previous settings, only the last call before instantiating the justtrack SDK applies.

Alternatively, you can trigger the integration yourself after creating an instance of the justtrack SDK. Only one method (configuring the builder or setting up the integration manually) is needed. To set up the integration manually, take a look at the following code snippet:

{% tabs %}
{% tab title="Java" %}
```java
JustTrackSdk sdk = JustTrack.getInstance();
// kick off the integration, it will eventually complete as long as getting a user ID succeeds
AsyncFuture<Void> integrationFuture = sdk.integrateWithIronSource(UserIdSource.JustTrack);
// you need for the integration to be finished before you initialize the IronSource SDK:
integrationFuture.get();
// if you want to provide your own user id, you don't need to wait for any futures:
sdk.integrateWithIronSource("...your user id...");
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
val sdk: JustTrackSdk = JustTrack.getInstance()
// kick off the integration, it will eventually complete as long as getting a user ID succeeds
val integrationFuture: AsyncFuture<Void> = sdk.integrateWithIronSource(UserIdSource.JustTrack)
// you need for the integration to be finished before you initialize the IronSource SDK:
integrationFuture.await()
// if you want to provide your own user id, you don't need to wait for any futures:
sdk.integrateWithIronSource("...your user id...")
```
{% endtab %}
{% endtabs %}

If you supply your own user id to IronSource (either by letting the justtrack SDK forward it or by specifying `UserIdSource.NoUserId`), you need to provide it as a custom user id to the justtrack SDK. Otherwise, we will not be able to import the IronSource revenue later on and match ad impressions to the correct user in every case.

## Firebase

If you are using the Firebase SDK next to the justtrack SDK, you can use the justtrack SDK to send the Firebase app instance id of a user to the justtrack backend. You can then later send events from the justtrack backend to Firebase to measure events happening outside of your app. To send these events, the justtrack backend needs the Firebase App instance id of the user. You can send this by calling `sdk.publishFirebaseAppInstanceId(firebaseAppInstanceId)`, but of course this requires you to add additional boilerplate code to your app.

To help you here, the justtrack SDK detects the presence of the Firebase SDK at runtime automatically and automatically performs a call to `publishFirebaseAppInstanceId` with the correct value. If you don't want this behavior, you can disable it by using `builder.setEnableFirebaseIntegration(false)` when constructing an instance of the justtrack SDK.
