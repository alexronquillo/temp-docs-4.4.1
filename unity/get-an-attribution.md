# Get an attribution

The justtrack SDK determines on each start (if necessary and not cached) the identity and attribution of a user. This includes information about the ad the user clicked on (if any), to which campaign that ad belongs as well as the network and channel of that campaign. The following example shows you you can access the campaign used to acquire the current user:

```cs
using JustTrack;

JustTrackSDKBehaviour.GetAttribution((attribution) => {
    Debug.Log(attribution.Campaign.Name);
}, (error) => {
    // handle error and wait for the attribution request to be retried automatically
});
```

If the attribution failed because we could not reach the justtrack backend (user might be offline), we will automatically retry it as soon as network connectivity is restored. In that case, you need to subscribe to the `OnAttributionResponse` callback to be notified about the attribution. Keep in mind that the callback can also be called if a user is attributed to a retargeting campaign.

```csharp
using JustTrack;

JustTrackSDK.OnAttributionResponse += (attribution) => {
    if (!userLoaded) {
      LoadUser(attribution.UserId);
    }
};
```

The `attribution` variable will be an instance of the `AttributionResponse` class. It looks like this:

```csharp
namespace JustTrack {
    public class AttributionResponse {
        public string UserId { get; }
        public string InstallId { get; }
        public AttributionCampaign Campaign { get; }
        public string UserType { get; }
        public string Type { get; }
        public AttributionChannel Channel { get; }
        public AttributionNetwork Network { get; }
        public string SourceId { get; }
        public string SourceBundleId { get; }
        public string SourcePlacement { get; }
        public string AdsetId { get; }
        public AttributionRecruiter Recruiter { get; }
        public DateTime CreatedAt { get; }
    }

    public class AttributionCampaign {
        public int Id { get; }
        public string Name { get; }
        public string Type { get; }
    }

    public class AttributionChannel {
        public int Id { get; }
        public string Name { get; }
        public bool Incent { get; }
    }

    public class AttributionNetwork {
        public int Id { get; }
        public string Name { get; }
    }

    public class AttributionRecruiter {
        public string AdvertiserId { get; }
        public string UserId { get; }
        public string PackageId { get; }
        public string Platform { get; }
    }
}
```
