# Set up the SDK

The justtrack SDK offers extensive tracking capabilities for attribution, events, and various other functionalities within your Android application. In this guide, you'll learn to:&#x20;

* Add the justtrack SDK to your app
* Copy your API token
* Instantiate a `JustTrackSdk` object
* Shutdown the SDK instance

### Add the SDK

In your `project-level build.gradle`, add the following repository to `allprojects`:

{% tabs %}
{% tab title="Groovy" %}
```groovy
allprojects {
    repositories {
        maven {
            url "https://sdk.justtrack.io/maven"
        }
    }
}
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
allprojects {
    repositories {
        maven {
            url = uri("https://sdk.justtrack.io/maven")
        }
    }
}
```
{% endtab %}
{% endtabs %}

Then, in your `module-level build.gradle`, add the following dependency to your `dependencies`:

{% tabs %}
{% tab title="Groovy" %}
```groovy
dependencies {
    implementation "io.justtrack:justtrack-android-sdk:4.4.1"
}
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
dependencies {
    implementation("io.justtrack:justtrack-android-sdk:4.4.1")
}
```
{% endtab %}
{% endtabs %}

### Copy your API Token

Before integrating the justtrack SDK into your app, you need to obtain an API token. Follow these steps to get an API token:

1. Go to the [justtrack dashboard](https://dashboard.justtrack.io/admin/products) and log in.
2. Navigate to your app.
3. Locate the API token for your app. It should be displayed on the dashboard page.
4. Copy the API token. It should be a string that looks like this:

```
prod-c6654a0ae88b2f21111b9d69b4539fb1186de983f0ad826f0febaf28e3e3b7ed
```

5. Store your token as a constant called `JUSTTRACK_SDK_API_TOKEN` in your `BuildConfig` file:

{% tabs %}
{% tab title="Groovy" %}
```groovy
android {
    defaultConfig {
        buildConfigField "String", "JUSTTRACK_SDK_API_TOKEN", '"prod-c6654a0ae88b2f21111b9d69b4539fb1186de983f0ad826f0febaf28e3e3b7ed"'
    }
}
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
android {
    defaultConfig {
        buildConfigField("String", "JUSTTRACK_SDK_API_TOKEN", "\"prod-c6654a0ae88b2f21111b9d69b4539fb1186de983f0ad826f0febaf28e3e3b7ed\"")
    }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
An API token is specific for a package id and platform. If you build more than one app from one code base, you have to configure different API tokens for them.
{% endhint %}

### Instantiate the SDK

In your main activity class, or the class where you want to use the SDK, create an instance variable, `SDK`, for the `JustTrackSdk`.&#x20;

{% tabs %}
{% tab title="Java" %}
```java
public class MainActivity extends SomeActivity {
    private JustTrackSdk sdk = null;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        // ...
        sdk = new JustTrackSdkBuilder(this, BuildConfig.JUSTTRACK_SDK_API_TOKEN).build();
    }
}
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
class MainActivity : Activity() {
    private lateinit var sdk: JustTrackSdk

    override fun onCreate(savedInstanceState: Bundle?) {
        // ...
        sdk = JustTrackSdkBuilder(this, BuildConfig.JUSTTRACK_SDK_API_TOKEN).build()
    }
}
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
If you define your own application class, it must extend `android.app.Application`.
{% endhint %}

### Shutdown the SDK

When your app terminates, you need to unregister listeners and tear down session tracking.

First, in `onDestroy()`, call your SDK object's `shutdown()` method. Then, set the SDK to `null` because it will no longer be used:

{% tabs %}
{% tab title="Java" %}
```java
@Override
protected void onDestroy() {
    sdk.shutdown();
    sdk = null;
    // ...
    super.onDestroy();
}
```
{% endtab %}

{% tab title="Kotlin" %}
<pre class="language-kotlin"><code class="lang-kotlin"><strong>override fun onDestroy() {
</strong>    sdk.shutdown()
    sdk = null
    // ...
    super.onDestroy()
}
</code></pre>
{% endtab %}
{% endtabs %}
