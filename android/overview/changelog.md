# Changelog

## Version 4.4.1 (30th August 2023)

### Changed

* Methods returning `Future<?>` have been changed to return `Future<Void>` (which always resolves to null).
* All methods returning `Future<T>` have been changed to return `AsyncFuture<T>`, allowing you to await the result from a coroutine using `AsyncFuture.await`.
* `JustTrackSdk.toPromise` has been deprecated in favor of `AsyncFuture.registerPromiseCallback`.

### Bug-Fixes

* Fixed a bug which would cause some methods of the predefined events to get obfuscated.
* Fixed a bug which would cause a crash if the AppSetId info is null.
* The handling of being in restricted mode when targeting Android SDK 33+ has been improved.

## Version 4.4.0 (14th July 2023)

### Added

* `JustTrackSdk.getUserId` was added to retrieve a future for the justtrack user id.

### Changed

* The justtrack SDK now provides the user and test group id without waiting for the justtrack backend.
* `JustTrackSdk.attributeUser` was renamed to `JustTrackSdk.getAttribution`.
* `JustTrackSdk.onDestroy` was renamed to `JustTrackSdk.shutdown`.
* `JustTrackSdk.getAffiliateLink` was renamed to `JustTrackSdk.createAffiliateLink`.
* `JustTrackSdk.publishFirebaseAppInstanceId` was renamed to `JustTrackSdk.setFirebaseAppInstanceId`.
* Support was added to track in-app purchases made by version 6 of the BillingClient library.
* The bounds of many dependencies have been relaxed. OkHttp has been removed as a dependency.

### Removed

* `JustTrackSdk.getCachedAttribution` was removed. Use `getUserId` or `getAttribution` instead.
* The version of `JustTrackSdk.integrateWithIronSource` without any parameters was removed. Use `integrateWithIronSource(IronSourceUserIdSource.JustTrack)` instead.
* `SdkBuilder.setEnableIronSourceIntegration(booean)` was removed. Use `setEnableIronSourceIntegration(boolean, IronSourceUserIdSource.JustTrack)` instead.

### Bug-Fixes

* The justtrack SDK no longer performs disk I/O on the main thread.
* A potential deadlock has been removed when calling `JustTrack.notifyAppStart` from another thread while initializing the justtrack SDK at the same time.

## Version 4.3.8 (29th March 2023)

### Added

* `JustTrack.getInstance` now returns the current SDK instance if one was already created.
* `setAutomaticInAppPurchaseTracking` was added to configure automatic in-app purchase tracking. It is enabled by default.

### Removed

* You can now no longer manually forward in-app purchases. Use the automatic integration instead.

## Version 4.3.7 (17th March 2023)

### Added

* `runCallbacksOnMainThread` was added to the SDK builder to ensure all callbacks from the SDK run on the main thread.

### Bug-Fixes

* Fixed a problem with the Unity Ads integration on minified builds.

## Version 4.3.6 (2nd March 2023)

### Added

* `forwardAdImpression` can now be called with an arbitrary string parameter instead of an `AdFormat`.
* `integrateWithAdColony` was added to automatically track ad impressions from AdColony.
* `integrateWithAppLovin` was added to automatically track ad impressions from AppLovin.
* `integrateWithChartboost` was added to automatically track ad impressions from Chartboost.
* `integrateWithUnityAds` was added to automatically track ad impressions from Unity Ads.

### Changed

* The `AdUnit` enum was renamed to `AdFormat` and the `RewardedVideo` value was renamed to `Rewarded`. `RewardedInterstitial`, `Native`, and `AppOpen` have been added as new values, too.
* The SDK now forwards the App Set ID to the backend.

### Removed

* The deprecated versions of `forwardAdImpression` have been removed.

## Version 4.3.5 (16th December 2022)

### Changed

* Removed the length restriction for the in-app purchase token.

## Version 4.3.4 (2nd December 2022)

### Added

* The SDK now provides a method to track in-app purchases.

### Changed

* You can now attach a currency to the events you are sending.
* If you are forwarding ad impressions, you now have to specify the currency for the revenue of the event.
* The integration with IronSource was changed to allow specifying the user id source.

## Version 4.3.3 (26th October 2022)

### Added

* The SDK now provides the install source to the backend.
* You can now provide a custom user id to the backend.
* Support for deep links was improved.

