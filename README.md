# Redis::ReconnectWithReadonly

Reconnect your redis connection if `Redis::CommandError READONLY You can't write against a read only slave.` occurs because of failover.

When redis cluster failovers, the redis master is depromoted to slave and will be READONLY. Such case we have to reconnect redis connection so that new connection goes to new master which is writable.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'redis-reconnect_with_readonly'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install redis-reconnect_with_readonly

## Usage

```ruby
require 'redis/reconnect_with_readonly'
```

## Configuration

This gem tries reconnection `reconnect_attempts` times.
It will wait `2 ^ num of retries` second interval on each retry.
The waiting interval can be suppressed up to `max_retry_interval`.

```
Redis::ReconnectWithReadonly.reconnect_attempts = 1
Redis::ReconnectWithReadonly.max_retry_interval = 3600
```

## Implementation

This gem monkey patches `Redis::Client`.

## Development

```
$ redis-server --port 6379
$ redis-server --port 6380
```

```
$ redis-cli -h localhost -p 6380
localhost:6380> slaveof localhost 6379
```

```
$ bin/console
> redis = Redis.new(:host => "localhost", :port => 6380)
> redis.set('key', 'val')
```

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/[USERNAME]/redis-reconnect_with_readonly. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](http://contributor-covenant.org) code of conduct.

## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

