# onboarding-web <img src="https://github.com/alice-biometrics/custom-emojis/blob/master/images/web.png" width="26"> [![npm version](https://img.shields.io/npm/v/aliceonboarding.svg?style=flat)](https://www.npmjs.com/package/aliceonboarding) [![demo](https://img.shields.io/badge/demo-jsfiddle-51CB56)](https://jsfiddle.net/alicebiometrics/62rfatbg/embedded/js,result,html,css/dark/) [![doc](https://img.shields.io/badge/doc-onboarding-51CB56)](https://docs.alicebiometrics.com/onboarding/) [![doc-web](https://img.shields.io/badge/doc-web-51CB56)](https://docs.alicebiometrics.com/onboarding/sdk/web/)

<img src="https://github.com/alice-biometrics/custom-emojis/blob/master/images/alice_header.png" width=auto>

ALiCE Onboarding Web component allows the automatic capture of documents and video selfie of the user in real time from the camera of your device. It also simplifies the communication with the onboarding API to facilitate rapid integration and development. It manages the onboarding flow configuration: requested documents and order.

The main features are:

- Automatic capture of documents and video selfie of the user in real time from the camera of your device.
- Communication with the onboarding API to facilitate rapid integration and development.
- Manage the onboarding flow configuration: requested documents and order.

