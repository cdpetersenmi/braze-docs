---
nav_title: Beacon Integration
platform: iOS
page_order: 4
search_rank: 5
---
# Beacon Integration

Here we will walk through how to integrate specific kinds of beacons with Braze to allow for segmentation and messaging.

## Gimbal Beacons

Once you have your Gimbal Beacons set up and integrated into your app, you can log Custom Events for things like a visit starting or ending, or a beacon being sighted. You can also log properties for these events, like the Place name or the Dwell time.

In order to log a Custom Event when a user enters a place, input this code into the `didBeginVisit` method:

```objc
[[Appboy sharedInstance] logCustomEvent:@"Entered %@", visit.place.name];
[[Appboy sharedInstance] flushDataAndProcessRequestQueue];
```

The `flushDataAndProcessRequestQueue` ensures that your event will log even if the app is in the background, and the same process can be implemented for leaving a location. Note that the above will create and increment a unique custom event for each new place that the user enters. As such, if you anticipate creating more than 50 places we recommend you create one generic "Place Entered" custom event and include the place name as an event property.
