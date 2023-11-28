# Android

Vital to measure campaign performance is to correctly identify the ad impressions and ad clicks that cause users to install an app. In general, justtrack uses multiple methods to identify these ad impressions and ad clicks as well as a last-click attribution model.

Taking into account that different attribution methods also have different margins for error, we determine the most likely ad impression or ad click using a staged approach. Moreover, we take into consideration that ad clicks are a stronger signal than ad views.

{% hint style="info" %}
With descending priority, we match installs and ad impressions/ad clicks by:‌

1. **Referrer**
2. **GAID**
3. **Fingerprinting**
{% endhint %}
