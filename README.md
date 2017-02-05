# :evergreen_tree: Timber Log Event JSON Schema

[Schema.json](schema.json) is the formal definition of the [Timber](https://timber.io) log event
JSON schema. It adheres to the [JSON Schema specification](http://json-schema.org/).

The Timber log event schema is a shared, normalized schema that log events can adhere to.
It's purpose is to normalize log events across *all* platforms into a predictable
consistent schema that down stream consumers can rely on.

## HTTP Response Example JSON payload

```javascript
{
  "dt": "2016-12-01T02:23:12.236543Z", // <-------- Consistent dates with nanosecond precision
  "level": "info", // <---------------------------- Log levels in your logs!
  "message": "Sent 200 OK in 117ms", // <---------- Human readable message preserved
  "context": { // <-------------------------------- Context is shared across all relevant logs and acts as join data
    "http": {
      "method": "GET",
      "path": "/checkout",
      "remote_addr": "123.456.789.10",
      "request_id": "abcd1234" // <---------------- Trace your requests!
    },
    "user": { // <--------------------------------- Associate users with your log events!
      "id": 2,
      "name": "Ben Johnson",
      "email": "ben@johnson.com"
    }
  },
  "event": { // <---------------------------------- Structured data for the event being logged
    "server_side_app": { // <---------------------- Top level "domain" for events
      "http_response": { // <---------------------- Event type
        "status": 200,
        "time_ms": 117,
        "headers": {
          "content_length": 894,
          "content_type": "application/json", // <- Example of data that wasn't in the log line itself
          "request_id": "gy23fbty523"
        }
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