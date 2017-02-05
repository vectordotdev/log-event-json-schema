# :evergreen_tree: Timber Log Event JSON Schema

[Schema.json](schema.json) is the formal definition of the [Timber](https://timber.io) log event
JSON schema. It adheres to the [JSON Schema specification](http://json-schema.org/).

The Timber log event schema is a shared, normalized schema that log events can adhere to.
It's purpose is to normalize log events across *all* platforms into a predictable
consistent schema that down stream consumers can rely on.

# Examples

<details><summary><strong>1. Exception</strong></summary><p>

```javascript
{
  "dt": "2016-12-01T02:23:12.236543Z", // Consistent dates with nanosecond precision
  "level": "info", // Log levels in your logs!
  "message": "(RuntimeError) MissingClass is undefined", // Human readable message preserved
  "context": { // Context is shared across all relevant logs and acts as join data
    "http": {
      "method": "GET",
      "path": "/checkout",
      "remote_addr": "123.456.789.10",
      "request_id": "abcd1234" // Trace your requests!
    },
    "user": { // Associate users with your log events!
      "id": 2,
      "name": "Ben Johnson",
      "email": "ben@johnson.com"
    }
  },
  "event": { // Structured data for the event being logged
    "server_side_app": { // Top level "domain" for events
      "exception": { // Event type
        "name": "RuntimeError",
        "message": "MissingClass is undefined",
        "backtrace": [
          {
            "file": "/path/to/file",
            "function": "myFunc",
            "line": 45
          },
          {
            "file": "/path/to/file",
            "function": "myFunc",
            "line": 45
          },
          {
            "file": "/path/to/file",
            "function": "myFunc",
            "line": 45
          },
          {
            "file": "/path/to/file",
            "function": "myFunc",
            "line": 45
          },
          {
            "file": "/path/to/file",
            "function": "myFunc",
            "line": 45
          }
        ]
      }
    }
  }
}
```

<details><summary><strong>2. Incoming HTTP Server Request</strong></summary><p>

```javascript
{
  "dt": "2016-12-01T02:23:12.236543Z", // Consistent dates with nanosecond precision
  "level": "info", // Log levels in your logs!
  "message": "POST /checkout for 192.321.22.21", // Human readable message preserved
  "context": { // Context is shared across all relevant logs and acts as join data
    "http": {
      "method": "GET",
      "path": "/checkout",
      "remote_addr": "123.456.789.10",
      "request_id": "abcd1234" // Trace your requests!
    },
    "user": { // Associate users with your log events!
      "id": 2,
      "name": "Ben Johnson",
      "email": "ben@johnson.com"
    }
  },
  "event": { // Structured data for the event being logged
    "server_side_app": { // Top level "domain" for events
      "http_server_request": { // Event type
        "method": "GET",
        "scheme": "https",
        "host": "timber.io",
        "path": "/checkout",
        "port": 443,
        "headers": {
          "content_length": 894,
          "content_type": "application/json", // <- Example of data that wasn't in the log line itself
          "remove_addr": "192.321.22.21",
          "request_id": "gy23fbty523",
          "user_agent": "Mozilla/3.0 (Win95; U)"
        }
      }
    }
  }
}
```

</p></details>

<details><summary><strong>3. Outgoing HTTP Server Response</strong></summary><p>

```javascript
{
  "dt": "2016-12-01T02:23:12.236543Z", // Consistent dates with nanosecond precision
  "level": "info", // Log levels in your logs!
  "message": "Sent 200 OK in 117ms", // Human readable message preserved
  "context": { // Context is shared across all relevant logs and acts as join data
    "http": {
      "method": "GET",
      "path": "/checkout",
      "remote_addr": "123.456.789.10",
      "request_id": "abcd1234" // Trace your requests!
    },
    "user": { // Associate users with your log events!
      "id": 2,
      "name": "Ben Johnson",
      "email": "ben@johnson.com"
    }
  },
  "event": { // Structured data for the event being logged
    "server_side_app": { // Top level "domain" for events
      "http_server_response": { // Event type
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
}
```

</p></details>

<details><summary><strong>4. SQL Query</strong></summary><p>

```javascript
{
  "dt": "2016-12-01T02:23:12.236543Z", // Consistent dates with nanosecond precision
  "level": "info", // Log levels in your logs!
  "message": "SELECT * FROM users WHERE id = 1 (54ms)", // Human readable message preserved
  "context": { // Context is shared across all relevant logs and acts as join data
    "http": {
      "method": "GET",
      "path": "/checkout",
      "remote_addr": "123.456.789.10",
      "request_id": "abcd1234" // Trace your requests!
    },
    "user": { // Associate users with your log events!
      "id": 2,
      "name": "Ben Johnson",
      "email": "ben@johnson.com"
    }
  },
  "event": { // Structured data for the event being logged
    "server_side_app": { // Top level "domain" for events
      "sql_query": { // Event type
        "sql": "SELECT * FROM users WHERE id = 1",
        "time_ms": 54
      }
    }
  }
}
```

</p></details>

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