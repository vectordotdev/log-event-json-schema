# :evergreen_tree: Timber Log Event JSON Schema

This repository contains the official JSON schema definitions for the Timber library log events:

1. [Timber for Elixir](https://github.com/timberio/timber-elixir)
2. [Timber for Go](https://github.com/timberio/timber-go)
3. [Timber for Node](https://github.com/timberio/timber-node)
4. [Timber for Python](https://github.com/timberio/timber-python)
5. [Timber for Ruby](https://github.com/timberio/timber-ruby)

The Timber libraries provide a better default log policy for the languages they serve. As a result,
they automatically structure logs into official events. Structuring events without a versioned
definition partially defeats the purpose. This JSON schema allows downstream consumers to
understand and rely on the structure of the data. It creates a contract and provides for a much
more stable and pleasant environment for any consumer of this data.


## Examples

Note, that the Timber libraries above handle automatically capturing these events within
your application. Below are example event JSON strucutures:

<details><summary><strong>1. Exception Event</strong></summary><p>

An event that represents an exception from within your application:

```javascript
{
  "dt": "2016-12-01T02:23:12.236543Z", // Consistent dates with nanosecond precision
  "level": "error", // Log levels in your logs!
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
```

</p></details>

<details><summary><strong>2. HTTP Server Request Event</strong></summary><p>

Am event that represents an incoming HTTP request to your application:

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
```

</p></details>

<details><summary><strong>3. HTTP Server Response Event</strong></summary><p>

An event that represents an outgoing HTTP response from your application:

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
```

</p></details>

<details><summary><strong>4. SQL Query</strong></summary><p>

An event that represents a SQL query:

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
    "sql_query": { // Event type
      "sql": "SELECT * FROM users WHERE id = 1",
      "time_ms": 54
    }
  }
}
```

</p></details>

<details><summary><strong>5. Serverless Platform Function Invocation</strong></summary><p>

An event that represents a function invocation on serverless platforms like AWS Lambda or Google
Cloud Functions:

```javascript
{
  "dt": "2016-12-01T02:23:12.236543Z", // Consistent dates with nanosecond precision
  "level": "info", // Log levels in your logs!
  "message": "REPORT RequestId: 86792069-eb43-11e6-af8c-d9dfd5859e88  Duration: 236.83 ms Billed Duration: 300 ms Memory Size: 256 MB Max Memory Used: 118 MB", // Human readable message preserved
  "event": { // Structured data for the event being logged
    "function_invocation": { // Event type
      "request_id": "86792069-eb43-11e6-af8c-d9dfd5859e88",
      "time_ms": 236.83,
      "billed_duration_ms": 300,
      "memory_size_mb": 256,
      "memory_used_mb": 118
    }
  }
}
```
</p></details>

<details><summary><strong>6. Platform Error</strong></summary><p>

An event that represents an error application platforms, such as Heroku or ElasticBeanstalk:

```javascript
{
  "dt": "2016-12-01T02:23:12.236543Z", // Consistent dates with nanosecond precision
  "level": "error", // Log levels in your logs!
  "message": "at=error code=H99 desc="Platform error" method=GET path="/" host=myapp.herokuapp.com fwd=17.17.17.17 dyno= connect= service= status=503 bytes=", // Human readable message preserved
  "context": {
    "http": {
      "method": "GET",
      "path": "/",
      "host": "myapp.herokuapp.com",
      "remove_addr": "123.34.22.34",
      "request_id": "x1235"
    }
  },
  "event": { // Structured data for the event being logged
    "platform_error": { // Event type
      "code": "H99",
      "message": "Platform error",
      "billed_duration_ms": 300,
      "memory_size_mb": 256,
      "memory_used_mb": 118,
      "http_status": 503
    }
  }
}
```

</p></details>

<strong>7. ...and many more, checkout the schema for a complete list.</strong>


## Versioning & Releases

Timber follows the [semver](http://semver.org/) specification for versioning. Releases can
be found in the [releases](https://github.com/timberio/log-event-json-schema/releases) sections.
You can also watch this repo to be notified of any upcoming changes.


## Validation

Data can be validated against the schema using any of [these open source validators](http://json-schema.org/implementations.html).


## Contributing

Contributions are welcome, please submit a pull request.