# onboarding-web :earth_africa: [![doc](https://img.shields.io/badge/doc-onboarding-51CB56)](https://docs.alicebiometrics.com/onboarding/) [![doc-web](https://img.shields.io/badge/doc-web-51CB56)](https://docs.alicebiometrics.com/onboarding/sdk/web/)

ALiCE Onboarding Web component allows the automatic capture of documents and video selfie of the user in real time from the camera of your device. It also simplifies the communication with the onboarding API to facilitate rapid integration and development. It manages the onboarding flow configuration: requested documents and order.

The main features are:

- Automatic capture of documents and video selfie of the user in real time from the camera of your device.
- Communication with the onboarding API to facilitate rapid integration and development.
- Manage the onboarding flow configuration: requested documents and order.

## Table of Contents
- [Installation :computer:](#installation-computer)
- [Getting Started :chart_with_upwards_trend:](#getting-started-chart_with_upwards_trend)
  * [Import the library](#import-the-library)
  * [HTML](#html)
  * [Configuration](#configuration)
  * [Run ALiCE Onboarding](#run-alice-onboarding)
- [Authentication :closed_lock_with_key:](#authentication-closed_lock_with_key)
  * [Trial](#trial)
  * [Production](#production)
- [Demo :rocket:](#demo-rocket)
- [Customisation :gear:](#customisation-gear)
- [Documentation :page_facing_up:](#documentation-page_facing_up)
- [Contact :mailbox_with_mail:](#contact-mailbox_with_mail)



## Installation :computer:

```
npm install aliceonboarding
```


## Getting Started :chart_with_upwards_trend:


### Import the library

Include ALiCE Onboarding as a regular script tag on your page:

```html
<script src='dist/aliceonboarding.umd.min.js'></script>
```

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
  .withAddDocumentStage(aliceonboarding.DocumentType.IDCARD, "ESP")
  .withAddDocumentStage(aliceonboarding.DocumentType.DRIVERLICENSE, "ESP")
```

Where `userToken` is used to secure requests made by the users on their mobile devices or web clients. You should obtain it from your Backend.

### Run ALiCE Onboarding

Once you configured the ALiCE Onboarding Flow, you can run the process with:

```js
function onSuccess(userInfo) {console.log("onSuccess: " + userInfo)}
function onFailure(error) {console.log("onFailure: " + error)}
function onCancel() { console.log("onCancel")}

new aliceonboarding.Onboarding("#alice-onboarding-mount", config).run(onSuccess, onFailure, onCancel);
```

## Authentication :closed_lock_with_key:

How can we get the `userToken` to start testing ALiCE Onboarding technology?

`AliceOnboarding` can be used with two differnet authentication modes:

* Trial (Using ALiCE Onboarding Sandbox): Recommended only in the early stages of integration.
    - Pros: This mode do not need backend integration.
    - Cons: Security.
* Production (Using your Backend): In a production deployment we strongly recommend to use your backend to obtain required TOKENS.
    - Pros: Security. Only your backend is able to do critical operations.
    - Cons: Needs some integration in your backend.

### Trial

If you want to test the technology without integrate it with your backend, you can use our Sandbox Service. This service associates a user mail with the ALiCE Onboarding `user_id`. You can create an user and obtain its `USER_TOKEN` already linked with the email.

Use the `SandboxAuthenticator` class to ease the integration.

```js
let sandboxToken = "<ADD-YOUR-SANDBOX-TOKEN-HERE>";
let userInfo = UserInfo(email: email, // required
                        firstName: firstName, // optional 
                        lastName: lastName);  // optional 
                        
let authenticator = new aliceonboarding.SandboxAuthenticator(sandboxToken = sandboxToken, userInfo = userInfo);

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
.catch(err => {
  // Inform the user about Authentication Errors
});
```

## Demo :rocket:

Check our JSFiddle demo [here](https://jsfiddle.net/alicebiometrics/nskyhfou/embedded/). Remember you must add your `SANDBOX_TOKEN` credentials. If you have already integrated ALiCE Onboarding in your backend, remove the `aliceonboarding.logInWithSandbox` function and use your retrieved `USER_TOKEN`. 

## Customisation :gear:

##### Localization

```js
let userToken = "<ADD-YOUR-USER-TOKEN-HERE>"
let language = "en" (default)

let config = new aliceonboarding.OnboardingConfig()
  .withUserToken(userToken)
  .withAddSelfieStage()
  .withAddDocumentStage(onboarding.DocumentType.IDCARD, "ESP")
  .withAddDocumentStage(onboarding.DocumentType.DRIVERLICENSE, "ESP")
  .withCustomLocalization(language)
```

##### Appearance

Modify the CSS styles:

```html
<link rel='stylesheet' href='dist/style.css>
```


## Documentation :page_facing_up:

For more information about ALiCE Onboarding:  https://docs.alicebiometrics.com/onboarding/

## Contact :mailbox_with_mail:

support@alicebiometrics.com
