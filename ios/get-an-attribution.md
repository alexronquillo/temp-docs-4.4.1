# Get an attribution

The justtrack SDK determines on each start (if necessary and not cached) the identity and attribution of a user. This includes information about the ad the user clicked on (if any), to which campaign that ad belongs as well as the network and channel of that campaign. The following example shows you you can access the campaign used to acquire the current user: After initializing the SDK you can access the `attribution` field to retrieve an attribution containing your justtrack user id as well as information about the source of the user.

```swift
sdk.attribution.observe(using: { result in
    switch result {
    case .failure(let error):
        // handle error
    case .success(let response):
        log("My user id is " + response.getUuid().uuidString)
    }
})
```

If you are only using the justtrack dashboard to keep track of your install sources, you can ignore the result of `attribution` in your app. To get the information about the user attribution in your app you need to call `observe` on the future returned by `attribution`. This will call the provided callback with the `Result` of the attribution of the user.

If the attribution can not be performed because the user is offline, it will eventually fail. The SDK will then wait for a new internet connection and retry the attribution. An attribution obtained after the returned `Future` resolved to a failure value will not update the future. You have to subscribe to updates of the attribution using `register(attributionListener:)` to receive the attribution.

