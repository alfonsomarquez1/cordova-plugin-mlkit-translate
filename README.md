# Cordova MLKit Blink

Cordova Plugin that implements MLKit Face Detection and Face Tracking for blink validation, returns a base64 image of the face after blink ends.

Supports both iOS and Android.

This plugin uses Google's MLKit On-device Face Detection and Face Tracking services, so make sure you follow the steps to create a firebase project ([Android](https://firebase.google.com/docs/android/setup) | [iOS](https://firebase.google.com/docs/ios/setup)) up to step 3 (where you get the google-services.json or GoogleService-Info.plist). Ignore if you already have google-services.json or GoogleService-Info.plist files.

Additionally, make sure you follow [Google's usage guidelines](https://firebase.google.com/docs/ml-kit/detect-faces) for MLKit Face Detection usage.

For face detection, MLKit and by extension this plugin support the [following features]https://firebase.google.com/docs/ml-kit/detect-faces).


#### Table of Contents

 - [Installation](#installation)
 	- [Version support](#version-support)
 	- [Dependencies](#dependencies)
 - [Variables](#variables)
 - [API](#api)
 	- [blink](#blink)


## Installation
- Clone the plugin from repo:


- Install the plugin by adding it to your project's config.xml:

```
cordova plugin add path-to-local-plugin-cloned/cordova-plugin-mlkit-blink --save
```

```
npm install @mariachi/mlkit-blink --save
```

- **Make sure you have your google-services.json or GoogleService-Info.plist file in your project's root folder.**

### Version Support

- cordova `>=9`
- cordova-android `>=8`
- cordova-ios `>=5`

### Dependencies

- This plugin depends on [cordova-plugin-firebasex](https://github.com/dpa99c/cordova-plugin-firebasex) on Android.
- This plugin depends on [cordova-support-kotlin](https://github.com/kainonly/cordova-support-kotlin) on Android.
- This plugin depends on [cordova-plugin-add-swift-support](https://github.com/akofman/cordova-plugin-add-swift-support) on iOS.


## Variables
These variables are android only

|                  Variable                   |    Default     |
| :----------------------------------------:  | :------------: |
|  CAMERAX_CORE_VERSION                       | 1.0.0-alpha09  |
|  CAMERAX_CAMERA2_VERSION                    | 1.0.0-alpha09  |
|  CAMERAX_VIEW_VERSION                       | 1.0.0-alpha06  |
|  CAMERAX_EXT_VERSION                        | 1.0.0-alpha06  |
|  CAMERAX_LIFECYCLE_VERSION                  | 1.0.0-alpha02  |
|  FIREBASE_ML_VISION_ID_VERSION              | 24.0.1         |
|  FIREBASE_ML_VISION_FACE_MODEL_ID_VERSION   | 19.0.0         |

## API

You can access all these methods via the window["MLKitBlink"] object. 

If you're using the Ionic Native wrapper, then add the plugin to your module

```
import { MLKitBlink } from '@mariachi/mlkit-blink/ngx';
@NgModule({
  declarations: [AppComponent],
  entryComponents: [],
  imports: [
    ...
  ],
  providers: [
    ...
    MLKitBlink,
    ...
  ],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

And then inject it into the component you want.

```
import { MLKitBlink } from '@mariachi/mlkit-blink';

constructor(private mlkitBlink: MLKitBlink) { }
...
```

#### blink

Open device camera and could perform a blink validation on demand, if blink validation is successful, takes a picture and returns the picture on a base64 string.

##### Parameters

- {string} title - Navigation Bar Title
- {function} success - (promisified in ionic native wrapper) callback function which takes a parameter data which will be invoked on success
- {function} error - (promisified in ionic native wrapper) callback function which takes a parameter err which will be invoked on failure

##### Example

```
window["MLKitBlink"].blink("Blink Validation",
    (data)=>console.log(`Base64 picture ${data}`),
    (err)=>console.log("Something went wrong with the blink validation")
})
```

with Ionic Native

```
this.mlkitBlink.blink("Blink Validation").then(base64Picture=>{
    console.log(console.log(`Base64 picture ${base64Picture}`))
})
```

## LICENSE

This plugin is licensed under the [MIT License](LICENSE)
