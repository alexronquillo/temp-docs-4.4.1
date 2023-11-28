# Get an advertiser and test group Id

The justtrack SDK can provide you with the advertiser id (GAID or IDFA) of the user if the user didn't limit ad tracking. Additionally, users are divided into three test groups (1, 2, and 3) based on their advertiser id (Android) or IDFV (iOS). You can retrieve that test group id from the SDK, implement a different logic for one of the test groups, and then compare the (different) performance of that group on the justtrack dashboard.

```cs
using JustTrack;

JustTrackSDK.GetAdvertiserIdInfo((info) => {
  // info.AdvertiserId will contain the advertiser id (lowercase) or null if it is not available
  // info.IsLimitedAdTracking tells you if the user opted out of ad tracking (the advertiser id might be null in that case depending on the Android version)
}, (error) => {
    // we failed to retrieve the advertiser id - this is a different case than the user limiting ad tracking
});
JustTrackSDK.GetTestGroupId((testGroupId) => {
    // testGroupId is either 1, 2, 3, or null
    // if it is null we could not retrieve an advertiser id or IDFV internally
}, (error) => {
    // we failed to determine the test group id. This will happen if retrieving the advertiser id fails
});
```
