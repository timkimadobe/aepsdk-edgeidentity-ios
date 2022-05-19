# Getting Started

# iOS Test App
## Testing tips with iOS advertising identifier (IDFA)
Please refer to Apple documentation on [`advertisingIdentifier`](https://developer.apple.com/documentation/adsupport/asidentifiermanager/1614151-advertisingidentifier) for official guidance surrounding the requirements for obtaining the ad ID value, best practices for usage, and special cases for various environments (simulator, iOS versions, etc.).

In the latest versions of iOS, users are opted out of ad ID tracking by default; the permission to allow apps to request tracking ("Allow Apps to Request to Track") is off by default. They can choose to opt in to tracking in iOS device Settings at the device level. This means that the vast majority of users will most likely not enable ad ID tracking.

Based on iOS simulator testing:
- once ad ID tracking permission is set for a given app, changes in opt-in/out status through device Settings terminates the app
    - practically, it may be cumbersome to detect changes at arbitrary points in the logic throughout the app;
    it may be helpful to use:
        - a getter helper for ad ID that detects and handles changes in value or opt-in/out
        - using app lifecycle foreground event to check for changes in ad ID value or opt-in/out
- In older versions of iOS (<= 14) it is possible for users to regenerate the value of the ID and control tracking permissions, while in iOS 15+ only the permission for ad ID tracking is available to the user. That is, the user cannot change the value of the ad ID.

