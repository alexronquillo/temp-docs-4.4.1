# iOS & SKAdNetwork

{% hint style="warning" %}
Justtrack supports the iOS 14 SKAdNetwork Integration starting from SDK version **2.4.2**

Please make sure you are using this version.
{% endhint %}

The iOS attribution uses the IDFA - if available through ATT - and other attributes if unavailable for probabilistic attribution.

For attribution with iOS 14 and above, a signature is added to each ad click. Apple will be able to verify which ad the user is coming from upon the first app open of a user. A postback without user identification is sent back to the ad network for attribution.

Upon app launch, two methods have to be implemented. We already support these two methods in our justtrack SDK.

* `registerAppForAdNetworkAttribution()`: This method is used to register the user for attribution
* `updateConversionValue(_:)`: This method is used to update the user’s conversion value. Updates will only be applied if the value is higher than the previous value.

{% hint style="info" %}
We need your **network id** to be able to do the attribution. Please contact your Account Manager directly to provide this information.\
Please check out the official documentation provided by Apple for further clarification: [https://developer.apple.com/documentation/storekit/skadnetwork](https://developer.apple.com/documentation/storekit/skadnetwork)
{% endhint %}

For web traffic, as well as all cases where the IDFA is still available, the current attribution flow will apply. We attribute using the IDFA and anonymized fingerprinting and send back all relevant information via a postback.

## Postbacks

Postbacks for installs and post install events can be provided by justtrack no matter if the IDFA is present or not. In the later case, without any user identifiers. Please refer to the [postback parameters](./#postbacks) for a detailed list.

If the IDFA is **not** present, the SKAdNetwork basic postback flow is the following:

1. The postback is sent to the attributed ad network by Apple after the timer expires.
2. The ad network forwards the postback to justtrack either through:
   1. Enriching the postback with specific data (such as campaignID, campaign name, IP address, …) and forward it to justtrack
   2. **Most common:** configuring a direct redirect of the postback to justtrack. On redirect, justtrack replies with the http status code to Apple.
3. justtrack enriches, validates and decodes the postback and sends it to the ad network.

justtrack sends one install postback, and several additional ones for in-app events based on the number of conversion value changes.\\

### Endpoint to forward Postbacks

Please forward the postbacks to this endpoint:

| Endpoint               | [`https://skadpostback.justtrack.io/postbacks`](https://skadpostback.justtrack.io/postbacks) |
| ---------------------- | -------------------------------------------------------------------------------------------- |
| HTTP method            | `POST`                                                                                       |
| Accepted content types | `application/json`                                                                           |
| Return codes           | `200 OK`, `400 Bad Request`, `500 Internal Server Error`                                     |

The body should contain a payload like:

```
 { 
  "version": "3.0", 
  "ad-network-id": "example123.skadnetwork", 
  "campaign-id": 42, 
  "transaction-id": "6aafb7a5-0170-41b5-bbe4-fe71dedf1e28", 
  "app-id": 525463029, 
  "attribution-signature": "MEYCIQD5eq3AUlamORiGovqFiHWI4RZT/PrM3VEiXUrsC+M51wIhAPMANZA9c07raZJ64gVaXhB9+9yZj/X6DcNxONdccQij", 
  "redownload": true, 
  "source-app-id": 1234567891, 
  "fidelity-type": 1, 
  "conversion-value": 20,
  "did-win": true,
  "sourceCampaignId" : "1234",
  "sourceCampaignName" : "example"
}
```

#### Enriching the Postback

When forwarding the postback to justtrack, please add the following parameters to the payload. By enriching the postback and adding the campaign information justtrack knows the exact campaign a user needs to be attributed to.

| Parameter                     | Description                                                       |
| ----------------------------- | ----------------------------------------------------------------- |
| `sourceCampaignId`            | Ad network internal campaign ID                                   |
| `sourceCampaignName`          | Ad network internal campaign name                                 |
| `pbReceivedAdNetworkDateTime` | Time ad network received the SKAd postback in YYYY-MM-DD HH:MM:SS |

### SKAdNetwork Postback Parameters

When the SKAd postback is reveived by justtrack, it can be forwarded to the ad network including the following parameters.

| SKAd Postback Parameter | Description                                                                                                                                                                                                                                                                                                                                                |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `{transaction-id}`      | A unique value for this validation; use to deduplicate install validation messages                                                                                                                                                                                                                                                                         |
| `{conversion-value}`    | An unsigned 6-bit value that the installed app provided by calling [`updateConversionValue(_:)`](https://developer.apple.com/documentation/storekit/skadnetwork/3566697-updateconversionvalue). Note: The `conversion-value` only appears in the postback if the installed app provides it, and if providing the parameter meets Apple’s privacy threshold |
| `{fidelity-type}`       | A value of `0` indicates a view-through ad presentation; a value of `1` indicates a StoreKit-rendered ad                                                                                                                                                                                                                                                   |
| `{redownload}`          | true/false: indicates whether the user downloaded the app from the App Store again                                                                                                                                                                                                                                                                         |
| `{pbReceivedJustTrack}` | Time justtrack received the postback from ad network                                                                                                                                                                                                                                                                                                       |

## Probabilistic Attribution

If no IDFA is present, a probabilistic attribution complying with Apple’s privacy guidelines is done.

Please refer to the [documentation about tracking links](./#required-tracking-parameters) to see which parameters you can add.\\
