# Support Livechat

The livechat is the component that allows customers to communicate with support operators.
Before embedding the livechat, make sure that you receive your `livechat-id` and `tenant-id` from our support.

We provide two ways to embed a livechat.
Both accept configuration options, language configuration and preconditions. 

## Options

A livechat instance has various `options` to control its behavior:

* `captureLinks: boolean` - a click on any link in the chat history will prevent transition and emit a `CapturedLink` event 

## Programmatic livechat creation

The livechat can be initialized via JavaScript.
This is the preferred way if you're planning to use smoope as a support chat on your website.

> To add a livechat to a regular HTML page, you have to first load the embeddable JavaScript file.

```html
<script async defer src="https://<tenantId>-chat.smoope.net/embed.js"></script>
```

> As the snippet uses `async defer` to prevent blocking your own JavaScript, 
you have to wait for the window to be loaded before initializing the livechat.

```html
<script type="text/javascript">
window.addEventListener('load', function () {
	// Initialize a livechat instance (Note: this doesn't start/insert it into DOM).
	var chat = new smoopeChat.default(document.querySelector('#chat'), {
    livechat: '<livechat-id>',
    language: 'fr',
    preconditions: {
      language: 'fr',
      zip: '1845'
    },
	});

	// Insert livechat instance into DOM.
	chat.insert();
});
</script>
```

Once the chat is inserted, smoope will automatically show a button to let the user request support if possible.
The returned `chat` instance acts as an event emitter.

Exposed events:

* `captureLinks`, if the `captureLinks` option is enabled with `{url: string}` as event payload

> Example on how to capture link clicks

```js
var chat = new smoopeChat.default(document.querySelector('#chat'), {
  // ... other options
  captureLinks: true
});
chat.on("capturedLink", function(data) {
  alert(`user clicked a link: ${data.url}`);
});
```

## Automatic livechat creation via URL

If you can't execute JavaScript in your App, you can easily embed the livechat by using a Webview.

```js
`http://${tenantId}-chat.smoope.net/${livechatId}`
```

To configure the instance, you can provide preconditions, language and other options as query parameters.
The complex values have to be stringified and url encoded.
For example to pass the preconditions `{"foo": "bar"}`, append a query parameter `preconditions=%7B%22foo%22%3A%22bar%22%7D`. 

The supported query parameters are `preconditions`, `language` and `options`

> Example code to build a embeddable chat url in JavaScript

```js
const language = 'fr';
const preconditions = encodeURIComponent(JSON.stringify({
  language,
  zip: '1845'
}));
const options = encodeURIComponent(JSON.stringify({
  captureLinks: true
}));

const url = `http://${tenantId}-chat.smoope.net/${livechatId}` + 
  `?language=${language}` + 
  `&preconditions=${preconditions}` +
  `&options=${options}`;
```

If the livechat is embedded via url, we emit the following events using the [postMessage](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage) API.
Messages are serialized as JSON objects.

Due to missing bridge between JavaScript and Android/iOS, we have to write some custom code to listen for emitted events. (See "Handling events in Android" and "Handling events in iOS")

> MessageEvent data type

```ts
type MessageEventData = {
  type: string, 
  data: Object, 
  _by: 'smoope',
};

// Example MessageEvent 
// {"type":"ReadyToSupport","data":{"room":"01CXWBKT6Q6YCV1X12FNSJ0QDR"},"_by":"smoope"}
``` 

We emit the following types via [postMessage](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage).

* `CapturedLink`, if the `captureLinks` option is enabled with `{url: string}` as event payload
* `ReadyToSupport`, if the chat is initialized far enough to accept support input from the user
* `RoomClosed`, if the current support room is closed 
* `SupportRequestRejected`, if the current support request got rejected

## Listening for events in Android

In Android, you have to define a JavaScript interface to inject into the [WebView](https://developer.android.com/reference/android/webkit/WebView).

Using the [addJavascriptInterface](https://developer.android.com/reference/android/webkit/WebView#addJavascriptInterface(java.lang.Object,%20java.lang.String)), register your `Interface` as `SMOOPE_IPC`.
This allows us to call your Interface, if defined.

> Custom JavaScript interface to add to the webview

```java
class dummy.app.snippets.support.integration.SmoopeIpc {
  @JavascriptInterface
  public void postMessage(String data, String origin) {
    // handle livechat event
    System.out.println("Smoope chat emitted data: " + data + " " + origin);
  }
}
```

> Enable JavaScript and register your interface as `SMOOPE_IPC`

```java
webView.getSettings().setJavaScriptEnabled(true);
webView.addJavascriptInterface(new dummy.app.snippets.support.integration.SmoopeIpc(), "SMOOPE_IPC");
webView.loadUrl("<webchat-url>");
```

## Listening for events in iOS

Similar to Android, in iOS, you have to register a listener for a custom `messageHandler` method on a [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview).
Use the `SMOOPE_IPC` name to add to the `WKUserContentController`.

> Setup the WKWebView to receive livechat events

```swift
let config = WKWebViewConfiguration()
config.preferences.javaScriptEnabled = true
let userContentController = WKUserContentController()
userContentController.add(self, name: "SMOOPE_IPC")
config.userContentController = userContentController

webView.load(URLRequest(url: URL(string: "<webchat-url>")!))
```

> Listen for messages with the `SMOOPE_IPC` name

```swift
extension ViewController: WKScriptMessageHandler {
  func userContentController(_ userContentController: WKUserContentController, didReceive message: WKScriptMessage) {
    if message.name == "SMOOPE_IPC", let messageBody = message.body as? String {
      // handle livechat event
      print(messageBody)
    }
  }
}
```
