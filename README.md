# Cordova MLKit Blink

[![npm](https://img.shields.io/npm/v/cordova-plugin-mlkit-blink?style=for-the-badge)](https://www.npmjs.com/package/cordova-plugin-mlkit-blink)
[![npm](https://img.shields.io/npm/dt/cordova-plugin-mlkit-blink?style=for-the-badge)](https://www.npmjs.com/package/cordova-plugin-mlkit-blink)
[![NPM](https://img.shields.io/npm/l/cordova-plugin-mlkit-blink?style=for-the-badge)](LICENSE)

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
 	- [translate](#translate)
 	- [identifyLanguage](#identifylanguage) 	 	
 	- [getDownloadedModels](#getdownloadedmodels)
 	- [getAvailableModels](#getavailablemodels)
 	- [downloadModel](#downloadmodel)
 	- [deleteModel](#deletemodel)


## Installation

- Install the plugin by adding it to your project's config.xml:

```
<plugin name="cordova-plugin-mlkit-blink" spec="latest" />
```

or

```
cordova plugin add cordova-plugin-mlkit-blink
```

- *(Optional) If you're using ionic, make sure you use the Ionic native wrapper.*
```
#npm install @ionic-native/mlkit-translate
npm install cordova-plugin-mlkit-blink
```

- **Make sure you have your google-services.json or GoogleService-Info.plist file in your project's root folder.**

### Version Support

- cordova `>=9`
- cordova-android `>=8`
- cordova-ios `>=5`

### Dependencies

- This plugin depends on [cordova-plugin-firebasex](https://github.com/dpa99c/cordova-plugin-firebasex) on Android.
- This plugin depends on [cordova-plugin-add-swift-support](https://github.com/akofman/cordova-plugin-add-swift-support) on iOS.


## Variables
These variables are android only

|                  Variable                   | Default |
| :----------------------------------------:  | :-----: |
|    FIREBASE_ML_NATURAL_LANGUAGE_VERSION     | 22.0.0  |
| FIREBASE_ML_NATURAL_LANGUAGE_MODEL_VERSION  | 20.0.7  |
|  FIREBASE_ML_NATURAL_LANGUAGE_ID_VERSION    | 20.0.7  |
|  FIREBASE_ML_VISION_ID_VERSION              | 24.0.1  |
|  FIREBASE_ML_VISION_FACE_MODEL_ID_VERSION   | 19.0.0  |

## API

You can access all these methods via the window["MLKitBlink"] object. 

If you're using the Ionic Native wrapper, then add the plugin to your module

```
import { MLKitBlink } from '@ionic-native/mlkit-blink/ngx';
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
import { MLKitBlink } from '@ionic-native/ml-kit-blink';

constructor(private mlkitBlink: MLKitBlink) { }
...
```

#### translate

Translates text from one language to another. Requires the source and target languages need to be downloaded. If not the languages are downloaded in the background automatically.

##### Parameters

- {string} text - text to be translated
- {string} targetLanguage - language code of the language to translate to
- {string} sourceLanguage - (optional) language code of the language to translate from, if not present the language is inferred.
- {function} success - (promisified in ionic native wrapper) callback function which takes a parameter data which will be invoked on success
- {function} error - (promisified in ionic native wrapper) callback function which takes a parameter err which will be invoked on failure

##### Example

```
window["MLKitTranslate"].translate("hello", "es", "en",
    (data)=>console.log(`Translated text is ${data}`),
    (err)=>console.log("Something went wrong with the translation")
})
// prints Translated text is hola
```

with Ionic Native

```
this.mlkitTranslate.translate("hello", "es", "en").then(translatedText=>{
    console.log(console.log(`Translated text is ${translatedText}`))
})
// prints Translated text is hola
```

#### identifyLanguage

Determines the language of a string of text.

##### Parameters

- {string} text - text to be identified
- {function} success - (promisified in ionic native wrapper) callback function which takes a parameter data which will be invoked on success
- {function} error - (promisified in ionic native wrapper) callback function which takes a parameter err which will be invoked on failure

#### Data format

Success data will be a language object with two properties

- {string} code - [BCP-47](https://en.wikipedia.org/wiki/IETF_language_tag) language code
- {string} displayName - Name of the language

##### Example

```
window["MLKitTranslate"].identifyLanguage("hello",
    (data)=>console.log("Identified text is",  data),
    (err)=>console.log("Something went wrong with the translation")
})

// prints Identified text is {"code": "en", "displayName": "English"}
```

or with Ionic Native

```
this.mlkitTranslate.identifyLanguage("hello").then(lang=>{
    console.log("Identified text is ", lang);
})

// prints Identified text is {"code": "en", "displayName": "English"}
```

#### getDownloadedModels

List of language models that have been downloaded to the device.

##### Parameters

- {function} success - (promisified in ionic native wrapper) callback function which takes a parameter data which will be invoked on success
- {function} error - (promisified in ionic native wrapper) callback function which takes a parameter err which will be invoked on failure

##### Data format

Success data will be an array with language objects (see above)

##### Example

```
window["MLKitTranslate"].getDownloadedModels(
    (data)=>console.log(data),
    (err)=>console.log(err)
})

// prints [{"code": "en", "displayName": "English"}]
```

or with Ionic Native

```
this.mlkitTranslate.getDownloadedModels().then(langs=>{
    console.log(langs);
})

// prints [{"code": "en", "displayName": "English"}]
```

#### getAvailableModels

List of language models that can be downloaded.

##### Parameters

- {function} success - (promisified in ionic native wrapper) callback function which takes a parameter data which will be invoked on success
- {function} error - (promisified in ionic native wrapper) callback function which takes a parameter err which will be invoked on failure

##### Data format

Success data will be an array with language objects (see above)

##### Example

```
window["MLKitTranslate"].getAvailableModels(
    (data)=>console.log(data),
    (err)=>console.log(err)
})

// prints [{"code": "en", "displayName": "English"}, {"code", "es", "displayName": "Spanish"}, ...]
```

or with Ionic Native

```
this.mlkitTranslate.getAvailableModels().then(langs=>{
    console.log(langs);
})

// prints [{"code": "en", "displayName": "English"}, {"code", "es", "displayName": "Spanish"}, ...]
```

#### downloadModel

Downloads a specified language model.

##### Parameters

- {string} code - [BCP-47](https://en.wikipedia.org/wiki/IETF_language_tag) language code of the language to download
- {function} success - (promisified in ionic native wrapper) callback function which takes a parameter data which will be invoked on success
- {function} error - (promisified in ionic native wrapper) callback function which takes a parameter err which will be invoked on failure

##### Data format

Success data will be a language object of the downloaded language.

##### Example

```
window["MLKitTranslate"].downloadModel("es",
    (data)=>console.log(data),
    (err)=>console.log(err)
})

// prints {"code", "es", "displayName": "Spanish"}
```

or with Ionic Native

```
this.mlkitTranslate.downloadModel("es").then(lang=>{
    console.log(langs);
})

// prints {"code", "es", "displayName": "Spanish"}
```

#### deleteModel

Delete a specified language model.

##### Parameters

- {string} code - [BCP-47](https://en.wikipedia.org/wiki/IETF_language_tag) language code of the language to delete
- {function} success - (promisified in ionic native wrapper) callback function which takes a parameter data which will be invoked on success
- {function} error - (promisified in ionic native wrapper) callback function which takes a parameter err which will be invoked on failure

##### Data format

Success data will be a language object of the deleted language.

##### Example

```
window["MLKitTranslate"].deleteModel("es",
    (data)=>console.log(data),
    (err)=>console.log(err)
})

// prints {"code", "es", "displayName": "Spanish"}
```

or with Ionic Native

```
this.mlkitTranslate.deleteModel("es").then(lang=>{
    console.log(langs);
})

// prints {"code", "es", "displayName": "Spanish"}
```

## LICENSE

This plugin is licensed under the [MIT License](LICENSE)
