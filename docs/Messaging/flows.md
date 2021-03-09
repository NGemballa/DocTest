# Flows

## Check if support is possible

At a high level, you have to use your `<livechat-id>` to create a support slot and wait until an operator is available.

### Create a support slot

A support slot is like a ticket that "registers" the user for proceeding to a support chat.
This means until the support-slot isn't telling you that we have a potential match, it makes no sense to prompt the user an option to start a chat.

If we compute a potential support operator match, then the slot will notify you that you can proceed.

First, create a support-slot (see [Creating Resource](https://jsonapi.org/format/#crud-creating) for more details):

{{#docs-snippet name='support-slot-request.json' title='Sample request for creating a support-slot'}}
// POST ${api}/support-slots

// accept: application/vnd.api+json
// content-type: application/vnd.api+json

{
  "data": {
    "relationships": {
      "livechat": {
        "data": { "type": "livechats", "id": "01C7DYBTZWQ2EE1BKQD6361EW7" }
      }
    },
    "type": "support-slots"
  }
}
{{/docs-snippet}}

The server will respond with the newly created support slot:

{{#docs-snippet name='support-slot-response.json' title='Sample response for the created support-slot'}}
{
  "data" : {
    "id" : "${support-slot-id}",
    "type" : "support-slots",
    "attributes" : {
      "preconditions" : { },
      "status-reason" : "NO_OPERATOR_AVAILABLE",
      "status" : "UNAVAILABLE"
    },
    "links" : {
      "self" : "${support-slot-self-url}"
    }
  }
}
{{/docs-snippet}}

Now you can observe the `status` attribute to determine if a potential support operator is available to handle a new support chat.

1. if `status` is:
  * `"UNAVAILABLE"` then wait a short time and follow the support-slot self link (1.)
  * `"AVAILABLE"` then trigger the code to let your user start a support chat