### Changed

* The AD\_ID permission was added to support Android 13.
* The SDK now imposes restrictions about the data you can send to the backend.
* The integration with Google Ads campaigns was improved.

## Version 4.3.2 (4th August 2022)

### Changed

* The SDK now provides a method `forwardAdImpression` to report ad impressions to the justtrack backend.

### Bug-Fixes

* The SDK now validates the advertiser id before handing it out to an app.

## Version 4.3.1 (7th July 2022)

### Changed

* Updated `androidx.lifecycle:lifecycle-process` from `2.4.1` to `2.5.0`.

### Bug-Fixes

* The SDK now stores internal logs and metrics more carefully to avoid excessive memory usage.

## Version 4.3.0 (6th May 2022)

### Changed

* The SDK now handles timeouts while collecting information for a correct attribution better. This will make it more likely to receive an attribution event after the initial `Future` resolves should you have registered a listener.

## Version 4.2.9 (1st April 2022)

### Changed

* Renamed all occurrences of the Firebase instance id to Firebase app instance id. For example, `publishFirebaseInstanceId` is now called `publishFirebaseAppInstanceId`.

### Bug-Fixes

* The SDK now retrieves the Firebase app instance id instead of the Firebase (installation) instance id during automatic integration.

## Version 4.2.8 (30th March 2022)

### Added

* The SDK can now automatically detect the Firebase instance id of a user and send it to the justtrack backend.

### Changed

* Updated `androidx.lifecycle:lifecycle-process` from `2.4.0` to `2.4.1`.
* Updated `com.squareup.okhttp3:okhttp` from `3.14.9` to `4.9.3`.
* The targeted Android SDK version was changed from 31 to 30.

### Bug-Fixes

* The SDK no longer crashes if it is destroyed while an HTTP request is still running.
* The SDK no longer deadlocks if someone is publishing hundreds of events per second.

## Version 4.2.6 (11th February 2022)

### Changed

* Updated `com.google.android.gms:play-services-ads-identifier` from `18.0.1` to `18.0.1`.
* The minimum Android SDK version was raised from 16 to 21.
* Improved attribution precision.

## Version 4.2.5 (17th December 2021)

### Changed

* Updated `com.google.android.gms:play-services-ads-identifier` from `17.1.0` to `18.0.0`.

## Version 4.2.4 (11th November 2021)

### Added

* The SDK now provides methods to get the advertiser id of the user and the test group id of the user.

### Changed

* The SDK now includes logic to retry fetching the attribution if it failed last time and some time has passed.

### Bug-Fixes

* The SDK no longer blocks upon handling a new intent, instead this is done on a worker thread.
* The SDK no longer deadlocks when processing an event while handling an event produced from the session manager.
* The SDK no longer reports the end of the last session a second time upon the next app start.

## Version 4.2.3 (25th August 2021)

### Changed

* The SDK now automatically handles a network reconnect when fetching an attribution.
* Improved the error messages when you try to use an invalid API token.
* The SDK will now automatically send the revenue value with user events generated from ad impressions to the backend.

## Version 4.2.2 (29th July 2021)

### Changed

* Increased version to align with unity version.

## Version 4.2.1 (28th July 2021)

### Changed

* Increased likelihood that events reach the backend if the user is only using the app for a short time.
* Removed session debouncing logic, this is now completely handled on backend side.

### Bug-Fixes

* Fixed a bug which could cause events to be published multiple times.

## Version 4.2.0 (25th June 2021)

### Changed

* Events tracking the progress of a user for a level or quest have been extended to automatically track the progress of the user and provide the duration the app was active during that time upon completion.
* The SDK no longer requests the `READ_PHONE_STATE` and `ACCESS_WIFI_STATE` permissions.
* The session id is now included in all events sent to the backend.
* `setTrackingId` now takes the provider of the tracking id as a second parameter.
* There can be only a single instance of the SDK from now on.
* Events without a unit are now tracked with a value of 0 (previously there were tracked with a value of 1).

### Removed

* Removed `setRealTime` from all interfaces again as well as realTime event functionality.
* It is no longer possible to set the duration for progression events by hand.

### Bug-Fixes

* Fixed a bug where in some locales string would be converted to upper/lower case with the wrong locale.

## Version 4.1.1 (18th Mai 2021)

