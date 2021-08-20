# Mati Flutter Plugin User Guide &mdash; BETA

_**IMPORTANT**_ The Mati Flutter plugin may change significantly as it undergoes testing.

## Dependencies


Add `mati_plugin_flutter` dependency to your `pubspec.yaml` file:

```yaml
  mati_plugin_flutter: 3.3.1
```

## Install the Flutter Plugin

Install the Mati Flutter plugin for:
* [Android](#android)
* [iOS](#ios)

### Android

For Android check that the `minSdkVersion` in `<YOUR_APP>/build.gradle` is &#8805;21

### iOS

Add the following to `info.plist`:

```xml
  <key>NSCameraUsageDescription</key>
  <string>Mati verification SDK requires camera use</string>

  <key>NSMicrophoneUsageDescription</key>
  <string>Mati verification SDK requires microphone use</string>

  <key>NSPhotoLibraryUsageDescription</key>
  <string>Mati verification SDK requires access to media library</string>
```

## Implement Mati in Your App

To start the Mati verification flow, call the following when the user taps on the Mati button:

```java
  MatiFlutter.showMatiFlow(CLIENT_ID, FLOW_ID, METADATA)
```


The verification results will arrive in `MatiFlutter.resultCompleter.future` in a reply similar to the following:

```c++
MatiFlutter.resultCompleter.future.then((result) => Fluttertoast.showToast(
      msg: result is ResultSuccess ? "Success ${result.verificationId}" : "Cancelled",
      toastLength: Toast.LENGTH_SHORT,
      gravity: ToastGravity.BOTTOM));
```

### Example Application

The following is an example application (MyApp) with the Mati verification flow:

```c++
import 'package:flutter/material.dart';
import 'package:mati_plugin_flutter/mati_plugin_flutter.dart';
import 'package:fluttertoast/fluttertoast.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Mati flutter plugin demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyHomePage(),
    );
  }
}

class MyHomePage extends StatefulWidget {
  MyHomePage({Key? key}) : super(key: key);

  @override
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {

  void showMatiFlow() {
    MatiFlutter.showMatiFlow("CLIENT_ID", "FLOW_ID", {});
    MatiFlutter.resultCompleter.future.then((result) => Fluttertoast.showToast(
        msg: result is ResultSuccess ? "Success ${result.verificationId}" : "Cancelled",
        toastLength: Toast.LENGTH_SHORT,
        gravity: ToastGravity.BOTTOM));
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Mati flutter plugin demo"),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: showMatiFlow,
          child: const Text('Verify me'),
        )
      )
    );
  }
}
```
