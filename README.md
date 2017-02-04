# :evergreen_tree: Timber Log Event JSON Schema

This is a formal definition of the [Timber](https://timber.io) log event JSON schema. It follows
the [JSON Schema](http://json-schema.org/) specification.

The Timber log event schema is a shared, normalized schema that log events, across all platforms,
can adhere to. It's goal is to normalize log events across *all* platforms into a predictable
consistent schema that down stream consumers can rely on. This opens a whole world of possibilities.
Here's an example payload:

```javascript
{
  "dt": "2016-12-01T02:23:12.236543Z",
  "level": "info",
  "message": "Completed 200 OK in 117ms (Views: 85.2ms | ActiveRecord: 25.3ms)",
  "context": {
    "http": {
      "method": "GET",
      "path": "/checkout",
      "remote_addr": "123.456.789.10",
      "request_id": "abcd1234"
    },
    "user": {  // <---- http://i.giphy.com/EldfH1VJdbrwY.gif
      "id": 2,
      "name": "Ben Johnson",
      "email": "ben@johnson.com"
    }
  },
  "event": {
    "http_response": {
      "status": 200,
      "time_ms": 117
    }
  }
}
```

## Releases

Timber follows the [semver](http://semver.org/) specification for versioning. Releases can
be found in the [releases](https://github.com/timberio/log-event-json-schema/releases) sections.
You can also watch this repo to be notified of any upcoming changes.

## Backwards compatibility

The Timber API will respect legacy versions. Simply sepcify the version in the `$schema` attribute
and we'll handle the rest.

## Clients

It's rare that anyone will have to create this payload theirselves. Please checkout out our
[language specific libraries](https://github.com/timberio) as they handle capturing and structuring
log events from within your application.

## Contributing

Contributions are welcome, please submit a pull request.