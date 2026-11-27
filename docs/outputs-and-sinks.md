# Outputs and sinks {#outputs-and-sinks}



## Writers {#writers}


A `Logger` writes to any `io.Writer` via `zerolog.New(w)`. Wrap or fan out the
writer to change where events land. The `zerolog.MultiLevelWriter` interface
sends each level to a different writer:

```go
mw := zerolog.MultiLevelWriter(consoleWriter, os.Stdout)
logger := zerolog.New(mw).With().Timestamp().Logger()
```


## Pretty console output {#pretty-console-output}


`zerolog.ConsoleWriter` renders events for a human at a terminal. It is
intentionally slower than the JSON path and is meant for development:

```go
consoleWriter := zerolog.ConsoleWriter{Out: os.Stderr, TimeFormat: time.RFC3339}
logger := zerolog.New(consoleWriter).With().Timestamp().Logger()
```


## Non-blocking writer {#non-blocking-writer}


The `diode` package provides a writer that buffers records on a ring buffer and
drops them once the buffer is full, so a slow sink never blocks the application:

```go
wr := diode.NewWriter(os.Stdout, 1000, 10*time.Millisecond, func(missed int) {
    fmt.Printf("Dropped %d messages\n", missed)
})
logger := zerolog.New(wr)
```


## Binary encoding {#binary-encoding}


Zerolog can emit CBOR instead of JSON. Set the encoder at the package level:

```go
zerolog.Encoder = &zerolog.CBOREncoder{}
```

This pairs with the `journald` package, whose writer frames events in the systemd
journal native protocol.


## Sampling {#sampling}


Sampling reduces log volume while preserving error visibility. `zerolog.Sampler`
combines a basic-rate sampler with a per-level sampler:

```go
sampled := log.Sample(&zerolog.BasicSampler{N: 10})
```

`LevelSampler` lets each level keep a different sample rate. See the
`zerolog.Sampler`, `BasicSampler` and `LevelSampler` entries in the Go API tab.
