# Get the justtrack user ID

Each user is assigned a unique user ID by the justtrack SDK which you can access inside your app. You can use this ID to recognize returning users in your app again. Should you already assign a unique ID to your users, you can also link your ID to the justtrack user ID.

The justtrack SDK provides you with a unique ID for each user. You can retrieve this ID by accessing the `userId` property on the justtrack SDK:

```swift
let userId: String = sdk.userId
log("My user id is \(userId)")
```

## Provide your own user ID

If you already have a mechanism to assign a unique ID to each user, you can share this information with the justtrack SDK. This then allows the backend of the justtrack SDK to associate events received from third parties via that user id with the correct user on justtrack side. You can provide an id multiple times, the justtrack SDK takes care about sending it to the justtrack backend as needed.

You can supply your own user id when constructing the SDK as well as sending it later once your app is already running for some time. Should your own user id change for some reason, you have to supply the new value to the justtrack SDK again:

```swift
var ownUserId = /* get your own user id from your app */
let builder = /* create the builder somehow */ JustTrackSdkBuilder(apiToken: "prod-...")
let sdk = builder
        .set(customUserId: ownUserId)
        // other options...
        .build()

ownUserId = /* new user id for some reason */
sdk.set(customUserId: ownUserId)
```

A custom user id must be shorter than 4096 characters and only consist of printable ASCII characters (U+0020 to U+007E).
