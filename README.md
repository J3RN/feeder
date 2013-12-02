# feeder - parse feeds (well, eventually)

[![Build Status](https://secure.travis-ci.org/michaelnisi/feeder.png)](http://travis-ci.org/michaelnisi/feeder)

Word on the street has it that Erlang is terrible at parsing strings. A fair reason for me to write an XML parser for RSS and Atom feeds in it. Let's see how this goes.

## Usage

### HTTP

```Erlang
-module(feeder_example).

-export([start/0, request/1]).

start() ->
  feeder:start().

request(Url) ->
  {ok, RequestId} = feeder:request(Url),
  loop(RequestId).

loop(RequestId) ->
  receive
    {feeder, {RequestId, feed, Feed}} ->
      io:format("feed: ~p~n", [Feed]),
      loop(RequestId);
    {feeder, {RequestId, entry, Entry}} ->
      io:format("entry: ~p~n", [Entry]),
      loop(RequestId);
    {feeder, {RequestId, stream_end}} ->
      io:format("stream_end~n")
  end.
```

## License

[MIT License](https://raw.github.com/michaelnisi/feeder/master/LICENSE)
