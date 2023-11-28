# Set up the SDK

The justtrack SDK offers extensive tracking capabilities for attribution, events, and various other functionalities within your iOS application. In this guide, you'll learn to:

* Add the justtrack SDK to your app
* Copy your API token
* Instantiate a JustTrackSdk object
* Shutdown the SDK instance

## Add the SDK

#### CocoaPods

You can use [CocoaPods](https://cocoapods.org/) to add the justtrack SDK to your app. Add the following line to your `Podfile`:

```ruby
pod 'JustTrackSDK', '4.4.1'
```

Afterwards, run `pod install` in your project to actually download and install the justtrack SDK for your project.

{% hint style="info" %}
We support Xcode version 14.0 and later.
{% endhint %}

## Copy your API Token

Before integrating the justtrack SDK into your app, you need to obtain an API token. Follow these steps to get an API token:

1. Go to the [justtrack dashboard](https://dashboard.justtrack.io/admin/products) and log in.
2. Navigate to your app.
3. Locate the API token for your app. It should be displayed on the dashboard page.
4. Copy the API token. It should be a string that looks like this:

```
prod-c6654a0ae88b2f21111b9d69b4539fb1186de983f0ad826f0febaf28e3e3b7ed
```

{% hint style="info" %}
An API token is specific for a package id and platform. If you build more than one app from one code base, you have to configure different API tokens for them.
{% endhint %}

## Instantiate the SDK

The SDK consists of a handful of (public) classes and protocols you can interact with. The main protocol of these is `JustTrackSdk` which allows you to attribute the current user, send notifications to the backend or record user events. To create an instance of the SDK you have to invoke the `JustTrackSdkBuilder` class. Instantiating the SDK could look like this:

```swift
do {
    let sdk = try JustTrackSdkBuilder(apiToken: "prod-...").build()
} catch {
    // apiToken has invalid format...
}
```
