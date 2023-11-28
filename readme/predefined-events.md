# Predefined Events

The SDK provides a list of predefined events as static fields of the `UserEvent` class. Below you can find a list of these events with their description and what dimensions are expected to be provided with them.

* `UserScreenShowEvent`:
  * A screen was shown to the user. Use element\_name or element\_id to specify which screen.
  * Dimension: element\_name
  * Dimension: element\_id
* `UserNotificationShowEvent`:
  * A notification was shown to the user. Use element\_name or element\_id to specify which notification.
  * Dimension: element\_name
  * Dimension: element\_id
* `UserDialogShowEvent`:
  * A dialog was shown to the user. Use element\_name or element\_id to specify which dialog.
  * Dimension: element\_name
  * Dimension: element\_id
* `UserButtonShowEvent`:
  * A button was shown to the user. Use element\_name or element\_id to specify which button.
  * Dimension: element\_name
  * Dimension: element\_id
* `UserCardShowEvent`:
  * A card was shown to the user. Use element\_name or element\_id to specify which card.
  * Dimension: element\_name
  * Dimension: element\_id
* `UserButtonClickEvent`:
  * A button was clicked by the user. Use element\_name or element\_id to specify which button.
  * Dimension: element\_name
  * Dimension: element\_id
* `UserNotificationClickEvent`:
  * A notification was clicked by the user. Use element\_name or element\_id to specify which notification.
  * Dimension: element\_name
  * Dimension: element\_id
* `UserCardClickEvent`:
  * A card was clicked by the user. Use element\_name or element\_id to specify which card.
  * Dimension: element\_name
  * Dimension: element\_id
* `UserRatingProvideEvent`:
  *   You can provide an exact numeric rating as value to see average on the dashboard and/or group the ratings using element\_name/element\_id

      Note: If you want to see the count per Rating (ex. 1-5 stars) you need to provide the rating as element\_name/element\_id or custom dimension.
  * Dimension: element\_name
  * Dimension: element\_id
  * Value: count
  * Unit: count
* `UserRatingPositiveEvent`:
  *   You can provide an exact numeric rating as value to see average on the dashboard and/or group the ratings using element\_name/element\_id

      Note: If you want to see the count per Rating (ex. 1-5 stars) you need to provide the rating as element\_name/element\_id or custom dimension.
  * Dimension: element\_name
  * Dimension: element\_id
  * Value: count
  * Unit: count
* `UserRatingNeutralEvent`:
  *   You can provide an exact numeric rating as value to see average on the dashboard and/or group the ratings using element\_name/element\_id

      Note: If you want to see the count per Rating (ex. 1-5 stars) you need to provide the rating as element\_name/element\_id or custom dimension.
  * Dimension: element\_name
  * Dimension: element\_id
  * Value: count
  * Unit: count
* `UserRatingNegativeEvent`:
  *   You can provide an exact numeric rating as value to see average on the dashboard and/or group the ratings using element\_name/element\_id

      Note: If you want to see the count per Rating (ex. 1-5 stars) you need to provide the rating as element\_name/element\_id or custom dimension.
  * Dimension: element\_name
  * Dimension: element\_id
  * Value: count
  * Unit: count
* `RegistrationProcessStartEvent`:
  * The user started the registration process.
* `RegistrationProcessFinishEvent`:
  * The user finished the registration process. You can provide the duration since the start of the process to see an average on the dashboard.
  * Value: duration
  * Unit: seconds, milliseconds
* `RegistrationProcessFailEvent`:
  * The user failed the registration process. You can provide the duration since the start of the process to see an average on the dashboard.
  * Value: duration
  * Unit: seconds, milliseconds
* `RegistrationTosShowEvent`:
  * Track the terms of services being shown to the user.
* `RegistrationTosAcceptEvent`:
  * Track the terms of services being accepted to the user.
* `RegistrationTosDeclineEvent`:
  * Track the terms of services being declined to the user.
* `RegistrationAgeRequestEvent`:
  * Track requesting users to provide their age.
* `RegistrationAgeProvideEvent`:
  * Track users providing their age.
  * Dimension: age
* `RegistrationAgeDeclineEvent`:
  * Track users declining providing their age.
* `RegistrationGenderRequestEvent`:
  * Track requesting users to provide their gender.
* `RegistrationGenderProvideEvent`:
  * Track users providing their gender.
  * Dimension: gender
* `RegistrationGenderDeclineEvent`:
  * Track users declining providing their gender.
* `LoginLoginmethodSelectEvent`:
  * Login method selected
  * Dimension: provider\_name
* `LoginProviderauthorizationStartEvent`:
  * Track 3rd party authorization progress
  * Dimension: provider\_name
* `LoginProviderauthorizationFinishEvent`:
  * Track 3rd party authorization progress
  * Dimension: provider\_name
  * Value: duration
  * Unit: seconds, milliseconds
