# Send user events

To track how far a user is progressing through your app you can send a user event at specific points to the justtrack backend. Using this, you can determine whether there are any specific steps at which a lot of users drop out (e.g., at a specific level or before making some in-app purcharse). The following calls are all equal and publish a user event called `event_name` to the backend:

```csharp
using JustTrack;

JustTrackSDK.PublishEvent("event_name");
JustTrackSDK.PublishEvent(new EventDetails("event_name"));
JustTrackSDK.PublishEvent(new CustomUserEvent("event_name"));
JustTrackSDK.PublishEvent(new CustomUserEvent(new EventDetails("event_name")));
```

You can however add additional information to an event to easier analyze it later using the justtrack dashboard. The `EventDetails` class allows you to amend an event with a _category_, an _element_, and an _action_. On the dashboard you can then filter for all events with, e.g., a specific _category_, looking at multiple connected events at the same time. The `CustomUserEvent` class allows you to amend your events with up to three custom dimensions as well as a value and a unit. The custom dimensions allow you to split events on the dashboard again by some criterium. For example, in a game a player might acquire an item. You could be interested in the rarity of each item acquired, e.g., to see how many users acquire a rare item on their first day (as the dashboard allows you to look at different cohorts of users):

```csharp
using JustTrack;

void recordItemAcquire(Item item) {
  var details = new EventDetails("progression_item_acquire", "progression", "item", "acquire");
  JustTrackSDK.PublishEvent(new CustomUserEvent(details).SetDimension1(item.Rare ? "rare" : "common"));
}
```

If you need to track how much time a user needs for a level, you can attach a value and a unit to an event. In the following example, we measure the time it takes a user to complete the level and then attach it to the event before sending it to the backend:

```csharp
using JustTrack;

void recordLevelDone(double duration) {
  var details = new EventDetails("progression_level_finish", "progression", "level", "finish");
  JustTrackSDK.PublishEvent(new CustomUserEvent(details).SetValueAndUnit(duration, Unit.Seconds));
}
```

The units supported by the justtrack SDK are **Count**, **Seconds** and **Milliseconds**. Alternatively, an event can carry a monetary value instead of a value with a unit:

```cs
using JustTrack;

void publishEventWithMoney(EventDetails details, double value, string currency) {
  JustTrackSDK.PublishEvent(new CustomUserEvent(details).SetValueAndCurrency(duration, currency));
}
```

### Standard events

In the previous example we actually did a bad emulation of an event already defined by the justtrack SDK. The code should actually have looked like this:

```csharp
using JustTrack;

void recordLevelDone() {
  JustTrackSDK.PublishEvent(new ProgressionLevelFinishEvent(null, null));
}
```

The first two arguments in the constructor are called `elementName` and `elementId`. Standard events accept additional dimensions next to the three custom dimensions you can use on custom user events. For the `ProgressionLevelFinishEvent` these are `elementName` and `elementId`, which you can (but don't have to) use to record about which level you are actually talking. Thus, a call could look like this (hardcoding the arguments for simplicity):

```csharp
using JustTrack;

void recordLevelDone() {
  JustTrackSDK.PublishEvent(new ProgressionLevelFinishEvent("Tutorial", "LVL001"));
}
```

As you can see, we dropped the duration, so it seems like it is no longer available. This is not a limitation of the standard events (for example, the `LoginProviderauthorizationFinishEvent` is a standard event which can provide a duration, too). Instead, the justtrack SDK is already measuring the duration for us and will provide it automatically.
