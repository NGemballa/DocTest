# Introduction

Smoope provides a public http api following the [JSON:API](https://jsonapi.org/) specification.

Our api resources use some standard formats for some of its attributes. You can find an overview of these in our {{#link-to 'docs.standards'}}Standards{{/link-to}} page.  

JSON:API allows us to [define custom error codes](https://jsonapi.org/format/#errors) for specific failures.
Smoope specific errors are listed in the {{#link-to 'docs.errors'}}list of error codes{{/link-to}}.

## Important resources

Due to smoope being a messaging service, we have some core resources:

* {{#link-to 'docs.resources.detail' 'User'}}User{{/link-to}} - a smoope user
* {{#link-to 'docs.resources.detail' 'Room'}}Room{{/link-to}} - contains many events and is usually used to allow users to communicate with each other 
* {{#link-to 'docs.resources.detail' 'Event'}}Event{{/link-to}} - represent things that happen in smoope. For example a text message or some updates like a user changing its name

You can find additional resources in the resource list in the navigation.
