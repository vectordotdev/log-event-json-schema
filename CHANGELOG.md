# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](http://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](http://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Changed

  - Context data structures have been moved to individual files under the `/contexts`
    directory for organizational purposes.
  - Event data structures have been moved to individual files under the `/events`
    directory for organizational purposes.
  - The `event` root level key was dropped in favor of moving the event type key to the root level.
    Instead of `event.sql_query` the path is now just `sql_query`.
  - The `context.platform` key has been removed in favor of moving the platform keys to the
    `context` level. Instead of `context.platform.aws_ec2` the path is now just `context.aws_ec2`.
  - Expanded the `http_request` event into `incoming_http_request` and `outgoing_http_request`
    events.
  - Expanded the `http_response` event in favor of the `outgoing_http_response` and
    `incoming_http_response` events.

## [4.1.0] - 2018-04-03

### Added

  - Support `kubernetes` as a platform.

## [4.0.1] - 2018-01-18

### Changed

  - Raise the `query_string` field limit from 2048 to 4096.

## [4.0.0] - 2018-01-17

### Removed

  - Removed the `tags` field in favor of using the `context`, `event`, or `meta` keys which offer
    a structured and more descriptive alternative to attaching data to a log line.

## [3.2.0] - 2017-10-23

### Added

  - Added `host` to the `http` context document. This will include the full domain, including any sub-domains.

## [3.1.3] - 2017-10-02

### Changed

  - Raised the limit of the `event.controller_call.params_json` field from `8192` to `32768`.

[Unreleased]: https://github.com/timberio/timber-elixir/compare/v4.1.0...HEAD
[4.1.0]: https://github.com/timberio/timber-elixir/compare/v4.0.1...v4.1.0
[4.0.1]: https://github.com/timberio/timber-elixir/compare/v4.0.0...v4.0.1
[4.0.0]: https://github.com/timberio/timber-elixir/compare/v3.2.0...v4.0.0
[3.2.0]: https://github.com/timberio/timber-elixir/compare/v3.1.3...v3.2.0
[3.1.2]: https://github.com/timberio/timber-elixir/compare/v3.1.2...v3.1.3
