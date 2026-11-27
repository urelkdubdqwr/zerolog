# log

Package log provides a global logger for zerolog.

## Logger

```go
var Logger = zerolog.New(os.Stderr).With().Timestamp().Logger()
```

Logger is the global logger.

## Ctx

```go
func Ctx(ctx context.Context) *zerolog.Logger
```

Ctx returns the Logger associated with the ctx. If no logger is associated, a disabled logger is returned.

### Example: Ctx

Example of using the Ctx function in the log package to log with context

```go
{
	setup()

	hooked := log.Hook(captainHook)
	ctx := context.WithValue(context.Background(), valueKey, "12345")
	logger := hooked.With().Ctx(ctx).Logger()
	log.Ctx(logger.WithContext(ctx)).Info().Msg("hello world")

}
```

Output:

```text
{"level":"info","time":1199811905,"key":"12345","is_error":false,"msg_len":11,"message":"hello world"}
```

## Debug

```go
func Debug() *zerolog.Event
```

Debug starts a new message with debug level.

You must call Msg on the returned event in order to send the event.

### Example: Debug

Example of a log at a particular "level" (in this case, "debug")

```go
{
	setup()
	log.Debug().Msg("hello world")

}
```

Output:

```text
{"level":"debug","time":1199811905,"message":"hello world"}
```

## Err

```go
func Err(err error) *zerolog.Event
```

Err starts a new message with error level with err as a field if not nil or with info level if err is nil.

You must call Msg on the returned event in order to send the event.

### Example: Err

Example of a conditional level based on the presence of an error.

```go
{
	setup()
	err := errors.New("some error")
	log.Err(err).Msg("hello world")
	log.Err(nil).Msg("hello world")

}
```

Output:

```text
{"level":"error","error":"some error","time":1199811905,"message":"hello world"}
{"level":"info","time":1199811905,"message":"hello world"}
```

## Error

```go
func Error() *zerolog.Event
```

Error starts a new message with error level.

You must call Msg on the returned event in order to send the event.

### Example: Error

Example of a log at a particular "level" (in this case, "error")

```go
{
	setup()
	log.Error().Msg("hello world")

}
```

Output:

```text
{"level":"error","time":1199811905,"message":"hello world"}
```

## Fatal

```go
func Fatal() *zerolog.Event
```

Fatal starts a new message with fatal level. The os.Exit(1) function is called by the Msg method.

You must call Msg on the returned event in order to send the event.

### Example: Fatal

Example of a log at a particular "level" (in this case, "fatal")

```go
{
	setup()
	err := errors.New("A repo man spends his life getting into tense situations")
	service := "myservice"

	log.Fatal().
		Err(err).
		Str("service", service).
		Msgf("Cannot start %s", service)

}
```

## Hook

```go
func Hook(h zerolog.Hook) zerolog.Logger
```

Hook returns a logger with the h Hook.

## Info

```go
func Info() *zerolog.Event
```

Info starts a new message with info level.

You must call Msg on the returned event in order to send the event.

### Example: Info

Example of a log at a particular "level" (in this case, "info")

```go
{
	setup()
	log.Info().Msg("hello world")

}
```

Output:

```text
{"level":"info","time":1199811905,"message":"hello world"}
```

## Level

```go
func Level(level zerolog.Level) zerolog.Logger
```

Level creates a child logger with the minimum accepted level set to level.

### Example: Level

Example of using the Level function to set the log level

```go
{
	setup()

	leveled := log.Level(zerolog.ErrorLevel)
	leveled.Info().Msg("hello world")
	leveled.Error().Msg("I said HELLO")

}
```

Output:

```text
{"level":"error","time":1199811905,"message":"I said HELLO"}
```

## Log

```go
func Log() *zerolog.Event
```

Log starts a new message with no level. Setting zerolog.GlobalLevel to zerolog.Disabled will still disable events produced by this method.

You must call Msg on the returned event in order to send the event.

### Example: Log

Example of a log with no particular "level"

```go
{
	setup()
	log.Log().Msg("hello world")

}
```

Output:

```text
{"time":1199811905,"message":"hello world"}
```

## Output

```go
func Output(w io.Writer) zerolog.Logger
```

Output duplicates the global logger and sets w as its output.

### Example: Output

Example of using the Output function in the log package to change the output destination

