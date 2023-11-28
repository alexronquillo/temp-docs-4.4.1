# Get an advertiser and test group ID

The justtrack SDK can provide you with the IDFA of the user if the user didn't limit ad tracking. Additionally, users are divided into three test groups (1, 2, and 3) based on their IDFV. You can retrieve that test group id from the SDK, implement a different logic for one of the test groups, and then compare the (different) performance of that group with the other test groups on the justtrack dashboard.

```swift
let info = sdk.advertiserIdInfo
let advertiserId: String? = info.advertiserId
let isLimitedAdTracking = info.limitedAdTracking
log("My advertiser id is \(advertiserId)")
log("Ad tracking is limited = \(isLimitedAdTracking)")
let testGroupId: Int? = sdk.testGroupId
log("My test group id is \(testGroupId)")
```
