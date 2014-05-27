# feeder - parse RSS and Atom

[![Build Status](https://secure.travis-ci.org/michaelnisi/feeder.svg)](http://travis-ci.org/michaelnisi/feeder)

The **feeder** [Erlang](http://www.erlang.org/) module parses RSS and Atom formatted XML feeds. It is a stream based parser that sends its events through a callback interface.

## Usage

Here is, for example, how you might parse a file and accumulate parser events:

```erlang
-module(acc).
-export([file/1]).

event({entry, Entry}, {Feed, Entries}) ->
  {Feed, [Entry|Entries]};
event({feed, Feed}, {[], Entries}) ->
  {Feed, Entries};
event(endFeed, S) ->
  S.

opts() ->
  [{event_state, {[],[]}}, {event_fun, fun event/2}].

file(Filename) ->
  {ok, EventState, _Rest} = feeder:file(Filename, opts()),
  EventState.
```

## types

### feed()

  - `author = binary()`
  - `id = binary()`
  - `image = binary()`
  - `link = binary()`
  - `subtitle = binary()`
  - `summary = binary()`
  - `title = binary()`
  - `updated = integer()`

### enclosure()

  - `url = binary()`
  - `length = binary()`
  - `type = binary()`

### entry()

  - `author = binary()`
  - `enclosure = enclosure()`
  - `id = binary()`
  - `image = binary()`
  - `link = binary()`
  - `subtitle = binary()`
  - `summary = binary()`
  - `title = binary()`
  - `updated = integer()`

### option()

Options to setup the parser.

`{continuation_fun, ContinuationFun}`
[`ContinuationFun`](http://www.erlang.org/doc/man/xmerl_sax_parser.html#ContinuationFun-1) is a call back function to decide what to do if the parser runs into EOF before the document is complete.

`{continuation_state, term()}`
State that is accessible in the continuation call back function.

`{event_fun, EventFun}`
[`EventFun`](http://www.erlang.org/doc/man/xmerl_sax_parser.html#EventFun-3) is the call back function for parser events.

`{event_state, term()}`
State that is accessible in the event call back function.

### event()

The events that are sent to the user via the callback.

`{feed, Feed}`

- `Feed = feed()`

Receive notification when the meta information of the feed or channel has been parsed.

`{entry, Entry}`

- `Entry = entry()`

Receive notification for each entry or article in the feed.

`endFeed`

Receive notification of the end of a document. **Feeder** will send this event only once, and it will be the last event during the parse.

## exports

### file(Filename, Opts) -> Result

- `Filename = string()`
- `Opts = [option()]`

### stream(Xml, Opts) -> Result

- `Xml = unicode_binary() | latin1_binary() | [unicode_char()]`
- `Opts = [option()]`

## License

[MIT License](https://raw.github.com/michaelnisi/feeder/master/LICENSE)
