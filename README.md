1. Follow https://docs.flutter.dev/perf/deferred-components
- Add dependency on `android/app/build.gradle`

```gradle
dependencies {
  implementation "com.google.android.play:core:1.8.0"
}
```

- Update `android/app/src/main/AndroidManifest.xml`

```xml
android:name="io.flutter.embedding.android.FlutterPlayStoreSplitApplication"
```

- Update Flutter project's `pubspec.yaml`

```yaml
flutter:
  deferred-components:
    - name: boxComponent
      libraries:
        - package:reproduce_deferred_components/box.dart
```

- Implement Dart code: https://docs.flutter.dev/perf/deferred-components#step-2-implementing-deferred-dart-libraries

- Add to `/android/app/src/main/res/values/strings.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="boxComponentName">boxComponent</string>
</resources>
```

- Update `android/app/src/main/AndroidManifest.xml`

```xml
<meta-data android:name="io.flutter.embedding.engine.deferredcomponents.DeferredComponentManager.loadingUnitMapping" android:value="2:boxComponent"/>
```

- Update `android/settings.gradle`

```gradle
include ":app", ":boxComponent"
```

2. build app

```console
flutter build appbundle
```
After the build, you will see it's incomplete:

```console
➜  reproduce_deferred_components flutter build appbundle
  build/android_deferred_components_setup_files/boxComponent/build.gradle (created)
  build/android_deferred_components_setup_files/boxComponent/src/main/AndroidManifest.xml (created)
=================================================================================
                     Deferred components prebuild validation
=================================================================================
Newly generated android files:
  - reproduce_deferred_components/build/android_deferred_components_setup_files/boxComponent/build.gradle
  - reproduce_deferred_components/build/android_deferred_components_setup_files/boxComponent/src/main/AndroidManifest.xml

The above files have been placed into `build/android_deferred_components_setup_files`,
a temporary directory. The files should be reviewed and moved into the project's
`android` directory.
---------------------------------------------------------------------------------

Setup verification can be skipped by passing the `--no-validate-deferred-components`
flag, however, doing so may put your app at risk of not functioning even if the
build is successful.
=================================================================================
Setup for deferred components incomplete. See recommended actions.
```

And there is `boxComponent` directory in `build/android_deferred_components_setup_files/`. Let's copy entire `boxComponent` directory to `android` directory.

3. build app again

```console
flutter build appbundle
```