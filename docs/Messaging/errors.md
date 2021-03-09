# Error codes

In some cases, our API uses the [error objects](https://jsonapi.org/format/#error-objects) `code` field to communicate what caused the request to error.

{{#docs-snippet name='example-error.json' title="Example error response containing an error code"}}
{
  "errors": [
    { "code": "1" }
  ]
}
{{/docs-snippet}}

<table>
  <thead>
    <tr>
      <th>Code</th>
      <th>Name</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    {{#each model as |error|}}
      <tr>
        <td>{{error.code}}</td>
        <td><span class="var-type">{{error.name}}</span></td>
        <td>{{error.comment}}</td>
      </tr>
    {{/each}}
  </tbody>
</table>