### Added

* Added `setRealTime` to `HasCustomDimensions` to send predefined events without any delay.

## Version 4.1.0 (17th Mai 2021)

### Added

* Added `setRealTime` to the `UserEventBuilder` to send events without any delay.

### Removed

* Removed `setCurrentActivity` from `SdkBuilder` interface as it is no longer required.

### Changed

* Replaced `com.google.android.gms:play-services-ads:20.0.0` dependency with `com.google.android.gms:play-services-ads-identifier:17.0.0`.
* Added new dependency `androidx.lifecycle:lifecycle-process:2.3.1`.
* Events are now persisted until they reached the justtrack servers at least once.
* Updated predefined user events:
  * Added `UserCardShowEvent`
  * Added `UserNotificationClickEvent`
  * Added `UserCardClickEvent`
  * Added `AdOfferwallShowEvent`
  * Added `AdInterstitialShowEvent`
  * Added `AdRewardedShowEvent`
  * Added `AdBannerShowEvent`
  * Changed unit on `AdOfferwallCloseEvent` from seconds or milliseconds to count.
  * Changed unit on `AdInterstitialCloseEvent` from seconds or milliseconds to count.
  * Changed unit on `AdRewardedCloseEvent` from seconds or milliseconds to count.
  * Changed unit on `AdBannerCloseEvent` from seconds or milliseconds to count.

### Bug-Fixes

* Fixed the timestamp on events which are constructed at app start and sent later.

## Version 4.0.1 (30th April 2021)

### Bug-Fixes

* Fixed predefined events to publish their custom parameters with the correct dimensions.

## Version 4.0.0 (22nd April 2021)

### Changed

* The implementation for standard events changed significantly. Each standard event now has an associated class and should be created via an instance of this class.
* Standard events now carry built-in dimensions next to the three custom dimensions.
* A `UserEvent` now takes a `EventDetails` instance instead of a `String` as the `name` parameter. You can create a `EventDetails` instance from a string, leaving category, element, and action empty.
* There is now a short delay before sending user events to batch them.
* The justtrack SDK is now licensed under the MIT license.
* Added the current version name (if available) to the app version send to the backend.
* The `JustTrackSdkBuilder` has been simplified and now only depends on an `Activity` or `Application`.

### Added

* Added the `EventDetails` class to group event name, category, element, and action. Only the event name is mandatory.
* Added a static class justtrack to record application start and load times before initializing the SDK.

### Removed

* Removed units for level, currency, and average count.
* Removed `setEnableActivityTracking` from the `SdkBuilder` interface.

## Version 3.2.0 (6th April 2021)

### Changed

* `AttributionResponse#getCampaign()`, `AttributionResponse#getChannel()` and `AttributionResponse#getNetwork()` now return `Campaign`, `Channel` and `Network` interfaces instead of the old `IdString` interface.
* `RetargetingParameters#getUrl()` was replaced by `RetargetingParameters#getUri()` to allow URIs with an unknown protocol (e.g., your app name) to be returned.

### Added

* Added `Campaign`, `Channel` and `Network` interfaces.
* Added `Campaign#getType()` to retrieve the type of a campaign.
* Added `Channel#isIncent()` to retrieve the incent flag of the channel.
* Added support for a preliminary retargeting result. This allows you to already display some welcome screen while the SDK is still waiting for the retargeting attribution to be confirmed by the justtrack backend (as this can take a few seconds). Therefore there is a new interface `PreliminaryRetargetingParameters`, a new method `JustTrackSdk#getPreliminaryRetargetingParameters()`, a new listener interface `PreliminaryRetargetingParametersListener` and a new method `JustTrackSdk#registerPreliminaryRetargetingParametersListener(PreliminaryRetargetingParametersListener)` to register said listener. After obtaining an instance of `PreliminaryRetargetingParameters`, you can `validate()` them to check whether the user did really click on a retargeting campaign or whether they e.g. crafted the `Intent` by hand.

## Version 3.1.0 (16th March 2021)

### Changed

* The SDK now fetches the attribution again after 5 seconds (default, can be changed) after a re-attribution opportunity was detected.

### Added

* Added `setReFetchReAttributionDelaySeconds` to the SDK builder to control when attributions are fetched a second time after a re-attribution opportunity was detected.

