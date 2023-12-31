# Send user events

The SDK enables you to monitor and record user behaviors as events. In this guide, you'll learn to:&#x20;

* Send both custom and predefined events
* Build events
* Add your own dimensions to events.

## Send User Events

To publish an Event, create a `UserEvent`, and pass it to `publishEvent()`:

{% tabs %}
{% tab title="Java" %}
```java
PublishableUserEvent event = new UserEvent("category_element_action").build();
sdk.publishEvent(event);
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
val event = UserEvent("category_element_action").build()
sdk.publishEvent(event)
```
{% endtab %}
{% endtabs %}

By calling `build()` on a `UserEvent`  you are creating an immutable `PublishableUserEvent` object, which you can pass to `publishEvent()` as often as you need. Calling `build()` does not modify the `UserEvent` object if you need to modify it again.

If you are interested in whether an event could be successfully sent to the backend, you can await the future returned by `publishEvent()`:

```java
interface JustTrackSdk {
    AsyncFuture<Void> publishEvent(@NonNull PublishableUserEvent event);
}
```

As there is no meaningful response to return to your app, the value the future resolves to is not specified by the SDK and may change with different versions of the SDK.

## Build a user event

A `UserEvent` instance is actually a builder for the event we later send to the backend. You can either provide all parameters via the constructor if you want or you can initialize an empty event and later call setters for the different properties. The only thing you need to create a new event is the name of the event, which may also not be empty (and can later no longer be changed). The following constructors are provided:

```java
class UserEvent {
    UserEvent(@NonNull String name);
    UserEvent(@NonNull EventDetails name);
    UserEvent(@NonNull EventDetails name, double value, @NonNull Unit unit);
    UserEvent(@NonNull String name, double value, @NonNull Unit unit);
    UserEvent(@NonNull EventDetails name, @NonNull Money money);
    UserEvent(@NonNull String name, @NonNull Money money);
    UserEvent(@NonNull EventDetails name, @NonNull String dimension1);
    UserEvent(@NonNull EventDetails name, @NonNull String dimension1, @NonNull String dimension2);
    UserEvent(@NonNull EventDetails name, @NonNull String dimension1, @NonNull String dimension2, @NonNull String dimension3);
    UserEvent(@NonNull EventDetails name, @NonNull String dimension1, @NonNull String dimension2, @NonNull String dimension3, double value, @NonNull Unit unit);
    UserEvent(@NonNull EventDetails name, @Nullable String dimension1, @Nullable String dimension2, @Nullable String dimension3, @NonNull Money money);
}
```

The `EventDetails` class captures the name of a user event with some additional details. Those are the `category`, `element`, and `action` of the event and can be used to group events together. For example, all events generated by the SDK regarding the session tracking share the category `session`.

The `value` and `unit` can only be provided together and default to `1` and `Unit.COUNT`. Alternatively, an event can carry a monetary value instead of a value with a unit. The three dimensions default to the empty string if not specified. You can also set the properties after initialization:

```java
interface UserEventBuilder {
    UserEventBuilder setDimension1(@NonNull String dimension1);
    UserEventBuilder addDimension(@NonNull String key, @NonNull String value);
    UserEventBuilder setDimension2(@NonNull String dimension2);
    UserEventBuilder removeDimension(@NonNull String key);
    UserEventBuilder setDimension3(@NonNull String dimension3);
    UserEventBuilder setValue(double value, @NonNull Unit unit);
    UserEventBuilder setValue(@NonNull Money money);
    // convenience wrappers for setValue
    UserEventBuilder setCount(double count);
    UserEventBuilder setSeconds(double seconds);
    UserEventBuilder setMilliseconds(double milliseconds);
}
```

`setCount`, `setSeconds`, and `setMilliseconds` are provided for your convenience if you know an event will always use a specific unit.

## Add your own dimensions

You can send additional information along with your events by adding Dimensions.

These Dimensions can be used to group events, enabling you to dive deeper into the specific set of events you are looking for and generate a report out of them.

Here is how to add your event with the additional Dimensions:

{% tabs %}
{% tab title="Java" %}
```java
sdk.publishEvent(
    new UserEvent("screen_view_event")
        .setDimension1("MainActivity")
        .setDimension2("Production")
        .setDimension3("...")
        .build()
);

// Alternatively you can also add them in the constructor
sdk.publishEvent(
    new UserEvent(
        new EventDetails("screen_view_event"),
        "MainActivity",
        "Production",
        "..."
    ).build()
);
```
{% endtab %}

{% tab title="Kotlin" %}
<pre class="language-kotlin"><code class="lang-kotlin"><strong>sdk.publishEvent(
</strong>    UserEvent("screen_view_event")
        .setDimension1("MainActivity")
        .setDimension2("Production")
        .setDimension3("...")
        .build()
)


// Alternatively you can also add them in the constructor
sdk.publishEvent(
    UserEvent(
        EventDetails("screen_view_event"),
        "MainActivity",
        "Production",
        "..."
    ).build()
)
</code></pre>
{% endtab %}
{% endtabs %}

## Predefined Events

The SDK defines a broad range of event classes with predefined semantics. When creating those events you can specify some predefined dimensions depending on the event. .  For example, to track the navigationuser fromconfim purchases of your menu screen to a game screenproduct, you could record the following event:

{% tabs %}
{% tab title="Java" %}
```java
sdk.publishEvent(
       new UserScreenShowEvent("Game Main", "SCREEN_GAME_MAIN")
        // You can also add additional dimensions in PredefinedEvent as well.
        .setDimension1("...")
        .setDimension2("...")
        .setDimension3("...")
        .build()
);
```
{% endtab %}

{% tab title="Kotlin" %}
<pre class="language-kotlin"><code class="lang-kotlin"><strong>sdk.publishEvent(
</strong>    JtPurchaseOptionConfirmEvent("book", "D&#x26;D", "ID_104", 2)
        // You can also add additional dimensions in PredefinedEvent as well.
        .setDimension1("...")
        .setDimension2("...")
        .setDimension3("...")
    .build()
)
</code></pre>
{% endtab %}
{% endtabs %}

In the case mentioned above, when using the 'JtPurchaseOptionConfirmEvent', there are four required fields for the event: `'jtItemType', 'jtItemName', 'jtItemId', and 'count'.` Please note that the first three fields are pre-filled dimensions or predefined dimensions, which means they will also be added to the event dimension list. This implies that you can only provide seven additional dimensions. Only the fields with titles starting with 'jt' are considered dimensions.

You can find all our Predefined Events here: [Predefined Events](send-user-events.md#predefined-event)

### User event restrictions

The following restrictions apply to user events:

* The name must not be empty.
* Name, action, category, and element must be shorter than 256 characters and can only consist of printable ISO 8859-1 characters (U+0020 to U+007E as well as U+00A0 to U+00FF).
* Dimension values must be shorter than 4096 characters and can only consist of printable ISO 8859-1 characters (U+0020 to U+007E as well as U+00A0 to U+00FF).
* The value of a user event must be finite (i.e., not NaN or ±Infinity).
* If you provide a monetary value, the currency needs to be a 3 letter uppercase ISO 4217 string.
