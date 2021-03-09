# Support livechat in a website

The support integration in a regular website happens by including a script from our server.

## Bootstrapping the livechat

The livechat can be initialized via JavaScript.
This is the preferred way if you're planning to use smoope as a support chat on your website.

To add a livechat to a regular HTML page, you have to first load the embeddable JavaScript file.

{{#docs-snippet name='your-snippet-name.hbs' title="Loading the embed script"}}
<script async defer src="https://<tenantId>-chat.smoope.net/embed.js"></script>
{{/docs-snippet}}

As the snippet uses `async defer` to prevent blocking your own JavaScript, 
you have to wait for the window to be loaded before initializing the livechat.

To do this, copy the following snippet, which starts the chat in the most straightforward way.

{{docs-snippet name='support/integration/embed-snippet.html' title='Example on how to setup the chat' language='html'}}

Once the chat is inserted, smoope will automatically show a button to let the user request support if possible.

<aside>

**Chat visibility**

Depending on your livechat configuration, the button will only appear in some cases, e.g. if a matching operator is available.

</aside>

## Options

The chat initialization has the following constructor signature:

{{#docs-demo as |demo|}}
  {{demo.snippet name='support/integration/chat-interface.ts' language='ts' label="Chat constructor"}}
  {{demo.snippet name='support/integration/chat-options.ts' language='ts' label="LiveChatOptions"}}
{{/docs-demo}}

This means to construct the chat you have to pass in:

* `parent: HTMLElement` - the element that the chat appends itself to
* `options: LiveChatOptions` - (optional) the options that configure the chat, as mentioned above

The instance can be configured by setting the `options` object. 
You can adjust the following fields:

* `livechat: string` - the livechat id
* `captureLinks: boolean` - (optional) a click on any link in the chat history will prevent transition and emit a `CapturedLink` event
* `preconditions: object` - (optional) an object that is used to improve operator routing. If you don't use any special routing logic, you shouldn't have to set it 
* `language: string` - (optional) a hint that influences the language, the system is using, for the support user. 


## Listening for events

The returned `chat` instance acts as an event emitter and emits various informations to your website.
The following events are observable from your website:

* `captureLinks`, if the `captureLinks` option is enabled with `{url: string}` as event payload

{{#docs-snippet name='listen-events.js' title='Example on how to capture link clicks' language='js'}}
var chat = new smoopeChat.default(document.querySelector('#chat'), {
  // ... other options
  captureLinks: true
});
chat.on("capturedLink", function(data) {
  alert(`user clicked a link: ${data.url}`);
});
{{/docs-snippet}}
