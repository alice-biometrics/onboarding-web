# onboarding-web :earth_africa: [![doc](https://img.shields.io/badge/doc-onboarding-51CB56)](https://docs.alicebiometrics.com/onboarding/) [![doc-web](https://img.shields.io/badge/doc-web-51CB56)](https://docs.alicebiometrics.com/onboarding/sdk/web/)

ALiCE Onboarding Web component allows the automatic capture of documents and video selfie of the user in real time from the camera of your device. It also simplifies the communication with the onboarding API to facilitate rapid integration and development. It manages the onboarding flow configuration: requested documents and order.

The main features are:

- Automatic capture of documents and video selfie of the user in real time from the camera of your device.
- Communication with the onboarding API to facilitate rapid integration and development.
- Manage the onboarding flow configuration: requested documents and order.


## Installation :computer:

We provide a npm module (`onboarding-react`) with a web component. 

### How do I get the package?

* Contact us through the following mail: support@alicebiometrics.com 📬
* Through public registry (coming soon 🚧)

### Import the library

Include ALiCE Onboarding as a regular script tag on your page:

```html
<script src='dist/aliceonboarding.umd.min.js'></script>
```

And the CSS styles:

```html
<link rel='stylesheet' href='dist/style.css>
```

## Usage :wave:

### Reserve a HTML element in your application

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
  .withAddDocumentStage(onboarding.DocumentType.IDCARD, "ESP")
  .withAddDocumentStage(onboarding.DocumentType.DRIVERLICENSE, "ESP")
```

### Using ALiCE Onboarding on Production

Add our Web component in your application adding:

```js
function onSuccess(userInfo) {console.log("onSuccess: " + userInfo)}
function onFailure(error) {console.log("onFailure: " + error)}
function onCancel() { console.log("onCancel")}

new aliceonboarding.Onboarding("#alice-onboarding-mount", config).run(onSuccess, onFailure, onCancel);
```

Where `userToken` is used to secure requests made by the users on their mobile devices or web clients. You should obtain it from your Backend.

TODO 🚧
see an example [here](app/components/OnboardingProduction/index.js)

### Using ALiCE Onboarding on Trial

Add our Web component in your application adding:

```js
sandboxToken = "<ADD-YOUR-SANDBOX-TOKEN-HERE>"
userInfo = new onboarding.UserInfo(email)

aliceonboarding.logInWithSandbox(sandboxToken, userInfo)
  .then(userToken => {
    // Obtain previously configured OnboardingConfig object
    let config = getOnboardingConfig(userToken, appSettings);
    
    function onSuccess(userInfo) {console.log("onSuccess: " + userInfo)}
    function onFailure(error) {console.log("onFailure: " + error)}
    function onCancel() { console.log("onCancel")}
        
    new aliceonboarding.Onboarding("#alice-onboarding-mount", config).run(onSuccess, onFailure, onCancel);
  })
  .catch(err => {
    console.error(err);
  });
```

Where `sandboxToken` is a temporal token for testing the technology in a development/testing environment. 

An `email` is required to associate it to an ALiCE `user_id`.

* TODO 🚧
see an example [here](app/components/OnboardingTrial/index.js)


## Documentation :page_facing_up:

For more information about ALiCE Onboarding:  https://docs.alicebiometrics.com/onboarding/

## Contact :mailbox_with_mail:

support@alicebiometrics.com
