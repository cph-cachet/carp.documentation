# CARP Mobile Sensing

This page contains design considerations and documentation for the [CARP mobile sensing framework](https://github.com/cph-cachet/carp.sensing), i.e. applications and libraries to run on e.g smartphones, smartwatches, etc. The purpose of **this** _private_ wiki pages are to dot down thoughs, plans, ideas, and resources related to the sensing framework. All the official (public) documentation goes to the [CARP sensing framework](https://github.com/cph-cachet/carp.sensing) github pages and wiki.

The [initial outline / architecture of the CARP Sensing Framework](https://github.com/cph-cachet/carp.documentation/raw/master/design_of_device_environment.pdf) was done on paper.

**Table of Content**

* [Probes](#probes)
* [Data Backends](#data-backends)
* [Extending CAMS](#extending-cams)
* [Micro-Service Architecture of CAMS](#micro-service-architecture)
* [Security & Privacy](#security-and-privacy)
* [Static Analysis](#static-analysis)
* [Incentive Mechanisms](#incentive-mechanisms)
* [Contribution](#contribution)

## Description

The CARP Sensing Flutter Plugin is a digital phenotyping research platform designed to collect (via [Probes](#probes)) research-quality smartphone sensor data, usage data, and data from wearable wellness and health trackers attached to the mobile device. 

It is designed to upload data to various local and remote [Data Backends](#data-backends).
It is born together with the [CARP](https://github.com/cph-cachet/carp.documentation/wiki) backend for handling data, studies, participants, etc. and hence follows the overall CARP ontology (domain model). 
But it is also designed to work stand-alone if you want to manage data on your own. Therefore, it comes with built-in support both for the CARP back-end, as well as for storing data locally in a file format, in the SQLlite DB, and remotely on Google Firestore cloud.

CARP Sensing Flutter is a framework and hence can be [extended](#extending-cams). The API documentation explains how you can e.g. create new sensing capabilities by adding your own probes (with their configuration and data formats), as well as add new data management implementation for e.g. storing data in your own data back-end.


# Probes

[CARP Mobile Sensing (CAMS)](CARP_MOBILE_SENSING.md) supports two types of probes; (i) sensing probes and (ii) wearable probes. This section contains a description of the design / plans for these probes. Once they are done (implemented, tested, and documented, and released), they will be documented at the `carp_mobile_sensing` [MeasureTypes](https://github.com/cph-cachet/carp.sensing-flutter/wiki/A.-Measure-Types) wiki page.

**Legend**
````
  ~ : not implemented yet
  + : implemented and tested
  - : partially/fully unavailable in OS
  ? : implemented but not tested
````


## Sensing Probes

Probes are cross-platform Flutter components that uses Dart combined with platform channels to access the underlying OS-specific hardware sensor APIs, OS logs, and applications. 

Probe         | Android | iOS | author | Dart Plugin
--------------|:-------:|:---:|:------:|:-----------
accelerometer | + | + | bardram | [sensors](https://pub.dartlang.org/packages/sensors)
gyroscope     | + | + | bardram | [sensors](https://pub.dartlang.org/packages/sensors)
pedometer     | + | + | thomas | [pedometer](https://pub.dartlang.org/packages/pedometer)
bluetooth     | + | + | bardram | [flutter_blue](https://pub.dartlang.org/packages/flutter_blue)
location      | + | + | bardram | [location](https://pub.dartlang.org/packages/location)
battery       | + | + | bardram | [battery](https://pub.dartlang.org/packages/battery)
light         | + | - | thomas | [light](https://pub.dartlang.org/packages/light)
telephony     | + | - | bardram | [phone_state_i](https://pub.dartlang.org/packages/phone_state_i)
connectivity  | + | + | bardram | [connectivity](https://pub.dartlang.org/packages/connectivity)
apps - installed | + | - | thomas | [device_apps](https://pub.dartlang.org/packages/device_apps)
apps - usage  | + | + | thomas | 
screen        | + | + | thomas | [screen_state](https://pub.dartlang.org/packages/screen_state)
noise.        | + | + | thomas | 
activity recognition | + | + | bardram | [activity_recognition](https://pub.dartlang.org/packages/activity_recognition)
weather       | + | + | thomas | [OpenWeatherMap API](https://openweathermap.org/api)
phone log     | + | - | bardram | https://pub.dartlang.org/packages/phone_log
text messages | + | - |  thomas | https://pub.dartlang.org/packages/sms
Apple HealthKit | - | ~ | thomas | https://pub.dartlang.org/packages/ios_health_kit
geofenching   | ~ | ~ | bardram | [FlutterGeofencing](https://github.com/bkonyi/FlutterGeofencing) 


### Tips & Tricks on sensor sampling

Here is a list of various lessons learned from our own testing and from other sensor frameworks:

* Location
     * StudentLife samples every [10 min](http://studentlife.cs.dartmouth.edu/dataset.html#sec:sensing:gps).
* Bluetooth scans
     * In the "Social Sensing for Epidimiological Behavior Change" Ubicomp 10 paper, they scan bluetooth and WLAN APs every *6 min.*

## Triggers

Types of triggers:

* One-off
* Periodic (frequency, duration)
* Fix-time based
* Random
* Event-based (e.g. on context/location change)


## Wearable Probes

Wearable probes integrates to wearable devices (which are connected to the phone via e.g., Bluetooth) and collects data from them. These include consumer grade wearables like a smartwatch as well as medical devices like an ECG monitor.

Device        | Status | Note
:-------------|:------:|:-------------
[Cortrium C3](https://cortrium.com)   | N/A    | part of REAFEL
[Movisens EcgMove 4](https://www.movisens.com/en/products/ecgmove-4/)  | N/A    | 
[Bittium Faros](https://www.bittium.com/products_services/medical/bittium_faros#overview___applications) | N/A | part of BHRP epilepsy study at ZUH

Note that in the review

* Torous J, Powell AC. Current research and trends in the use of smartphone applications for mood disorders. Internet Interv. 2015;2(2). <doi:10.1016/j.invent.2015.03.002>.

they state that "None of the studies collected any data on physiology, despite such being increasingly possible especially in the form of heart rate or pulse monitoring through smartphones' sensors."

Hence, we should add support for this.

## Server-Server Probes

Cloud-based applications -- such as Facebook, FitBit, Withings -- that has a cloud-based api is not accessed via the phone but via a server-to-server integration in the back-end (this is the software architecture design used in CARP as opposed to e.g. AWARE and the way they integrate to e.g. [FitBit](http://www.awareframework.com/plugin/?package=com.aware.plugin.fitbit)).



## Device Health

In general, CAMS should support the monitoring of device health, i.e. a status on how a device (mobile phone or wearable) is doing:

* power (battery level, charging)
* data sampling
* online
* in use
* ...


# Data Backends

CARP Sensing supports the following data back-ends.

## Files

The `FileDataEndPoint` allow sensing data to be stored in a JSON file on the local device. A `FileDataEndPoint` endpoint can be created and added to the study protocol, like this:

```
Study myStudy = ...

final FileDataEndPoint fileEndPoint = new FileDataEndPoint(DataEndPointType.FILE);
fileEndPoint.bufferSize = 500 * 1000;
fileEndPoint.zip = true;
fileEndPoint.encrypt = false;
myStudy.dataEndPoint = fileEndPoint;
```

Files will be stored in the directory where the application may place files that are private to the application. On iOS, this is the `NSDocumentsDirectory` and on Android, this is the `AppData` directory. CARP sensing data is store in a subfolder called `carp/data/<study_id>/`. 
Filenames are following the schema of `carp-data-yyyy-mm-dd-hh-mm-ss-ms.json`
So a CARP JSON data file would look like.

`
carp/data/<study_id>/carp-data-yyyy-mm-dd-hh-mm-ss-ms.json
`

`.zip` is added, if the JSON file is zipped.


## Google Firebase

### Firebase Resources and References

* **[Firebase](https://firebase.google.com)** | [FlutterFire plugins](https://github.com/flutter/plugins/blob/master/FlutterFire.md) | [firebase_storage](https://pub.dartlang.org/packages/firebase_storage) | [flutter firebase codelab](https://codelabs.developers.google.com/codelabs/flutter-firebase/#0)

### Firebase Storage

carp.sensing-flutter can upload files to Goggle Firebase Storage (GFS) as raw (zipped) JSON file (generated using the `FileDataEndPoint` described above). In GFS, sensing data is stored in the `path` specified in the `FirebaseStorageDataEndPoint` plus subfolders for each study and device. The path on GFS hence follow this pattern:

`/<path>/<study_id>/<device_id>/`

### Setting up support for GFS

#### In GFS
* create a Firebase project with a [cloud storage](https://firebase.google.com/docs/storage/)
* add Firebase to your [Android](https://firebase.google.com/docs/android/setup) or [iOS](https://firebase.google.com/docs/ios/setup) project
* [download the  configuration file](https://support.google.com/firebase/answer/7015592)
    * and make sure to put in in your app folder like `<appname>/android/app/google-services.json`
* configure [authentication](https://firebase.google.com/docs/auth/)
    * in carp.sensing-flutter we use [username/password](https://firebase.google.com/docs/auth/android/password-auth) authentication
* add users that can upload data e.g. via the console
* add [storage security rules](https://firebase.google.com/docs/storage/security/start)

```
# Default access rule
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

```
# No authentication rule
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read, write;
    }
  }
}
```

#### In Flutter
* use [firebase_storage](https://pub.dartlang.org/packages/firebase_storage) for access to GFS
* use [firebase_auth](https://pub.dartlang.org/packages/firebase_auth) for authentication to GFS
    * note that you need to edit two `build.gradle` files in Android - follow the instructions on the [firebase_auth](https://pub.dartlang.org/packages/firebase_auth) page.
* use [google_sign_in](https://pub.dartlang.org/packages/google_sign_in) for signing into Google
    * note that you have to register your app in GFS (see above) before this works
* use the [`signInWithEmailAndPassword`](https://pub.dartlang.org/documentation/firebase_auth/latest/firebase_auth/FirebaseAuth/signInWithEmailAndPassword.html) method to sign in using email / password before uploading data


### SDK version numbers

As described as item #7 in [Getting started with Firebase and Flutter](https://firebase.google.com/docs/flutter/setup), we should add support for the [Google Services Gradle plugin](https://developers.google.com/android/guides/google-services-plugin) to read the `google-services.json` file that was generated by Firebase.

In your IDE, open `android/app/build.gradle`, and add the following line as the last line in the file:    

`apply plugin: 'com.google.gms.google-services'`
    
In `android/build.gradle`, inside the `buildscript` tag, add a new dependency:

```
 dependencies {
        // ...
        classpath 'com.google.gms:google-services:4.0.1'   // new
    }
```

Note the version of this SDK may change - see [Add Firebase to Your Android Project](https://firebase.google.com/docs/android/setup#manually_add_firebase). Currently, the suggested version `3.2.1` or `4.1.0` **does not** work?#!? The only version that seems to work is `4.0.1` or `4.1.0`.

You may also get the following compile error

```
FAILURE: Build failed with an exception.

* What went wrong:
Failed to capture snapshot of input files for task ':app:preDebugBuild' property 'compileManifests' during up-to-date check.
> The library com.google.android.gms:play-services-base is being requested by various other libraries at [[15.0.1,15.0.1]], but resolves to 16.0.1. Disable the plugin and check your dependencies tree using ./gradlew :app:dependencies.
```

This is a conflict between SDK v. 15 and 16 and arose around Oct'18 due to some Firebase update. There is lengthy discussion of it at this [Github issue](https://github.com/evollu/react-native-fcm/issues/1026) and this [Stack Overflow question](https://stackoverflow.com/questions/50577437/com-google-android-gmsplay-services-measurement-base-is-being-requested-by-vari).

The solution that (temporary) worked for me was to add

`com.google.gms.googleservices.GoogleServicesPlugin.config.disableVersionCheck = true`

to the end of the `android/app/build.graddle` file, so it look like:

```
// from firebase_auth plugin
// see https://pub.dartlang.org/packages/firebase_auth
apply plugin: 'com.google.gms.google-services'
// SDK 15 vs. 16 conflict resolution
// See https://github.com/evollu/react-native-fcm/issues/1026
com.google.gms.googleservices.GoogleServicesPlugin.config.disableVersionCheck = true
```

### DEX Compile Error in Android

Once you're ready to compile/run your application which is accessing Firebase, you might encounter the following DEX Compile error:

```
D8: Cannot fit requested classes in a single dex file. Try supplying a main-dex list.
# methods: 68134 > 65536

FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':app:transformDexArchiveWithExternalLibsDexMergerForDebug'.
> com.android.builder.dexing.DexArchiveMergerException: Error while merging dex archives: /Users/bardram/dev/carp/carp.sensing-flutter/example/build/app/intermediates/transforms/dexBuilder/debug/293.jar, ...
  The number of method references in a .dex file cannot exceed 64K.
  Learn how to resolve this issue at https://developer.android.com/tools/building/multidex.html
```

The Android API has an explanation on how to [Enable multidex for apps with over 64K methods](https://developer.android.com/studio/build/multidex). You need to update the file 

```
<project_name>/android/app/build.gradle
```

and add **`multiDexEnabled true`** like this:

```
android {
    defaultConfig {
        ...
        minSdkVersion 21 
        targetSdkVersion 26
        multiDexEnabled true
    }
    ...
}
```

## CARP

## Open mHealth (OMH)

# Micro Service Architecture

Implemented using [`dart:isolate`](https://api.dartlang.org/stable/2.1.0/dart-isolate/dart-isolate-library.html) library. Useful links:

## Isolates

* [Isolates](https://api.dartlang.org/stable/2.3.1/dart-isolate/dart-isolate-library.html)
* [Dart Fundamentals – Isolates](https://codingwithjoe.com/dart-fundamentals-isolates/).
* [Isolates: how to work with multithreading in Dart](https://lucamezzalira.com/2013/06/11/isolates-how-to-work-with-multithreading-in-dart/).
* [Futures - Isolates - Event Loop](https://www.didierboelens.com/2019/01/futures---isolates---event-loop/) Tutorial
* [Example](http://jpryan.me/dartbyexample/examples/isolates/)
* [issue #13937](https://github.com/flutter/flutter/issues/13937) - Unable to call a platform channel method from a another isolate. 


## Background Service

* [Background processes](https://flutter.dev/docs/development/packages-and-plugins/background-processes)
* [Executing Dart in the Background with Flutter Plugins and Geofencing](https://medium.com/flutter/executing-dart-in-the-background-with-flutter-plugins-and-geofencing-2b3e40a1a124)
* [Flutter GeoFencing Framework](https://github.com/bkonyi/FlutterGeofencing) -- uses an isolate for geofencing service using the [IsolateNameServer](https://docs.flutter.io/flutter/dart-ui/IsolateNameServer-class.html) class.
* [`android_alarm_manager`](https://pub.dartlang.org/packages/android_alarm_manager#-readme-tab-).

## Flutter Platform Channels

* [Flutter Platform Channels Tutorial](https://medium.com/flutter/flutter-platform-channels-ce7f540a104e)


# Extending CAMS

The CARP Mobile Sensing (CAMS) framework is designed to be extensible. Below are three typical scenarios.

## Using CARP Sensing in your own app

## Creating a new sensing capability

## Connecting to a new data backend

## Supporting custom data formats


## Handling JSON de/serialization in CARP Sensing

### JSON Example

CARP Sensing uses [`json_serializable`](https://pub.dartlang.org/packages/json_serializable) for JSON de/serialization. Here is an example of how this is used.

````dart
import 'package:carp_sensing/carp_sensing.dart';
import 'package:json_annotation/json_annotation.dart';
import 'package:carp_sensing/domain/serialization.dart';

part 'study.g.dart';

/**
 * The [Study] holds information about the study to be performed on this device.
 * The study may be fetched in a [StudyManager] who knows how to fetch a study protocol for this device.
 *
 * A [Study] mainly consists of a list of [Task]s.
 */
@JsonSerializable(fieldRename: FieldRename.snake, includeIfNull: false)
class Study extends Serializable {
  String id;
  String userId;
  String name;
  String description;

  /// Specify where and how to upload this study data.
  DataEndPoint dataEndPoint;

  List<Task> tasks = new List<Task>();

  Study(this.id, this.userId, {this.name, this.description}) : super();

  static Function get fromJsonFunction => _$StudyFromJson;
  factory Study.fromJson(Map<String, dynamic> json) => _$StudyFromJson(json);
  Map<String, dynamic> toJson() => _$StudyToJson(this);

  void addTask(Task task) => tasks.add(task);
  }
}
````

### Step-by-Step 

The following thing has to be done.

1. Add the right import statements

````dart
import 'package:json_annotation/json_annotation.dart';
import 'package:carp_sensing/domain/serialization.dart';
````

2. Make this file as part of the .g file

````dart
part 'study.g.dart';
````

3. Annotate the class and extend `Serializable`

````dart
@JsonSerializable(fieldRename: FieldRename.snake, includeIfNull: false)
class Study extends Serializable {
````

4. Make sure to call the empty super constructor

````dart
  Study(this.id, this.userId, {this.name, this.description}) : super();
````

5. Add the three factory methods

````dart
  static Function get fromJsonFunction => _$StudyFromJson;
  factory Study.fromJson(Map<String, dynamic> json) => _$StudyFromJson(json);
  Map<String, dynamic> toJson() => _$StudyToJson(this);
````

### Generate .g Files

When the new data format are done, run

`flutter packages pub run build_runner build --delete-conflicting-outputs` 

in order to generate the .g files for JSON de/serialization. The command should be run from the root of the Flutter project.

# Security and Privacy

## Principles

The following key security and privacy principles applies for carp.sensing-flutter:

**Security:**
* All data is encrypted in transit and at rest. The application will not store data on the participants’ mobile device in an unencrypted form.
* All data is encrypted asymmetric using a public RSA key provided in the Study protocol. This means that only the owner of the private key (the study owner) can decrypt data.

**Privacy:**

CARP Mobile Sensing tries to ensure privacy via a few principles.

* Participant are identified via a unique 8-character ID. Names or other person-identifable information is not available to the sensing framework and all data collection is tied to the ID.
* Indirect identifiers (e.g., location data, telephone numbers, and IP addresses) can be obfuscated via plugin obfuscating techniques. For example, a hashing algorithm for phone numbers or "space warp" for geolocation.
    * common strategies (e.g., Sensus & RADAR-base) is to space warp geolocation, one-way hash phone numbers, and only collect length of sms body text.

However, in CARP we generally don't believe in "privacy-by-collection" strategy -- if you have sufficient data, you can probably reverse engineer the identity of people. Therefore, we apply a "privacy-by-storage" stragegy. Also -- if you remove data at the source, you may later need this (e.g. the real location of a person in order to compare to other real location of other persons, etc.).

One thing we could do w. location == have the user specify significant places like "home" and "work" and then only report/store this information. Calculation of when the user is at a significant place, only happens on the phone and is not stored.

## Implementation Notes

* Add PUBLIC_KEY to Study.
* Use [ninja 1.0.0](https://pub.dartlang.org/packages/ninja) for asymmetric-key encryption (RSA) encryption.

## Related work on Privacy

### PrivacyStreams

```
uqi.getData(Audio.recordPeriodic(10*1000, 2*60*1000), Purpose.HEALTH("monitoring sleep")) // Record a 10-second audio periodically with a 2-minute interval between each two records.
    .setField("loudness", AudioOperators.calcLoudness("audio_data")) // Set a customized field "loudness" for each record as the audio loudness
    .onChange("loudness", callback) // Callback with loudness value when "loudness" changes
```

"... the **granularity** and the **purpose** of private data accesses are important dimensions in determining how comfortable users are with the app behavior, as well as influencing the decision to install the app in the first place." [1, p. 4]

"Such design would make it much easier to get the purpose information, although it is just a placeholder currently and we haven’t done anything with it." [1, p. 11]

See the [Purpose](https://privacystreams.github.io/javadoc/io/github/privacystreams/core/purposes/Purpose.html) class in PrivacyStreams.

Observations on PrivacyStreams:
* Privacy is not "enforced" -- the fact that the pipeline in the code example 1 ensures privacy is because it is programmed so. The advanced pipe-and-filter transform a data point into one which is "private". BUT - this is not in anyway due to the framework as such; the framework is basically a Stream implementation, which allows a programmer to implements this. Hence, the framework does not in any way support -- and even less enforce -- privacy. Hence, the name "PrivacyStreams" is an **oversell**; it is not a "privacy" framework, merely a "stream" framework.
* The state that *"The purpose information can help end-users better understand privacy"* [p. 11]. Hence, the `Purpose` mechanism is merely to be used for showing even more information to the user, which is already completely overwhelmed with "end-user-term" and "end-user-info". This will not scale / work in practice. Hence, `Purpose` should be attached to the entire **Study**.

### Incognisense

* [paper](https://www.sciencedirect.com/science/article/pii/S1574119213000382)

## Typical Privacy Methods

See review in [2-3].

* anonymity 
* spatial cloaking / space warping >> to protect privacy in location tracking
* hashing of phone numbers, emails, etc.
* path jumbling [3] >> sensing data P2P amongst participants, before upload to de-identify users
* trusted 3rd party >> aggregation / merging of participants' data before 'upload'
* pseudonymity - replace the participants' real identity by a unique pseudonym - however, it has already been shown that the provided protection is insufficient, as the real identifies may be inferred from location information
* periodic pseudonyms - change pseudonyms on a regular basis, and a trusted 3rd party knows the link between the participants’ identity and their pseudonyms
* data perturbation - to hide the individual participants’ contributions while allowing the application server to computes statistical trends over the whole participants’ set.
* data aggregation - sensor readings from different participants are merged together to build aggregates


"integrated holistic solution is required to help them in selecting appropriate mechanisms depending on the specifications of their application" [3]

"We now observe an increasing inclusion of the participants in the control of novel privacy-preserving mechanisms. For example, their privacy preferences are taken into account during both the tasking and the reporting processes. To cater for this control, specific interfaces have been designed and evaluated based on user studies. In addition to server in the evaluation of technical approaches, user studies, such as Shilton and Martin (2013) and Christin et al. (2013a ), have been conducted to analyze potential users’ privacy expectations and how these are influenced by contextual factors (e.g., sensing modality, duration, or purpose of the data collection)." [3 p. 64]

From [2]: "To protect data privacy, PPIMs must ensure that adversaries cannot:

1. identify which user has submitted given sensed data; 
2. infer a user’s exact location during the negotiation process of an incentive mechanism;
3. cannot infer a user’s bidding profile, i.e. which task(s) the user is willing to do, when and for how long the user will contribute, and so on—during the negotiation process of an incentive mechanism;
4. determine if multiple reports were submitted by the same user; and 
5. cannot tell multiple tasks were completed by the same user.

### What does this mean for CARP?

CARP is design for health-related studies and hence anonymity etc. seems not to be relevant. I makes very little sense to remove patient identification from medical sensing data. Hence item 1-5 makes little or no sense in CARP. However, on the other hand, the protection of privacy is important. Therefore CARP apply the following design/approach:

... TBD

# Incentive Mechanisms

In mobile crowdsensing, various incentive mechanisms have been proposed to increase the number of participating users [2].


See the [Tutorial on Mobile Crowdsourcing](https://tonylt.github.io/download/ICC16_tutorial_Mobile_Crowdsourcing_incentives_trust_privacy.pdf) - incentives, trust, privacy

"The PPIMs described in this article are promising for motivating numerous users, but most do not consider the quality of the collected data. In a real-world situation, low-quality data submit- ted by malicious users may decrease system performance. To tackle this problem, researchers must design incentive mechanisms that not only protect user privacy but also encourage the sub- mission of high-quality data." [2]

"A more flexible platform would assign the most valuable tasks to the selected users to improve overall efficiency [41]" [2]

What is the incentive mechanisms for CARP? A place to start is to ask; what kind of data would we like to get?

* compliance / adherence (quantity)
* quality
* "rare" data points (e.g. manic episodes in bipolar disorder) - don't want a lot of "health subjects" (necessarily)
* allocation of tasks?

# Static Analysis

Can we use the [analyzer](https://pub.dev/packages/analyzer) Dart package to do static analysis of CAMS?
For example, look for privacy issues. See PCAF paper.

# Contribution 

What is specific / novel to CAMS?

The following is brain storm on what could be the (scientific) contribution further down the road.

* **Dedicated Programming Framework** - Most other approaches (AWARE, Sensus, Beiwe) are designed as stand-alone, dedicated mobile sensing apps / tools, and are not designed to be extensible or reusable. CARP Sensing is designed for other to extend and use in their own app development. As a framework, it is extensible along the following dimensions:
    - Sensing capability
    - Data formats (e.g. Open mHealth)
    - Data management, including upload to different back-end cloud infrastructures
    - Privacy model
* Multi-study setup -- you can create and run multiple studies on the same phone.

````dart
  Study study_1 = Study('1234', ...);
  StudyExecutor executor_1 = new StudyExecutor(study_1);
  executor_1.initialize();
  executor_1.start();

  Study study_2 = Study('5678', ...);
  StudyExecutor executor_2 = new StudyExecutor(study_2);
  executor_2.initialize();
  executor_2.start();

````
* **Cross-platform** sensing (i.e. implemented using Dart/Flutter)
    - cross-platform framework comes from using Dart/Flutter
    - however, several features are built into CARP sensing to support cross-platform
    - sampling schemas that fit / can be tailored to the different platforms
    - sampling packages that tailor for platform channels
* **Plug-in support** for different data formats, e.g. Open mHealth
* **Micro service** for sensing frameworks
    - Jeuris : "...maybe the problem with these frameworks is that they are monolitic and hard to update as the underlying OS and sensors upgrade..."
    - can be implemented as `isolates` in Dart / Flutter.
    - also the maintenance problem is solved by putting the "sensing" part of the probes into Flutter libraries.
* **Provenance** - most sensing framework does not have any way to store information about where data came from, i.e. provenance. 
    - Open mHealth have a (simple) model for provenance in their [DataPointAcquisitionProvenance](https://github.com/openmhealth/schemas/blob/master/java-schema-sdk/src/main/java/org/openmhealth/schema/domain/omh/DataPointAcquisitionProvenance.java) class.
* **Privacy** - similar to PrivacyStreams, but more elaborate. Using e.g. more formal methods to ensure that data is not misused for purposes of which was given consent to in the future processing.

When [writing](http://cs.au.dk/~amoeller/papers/jwig2/) about [JWIG](http://www.brics.dk/JWIG/), they state the following, which seems to be a good way to put it ;-)

"Although numerous frameworks for web application programming have been developed in recent years, writing web applications remains a challenging task. Guided by a collection of classical design principles, we propose yet another framework. It is based on a simple but flexible server-oriented architecture that coherently supports general aspects of modern web applications, including dynamic XML construction, session management, data persistence, caching, and authentication, but it also simplifies programming of server-push communication and integration of XHTML-based applications and XML-based web services. Through a number of examples, we argue that the framework provides a novel foundation for developing more maintainable and secure web applications." [4]

# References

1. Li, Yuanchun, et al. "PrivacyStreams: Enabling Transparency in Personal Data Processing for Mobile Apps." Proceedings of the ACM on Interactive, Mobile, Wearable and Ubiquitous Technologies 1.3 (2017): 76.
2. Zhang X, Liang L, Luo C, Cheng L. Privacy-Preserving Incentive Mechanisms for Mobile Crowdsensing. IEEE Pervasive Comput. 2018;17(3):47-57.
3. Christin D. Privacy in mobile participatory sensing: Current trends and future challenges. J Syst Softw. 2016;116:57-68.
4. Christensen, Aske Simon, Anders Møller, and Michael I. Schwartzbach. "Extending Java for high-level Web service construction." ACM Transactions on Programming Languages and Systems (TOPLAS) 25.6 (2003): 814-875.
