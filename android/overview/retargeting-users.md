# Retargeting users

Retargeting allows you to re-engage users with your app which already did install the app once in the past. As they already installed the app in the past, you have a high chance to get them to do it again - after all, they did like your app enough to do it once already. Maybe they just did run out of things to do in your game (but you now added new content), focused on something else for some time (but maybe need now another change in their favorite app), or were using your app on their ride to work every day, but then a global pandemic caused everyone to work from home.

A retargeting campaign thus is a little bit different from a normal campaign. Some users still have your app installed and thus the app can be opened directly with the right `Intent` send to their phones. Also a user might expect some _Welcome back_ screen upon returning into the app.

### How retargeting works

Upon encountering a _retargeting opportunity_ the SDK checks with the justtrack backend whether a user needs to be re-attributed to a retargeting campaign. Such a retargeting opportunity arises if:

* The user did not open the app for quite some time. Default: 48 hours
* The last time we fetched an attribution for the user is some time ago. Default: 14 days
* The app was opened by an intent which looks like it might be from a click on a retargeting campaign
* The app was resumed and the current active activity now has a different intent set as before

Upon encountering a retargeting opportunity the SDK will check the justtrack backend for a new attribution. If the user was just old (second case, older than 14 days) and did not click on a retargeting campaign the old attribution will be returned. If there was however a click, we might get a new attribution back. If we receive a retargeting attribution the method `sdk.getRetargetingParameters()` will return data about the campaign the user got attributed to. These include the promotion code for the campaign as well as additional parameters configured for the campaign. You can then use those to e.g. present the user with a special _Welcome back!_ screen or grant the user a promised gift.

The retargeting parameters are only available on the first start of the SDK after the re-attribution of the user to a retargeting campaign. Thus, if the user opens the app again while still being attributed to the retargeting campaign, `sdk.getRetargetingParameters()` will return `null` this time, so your app can avoid granting the user gifts multiple times or similar.

As we can now trigger an attribution request while the app is already running (and therefore we already requested the attribution once) there are now new callbacks you can subscribe to:

* `sdk.registerAttributionListener`: Use this to get notified about the new attribution while the app is running.
* `sdk.registerRetargetingParametersListener`: use this to get notified about new retargeting parameters while the app is running.

### Retargeting requirements and configuration

While most of the work is done automatically by the SDK to handle re-attribution, it can not automatically listen to intents which arrive while your app is already running. Thus you have to forward them manually to the SDK, otherwise you can miss some re-attribution opportunities and not all users clicking on an ad will get the reward you promised them for returning.

{% tabs %}
{% tab title="Java" %}
```java
public class MainActivity extends SomeActivity {
    // ...

    @Override
    protected void onNewIntent(Intent intent) {
        super.onNewIntent(intent);

        // EITHER:

        // If your app does not make use of the intent returned by `Activity#getIntent()` or you need
        // it to return the latest intent anyway you can just set the intent on the activity and the
        // SDK will take care of the rest
        setIntent(intent);

        // OR:

        // Alternatively you can forward the intent directly to the SDK. In that case you don't need
        // to call `setIntent` on your activity, but have to forward all future intents like this, too:
        JustTrackSdk sdk = getSdkInstance(); // get an sdk instance from somewhere in your app...
        sdk.onNewIntent(intent);
    }

    // ...
}
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
class MainActivity : SomeActivity() {

    // ...

    override fun onNewIntent(intent: Intent) {
        super.onNewIntent(intent)

        // EITHER:

        // If your app does not make use of the intent returned by `Activity#getIntent()` or you need
        // it to return the latest intent anyway you can just set the intent on the activity and the
        // SDK will take care of the rest
        setIntent(intent)

        // OR:

        // Alternatively you can forward the intent directly to the SDK. In that case you don't need
        // to call `setIntent` on your activity, but have to forward all future intents like this, too:
        val sdk: JustTrackSdk = getSdkInstance() // get an sdk instance from somewhere in your app...
        sdk.onNewIntent(intent)
    }

