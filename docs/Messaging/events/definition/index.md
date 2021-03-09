# Definition

[Event resources](/docs/resources/resource/Event) behave differently depending on their event type.
The following table gives an overview of which event type implies what functionality.

Click on the type to get a detailed view on the events content.

## Public event types

{{event-def-table definitions=model}}

## Scope

The scope of an event is not part of an Event's attribute set, but part of its definition.
The scope specifies where the event is defined to occur. Currently the following scopes are defined:

* **Room** - Event can only be observed in a room. 
* **Global** - Event can only be observed globally, i.e. without a reference to a room. 

## Deletability

Specifies if an event can be deleted.

## Ephemeral event

An ephemeral event cannot be retrieved via Events resource. It can only be observed by sync.
The ephemerality of an event is not part of its attribute set, but part of its definition.

## Annotatable

Specifies if an event can be annotated.
