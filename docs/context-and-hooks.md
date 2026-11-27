# Context and hooks {#context-and-hooks}



## Deriving child loggers {#deriving-child-loggers}


A `Logger` is cheap to copy, so build a base logger once and derive children
with `.With()`:

```go
base := zerolog.New(os.Stdout).With().Timestamp().Logger()
reqLog := base.With().Str("request_id", id).Logger()
reqLog.Info().Msg("handling request")
```

`With()` returns a `Context` whose fields are copied into every event the child
logger writes.


## context.Context integration {#contextcontext-integration}


Zerolog stores a logger inside a `context.Context` with
`logger.WithContext(ctx)` and reads it back with `zerolog.Ctx(ctx)`:

```go
ctx = logger.WithContext(ctx)
log  := zerolog.Ctx(ctx).With().Str("component", "worker").Logger()
```

`*zerolog.Logger` implements `context.Context` directly, so a logger can be
passed where a context is expected. `zerolog.Ctx` returns a disabled logger
instead of `nil` when the context holds none, which keeps call sites free of
nil checks.


## Hooks {#hooks}


A `Hook` runs on every event, letting you enrich, sample or route messages.
Implement the `Hook` interface:

```go
type Hook interface {
    Run(e *zerolog.Event, level zerolog.Level, message string)
}
```

Hooks are attached through `Logger.Hook(...)` or the global
`zerolog.AddGlobalHook(...)`. The `pkgerrors` package ships a hook that records
a stack trace from a `github.com/pkg/errors` value on error events.


## HTTP middleware {#http-middleware}


The `hlog` package provides `net/http` middleware that injects a logger into the
request context and exposes helpers for request IDs and remote addresses:

```go
h := hlog.NewHandler(log.Logger)
a := hlog.AccessHandler(func(r *http.Request, status, size int, dur time.Duration) { /* ... */ })
r := hlog.RemoteAddrHandler("ip")
```

See the `hlog` package page in the **Go API** tab for the full handler set.