## Version 3.0.0 (22nd February 2021)

### Changed

* Renamed various parts of the attribution response:
  * `uuid` -> `userId`
  * `subId` -> `sourceId`
  * `subAppId` -> `sourceBundleId`
  * `adSetId` -> `adsetId` (small S)
  * `recruiter.clientId` -> `recruiter.packageId`
* Added recruiter.userId, recruiter.platform, installId, userType, sourcePlacement to the attribution response
* Updated okhttp dependency to 3.14.9 and play-services-ads dependency to 19.7.0
* `JustTrackSdkBuilder` and `ExistingJustTrackSdkBuilder` now take an `Intent` as the second parameter.
* Starting with this version, you have to implement `Activity#onNewIntent(Intent)` to allow the SDK to detect receiving intents while the app is already running.

### Added

* Added `setInactivityTimeFrame(long)` and `setReAttributionTimeFrame(long)` to the `SdkBuilder` interface. Use it to specify how often we need to fetch for a new (re-)attribution. By default we will fetch a new attribution every 14 days or after the SDK was not initialized for more than 48 hours.
* Added `getRetargetingParameters` to the `JustTrackSdk` interface. It provides the retargeting parameters the app was started with (if any, otherwise `null`).
* Added `registerAttributionListener` and `registerRetargetingParametersListener` to the SDK as well as needed companion types.
* Added `onNewIntent` to the SDK to notify it about new intents (if you don't call `setIntent` in your activity).

## Version 2.4.2 (11th January 2021)

### Changed

* The SDK now waits for some time before retrying requests.
* Allow the advertiser id to be missing if we have a tracking id.

## Version 2.4.1 (11th December 2020)

### Bug-Fixes

* Fixed wrong ad unit names in IronSource integration.

## Version 2.4.0 (8th December 2020)

### Added

* Added `setTrackingId(String)` to `SdkBuilder` interface to provide a tracking id (e.g., the Appsflyer id of the user) as an additional means of identifying the user.
* Added `integrateWithIronSource()` to `JustTrackSdk` interface.
* Added `setEnableIronSourceIntegration(boolean)` to `SdkBuilder` interface.
* When you have the IronSource SDK integrated into your app, you can now automatically sent the userId to IronSource as well as subscribing to impression events as user events by calling `integrateWithIronSource()` or `setEnableIronSourceIntegration(true)`.

## Version 2.3.3 (22th October 2020)

### Changed

* The Logger interface was changed and supports additional fields now.
* The SDK now automatically sends logs to the justtrack backend.
* The SDK now sends additional information about the install referrer to the backend.

### Bug-Fixes

* The SDK now correctly reports the Version of the app upon install.

## Version 2.3.2 (8th October 2020)

### Added

* Added setFirebaseInstanceId and publishFirebaseInstanceId methods to forward a firebase id to the justtrack backend.

## Version 2.3.1 (17th September 2020)

### Changed

* The SDK now automatically publishes SESSION\_APP\_OPEN events each time the SDK detects an app launch.

## Version 2.3.0 (7th September 2020)

### Added

* Added getCachedAttribution method.

### Changed

* The SDK will now automatically fetch a new attribution should the first attribution be younger than 15 minutes to make sure any changes on the backend side are propagated to the app eventually.
* An attribution now includes the campaign ID and name instead of just the ID.

### Bug-Fixes

* Fixed a race condition resulting in invalid date strings.

### Removed

* Removed the old event tracking ("notify"-calls) as the server side no longer accepts these messages.
* Removed `getAppInstallCount` method.

## Version 2.2.3 (29th July 2020)

### Added

* You can now get an existing SDK instance if one exists
* You can specify an activity which is currently active should you instantiate the SDK while the activity is already running (see `SdkBuilder#setCurrentActivity`).

### Changed

* We are now sending the Android version instead of the Linux kernel version to the backend.

## Version 2.2.2 (17th July 2020)

### Added

* Added `level` and `currency` units.

## Version 2.2.1 (13th July 2020)

### Added

* Added `setEnableBroadcastReceiver(boolean)` and `setEnableActivityTracking(boolean)` methods to `SdkBuilder`.

### Changed

* Changed package name to `io.justtrack` and updated domains to `*.justtrack.io`.
* It is no longer legal to call `get()` of a future returned by the SDK on the main thread.

***
