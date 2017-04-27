# Simple Log Event JSON Schema

The purpose of this schema is to define a _simple_ structure for logging structured events.
This helps normalize log data across applications and teams, and it provides a contract for
downstream consumers of the data (graphs, alerts, etc). Defining a schema reduces unexpected issues,
improves data consumption reliability, and solves one of the major issues of working with log data.

This schema is used internally at [Timber](https://timber.io) and across thousands of companies
with success. It's the core reason the Timber logging platform is able to provide a great user
experience out of the box. It enables us to make assumptions about your data and work with
predictable structures. There's no reason any consumer of this data, even your own internal ones,
can't get the same benefit.


## Implementation

You are welcome to log your own structured events that adhere to this schema, or you can use
one of the Timber libraries that do it for you:

1. [Timber for Elixir](https://github.com/timberio/timber-elixir)
2. [Timber for Go](https://github.com/timberio/timber-go)
3. [Timber for Node](https://github.com/timberio/timber-node)
4. [Timber for Python](https://github.com/timberio/timber-python)
5. [Timber for Ruby](https://github.com/timberio/timber-ruby)


## The Schema

You can check out [schema.json](schema.json) for the full schema definition, but here's a
quick rundown:

```javascript
{
  "dt": "2016-12-01T02:23:12.236543Z",           // Consistent ISO8601 dates with nanosecond precision
  "level": "info",                               // The level of the log
  "message": "POST /checkout for 192.321.22.21", // Human readable message
  "context": { ... },                            // Context data shared across log line, think of it like join data for your logs
  "event": { ... }                               // Structured representation of this log event
}
```

For actual events, see below.


## Event Examples

<details><summary><strong>1. Exception Event</strong></summary><p>

A structured event that represents an exception from within your application:

```javascript
{
  "dt": "2016-12-01T02:23:12.236543Z",
  "level": "error",
  "message": "(RuntimeError) MissingClass is undefined",
  "context": {
    "http": {
      "method": "GET",
      "path": "/checkout",
      "remote_addr": "123.456.789.10",
      "request_id": "abcd1234" // <------------- View all logs within a requst!
    },
    "user": { // <------------------------------ Associate users with your log events!
      "id": 2,
      "name": "Ben Johnson",
      "email": "ben@johnson.com"
    }
  },
  "event": {
    "exception": {
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

Am event that represents an incoming HTTP request to your application's HTTP server:

```javascript
{
  "dt": "2016-12-01T02:23:12.236543Z",
  "level": "info",
  "message": "POST /checkout for 192.321.22.21",
  "context": {
    "http": {
      "method": "GET",
      "path": "/checkout",
      "remote_addr": "123.456.789.10",
      "request_id": "abcd1234" // <------------- View all logs within a requst!
    },
    "user": { // <------------------------------ Associate users with your log events!
      "id": 2,
      "name": "Ben Johnson",
      "email": "ben@johnson.com"
    }
  },
  "event": {
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
  "dt": "2016-12-01T02:23:12.236543Z",
  "level": "info",
  "message": "Sent 200 OK in 117ms",
  "context": {
    "http": {
      "method": "GET",
      "path": "/checkout",
      "remote_addr": "123.456.789.10",
      "request_id": "abcd1234" // <------------- View all logs within a requst!
    },
    "user": { // <------------------------------ Associate users with your log events!
      "id": 2,
      "name": "Ben Johnson",
      "email": "ben@johnson.com"
    }
  },
  "event": {
    "http_server_response": { // Event type
      "request_id": "gy23fbty523",
      "status": 200,
      "time_ms": 117,
      "headers": {
        "content_length": 894,
        "content_type": "application/json",
        "x_request_id": "gy23fbty523"
      }
    }
  }
}
```

</p></details>

<details><summary><strong>4. SQL Query Event</strong></summary><p>

An event that represents a SQL query:

```javascript
{
  "dt": "2016-12-01T02:23:12.236543Z",
  "level": "info",
  "message": "SELECT * FROM users WHERE id = 1 (54ms)",
  "context": {
    "http": {
      "method": "GET",
      "path": "/checkout",
      "remote_addr": "123.456.789.10",
      "request_id": "abcd1234" // <------------- View all logs within a requst!
    },
    "user": { // <------------------------------ Associate users with your log events!
      "id": 2,
      "name": "Ben Johnson",
      "email": "ben@johnson.com"
    }
  },
  "event": {
    "sql_query": { // Event type
      "sql": "SELECT * FROM users WHERE id = 1",
      "time_ms": 54
    }
  }
}
```

</p></details>


<strong>5. ...and many more, checkout the [schema](schema.json) for a complete list.</strong>


## Versioning & Releases

Timber follows the [semver](http://semver.org/) specification for versioning. Releases can
be found in the [releases](https://github.com/timberio/log-event-json-schema/releases) sections.
You can also watch this repo to be notified of any upcoming changes.


## Validation

Data can be validated against the schema using any of [these open source validators](http://json-schema.org/implementations.html).


## Contributing

Contributions are very much welcome, please submit a pull request.