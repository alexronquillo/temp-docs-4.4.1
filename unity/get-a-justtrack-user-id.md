# Get a justtrack user ID

You can retrieve the justtrack user ID using `JustTrackSDK.GetUserId`:

```cs
using JustTrack;

JustTrackSDK.GetUserId((userId) => {
    Debug.Log(userId); // use the justtrack user ID somehow...
}, (error) => {
    // the SDK is not correctly configured or there was a problem generating the justtrack user ID
});
```
