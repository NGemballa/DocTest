# Support livechat in a webview

The support integration in a webview happens by opening the `WebView` with a preconfigured url.
This will automatically create a support conversation when loaded.

{{docs-snippet name='support/integration/url.js' title='Livechat URL structure' language='js'}}

To configure the instance, you can provide preconditions, language and other options as query parameters.
The complex values have to be stringified and url encoded.
For example to pass the preconditions `{"foo": "bar"}`, append a query parameter `preconditions=%7B%22foo%22%3A%22bar%22%7D`. 

The supported query parameters are `preconditions`, `language` and `options`

{{docs-snippet name='support/integration/building-url.js' title='Example code to build a embeddable chat url in JavaScript' language='js'}}

If the livechat is embedded via url, we emit the following events using the [postMessage](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage) API.
Messages are serialized as JSON objects.

{{#docs-demo as |demo|}}
  {{demo.snippet name='support/integration/message-event-data.ts' language='ts' label="Message"}}
  {{demo.snippet name='support/integration/ReadyToSupportData.ts' language='ts' label="ReadyToSupport"}}
  {{demo.snippet name='support/integration/CapturedLinkData.ts' language='ts' label="CapturedLink"}}
  {{demo.snippet name='support/integration/RoomClosedData.ts' language='ts' label="RoomClosed"}}
  {{demo.snippet name='support/integration/SupportRequestRejectedData.ts' language='ts' label="SupportRequestRejected"}}
  {{demo.snippet name='support/integration/RequestTokenData.ts' language='ts' label="RequestToken"}}
{{/docs-demo}}

We emit the following types via [postMessage](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage).

* `ReadyToSupport`, if the chat is initialized far enough to accept support input from the user
* `CapturedLink`, if the `captureLinks` option is enabled with `{url: string}` as event payload
* `RoomClosed`, if the current support room is closed 
* `SupportRequestRejected`, if the current support request got rejected
* `RequestTokenData`, if the provided token became invalid

Due to missing bridge between JavaScript and Android/iOS, we have to write some custom code to listen for emitted events. (See "Android Setup" and "iOS Setup")

## Android Setup

In Android, you have to define a JavaScript interface to inject into the [WebView](https://developer.android.com/reference/android/webkit/WebView).

{{#docs-demo as |demo|}}
  {{demo.snippet name='support/integration/ipc-android.kt' language='kotlin' label="Kotlin"}}
  {{demo.snippet name='support/integration/ipc-android.java' language='java' label="Java"}}
{{/docs-demo}}

Using the [addJavascriptInterface](https://developer.android.com/reference/android/webkit/WebView#addJavascriptInterface(java.lang.Object,%20java.lang.String)), register your `Interface` as `SMOOPE_IPC`.
This allows us to call your Interface, if defined.

{{#docs-demo as |demo|}}
  {{demo.snippet name='support/integration/webview-setup-android.kt' language='kotlin' label="Kotlin"}}
  {{demo.snippet name='support/integration/webview-setup-android.java' language='java' label="Java"}}
{{/docs-demo}}

<aside>

**File upload**

If you want to allow the user to select a file, you have to make some adjustments.
First [set](https://developer.android.com/reference/android/webkit/WebView.html#setWebChromeClient(android.webkit.WebChromeClient)) a [`WebChromeClient`](https://developer.android.com/reference/android/webkit/WebChromeClient) on the `WebView` instance.

Next you can implement the [`onShowFileChooser`](https://developer.android.com/reference/android/webkit/WebChromeClient#onShowFileChooser(android.webkit.WebView,%2520android.webkit.ValueCallback%3Candroid.net.Uri%5B%5D%3E,%2520android.webkit.WebChromeClient.FileChooserParams)) to fit your use-case.

</aside>

## iOS Setup

Similar to Android, in iOS, you have to register a listener for a custom `messageHandler` method on a [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview).
Use the `SMOOPE_IPC` name to add to the `WKUserContentController`.

Setup the WKWebView to receive livechat events:

{{#docs-demo as |demo|}}
  {{demo.snippet name='support/integration/webview-setup-ios.swift' language='swift' label="Swift"}}
  {{demo.snippet name='support/integration/webview-setup-ios.m' language='obj-c' label="Objective-C"}}
{{/docs-demo}}

Listen for messages with the `SMOOPE_IPC` name:

{{#docs-demo as |demo|}}
  {{demo.snippet name='support/integration/ipc-ios.swift' language='swift' label="Swift"}}
  {{demo.snippet name='support/integration/ipc-ios.m' language='obj-c' label="Objective-C"}}
{{/docs-demo}}

### Opening external links

If you want to allow the livechat to open external links, you have to make additional adjustments.
The following snippets allows the webview to open external links by intercepting navigation actions.

{{#docs-demo as |demo|}}
  {{demo.snippet name='support/integration/external-links-ios.swift' language='swift' label="Swift"}}
  {{demo.snippet name='support/integration/external-links-ios.m' language='obj-c' label="Objective-C"}}
{{/docs-demo}}

## Authenticating a user

In cases where you have an authenticated user and want to pass its identity to our system, you can do that by configuring the `token` query parameter.
Simply append the parameter to your existing WebView url and load the chat.

The livechat will use that token to create request against our api.
If the provided token expires, the chat emits a `RequestToken` event which allows you to refresh the token.

To do that, post a web message, using the `RequestTokenResponse` event data:

{{#docs-demo as |demo|}}
  {{demo.snippet name='support/integration/token-response.kt' language='kotlin' label="Kotlin"}}
  {{demo.snippet name='support/integration/token-response.java' language='java' label="Java"}}
  {{demo.snippet name='support/integration/RequestTokenResponseData.ts' language='ts' label="RequestTokenResponse type"}}
{{/docs-demo}}

### Posting a message from iOS

Because iOS exposes no way to communicate from iOS context to the [WKWebview](https://developer.apple.com/documentation/webkit/wkwebview) context, you have to make additional adjustments.

If the app is launched inside a WKWebview, we register a global `wkSmoopePostMessage` method. 
This allows you to use the [evaluateJavaScript](https://developer.apple.com/documentation/webkit/wkwebview/1415017-evaluatejavascript) method, to emit data to the chat.

The method accepts a JSON-Object which contains the message information.

{{docs-snippet name='support/integration/token-response.swift' title='Responding to a token request with a new token' language='swift'}}