    // ...
}
```
{% endtab %}
{% endtabs %}

Upon creation of the SDK you might have already noticed that you also have to pass an Intent to the SDK. The SDK needs this intent to detect whether your app was launched from e.g. a click on a retargeting campaign. You should normally pass the result of `Activity#getIntent()` to the SDK builder.

If you want to control how often the SDK checks by itself for a new attribution, you can use `setInactivityTimeFrame` and `setReAttributionTimeFrame` on the SDK builder. Using those you can change the defaults of 48 hours for inactive users as well as 14 days for the maximum age of an attribution.

### Preliminary retargeting parameters

If a user clicks on a retargeting campaign and already has the app installed, we open the app via an `Intent`. This can happen so fast that the justtrack backend might still be processing the click and therefore it would take a few seconds to validate with the server that the re-attribution of the user was correct. If you want to display a special _Welcome back!_ screen to the user, it can be useful to already know some of the parameters of the click in advance and just display the screen, even if it later turns out the user was not re-attributed. Therefore the justtrack SDK provides a method `getPreliminaryRetargetingParameters` to retrieve an instance of `PreliminaryRetargetingParameters` if any are available. Those parameters are created from data not yet validated and should not be relied on for anything which can not be reversed if they do not validate.

{% tabs %}
{% tab title="Java" %}
```java
JustTrackSdk sdk = JustTrack.getInstance();
PreliminaryRetargetingParameters parameters = sdk.getPreliminaryRetargetingParameters();
if (parameters != null) {
    // your app got started because of a retargeting click:
    showWelcomeDialog(sdk, parameters);
} else {
    Subscription subscription = sdk.registerPreliminaryRetargetingParametersListener((parameters) -> {
        // your app was already running in the background and the user clicked on a retargeting campaign
        showWelcomeDialog(sdk, parameters);
    });
    // upon termination of your app:
    subscription.unsubscribe();
}

void showWelcomeScreen(JustTrackSdk sdk, PreliminaryRetargetingParameters parameters) {
    navigateTo(welcomeScreen);
    parameters.validate().registerPromiseCallback(new Promise<PreliminaryRetargetingParameters.ValidateResult, Exception>() {
        @Override
        public void resolve(PreliminaryRetargetingParameters.ValidateResult response) {
            if (response.isValid()) {
                // the click was valid, you can now persist any rewards you promised
                persistWelcomeBonus();
            } else {
                // it was not valid, handle somehow (here we just navigate to some other screen,
                // you might want to have some better handling here)
                navigateTo(mainScreen);
            }
        }

        @Override
        public void reject(@NonNull Exception exception) {
            // we could not communicate with the JustTrack backend, handle the error somehow
            handleError(exception);
        }
    });
}
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
val sdk: JustTrackSdk = JustTrack.getInstance()
val parameters: PreliminaryRetargetingParameters? = sdk.getPreliminaryRetargetingParameters()

if (parameters != null) {
    // your app got started because of a retargeting click:
    showWelcomeDialog(sdk, parameters)
} else {
    val subscription = sdk.registerPreliminaryRetargetingParametersListener { parameters ->
        // your app was already running in the background and the user clicked on a retargeting campaign
        showWelcomeDialog(sdk, parameters)
    }
    // upon termination of your app:
    subscription.unsubscribe()
}

fun showWelcomeScreen(sdk: JustTrackSdk, parameters: PreliminaryRetargetingParameters) {
    navigateTo(welcomeScreen)
    parameters.validate().registerPromiseCallback(object : Promise<PreliminaryRetargetingParameters.ValidateResult, Exception>() {
        override fun resolve(response: PreliminaryRetargetingParameters.ValidateResult) {
            if (response.isValid()) {
                // the click was valid, you can now persist any rewards you promised
                persistWelcomeBonus()
            } else {
                // it was not valid, handle somehow (here we just navigate to some other screen,
                // you might want to have some better handling here)
                navigateTo(mainScreen)
            }
        }

        override fun reject(exception: Exception) {
            // we could not communicate with the JustTrack backend, handle the error somehow
            handleError(exception)
        }
    })
}
```
{% endtab %}
{% endtabs %}
