# Install the SDK

The justtrack SDK is available as a Unity package. You have to add [https://registry.npmjs.org](https://registry.npmjs.org) as the scoped registry to add it to your game. Navigate to `Window` → `Package Manager`, then select `Advanced Project Settings` from the gear menu. Now add the justtrack Package Registry to your game:

<figure><img src="../.gitbook/assets/Screenshot 2023-09-15 at 14.00.19.png" alt="" width="375"><figcaption></figcaption></figure>

After adding the scoped registry, you need to add the justtrack SDK to your project. Select `Packages: My Registries` in the drop-down menu, select the `justtrack SDK` and install it into your game. Use the latest available version if possible:

<figure><img src="../.gitbook/assets/Screenshot 2023-09-15 at 14.01.50.png" alt="" width="563"><figcaption></figcaption></figure>

You can also add the scoped registry and package directly to your `Packages/manifest.json`. Afterwards, your `manifest.json` should look like this:

```javascript
{
  "dependencies": {
    // ... other dependencies of your game ...
    "io.justtrack.justtrack-unity-sdk": "4.4.0"
  },
  "scopedRegistries": [
    // ... other scoped registries ...
    {
      "name": "justtrack Package Registry",
      "url": "https://registry.npmjs.org",
      "scopes": [
        "io.justtrack"
      ]
    }
  ]
}
```

## Configuration

The minimal configuration of the SDK consists of setting the correct API token (see the [install instructions](https://app.gitbook.com/s/-MABmVDqgMGVwN-wnOZT/sdk/INSTALL.md)). If you are using **Appsflyer** as the attribution provider instead of justtrack, you have to set the attribution provider accordingly. You have to integrate the Appsflyer SDK in your app, otherwise the justtrack SDK can't pick up the Appsflyer ID of the user.
