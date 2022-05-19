# Getting Started

# iOS Test App
## Testing tips with iOS advertising identifier (IDFA)
Please refer to Apple documentation on [`advertisingIdentifier`](https://developer.apple.com/documentation/adsupport/asidentifiermanager/1614151-advertisingidentifier) for official guidance surrounding the requirements for obtaining the ad ID value, best practices for usage, and special cases for various environments (simulator, iOS versions, etc.).

In the latest versions of iOS, users are opted out of ad ID tracking by default; the permission to allow apps to request tracking ("Allow Apps to Request to Track") is off by default. They can choose to opt in to tracking in iOS device Settings at the device level. This means that the vast majority of users will most likely not enable ad ID tracking.

Based on iOS simulator testing:
- once ad ID tracking permission is set for a given app, changes in opt-in/out status through device Settings terminates the app.
    - practically, it may be cumbersome to detect changes at arbitrary points in the logic throughout the app;
    it may be helpful to use:
        - a getter helper for ad ID that detects and handles changes in value or opt-in/out.
        - using app lifecycle foreground event to check for changes in ad ID value or opt-in/out.
- In older versions of iOS (<= 14) it is possible for users to regenerate the value of the ID and control tracking permissions, while in iOS 15+ only the permission for ad ID tracking is available to the user. That is, the user cannot change the value of the ad ID.
- it is possible for system-wide tracking to be off but individual per-app permissions granted.
- If "Allow Apps to Request to Track" at the system level was on and is turned off, a system prompt appears asking if previously provided individual per-app tracking permissions should be kept as-is or all turned off
- Based on Apple documentation for `ASIdentifierManager.shared().advertisingIdentifier`, iOS 14.5+ is the cutoff for required permissions request to use IDFA
    - however, based on testing with iOS 14.0.1 simulator, `isAdvertisingTrackingEnabled` is false on fresh app install, even if device has device level tracking enabled; prompt is never given and app does not show up in Privacy -> Tracking app list

### Advertising Identifier in Edge Identity + Mobile Core
Set IDFA using Core API, which will be routed to Edge Identity extension.
Note that this will automatically update ad ID consent (consent event dispatched)
to "y" but only if the ad ID is not nil, all-zeros, or ""; in the case of the simulator it will be all-zeros.
Set the ad ID manually after getting authorization to get consent updated properly

