# Webhooks

## Outgoing webhooks

This feature allows customer's 3rd-party systems to be notified about events happened in smoope via HTTP protocol.

### Handling requests

Each time new event registed in smoope it will be send out via HTTP POST request with `Content-Type: application/json` header and predefined payload to remote url.

### Payload

| Attribute | Required | Description |
|--- |--- |--- |
| `type` | * | Original event's [type](#supported-events) |
| `content` | * | Original event's content |
| `version` | * | Version, currently always set to 2 |
| `roomId` | | Room's identifier event was posted in |
| `senderId` | | Sender's identifier |
| `senderExternalId` | | Sender's external identifier |
| `recipients` | | Collection of [recipients](#recipient) |
| `referenceId` | | Refererenced event (in case of original event references another) |
| `livechatId` | | Livechat's identifier used in conversation |
| `preconditions` | | Livechat's preconditions used while creating a conversation |

### Recipient
| Attribute | Required | Description |
|--- |--- |--- |
| `id` | * | Recipient's identifier  |
| `externalId` |  | Recipient's external identifier |

### Supported events

| Type | Description |
|--- |--- |
| `s.room.create` | Room was created |
| `s.room.membership` | User has joined or left a room |
| `s.message.text` | User has created a text message |
| `s.message.media` | User has created a media message |
| `s.message.edit` | User has edited a message |
| `s.message.delete` | User has removed a message |
| `s.room.close` | Room was closed |

### Signature and its verification

Signature is a string formatted in the way of `{timestamp}:{content}` and signed with predefined signing key using [HMAC](https://en.wikipedia.org/wiki/HMAC) algorithm and SHA-512 as underlying hash function.
In string above `content` represents an [outgoing payload](#payload).

Both signature and timestamp values are served via HTTP headers:

`smoope-signature` - unique signature allows to verify incoming request at 3rd-party system

`smoope-timestamp` - [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) formatted string, represents when request was performed

Signing process is performed on server each time it attempts to request remote url.
While receiving requests from smoope, 3rd-party system is responsible to verify incoming payloads by performing same signing process at its side and
comparing result with original signature sent within the request. Same signature means request was not intercepted or changed by other software or persons.

### Example

The following snippet represents a [curl](https://curl.haxx.se/) request which will be sent in case of `s.message.text` and signed with `hmac` key:

{{#docs-snippet name='outgoing-hook.sh' title='Simulating a webhook with curl' language='sh'}}
curl -X POST  \
  -H "Content-Type:application/json;charset=UTF-8"  \
  -H "smoope-timestamp:2019-05-15T12:58:34.758710Z"  \
  -H "smoope-signature:wT50Do1_-KU1Ga8u6nFuFl_ZPzlXbmg1bLukr5zGdTf5P0E51VHvuq71NUXVe-m6P9HgW58WL8_T2F5BX-wZPg"  \
  -d '{"senderId":"sender","type":"s.message.text","version":1,"content":{"body":"ho-ho"},"roomId":"room","livechatId":"livechat"}' \ 
  "http://some.org/webhook"
{{/docs-snippet}}