## Table of Contents
- [Requirements](#requirements)
- [Getting Started :chart_with_upwards_trend:](#getting-started-chart_with_upwards_trend)
  * [Import the library](#import-the-library)
  * [HTML](#html)
  * [Configuration](#configuration)
  * [Run ALiCE Onboarding](#run-alice-onboarding)
- [Onboarding Commands](#onboarding-commands)
- [Authentication :closed_lock_with_key:](#authentication-closed_lock_with_key)
  * [Trial](#trial)
  * [Production](#production)
- [Demo :rocket:](#demo-rocket)
- [Customisation :gear:](#customisation-gear)
- [Documentation :page_facing_up:](#documentation-page_facing_up)
- [Contact :mailbox_with_mail:](#contact-mailbox_with_mail)


## Requirements

As this SDK uses [`getUserMedia`](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia) for accessing the webcam a [secure context](https://developer.mozilla.org/en-US/docs/Web/Security/Secure_Contexts) is required when using it.

## Getting Started :chart_with_upwards_trend:

### Import the library

#### HTML Script Tag Include

Include ALiCE Onboarding as a regular script tag on your page:

```html
<script src='https://unpkg.com/aliceonboarding@latest/dist/aliceonboarding.min.js'></script>
```

And its CSS:

```html
<link rel="stylesheet" href="https://unpkg.com/aliceonboarding@latest/dist/aliceonboarding.css" />
```

#### NPM style import

You can also import it as a module into your own JS build system (tested with Webpack).

```
npm install aliceonboarding
```

```
import { Onboarding, OnboardingConfig, DocumentType } from "aliceonboarding";
import "aliceonboarding/dist/aliceonboarding.css";
```

> :warning: Using this import style you won't need the namespace `aliceonboarding` on the examples below.

> :warning: This module requires ES9.

### HTML

Add a div element with onboarding tag inside in your page to load the Alice Onboarding component:

```html
<div id="alice-onboarding-mount"/>
```

### Configuration

You can configure the onboarding flow with the following code:

```js
let userToken = "<ADD-YOUR-USER-TOKEN-HERE>"

let config = new aliceonboarding.OnboardingConfig()
  .withUserToken(userToken)
  .withAddSelfieStage()
  .withAddDocumentStage(aliceonboarding.DocumentType.IDCARD)
  .withAddDocumentStage(aliceonboarding.DocumentType.DRIVERLICENSE)
```

Where `userToken` is used to secure requests made by the users on their mobile devices or web clients. You should obtain it from your Backend.

#### Selfie stage configuration parameters

`withAddSelfieStage` has a boolean parameter to indicate if the selfie capture should be triggered manually by clicking a buton or automatically when a face is detected. Its default value is `false`.

#### Document stage configuration parameters

`withAddDocumentStage` has the following parameters (in order):
* `documentType`: type of document to capture. The enum `aliceonboarding.DocumentType` can be used to indicate the type.
* `documentCapturerType`: type of capturer to use. The enum `aliceonboarding.DocumentCapturerType` can be used to indicate the capturer.  Default value is `DocumentCapturerType.FILE` meaning that the documents will be uploaded as files. You can set it to `DocumentCapturerType.CAMERA` to use the webcam or set it to `DocumentCapturerType.ALL` to allow both options to the user.
* `automaticCapture`: boolean parameter to indicate if the document capture should be triggered manually by clicking a buton or automatically when a document is detected. Default value is `false`.
* `issuingCountry`: country ISO 3166-1 alpha-3 code. Don't set it to allow the user to choose the country.

```js
export const DocumentType = {
  IDCARD: "idcard",
  DRIVERLICENSE: "driverlicense",
  RESIDENCEPERMIT: "residencepermit",
  PASSPORT: "passport"
};

export const DocumentCapturerType = {
  CAMERA: "camera",
  FILE: "file",
  ALL: "all"
};
```

### Run ALiCE Onboarding

Once you configured the ALiCE Onboarding Flow, you can run the process with:

```js
function onSuccess(userInfo) {console.log("onSuccess: " + userInfo)}
function onFailure(error) {console.log("onFailure: " + error)}
function onCancel() { console.log("onCancel")}

new aliceonboarding.Onboarding("alice-onboarding-mount", config).run(onSuccess, onFailure, onCancel);
```

## Onboarding Commands
ALiCE Onboarding Client SDK can be used in two different ways, by commands (OnboardingCommands) or by full flow (Onboarding). The commands allow you to open the grabber and do a series of operations independently. Available operations are as follows:

* Get User Status
* Add Selfie
* Create Document
* Add Document
* Get supported documents
* Authenticate a User already enrolled and authorized
* Get Documents supported

### Initialize Onboarding Commands

To get the userToken please see the authentication section.

```js
let config = new aliceonboarding.OnboardingConfig()
.withCustomLocalization("en");

let onboardingCommands = new aliceonboarding.OnboardingCommands("alice-onboarding-mount", userToken, config);
```

### Get status command

Returns an ALiCE Onboarding User Status

* Parameter onSuccess: This handler will give you call back inside block when success response with the user status.

* Parameter onError: This handler will give you call back inside block when error response is recieved.

```js
onboardingCommands.getUserStatus(
      (userStatusResult) => { // onSuccess
              console.log(userStatusResult)
          },
      (error) => { //onError
              console.error(error)
          }
  );
```

### Adding selfie command

Presents an ALiCE Onboarding Selfie Capture View sending a video directly to ALiCE Onboarding platform.

* Parameter onSuccess: This handler will give you call back inside block when the Selfie was added.

* Parameter onCancel: This handler will give you call back inside block when user cancel the stage.

```js
onboardingCommands.addSelfie(
      (succesResult) => { // onSuccess
              console.log(succesResult)
      },
      (cancelResult) => { //onCancel
              console.error(cancelResult)
          }
     );
```

### Create document command

Creates a document given the type and the issuingCountry:

* Parameter documentType: DocumentType.

* Parameter issuingCountry: String value. You can check supported documents with the command getDocumentsSupported.

* Parameter callback: This handler will give you call back inside block when response is recieved.

On onSuccess, it returns a result with the document_id on the success content. This identifier (documentId) is neccessary to add documents.

```js
onboardingCommands.createDocument(
    "passport",   //documentType
    "ESP",        //issuingCountry
    (result) => { //onSuccess
            console.log(result.document_id)   //returns the document id of the created document
        },
    (error) => { //onError
            console.error(error)
        }
    );
```

### Add document command
Presents an ALiCE Onboarding Document Capture View sending images to ALiCE Onboarding platform

* Parameter documentId: Document identifier given by createDocument command

* Parameter documentType: DocumentType.

* Parameter issuingCountry: String value. You can check supported documents with the command getDocumentsSupported.

* Parameter side: DocumentSide.

* Parameter onSuccess: This handler will give you call back inside block when response the document was captured.

* Parameter onCancel: This handler will give you call back inside block when user cancel the stage.

```js
onboardingCommands.addDocument(
    result.document_id,        //with the document_id of the previously created document, look at the previous command.
    "passport",                //documentType
    "ESP",                     //issuingCountry
    "back",                     //documentSide  (front or back)
    (result) => { //onSuccess
            console.log(result)
         },
    (error) => { //onError
            console.error(error)
        }
    (cancel) => { //onCancel
            console.error(cancel)
    }
);
```

### Authenticate a User already enrolled and authorized

Present an ALiCE Onboarding Authenticate Capture View sending a video directly to ALiCE Onboarding platform and checking if user is Login allowed or not.

* Parameter onSuccess: This handler will give you call back inside block when the authentication is done.

* Parameter onError: This handler will give you call back inside block when errors response is recieved.

* Parameter onCancel: This handler will give you call back inside block when user cancel the stage.

```js
onboardingCommands.authenticate(
       (result) => { // onSuccess
              console.log(result)
          },
      (cancel) => { //onCancel
              console.error(cancel)
          }
      (error) => { //onError
              console.error(error)
          }
);
```

### Get supported documents

Returns an ALiCE Onboarding supported documents (hierarchically ordered).

* Parameter onSuccess: This handler will give you call back inside block when success response is recieved.

* Parameter onError: This handler will give you call back inside block when error response is recieved.

```js
onboardingCommands.getDocumentsSupported(
     (suportedDocuments) => { // onSuccess
              console.log(suportedDocuments)
          },
      (error) => { //onError
              console.error(error)
          }
);
``` 


## Authentication :closed_lock_with_key:

How can we get the `userToken` to start testing ALiCE Onboarding technology?

`AliceOnboarding` can be used with two differnet authentication modes:

* Trial (Using ALiCE Onboarding Sandbox): Recommended only in the early stages of integration.
    - Pros: This mode does not need backend integration.
    - Cons: Security.
* Production (Using your Backend): In a production deployment we strongly recommend to use your backend to obtain required TOKENS.
    - Pros: Security. Only your backend is able to do critical operations.
    - Cons: Needs some integration in your backend.

### Trial

If you want to test the technology without integrate it with your backend, you can use our Sandbox Service. This service associates a user mail with the ALiCE Onboarding `user_id`. You can create a user and obtain his `USER_TOKEN` already linked with the email.

Use the `SandboxAuthenticator` class to ease the integration.

```js
let sandboxToken = "<ADD-YOUR-SANDBOX-TOKEN-HERE>";
let userInfo = new aliceonboarding.UserInfo(email: email, // required
                        firstName: firstName, // optional 
                        lastName: lastName);  // optional 
                        
let authenticator = new aliceonboarding.SandboxAuthenticator(sandboxToken, userInfo);

authenticator.execute()
.then( userToken => {  
   // Configure ALiCE Onboarding with the OnboardingConfig
   // Then, Run the ALiCE Onboarding Flow
})
.catch(err => {
  // Inform the user about Authentication Errors
});
```

Where `sandboxToken` is a temporal token for testing the technology in a development/testing environment. 

An `email` parameter in `UserInfo` is required to associate it to an ALiCE Onboarding `user_id`. You can also add some additional information from your user as `firstName` and `lastName`.

For more information about the Sandbox, please check the following [doc](https://docs.alicebiometrics.com/onboarding/access.html#using-alice-onboarding-sandbox).

### Production

On the other hand, for a production environments we strongly recommend to use your backend to obtain required `USER_TOKEN`.

You can implement the `Authenticator` protocol available in the `AliceOnboarding` framework.

```js
export class MyBackendAuthenticator extends Authenticator {
  constructor() {
    super();
  }
  execute() {
    return new Promise((resolve, reject) => {
    
      // Add here your code to retrieve the user token from your backend

      let userToken = "fakeUserToken";
      resolve(userToken);
    });
  }
}
```

In a very similar way to the authentication available with the sandbox:

```swift
                        
let authenticator = MyBackendAuthenticator()

authenticator.execute()
.then( userToken => {  
   // Configure ALiCE Onboarding with the OnboardingConfig
   // Then, Run the ALiCE Onboarding Flow
})
.catch( error => {
  // Inform the user about Authentication Errors
});
```

## Demo :rocket:

Check our JSFiddle demo [here](https://jsfiddle.net/alicebiometrics/62rfatbg/embedded/js,result,html,css/dark/). Remember you must add your `SANDBOX_TOKEN` credentials. If you have already integrated ALiCE Onboarding in your backend, remove the `aliceonboarding.logInWithSandbox` function and use your retrieved `USER_TOKEN`. 


## Customisation :gear:

##### Localization

Supported languages are:
- Catalan
- Chinese (simplified)
- English
- French
- Galician
- German
- Italian
- Spanish
- Polish
- Portuguese
- Russian
- Turkish

```js
let userToken = "<ADD-YOUR-USER-TOKEN-HERE>"
let language = "en" (default)

let config = new aliceonboarding.OnboardingConfig()
  .withUserToken(userToken)
  .withAddSelfieStage()
  .withAddDocumentStage(onboarding.DocumentType.IDCARD)
  .withAddDocumentStage(onboarding.DocumentType.DRIVERLICENSE)
  .withCustomLocalization(language)
```

##### Themes

The SDK is provided with themes for:
- Typography (**"corporate"**, "modern", "classic")
- Icons (**"classic"**, "modern", "minimal")
- Icons variant (**"circle"**, "square", "shape")

Note: default values are in bold.

You can choose the themes with the OnboardingConfig method `withTheme`. That has the following parameters:
- Typography theme
- Icon theme (Optional. Default value is "minimal")
- Icon variant (Optional. Default value is "circle")

Example:
```
let config = new aliceonboarding.OnboardingConfig()
  .withUserToken(userToken)
  .withAddSelfieStage()
  .withAddDocumentStage(onboarding.DocumentType.IDCARD)
  .withTheme("corporate", "minimal", "square")
```
NOTE: only supported with Onboarding Flow.

## Extra

### Welcome 🎁

We provide an additional component for retrieving the user information (email, first name and/or last name). You can use this component before the onboarding process to retrieve this info in case you don't already have it.

```js
  let config = {
    language: "en",
    requiredInfo: ["email"] // Possible options are "email", "firstName" and/or "lastName"
  };
  
  function onUserInfo(userInfo) {} // Callback that will receive the user information
  function onCancel() {} // Callback to handle cancellation

  new aliceonboarding.OnboardingWelcome("alice-onboarding-mount", config).run(
    onUserInfo, onCancel
  );
 ```



## Documentation :page_facing_up:

For more information about ALiCE Onboarding:  https://docs.alicebiometrics.com/onboarding/

## Contact :mailbox_with_mail:

support@alicebiometrics.com
