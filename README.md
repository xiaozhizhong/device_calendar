# Device Calendar Plugin

[![pub package](https://img.shields.io/pub/v/device_calendar.svg)](https://pub.dartlang.org/packages/device_calendar) [![Build Status](https://dev.azure.com/builttoroam/Flutter%20Plugins/_apis/build/status/Device%20Calendar)](https://dev.azure.com/builttoroam/Flutter%20Plugins/_build/latest?definitionId=111)

A cross platform plugin for modifying calendars on the user's device.

## Features

* Request permissions to modify calendars on the user's device
* Check if permissions to modify the calendars on the user's device have been granted
* Add or retrieve calendars on the user's device
* Retrieve events associated with a calendar
* Add, update or delete events from a calendar
* Set up, edit or delete recurring events
  * **NOTE**: Editing a recurring event will currently edit all instances of it
  * **NOTE**: Deleting multiple instances in **Android** takes time to update, you'll see the changes after a few seconds
* Add, modify or remove attendees and receive if an attendee is an organiser for an event
* Setup reminders for an event
* Specify a time zone for event start and end date
  * **NOTE**: Due to a limitation of iOS API, single time zone property is used for iOS (`event.startTimeZone`)
  * **NOTE**: For the time zone list, please refer to the `TZ database name` column on [Wikipedia](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)
  * **NOTE**: If the time zone values are null or invalid, it will be defaulted to the device's current time zone.

## Null migration

From v4.0.0, device_calendar fits null safety. However, not all workflow had been checked and bugs from 3.2 still presists.

You are strongly advised to test your workflow with the new package before shipping. 
Better yet, please leave a note for what works and what doesn't, or contribute some bug fixes!

## Android Integration

The following will need to be added to the manifest file for your application to indicate permissions to modify calendars a needed

```xml
<uses-permission android:name="android.permission.READ_CALENDAR" />
<uses-permission android:name="android.permission.WRITE_CALENDAR" />
```
### Proguard / R8 exceptions
By default, all android apps go through R8 for file shrinking when building a release version. Currently, it interferes with some functions such as `retrieveCalendars()`.

You may add the following setting to the ProGuard rules file (thanks to [Britannio Jarrett](https://github.com/britannio)). Read more about the issue [here](https://github.com/builttoroam/device_calendar/issues/99)

```
-keep class com.builttoroam.devicecalendar.** { *; }
```

See [here](https://github.com/builttoroam/device_calendar/issues/99#issuecomment-612449677) for an example setup.

For more information, refer to the guide at [Android Developer](https://developer.android.com/studio/build/shrink-code#keep-code)

### AndroidX migration
**IMPORTANT**: Since version 0.1.0, this version has migrated to use AndroidX instead of the deprecated Android support libraries. When using version 0.10.0 and onwards for this plugin, please ensure your application has been migrated following the guide [here](https://developer.android.com/jetpack/androidx/migrate)

## iOS Integration

For iOS 10 support, you'll need to modify the Info.plist to add the following key/value pair

```xml
<key>NSCalendarsUsageDescription</key>
<string>INSERT_REASON_HERE</string>
```

Note that on iOS, this is a Swift plugin. There is a known issue being tracked [here](https://github.com/flutter/flutter/issues/16049) by the Flutter team, where adding a plugin developed in Swift to an Objective-C project causes problems. If you run into such issues, please look at the suggested workarounds there.