```go
{
	setup()

	out := &bytes.Buffer{}
	tee := log.Output(out)
	tee.Info().Msg("hello world")
	written := out.Len()

	log.Info().Int("bytes", written).Msg("wrote")

}
```

Output:

```text
{"level":"info","bytes":59,"time":1199811905,"message":"wrote"}
```

## Panic

```go
func Panic() *zerolog.Event
```

Panic starts a new message with panic level. The message is also sent to the panic function.

You must call Msg on the returned event in order to send the event.

### Example: Panic

Example of a log at a particular "level" (in this case, "panic")

```go
{
	setup()

	log.Panic().Msg("Cannot start")

}
```

## Print

```go
func Print(v ...interface{})
```

Print sends a log event using debug level and no extra field. Arguments are handled in the manner of fmt.Print.

### Example: Print

Simple logging example using the Print function in the log package Note that both Print and Printf are at the debug log level by default

```go
{
	setup()

	log.Print("hello world")

}
```

Output:

```text
{"level":"debug","time":1199811905,"message":"hello world"}
```

## Printf

```go
func Printf(format string, v ...interface{})
```

Printf sends a log event using debug level and no extra field. Arguments are handled in the manner of fmt.Printf.

### Example: Printf

Simple logging example using the Printf function in the log package

```go
{
	setup()

	log.Printf("hello %s", "world")

}
```

Output:

```text
{"level":"debug","time":1199811905,"message":"hello world"}
```

## Sample

```go
func Sample(s zerolog.Sampler) zerolog.Logger
```

Sample returns a logger with the s sampler.

### Example: Sample

Example of using the Sample function in the log package to set a sampler

```go
{
	setup()

	sampled := log.Sample(&zerolog.BasicSampler{N: 2})
	sampled.Info().Msg("hello world")
	sampled.Info().Msg("I said, hello world")
	sampled.Info().Msg("Can you here me now world")

}
```

Output:

```text
{"level":"info","time":1199811905,"message":"hello world"}
{"level":"info","time":1199811905,"message":"Can you here me now world"}
```

## Trace

```go
func Trace() *zerolog.Event
```

Trace starts a new message with trace level.

You must call Msg on the returned event in order to send the event.

### Example: Trace

Example of a log at a particular "level" (in this case, "trace")

```go
{
	setup()
	log.Trace().Msg("hello world")

}
```

Output:

```text
{"level":"trace","time":1199811905,"message":"hello world"}
```

## Warn

```go
func Warn() *zerolog.Event
```

Warn starts a new message with warn level.

You must call Msg on the returned event in order to send the event.

### Example: Warn

Example of a log at a particular "level" (in this case, "warn")

```go
{
	setup()
	log.Warn().Msg("hello world")

}
```

Output:

```text
{"level":"warn","time":1199811905,"message":"hello world"}
```

## With

```go
func With() zerolog.Context
```

With creates a child logger with the field added to its context.

### Example: With

Example of using the With function to add context fields

```go
{
	setup()

	augmented := log.With().Str("service", "myservice").Logger()
	augmented.Info().Msg("hello world")

}
```

Output:

```text
{"level":"info","service":"myservice","time":1199811905,"message":"hello world"}
```

## WithLevel

```go
func WithLevel(level zerolog.Level) *zerolog.Event
```

WithLevel starts a new message with level.

You must call Msg on the returned event in order to send the event.

### Example: WithLevel

Example of using the WithLevel function to set the log level

```go
{
	setup()

	event := log.WithLevel(zerolog.ErrorLevel)
	event.Msg("taxes are due")

}
```

Output:

```text
{"level":"error","time":1199811905,"message":"taxes are due"}
```

### Example

This example uses command-line flags to demonstrate various outputs depending on the chosen log level.

```go
{
	setup()
	debug := flag.Bool("debug", false, "sets log level to debug")

	flag.Parse()

	zerolog.SetGlobalLevel(zerolog.InfoLevel)
	if *debug {
		zerolog.SetGlobalLevel(zerolog.DebugLevel)
	}

	log.Debug().Msg("This message appears only when log level set to Debug")
	log.Info().Msg("This message appears when log level set to Debug or Info")

	if e := log.Debug(); e.Enabled() {

		value := "bar"
		e.Str("foo", value).Msg("some debug message")
	}

}
```

Output:

```text
{"level":"info","time":1199811905,"message":"This message appears when log level set to Debug or Info"}
```
