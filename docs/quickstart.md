# Quickstart {#quickstart}



## Install {#install}


```bash
go get -u github.com/rs/zerolog/log
```


## Simple logging {#simple-logging}


Import the global logger package, `github.com/rs/zerolog/log`:

```go
package main

import (
    "github.com/rs/zerolog"
    "github.com/rs/zerolog/log"
)

func main() {
    // UNIX time is faster and smaller than most timestamp formats.
    zerolog.TimeFieldFormat = zerolog.TimeFormatUnix

    log.Print("hello world")
}
```

```text
{"time":1516134303,"level":"debug","message":"hello world"}
```

`log.Print` writes to `os.Stderr` and defaults to the `trace` level, so a plain
`log.Info(...)` call appears on stderr out of the box.


## Contextual logging {#contextual-logging}


Attach strongly typed key/value pairs to an event. The chain must end in `Msg`
or `Msgf`, otherwise nothing is written:

```go
log.Debug().
    Str("Scale", "833 cents").
    Float64("Interval", 833.09).
    Msg("Fibonacci is everywhere")

log.Debug().
    Str("Name", "Tom").
    Send()
```

```text
{"level":"debug","Scale":"833 cents","Interval":833.09,"time":1562212768,"message":"Fibonacci is everywhere"}
{"level":"debug","Name":"Tom","time":1562212768}
```


## Levelled logging {#levelled-logging}


```go
zerolog.SetGlobalLevel(zerolog.InfoLevel)
log.Info().Msg("hello world")
```

```text
{"time":1516134303,"level":"info","message":"hello world"}
```

Levels, from highest to lowest: `panic`, `fatal`, `error`, `warn`, `info`,
`debug`, `trace`. Use `zerolog.Disabled` to turn logging off entirely.


## Next {#next}


- [Context and hooks](https://urelkdubdqwr.github.io/zerolog/context-and-hooks.md) for request-scoped loggers and hooks.
- [Outputs and sinks](https://urelkdubdqwr.github.io/zerolog/outputs-and-sinks.md) for console output, CBOR and samplers.
- The **Go API** tab for the generated reference of every package.
