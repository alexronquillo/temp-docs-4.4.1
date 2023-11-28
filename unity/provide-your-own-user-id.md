# Provide your own user Id

If you already have a mechanism to assign a unique id to each user, you can share this information with the justtrack SDK. This then allows the backend of the justtrack SDK to associate events received from third parties via that user id with the correct user on justtrack side. You can provide as an id multiple times, the justtrack SDK takes care about sending it to the justtrack backend as needed.

You can supply the own user id at any time while your game is running. Should your own user id change for some reason, you have to supply the new value to the justtrack SDK again:

```cs
using JustTrack;

string ownUserId = /* get your own user id from your game */;
JustTrackSDK.SetCustomUserId(ownUserId);
```

A custom user id must be shorter than 4096 characters and only consist of printable ASCII characters (U+0020 to U+007E).