* `LoginProviderauthorizationFailEvent`:
  * Track 3rd party authorization progress
  * Dimension: provider\_name
  * Value: duration
  * Unit: seconds, milliseconds
* `LoginProcessStartEvent`:
  * Track login progress
* `LoginProcessFinishEvent`:
  * Track login progress
  * Value: duration
  * Unit: seconds, milliseconds
* `LoginProcessFailEvent`:
  * Track login progress
  * Value: duration
  * Unit: seconds, milliseconds
* `UsagePermissionRequiredEvent`:
  * Provide information when user has granted the usage permission or if it is missing
* `UsagePermissionGrantEvent`:
  * Provide information when user has granted the usage permission or if it is missing
* `UsageRequestShowEvent`:
  * Used for dialog before sending the user to the system settings
* `UsageRequestAcceptEvent`:
  * Used for dialog before sending the user to the system settings
* `UsageRequestDeclineEvent`:
  * Used for dialog before sending the user to the system settings
* `VerificationProcessStartEvent`:
  *   Track verification progress

      Verification is typically needed before payout
  * Dimension: method
* `VerificationProcessCancelEvent`:
  *   Track verification progress

      Verification is typically needed before payout
  * Dimension: method
* `VerificationProcessFinishEvent`:
  *   Track verification progress

      Verification is typically needed before payout
  * Dimension: method
* `VerificationProcessFailEvent`:
  *   Track verification progress

      Verification is typically needed before payout
  * Dimension: method
* `VerificationProcessNumberrequestedEvent`:
  *   Track verification progress

      Verification is typically needed before payout
  * Dimension: method
* `VerificationProcessNumbervalidEvent`:
  *   Track verification progress

      Verification is typically needed before payout
  * Dimension: method
* `VerificationProcessCoderequestedEvent`:
  *   Track verification progress

      Verification is typically needed before payout
  * Dimension: method
* `VerificationProcessCodevalidEvent`:
  *   Track verification progress

      Verification is typically needed before payout
  * Dimension: method
* `AdOfferwallLoadEvent`:
  * Track ads
  * Dimension: ad\_sdk\_name
  * Dimension: ad\_network
  * Dimension: ad\_placement
  * Value: duration
  * Unit: seconds, milliseconds
* `AdOfferwallOpenEvent`:
  * Track ads
  * Dimension: ad\_sdk\_name
  * Dimension: ad\_network
  * Dimension: ad\_placement
  * Value: duration
  * Unit: seconds, milliseconds
* `AdOfferwallCloseEvent`:
  * Track ads
  * Dimension: ad\_sdk\_name
  * Dimension: ad\_network
  * Dimension: ad\_placement
  * Value: count
  * Unit: count
* `AdOfferwallClickEvent`:
  * Track ads
  * Dimension: ad\_sdk\_name
  * Dimension: ad\_network
  * Dimension: ad\_placement
  * Value: count
  * Unit: count
* `AdOfferwallShowEvent`:
  * Track ads
  * Dimension: ad\_sdk\_name
  * Dimension: ad\_network
  * Dimension: ad\_placement
  * Value: count
  * Unit: count
* `AdInterstitialLoadEvent`:
  * Track ads
  * Dimension: ad\_sdk\_name
  * Dimension: ad\_network
  * Dimension: ad\_placement
  * Value: duration
  * Unit: seconds, milliseconds
* `AdInterstitialOpenEvent`:
  * Track ads
  * Dimension: ad\_sdk\_name
  * Dimension: ad\_network
  * Dimension: ad\_placement
  * Value: duration
  * Unit: seconds, milliseconds
* `AdInterstitialCloseEvent`:
  * Track ads
  * Dimension: ad\_sdk\_name
  * Dimension: ad\_network
  * Dimension: ad\_placement
  * Value: count
  * Unit: count
* `AdInterstitialClickEvent`:
  * Track ads
  * Dimension: ad\_sdk\_name
  * Dimension: ad\_network
  * Dimension: ad\_placement
  * Value: count
  * Unit: count
* `AdInterstitialShowEvent`:
  * Track ads
  * Dimension: ad\_sdk\_name
  * Dimension: ad\_network
  * Dimension: ad\_placement
  * Value: count
  * Unit: count
* `AdRewardedLoadEvent`:
  * Track ads
  * Dimension: ad\_sdk\_name
  * Dimension: ad\_network
  * Dimension: ad\_placement
  * Value: duration
  * Unit: seconds, milliseconds
* `AdRewardedOpenEvent`:
  * Track ads
  * Dimension: ad\_sdk\_name
  * Dimension: ad\_network
  * Dimension: ad\_placement
  * Value: duration
  * Unit: seconds, milliseconds
* `AdRewardedCloseEvent`:
  * Track ads
  * Dimension: ad\_sdk\_name
  * Dimension: ad\_network
  * Dimension: ad\_placement
  * Value: count
  * Unit: count
* `AdRewardedClickEvent`:
  * Track ads
  * Dimension: ad\_sdk\_name
  * Dimension: ad\_network
  * Dimension: ad\_placement
  * Value: count
  * Unit: count
