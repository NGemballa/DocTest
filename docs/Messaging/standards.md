# Standards

Aside from all types linked in the resources view, we have some conventions for other attribute types:

name | type | validation | description
--------- | ------- | ------- | -------
`OffsetDateTime` | String | | [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) formatted string
`id` | String | *notBlank* *Size(0..26)* | variable length string

## Format examples

```shell
# OffsetDateTime
"2018-02-27T08:15:12.333Z"

# id
"01ARZ3NDEKTSV4RRFFQ69G5FAV"
```

## Resource idempotency

When clients create resources and they want idempotence for creating them. They should send the a header with the name `Smoope-Idempotency-Key` and a value that identifies this resource for the client. This value will be returned in a meta dictionary on the returning resource object under the key `idempotency-key`.

Current supported idempotence requests: 

* events

__Request__:
```sh
curl -X POST \
  https://smck-api.smoope.it/events \
  -H 'Authorization: {{auth}}' \
  -H 'Content-Type: application/vnd.api+json' \
  -d '{
    "data": {
        "type": "events",
        "attributes": {
            …
        },
        "relationships": {
            …
        },
        "meta": {
            "idempotency-key": "{{idempotency-key}}"
        }
    }
}
```

__Response__:

```json
{
    "data": {
        "id": "{{event-id}}",
        "type": "events",
        "attributes": {
            …
        },
        …
        "meta": {
            "idempotency-key": "{{idempotency-key}}"
        }
    }
}
```

{{outlet}}
