# Get an affiliate link

Invite other users to also use our app (i.e., get an affiliate link for the current user):

```csharp
var onSuccess = (string link) => {
  ...
};

var onFailure = (string msg) => {
  ...
};

JustTrackSDK.CreateAffiliateLink(channel, onSuccess, onFailure);
```
