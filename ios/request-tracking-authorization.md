# Request tracking authorization

Starting with iOS 14, the justtrack SDK can only access the IDFA if the user allowed the app to track them. By default, the justtrack SDK does not request this permission and can (albeit somewhat limited) still attribute the user to the correct campaign. To improve this precision, you can ask the user for permission to track them.

You can use `JustTrack.requestTrackingAuthorization` to prompt the user for permission on iOS 14+. On earlier versions it will fall back to a simple check whether the user allowed tracking at all (there is nothing to prompt the user for). It will also track the decision of the user (only for the first time a user got a prompt), so you can check, how many of your users actually grant you permission to track them. Initialization of the justtrack SDK could then look like this:

```swift
JustTrack.requestTrackingAuthorization { success in
    // success is true if we got permission, false otherwise. You can use this for your own logic,
    // we will ignore this here
    do {
        // create the instance of the JustTrack SDK only after trying to get permission to read the IDFA
        let sdk = try JustTrackSdkBuilder(apiToken: apiToken).build()
        // use the SDK for your app
    } catch {
        // your API token had an invalid format
    }
}
```

You have to set the `NSUserTrackingUsageDescription` \[1] key in your `Info.plist` file when requesting the tracking permission from the user.

## References

* \[[1](https://support.appsflyer.com/hc/en-us/articles/207032066#introduction)] [https://developer.apple.com/documentation/bundleresources/information\_property\_list/nsusertrackingusagedescription](https://developer.apple.com/documentation/bundleresources/information\_property\_list/nsusertrackingusagedescription)