* `AdRewardedShowEvent`:
  * Track ads
  * Dimension: ad\_sdk\_name
  * Dimension: ad\_network
  * Dimension: ad\_placement
  * Value: count
  * Unit: count
* `AdBannerLoadEvent`:
  * Track ads
  * Dimension: ad\_sdk\_name
  * Dimension: ad\_network
  * Dimension: ad\_placement
  * Value: duration
  * Unit: seconds, milliseconds
* `AdBannerOpenEvent`:
  * Track ads
  * Dimension: ad\_sdk\_name
  * Dimension: ad\_network
  * Dimension: ad\_placement
  * Value: duration
  * Unit: seconds, milliseconds
* `AdBannerCloseEvent`:
  * Track ads
  * Dimension: ad\_sdk\_name
  * Dimension: ad\_network
  * Dimension: ad\_placement
  * Value: count
  * Unit: count
* `AdBannerClickEvent`:
  * Track ads
  * Dimension: ad\_sdk\_name
  * Dimension: ad\_network
  * Dimension: ad\_placement
  * Value: count
  * Unit: count
* `AdBannerShowEvent`:
  * Track ads
  * Dimension: ad\_sdk\_name
  * Dimension: ad\_network
  * Dimension: ad\_placement
  * Value: count
  * Unit: count
* `AdInterstitialStartEvent`:
  * Track interstitial video starts.
  * Dimension: ad\_sdk\_name
  * Dimension: ad\_network
  * Dimension: ad\_placement
  * Value: duration
  * Unit: seconds, milliseconds
* `AdInterstitialStopEvent`:
  * Track interstitial video stops.
  * Dimension: ad\_sdk\_name
  * Dimension: ad\_network
  * Dimension: ad\_placement
  * Value: duration
  * Unit: seconds, milliseconds
* `AdRewardedStartEvent`:
  * Track rewarded video starts.
  * Dimension: ad\_sdk\_name
  * Dimension: ad\_network
  * Dimension: ad\_placement
  * Value: duration
  * Unit: seconds, milliseconds
* `AdRewardedStopEvent`:
  * Track rewarded video stops.
  * Dimension: ad\_sdk\_name
  * Dimension: ad\_network
  * Dimension: ad\_placement
  * Value: duration
  * Unit: seconds, milliseconds
* `ProgressionLevelStartEvent`:
  * Used to track the player\`s progress. Use element\_name or element\_id to specify the level.
  * Dimension: element\_name
  * Dimension: element\_id
* `ProgressionQuestStartEvent`:
  * Used to track the player\`s progress. Use element\_name or element\_id to specify the quest.
  * Dimension: element\_name
  * Dimension: element\_id
* `ProgressionLevelFinishEvent`:
  * Used to track the player\`s progress. Use element\_name or element\_id to specify the level.
  * Dimension: element\_name
  * Dimension: element\_id
* `ProgressionLevelFailEvent`:
  * Used to track the player\`s progress. Use element\_name or element\_id to specify the level.
  * Dimension: element\_name
  * Dimension: element\_id
* `ProgressionQuestFinishEvent`:
  * Used to track the player\`s progress. Use element\_name or element\_id to specify the quest.
  * Dimension: element\_name
  * Dimension: element\_id
* `ProgressionQuestFailEvent`:
  * Used to track the player\`s progress. Use element\_name or element\_id to specify the quest.
  * Dimension: element\_name
  * Dimension: element\_id
* `ProgressionResourceSinkEvent`:
  * {count} can reflect the number of items or their value (ex. value in in-app currency)
  * Dimension: item\_type
  * Dimension: item\_name
  * Dimension: item\_id
  * Value: count
  * Unit: count
* `ProgressionResourceSourceEvent`:
  * {count} can reflect the number of items or their value (ex. value in in-app currency)
  * Dimension: item\_type
  * Dimension: item\_name
  * Dimension: item\_id
  * Value: count
  * Unit: count
* `PurchaseOptionClickEvent`:
  * {count} can reflect the number of items or their value (ex. value in in-app currency)
  * Dimension: item\_type
  * Dimension: item\_name
  * Dimension: item\_id
  * Value: count
  * Unit: count
* `PurchaseOptionConfirmEvent`:
  * {count} can reflect the number of items or their value (ex. value in in-app currency)
  * Dimension: item\_type
  * Dimension: item\_name
  * Dimension: item\_id
  * Value: count
  * Unit: count
* `PayoutOptionClickEvent`:
  * {count} can reflect the number of items or their value (ex. value in in-app currency)
  * Dimension: category\_name
  * Dimension: provider\_name
  * Value: count
  * Unit: count
* `PayoutOptionConfirmEvent`:
  * {count} can reflect the number of items or their value (ex. value in in-app currency)
  * Dimension: category\_name
  * Dimension: provider\_name
  * Value: count
  * Unit: count
