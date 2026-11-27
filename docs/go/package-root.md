# github.com/rs/zerolog

Package zerolog provides a lightweight logging library dedicated to JSON logging.

A global Logger can be used for simple logging:

	import "github.com/rs/zerolog/log"

	log.Info().Msg("hello world")
	// Output: {"time":1494567715,"level":"info","message":"hello world"}

NOTE: To import the global logger, import the "log" subpackage "github.com/rs/zerolog/log".

Fields can be added to log messages:

	log.Info().Str("foo", "bar").Msg("hello world")
	// Output: {"time":1494567715,"level":"info","message":"hello world","foo":"bar"}

Create logger instance to manage different outputs:

	logger := zerolog.New(os.Stderr).With().Timestamp().Logger()
	logger.Info().
	       Str("foo", "bar").
	       Msg("hello world")
	// Output: {"time":1494567715,"level":"info","message":"hello world","foo":"bar"}

Sub-loggers let you chain loggers with additional context:

	sublogger := log.With().Str("component", "foo").Logger()
	sublogger.Info().Msg("hello world")
	// Output: {"time":1494567715,"level":"info","message":"hello world","component":"foo"}

Level logging

	zerolog.SetGlobalLevel(zerolog.InfoLevel)

	log.Debug().Msg("filtered out message")
	log.Info().Msg("routed message")

	if e := log.Debug(); e.Enabled() {
	    // Compute log output only if enabled.
	    value := compute()
	    e.Str("foo", value).Msg("some debug message")
	}
	// Output: {"level":"info","time":1494567715,"routed message"}

Customize automatic field names:

	log.TimestampFieldName = "t"
	log.LevelFieldName = "p"
	log.MessageFieldName = "m"

	log.Info().Msg("hello world")
	// Output: {"t":1494567715,"p":"info","m":"hello world"}

Log with no level and message:

	log.Log().Str("foo","bar").Msg("")
	// Output: {"time":1494567715,"foo":"bar"}

Add contextual fields to global Logger:

	log.Logger = log.With().Str("foo", "bar").Logger()

Sample logs:

	sampled := log.Sample(&zerolog.BasicSampler{N: 10})
	sampled.Info().Msg("will be logged every 10 messages")

Log with contextual hooks:

	// Create the hook:
	type SeverityHook struct{}

	func (h SeverityHook) Run(e *zerolog.Event, level zerolog.Level, msg string) {
	     if level != zerolog.NoLevel {
	         e.Str("severity", level.String())
	     }
	}

	// And use it:
	var h SeverityHook
	log := zerolog.New(os.Stdout).Hook(h)
	log.Warn().Msg("")
	// Output: {"level":"warn","severity":"warn"}

### Caveats

Field duplication:

There is no field deduplication out-of-the-box. Using the same key multiple times creates a new key in the final JSON each time.

	logger := zerolog.New(os.Stderr).With().Timestamp().Logger()
	logger.Info().
	       Timestamp().
	       Msg("dup")
	// Output: {"level":"info","time":1494567715,"time":1494567715,"message":"dup"}

In this case, many consumers will take the last value, but this is not guaranteed; check yours if in doubt.

Concurrency safety:

Be careful when calling UpdateContext. It is not concurrency safe. Use the With method to create a child logger:

	func handler(w http.ResponseWriter, r *http.Request) {
	    // Create a child logger for concurrency safety
	    logger := log.Logger.With().Logger()

	    // Add context fields, for example User-Agent from HTTP headers
	    logger.UpdateContext(func(c zerolog.Context) zerolog.Context {
	        ...
	    })
	}

## TimeFormatUnix

```go
const (
	// TimeFormatUnix defines a time format that makes time fields to be
	// serialized as Unix timestamp integers.
	TimeFormatUnix = ""

	// TimeFormatUnixMs defines a time format that makes time fields to be
	// serialized as Unix timestamp integers in milliseconds.
	TimeFormatUnixMs = "UNIXMS"

	// TimeFormatUnixMicro defines a time format that makes time fields to be
	// serialized as Unix timestamp integers in microseconds.
	TimeFormatUnixMicro = "UNIXMICRO"

	// TimeFormatUnixNano defines a time format that makes time fields to be
	// serialized as Unix timestamp integers in nanoseconds.
	TimeFormatUnixNano = "UNIXNANO"

	// DurationFormatFloat defines a format for Duration fields that makes duration fields to be
	// serialized as floating point numbers.
	DurationFormatFloat = "float"
	// DurationFormatInt defines a format for Duration fields that makes duration fields to be
	// serialized as integers.
	DurationFormatInt = "int"
	// DurationFormatString defines a format for Duration fields that makes duration fields to be
	// serialized as string.
	DurationFormatString = "string"
)
```

## TimeFormatUnixMs

```go
const (
	// TimeFormatUnix defines a time format that makes time fields to be
	// serialized as Unix timestamp integers.
	TimeFormatUnix = ""

	// TimeFormatUnixMs defines a time format that makes time fields to be
	// serialized as Unix timestamp integers in milliseconds.
	TimeFormatUnixMs = "UNIXMS"

	// TimeFormatUnixMicro defines a time format that makes time fields to be
	// serialized as Unix timestamp integers in microseconds.
	TimeFormatUnixMicro = "UNIXMICRO"

	// TimeFormatUnixNano defines a time format that makes time fields to be
	// serialized as Unix timestamp integers in nanoseconds.
	TimeFormatUnixNano = "UNIXNANO"

	// DurationFormatFloat defines a format for Duration fields that makes duration fields to be
	// serialized as floating point numbers.
	DurationFormatFloat = "float"
	// DurationFormatInt defines a format for Duration fields that makes duration fields to be
	// serialized as integers.
	DurationFormatInt = "int"
	// DurationFormatString defines a format for Duration fields that makes duration fields to be
	// serialized as string.
	DurationFormatString = "string"
)
```

## TimeFormatUnixMicro

```go
const (
	// TimeFormatUnix defines a time format that makes time fields to be
	// serialized as Unix timestamp integers.
	TimeFormatUnix = ""

	// TimeFormatUnixMs defines a time format that makes time fields to be
	// serialized as Unix timestamp integers in milliseconds.
	TimeFormatUnixMs = "UNIXMS"

	// TimeFormatUnixMicro defines a time format that makes time fields to be
	// serialized as Unix timestamp integers in microseconds.
	TimeFormatUnixMicro = "UNIXMICRO"

	// TimeFormatUnixNano defines a time format that makes time fields to be
	// serialized as Unix timestamp integers in nanoseconds.
	TimeFormatUnixNano = "UNIXNANO"

	// DurationFormatFloat defines a format for Duration fields that makes duration fields to be
	// serialized as floating point numbers.
	DurationFormatFloat = "float"
	// DurationFormatInt defines a format for Duration fields that makes duration fields to be
	// serialized as integers.
	DurationFormatInt = "int"
	// DurationFormatString defines a format for Duration fields that makes duration fields to be
	// serialized as string.
	DurationFormatString = "string"
)
```

## TimeFormatUnixNano

```go
const (
	// TimeFormatUnix defines a time format that makes time fields to be
	// serialized as Unix timestamp integers.
	TimeFormatUnix = ""

	// TimeFormatUnixMs defines a time format that makes time fields to be
	// serialized as Unix timestamp integers in milliseconds.
	TimeFormatUnixMs = "UNIXMS"

	// TimeFormatUnixMicro defines a time format that makes time fields to be
	// serialized as Unix timestamp integers in microseconds.
	TimeFormatUnixMicro = "UNIXMICRO"

	// TimeFormatUnixNano defines a time format that makes time fields to be
	// serialized as Unix timestamp integers in nanoseconds.
	TimeFormatUnixNano = "UNIXNANO"

	// DurationFormatFloat defines a format for Duration fields that makes duration fields to be
	// serialized as floating point numbers.
	DurationFormatFloat = "float"
	// DurationFormatInt defines a format for Duration fields that makes duration fields to be
	// serialized as integers.
	DurationFormatInt = "int"
	// DurationFormatString defines a format for Duration fields that makes duration fields to be
	// serialized as string.
	DurationFormatString = "string"
)
```

## DurationFormatFloat

```go
const (
	// TimeFormatUnix defines a time format that makes time fields to be
	// serialized as Unix timestamp integers.
	TimeFormatUnix = ""

	// TimeFormatUnixMs defines a time format that makes time fields to be
	// serialized as Unix timestamp integers in milliseconds.
	TimeFormatUnixMs = "UNIXMS"

	// TimeFormatUnixMicro defines a time format that makes time fields to be
	// serialized as Unix timestamp integers in microseconds.
	TimeFormatUnixMicro = "UNIXMICRO"

	// TimeFormatUnixNano defines a time format that makes time fields to be
	// serialized as Unix timestamp integers in nanoseconds.
	TimeFormatUnixNano = "UNIXNANO"

	// DurationFormatFloat defines a format for Duration fields that makes duration fields to be
	// serialized as floating point numbers.
	DurationFormatFloat = "float"
	// DurationFormatInt defines a format for Duration fields that makes duration fields to be
	// serialized as integers.
	DurationFormatInt = "int"
	// DurationFormatString defines a format for Duration fields that makes duration fields to be
	// serialized as string.
	DurationFormatString = "string"
)
```

## DurationFormatInt

```go
const (
	// TimeFormatUnix defines a time format that makes time fields to be
	// serialized as Unix timestamp integers.
	TimeFormatUnix = ""

	// TimeFormatUnixMs defines a time format that makes time fields to be
	// serialized as Unix timestamp integers in milliseconds.
	TimeFormatUnixMs = "UNIXMS"

	// TimeFormatUnixMicro defines a time format that makes time fields to be
	// serialized as Unix timestamp integers in microseconds.
	TimeFormatUnixMicro = "UNIXMICRO"

	// TimeFormatUnixNano defines a time format that makes time fields to be
	// serialized as Unix timestamp integers in nanoseconds.
	TimeFormatUnixNano = "UNIXNANO"

	// DurationFormatFloat defines a format for Duration fields that makes duration fields to be
	// serialized as floating point numbers.
	DurationFormatFloat = "float"
	// DurationFormatInt defines a format for Duration fields that makes duration fields to be
	// serialized as integers.
	DurationFormatInt = "int"
	// DurationFormatString defines a format for Duration fields that makes duration fields to be
	// serialized as string.
	DurationFormatString = "string"
)
```

## DurationFormatString

```go
const (
	// TimeFormatUnix defines a time format that makes time fields to be
	// serialized as Unix timestamp integers.
	TimeFormatUnix = ""

	// TimeFormatUnixMs defines a time format that makes time fields to be
	// serialized as Unix timestamp integers in milliseconds.
	TimeFormatUnixMs = "UNIXMS"

	// TimeFormatUnixMicro defines a time format that makes time fields to be
	// serialized as Unix timestamp integers in microseconds.
	TimeFormatUnixMicro = "UNIXMICRO"

	// TimeFormatUnixNano defines a time format that makes time fields to be
	// serialized as Unix timestamp integers in nanoseconds.
	TimeFormatUnixNano = "UNIXNANO"

	// DurationFormatFloat defines a format for Duration fields that makes duration fields to be
	// serialized as floating point numbers.
	DurationFormatFloat = "float"
	// DurationFormatInt defines a format for Duration fields that makes duration fields to be
	// serialized as integers.
	DurationFormatInt = "int"
	// DurationFormatString defines a format for Duration fields that makes duration fields to be
	// serialized as string.
	DurationFormatString = "string"
)
```

## DebugLevel

```go
const (
	// DebugLevel defines debug log level.
	DebugLevel Level = iota
	// InfoLevel defines info log level.
	InfoLevel
	// WarnLevel defines warn log level.
	WarnLevel
	// ErrorLevel defines error log level.
	ErrorLevel
	// FatalLevel defines fatal log level.
	FatalLevel
	// PanicLevel defines panic log level.
	PanicLevel
	// NoLevel defines an absent log level.
	NoLevel
	// Disabled disables the logger.
	Disabled

	// TraceLevel defines trace log level.
	TraceLevel Level = -1
)
```

## InfoLevel

```go
const (
	// DebugLevel defines debug log level.
	DebugLevel Level = iota
	// InfoLevel defines info log level.
	InfoLevel
	// WarnLevel defines warn log level.
	WarnLevel
	// ErrorLevel defines error log level.
	ErrorLevel
	// FatalLevel defines fatal log level.
	FatalLevel
	// PanicLevel defines panic log level.
	PanicLevel
	// NoLevel defines an absent log level.
	NoLevel
	// Disabled disables the logger.
	Disabled

	// TraceLevel defines trace log level.
	TraceLevel Level = -1
)
```

## WarnLevel

```go
const (
	// DebugLevel defines debug log level.
	DebugLevel Level = iota
	// InfoLevel defines info log level.
	InfoLevel
	// WarnLevel defines warn log level.
	WarnLevel
	// ErrorLevel defines error log level.
	ErrorLevel
	// FatalLevel defines fatal log level.
	FatalLevel
	// PanicLevel defines panic log level.
	PanicLevel
	// NoLevel defines an absent log level.
	NoLevel
	// Disabled disables the logger.
	Disabled

	// TraceLevel defines trace log level.
	TraceLevel Level = -1
)
```

## ErrorLevel

```go
const (
	// DebugLevel defines debug log level.
	DebugLevel Level = iota
	// InfoLevel defines info log level.
	InfoLevel
	// WarnLevel defines warn log level.
	WarnLevel
	// ErrorLevel defines error log level.
	ErrorLevel
	// FatalLevel defines fatal log level.
	FatalLevel
	// PanicLevel defines panic log level.
	PanicLevel
	// NoLevel defines an absent log level.
	NoLevel
	// Disabled disables the logger.
	Disabled

	// TraceLevel defines trace log level.
	TraceLevel Level = -1
)
```

## FatalLevel

```go
const (
	// DebugLevel defines debug log level.
	DebugLevel Level = iota
	// InfoLevel defines info log level.
	InfoLevel
	// WarnLevel defines warn log level.
	WarnLevel
	// ErrorLevel defines error log level.
	ErrorLevel
	// FatalLevel defines fatal log level.
	FatalLevel
	// PanicLevel defines panic log level.
	PanicLevel
	// NoLevel defines an absent log level.
	NoLevel
	// Disabled disables the logger.
	Disabled

	// TraceLevel defines trace log level.
	TraceLevel Level = -1
)
```

## PanicLevel

```go
const (
	// DebugLevel defines debug log level.
	DebugLevel Level = iota
	// InfoLevel defines info log level.
	InfoLevel
	// WarnLevel defines warn log level.
	WarnLevel
	// ErrorLevel defines error log level.
	ErrorLevel
	// FatalLevel defines fatal log level.
	FatalLevel
	// PanicLevel defines panic log level.
	PanicLevel
	// NoLevel defines an absent log level.
	NoLevel
	// Disabled disables the logger.
	Disabled

	// TraceLevel defines trace log level.
	TraceLevel Level = -1
)
```

## NoLevel

```go
const (
	// DebugLevel defines debug log level.
	DebugLevel Level = iota
	// InfoLevel defines info log level.
	InfoLevel
	// WarnLevel defines warn log level.
	WarnLevel
	// ErrorLevel defines error log level.
	ErrorLevel
	// FatalLevel defines fatal log level.
	FatalLevel
	// PanicLevel defines panic log level.
	PanicLevel
	// NoLevel defines an absent log level.
	NoLevel
	// Disabled disables the logger.
	Disabled

	// TraceLevel defines trace log level.
	TraceLevel Level = -1
)
```

## Disabled

```go
const (
	// DebugLevel defines debug log level.
	DebugLevel Level = iota
	// InfoLevel defines info log level.
	InfoLevel
	// WarnLevel defines warn log level.
	WarnLevel
	// ErrorLevel defines error log level.
	ErrorLevel
	// FatalLevel defines fatal log level.
	FatalLevel
	// PanicLevel defines panic log level.
	PanicLevel
	// NoLevel defines an absent log level.
	NoLevel
	// Disabled disables the logger.
	Disabled

	// TraceLevel defines trace log level.
	TraceLevel Level = -1
)
```

## TraceLevel

```go
const (
	// DebugLevel defines debug log level.
	DebugLevel Level = iota
	// InfoLevel defines info log level.
	InfoLevel
	// WarnLevel defines warn log level.
	WarnLevel
	// ErrorLevel defines error log level.
	ErrorLevel
	// FatalLevel defines fatal log level.
	FatalLevel
	// PanicLevel defines panic log level.
	PanicLevel
	// NoLevel defines an absent log level.
	NoLevel
	// Disabled disables the logger.
	Disabled

	// TraceLevel defines trace log level.
	TraceLevel Level = -1
)
```

## TimestampFieldName

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## LevelFieldName

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## LevelTraceValue

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## LevelDebugValue

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## LevelInfoValue

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## LevelWarnValue

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## LevelErrorValue

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## LevelFatalValue

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## LevelPanicValue

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## LevelFieldMarshalFunc

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## MessageFieldName

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## ErrorFieldName

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## CallerFieldName

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## CallerSkipFrameCount

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## CallerMarshalFunc

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## ErrorStackFieldName

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## ErrorStackMarshaler

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## ErrorMarshalFunc

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## InterfaceMarshalFunc

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## TimeFieldFormat

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## TimestampFunc

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## DurationFieldFormat

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## DurationFieldUnit

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## DurationFieldInteger

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## ErrorHandler

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## FatalExitFunc

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## DefaultContextLogger

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## LevelColors

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## FormattedLevels

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## TriggerLevelWriterBufferReuseLimit

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## FloatingPointPrecision

```go
var (
	// TimestampFieldName is the field name used for the timestamp field.
	TimestampFieldName = "time"

	// LevelFieldName is the field name used for the level field.
	LevelFieldName = "level"

	// LevelTraceValue is the value used for the trace level field.
	LevelTraceValue = "trace"
	// LevelDebugValue is the value used for the debug level field.
	LevelDebugValue = "debug"
	// LevelInfoValue is the value used for the info level field.
	LevelInfoValue = "info"
	// LevelWarnValue is the value used for the warn level field.
	LevelWarnValue = "warn"
	// LevelErrorValue is the value used for the error level field.
	LevelErrorValue = "error"
	// LevelFatalValue is the value used for the fatal level field.
	LevelFatalValue = "fatal"
	// LevelPanicValue is the value used for the panic level field.
	LevelPanicValue = "panic"

	// LevelFieldMarshalFunc allows customization of global level field marshaling.
	LevelFieldMarshalFunc = func(l Level) string {
		return l.String()
	}

	// MessageFieldName is the field name used for the message field.
	MessageFieldName = "message"

	// ErrorFieldName is the field name used for error fields.
	ErrorFieldName = "error"

	// CallerFieldName is the field name used for caller field.
	CallerFieldName = "caller"

	// CallerSkipFrameCount is the number of stack frames to skip to find the caller.
	CallerSkipFrameCount = 2

	// CallerMarshalFunc allows customization of global caller marshaling
	CallerMarshalFunc = func(pc uintptr, file string, line int) string {
		return file + ":" + strconv.Itoa(line)
	}

	// ErrorStackFieldName is the field name used for error stacks.
	ErrorStackFieldName = "stack"

	// ErrorStackMarshaler extract the stack from err if any.
	ErrorStackMarshaler func(err error) interface{}

	// ErrorMarshalFunc allows customization of global error marshaling
	ErrorMarshalFunc = func(err error) interface{} {
		return err
	}

	// InterfaceMarshalFunc allows customization of interface marshaling.
	// Default: "encoding/json.Marshal" with disabled HTML escaping
	InterfaceMarshalFunc = func(v interface{}) ([]byte, error) {
		var buf bytes.Buffer
		encoder := json.NewEncoder(&buf)
		encoder.SetEscapeHTML(false)
		err := encoder.Encode(v)
		if err != nil {
			return nil, err
		}
		b := buf.Bytes()
		if len(b) > 0 {

			return b[:len(b)-1], nil
		}
		return b, nil
	}

	// TimeFieldFormat defines the time format of the Time field type. If set to
	// TimeFormatUnix, TimeFormatUnixMs, TimeFormatUnixMicro or TimeFormatUnixNano, the time is formatted as a UNIX
	// timestamp as integer.
	TimeFieldFormat = time.RFC3339

	// TimestampFunc defines the function called to generate a timestamp.
	TimestampFunc = time.Now

	// DurationFieldFormat defines the format of the Duration field type.
	DurationFieldFormat = DurationFormatFloat

	// DurationFieldUnit defines the unit for time.Duration type fields added
	// using the Dur method.
	DurationFieldUnit = time.Millisecond

	// DurationFieldInteger renders Dur fields as integer instead of float if
	// set to true.
	// Deprecated: use DurationFieldFormat with DurationFormatInt instead.
	DurationFieldInteger = false

	// ErrorHandler is called whenever zerolog fails to write an event on its
	// output. If not set, an error is printed on the stderr. This handler must
	// be thread safe and non-blocking.
	ErrorHandler func(err error)

	// FatalExitFunc is called by log.Fatal() instead of os.Exit(1). If not set,
	// os.Exit(1) is called.
	FatalExitFunc func()

	// DefaultContextLogger is returned from Ctx() if there is no logger associated
	// with the context.
	DefaultContextLogger *Logger

	// LevelColors are used by ConsoleWriter's consoleDefaultFormatLevel to color
	// log levels.
	LevelColors = map[Level]int{
		TraceLevel: colorBlue,
		DebugLevel: 0,
		InfoLevel:  colorGreen,
		WarnLevel:  colorYellow,
		ErrorLevel: colorRed,
		FatalLevel: colorRed,
		PanicLevel: colorRed,
	}

	// FormattedLevels are used by ConsoleWriter's consoleDefaultFormatLevel
	// for a short level name.
	FormattedLevels = map[Level]string{
		TraceLevel: "TRC",
		DebugLevel: "DBG",
		InfoLevel:  "INF",
		WarnLevel:  "WRN",
		ErrorLevel: "ERR",
		FatalLevel: "FTL",
		PanicLevel: "PNC",
	}

	// TriggerLevelWriterBufferReuseLimit is a limit in bytes that a buffer is dropped
	// from the TriggerLevelWriter buffer pool if the buffer grows above the limit.
	TriggerLevelWriterBufferReuseLimit = 64 * 1024

	// FloatingPointPrecision, if set to a value other than -1, controls the number
	// of digits when formatting float numbers in JSON. See strconv.FormatFloat for
	// more details.
	FloatingPointPrecision = -1
)
```

## Often

```go
var (
	// Often samples log every ~ 10 events.
	Often = RandomSampler(10)
	// Sometimes samples log every ~ 100 events.
	Sometimes = RandomSampler(100)
	// Rarely samples log every ~ 1000 events.
	Rarely = RandomSampler(1000)
)
```

## Sometimes

```go
var (
	// Often samples log every ~ 10 events.
	Often = RandomSampler(10)
	// Sometimes samples log every ~ 100 events.
	Sometimes = RandomSampler(100)
	// Rarely samples log every ~ 1000 events.
	Rarely = RandomSampler(1000)
)
```

## Rarely

```go
var (
	// Often samples log every ~ 10 events.
	Often = RandomSampler(10)
	// Sometimes samples log every ~ 100 events.
	Sometimes = RandomSampler(100)
	// Rarely samples log every ~ 1000 events.
	Rarely = RandomSampler(1000)
)
```

## Arr

```go
func Arr() *Array
```

Arr creates an array to be added to an Event or Context. WARNING: This function is deprecated because it does not preserve the stack, hooks, and context from the parent event. Deprecated: Use Event.CreateArray or Context.CreateArray instead.

## AsLogObjectMarshalers

```go
func AsLogObjectMarshalers[T LogObjectMarshaler](objs []T) []LogObjectMarshaler
```

## AsStringers

```go
func AsStringers[T fmt.Stringer](objs []T) []fmt.Stringer
```

## ConsoleTestWriter

```go
func ConsoleTestWriter(t TestingLog) func(w *ConsoleWriter)
```

ConsoleTestWriter creates an option that correctly sets the file frame depth for testing.TB log.

## Ctx

```go
func Ctx(ctx context.Context) *Logger
```

Ctx returns the Logger associated with the ctx. If no logger is associated, DefaultContextLogger is returned, unless DefaultContextLogger is nil, in which case a disabled logger is returned.

## Dict

```go
func Dict() *Event
```

Dict creates an Event to be used with the \*Event.Dict method. Call usual field methods like Str, Int etc to add fields to this event and give it as argument the \*Event.Dict method. NOTE: This function is deprecated because it does not preserve the stack, hooks, and context from the parent event. Deprecated: Use Event.CreateDict instead.

## DisableSampling

```go
func DisableSampling(v bool)
```

DisableSampling will disable sampling in all Loggers if true.

## GlobalLevel

```go
func GlobalLevel() Level
```

GlobalLevel returns the current global log level

## MultiLevelWriter

```go
func MultiLevelWriter(writers ...io.Writer) LevelWriter
```

MultiLevelWriter creates a writer that duplicates its writes to all the provided writers, similar to the Unix tee(1) command. If some writers implement LevelWriter, their WriteLevel method will be used instead of Write.

## New

```go
func New(w io.Writer) Logger
```

New creates a root logger with given output writer. If the output writer implements the LevelWriter interface, the WriteLevel method will be called instead of the Write one.

Each logging operation makes a single call to the Writer's Write method. There is no guarantee on access serialization to the Writer. If your Writer is not thread safe, you may consider using sync wrapper.

### Example: New

```go
{
	log := zerolog.New(os.Stdout)

	log.Info().Msg("hello world")

}
```

Output:

```text
{"level":"info","message":"hello world"}
```

## NewConsoleWriter

```go
func NewConsoleWriter(options ...func(w *ConsoleWriter)) ConsoleWriter
```

NewConsoleWriter creates and initializes a new ConsoleWriter.

### Example: NewConsoleWriter

```go
{
	out := zerolog.NewConsoleWriter()
	out.NoColor = true
	log := zerolog.New(out)

	log.Debug().Str("foo", "bar").Msg("Hello World")

}
```

Output:

```text
<nil> DBG Hello World foo=bar
```

## NewLevelHook

```go
func NewLevelHook() LevelHook
```

NewLevelHook returns a new LevelHook.

## NewSlogHandler

```go
func NewSlogHandler(logger Logger) *SlogHandler
```

NewSlogHandler creates a new slog.Handler that writes log records to the given zerolog.Logger. The handler maps slog levels to zerolog levels and converts slog attributes to zerolog fields.

## NewTestWriter

```go
func NewTestWriter(t TestingLog) TestWriter
```

NewTestWriter creates a writer that logs to the testing.TB.

## Nop

```go
func Nop() Logger
```

Nop returns a disabled logger for which all operation are no-op.

## ParseLevel

```go
func ParseLevel(levelStr string) (Level, error)
```

ParseLevel converts a level string into a zerolog Level value. returns an error if the input string does not match known values.

## SetGlobalLevel

```go
func SetGlobalLevel(l Level)
```

SetGlobalLevel sets the global override for log level. If this values is raised, all Loggers will use at least this value.

To globally disable logs, set GlobalLevel to Disabled.

## SyncWriter

```go
func SyncWriter(w io.Writer) io.Writer
```

SyncWriter wraps w so that each call to Write is synchronized with a mutex. This syncer can be used to wrap the call to writer's Write method if it is not thread safe. Note that you do not need this wrapper for os.File Write operations on POSIX and Windows systems as they are already thread-safe.

## SyslogCEEWriter

```go
func SyslogCEEWriter(w SyslogWriter) LevelWriter
```

SyslogCEEWriter wraps a SyslogWriter with a SyslogLevelWriter that adds a MITRE CEE prefix for JSON syslog entries, compatible with rsyslog and syslog-ng JSON logging support. See [https://www.rsyslog.com/json-elasticsearch/](https://www.rsyslog.com/json-elasticsearch/)

## SyslogLevelWriter

```go
func SyslogLevelWriter(w SyslogWriter) LevelWriter
```

SyslogLevelWriter wraps a SyslogWriter and call the right syslog level method matching the zerolog level.

## Array

```go
type Array struct {
	// contains filtered or unexported fields
}
```

Array is used to prepopulate an array of items which can be re-used to add to log messages.

## Bool

```go
func (a *Array) Bool(b bool) *Array
```

Bool appends the val as a bool to the array.

## Bytes

```go
func (a *Array) Bytes(val []byte) *Array
```

Bytes appends the val as a string to the array.

## Dict

```go
func (a *Array) Dict(dict *Event) *Array
```

Dict adds the dict Event to the array

## Dur

```go
func (a *Array) Dur(d time.Duration) *Array
```

Dur appends d to the array.

## Err

```go
func (a *Array) Err(err error) *Array
```

Err serializes and appends the err to the array.

## Errs

```go
func (a *Array) Errs(errs []error) *Array
```

Errs serializes and appends errors to the array.

## Float32

```go
func (a *Array) Float32(f float32) *Array
```

Float32 appends f as a float32 to the array.

## Float64

```go
func (a *Array) Float64(f float64) *Array
```

Float64 appends f as a float64 to the array.

## Hex

```go
func (a *Array) Hex(val []byte) *Array
```

Hex appends the val as a hex string to the array.

## IPAddr

```go
func (a *Array) IPAddr(ip net.IP) *Array
```

IPAddr adds a net.IP IPv4 or IPv6 address to the array

## IPPrefix

```go
func (a *Array) IPPrefix(pfx net.IPNet) *Array
```

IPPrefix adds a net.IPNet IPv4 or IPv6 Prefix (IP + mask) to the array

## Int

```go
func (a *Array) Int(i int) *Array
```

Int appends i as a int to the array.

## Int16

```go
func (a *Array) Int16(i int16) *Array
```

Int16 appends i as a int16 to the array.

## Int32

```go
func (a *Array) Int32(i int32) *Array
```

Int32 appends i as a int32 to the array.

## Int64

```go
func (a *Array) Int64(i int64) *Array
```

Int64 appends i as a int64 to the array.

## Int8

```go
func (a *Array) Int8(i int8) *Array
```

Int8 appends i as a int8 to the array.

## Interface

```go
func (a *Array) Interface(i interface{}) *Array
```

Interface appends i marshaled using reflection.

## MACAddr

```go
func (a *Array) MACAddr(ha net.HardwareAddr) *Array
```

MACAddr adds a net.HardwareAddr MAC (Ethernet) address to the array

## MarshalZerologArray

```go
func (*Array) MarshalZerologArray(*Array)
```

MarshalZerologArray method here is no-op - since data is already in the needed format.

## Object

```go
func (a *Array) Object(obj LogObjectMarshaler) *Array
```

Object marshals an object that implement the LogObjectMarshaler interface and appends it to the array.

## RawJSON

```go
func (a *Array) RawJSON(val []byte) *Array
```

RawJSON adds already encoded JSON to the array.

## Str

```go
func (a *Array) Str(val string) *Array
```

Str appends the val as a string to the array.

## Time

```go
func (a *Array) Time(t time.Time) *Array
```

Time appends t formatted as string using zerolog.TimeFieldFormat.

## Type

```go
func (a *Array) Type(val interface{}) *Array
```

Type adds the val's type using reflection to the array.

## Uint

```go
func (a *Array) Uint(i uint) *Array
```

Uint appends i as a uint to the array.

## Uint16

```go
func (a *Array) Uint16(i uint16) *Array
```

Uint16 appends i as a uint16 to the array.

## Uint32

```go
func (a *Array) Uint32(i uint32) *Array
```

Uint32 appends i as a uint32 to the array.

## Uint64

```go
func (a *Array) Uint64(i uint64) *Array
```

Uint64 appends i as a uint64 to the array.

## Uint8

```go
func (a *Array) Uint8(i uint8) *Array
```

Uint8 appends i as a uint8 to the array.

## BasicSampler

```go
type BasicSampler struct {
	N uint32
	// contains filtered or unexported fields
}
```

BasicSampler is a sampler that will send every Nth event, regardless of their level.

- `N: uint32`: 

## Sample

```go
func (s *BasicSampler) Sample(lvl Level) bool
```

Sample implements the Sampler interface.

## BurstSampler

```go
type BurstSampler struct {
	// Burst is the maximum number of event per period allowed before calling
	// NextSampler.
	Burst uint32
	// Period defines the burst period. If 0, NextSampler is always called.
	Period time.Duration
	// NextSampler is the sampler used after the burst is reached. If nil,
	// events are always rejected after the burst.
	NextSampler Sampler
	// contains filtered or unexported fields
}
```

BurstSampler lets Burst events pass per Period then pass the decision to NextSampler. If NextSampler is not set, all subsequent events are rejected.

- `Burst: uint32`: Burst is the maximum number of event per period allowed before calling NextSampler.
- `Period: time.Duration`: Period defines the burst period. If 0, NextSampler is always called.
- `NextSampler: Sampler`: NextSampler is the sampler used after the burst is reached. If nil, events are always rejected after the burst.

## Sample

```go
func (s *BurstSampler) Sample(lvl Level) bool
```

Sample implements the Sampler interface.

## ConsoleWriter

```go
type ConsoleWriter struct {
	// Out is the output destination.
	Out io.Writer

	// NoColor disables the colorized output.
	NoColor bool

	// TimeFormat specifies the format for timestamp in output.
	TimeFormat string

	// TimeLocation tells ConsoleWriter’s default FormatTimestamp
	// how to localize the time.
	TimeLocation *time.Location

	// PartsOrder defines the order of parts in output.
	PartsOrder []string

	// PartsExclude defines parts to not display in output.
	PartsExclude []string

	// FieldsOrder defines the order of contextual fields in output.
	FieldsOrder []string

	// FieldsExclude defines contextual fields to not display in output.
	FieldsExclude []string

	FormatTimestamp     Formatter
	FormatLevel         Formatter
	FormatCaller        Formatter
	FormatMessage       Formatter
	FormatFieldName     Formatter
	FormatFieldValue    Formatter
	FormatErrFieldName  Formatter
	FormatErrFieldValue Formatter
	// If this is configured it is used for "part" values and
	// has precedence on FormatFieldValue
	FormatPartValueByName FormatterByFieldName

	FormatExtra func(map[string]interface{}, *bytes.Buffer) error

	FormatPrepare func(map[string]interface{}) error
	// contains filtered or unexported fields
}
```

ConsoleWriter parses the JSON input and writes it in an (optionally) colorized, human-friendly format to Out.

- `Out: io.Writer`: Out is the output destination.
- `NoColor: bool`: NoColor disables the colorized output.
- `TimeFormat: string`: TimeFormat specifies the format for timestamp in output.
- `TimeLocation: *time.Location`: TimeLocation tells ConsoleWriter’s default FormatTimestamp how to localize the time.
- `PartsOrder: []string`: PartsOrder defines the order of parts in output.
- `PartsExclude: []string`: PartsExclude defines parts to not display in output.
- `FieldsOrder: []string`: FieldsOrder defines the order of contextual fields in output.
- `FieldsExclude: []string`: FieldsExclude defines contextual fields to not display in output.
- `FormatTimestamp: Formatter`: 
- `FormatLevel: Formatter`: 
- `FormatCaller: Formatter`: 
- `FormatMessage: Formatter`: 
- `FormatFieldName: Formatter`: 
- `FormatFieldValue: Formatter`: 
- `FormatErrFieldName: Formatter`: 
- `FormatErrFieldValue: Formatter`: 
- `FormatPartValueByName: FormatterByFieldName`: If this is configured it is used for "part" values and has precedence on FormatFieldValue
- `FormatExtra: func(map[string]interface{}, *bytes.Buffer) error`: 
- `FormatPrepare: func(map[string]interface{}) error`: 

## Close

```go
func (w ConsoleWriter) Close() error
```

Call the underlying writer's Close method if it is an io.Closer. Otherwise does nothing.

## Write

```go
func (w ConsoleWriter) Write(p []byte) (n int, err error)
```

Write transforms the JSON input with formatters and appends to w.Out.

### Example: ConsoleWriter

```go
{
	log := zerolog.New(zerolog.ConsoleWriter{Out: os.Stdout, NoColor: true})

	log.Info().Str("foo", "bar").Msg("Hello World")

}
```

Output:

```text
<nil> INF Hello World foo=bar
```

## Context

```go
type Context struct {
	// contains filtered or unexported fields
}
```

Context configures a new sub-logger with contextual fields.

## AnErr

```go
func (c Context) AnErr(key string, err error) Context
```

AnErr adds the field key with serialized err to the logger context. If err is nil, no field is added.

## Any

```go
func (c Context) Any(key string, i interface{}) Context
```

Any is a wrapper around Context.Interface.

## Array

```go
func (c Context) Array(key string, arr LogArrayMarshaler) Context
```

Array adds the field key with an array to the event context. Use c.CreateArray() to create the array or pass a type that implement the LogArrayMarshaler interface.

### Example: Context_Array

```go
{
	ctx := zerolog.New(os.Stdout).With().
		Str("foo", "bar")

	logger := ctx.Array("array", ctx.CreateArray().
		Str("baz").
		Int(1),
	).Logger()

	logger.Log().Msg("hello world")

}
```

Output:

```text
{"foo":"bar","array":["baz",1],"message":"hello world"}
```

## Bool

```go
func (c Context) Bool(key string, b bool) Context
```

Bool adds the field key with val as a bool to the logger context.

## Bools

```go
func (c Context) Bools(key string, b []bool) Context
```

Bools adds the field key with val as a \[]bool to the logger context.

## Bytes

```go
func (c Context) Bytes(key string, val []byte) Context
```

Bytes adds the field key with val as a \[]byte to the logger context.

## Caller

```go
func (c Context) Caller() Context
```

Caller adds the file:line of the caller with the zerolog.CallerFieldName key.

## CallerWithSkipFrameCount

```go
func (c Context) CallerWithSkipFrameCount(skipFrameCount int) Context
```

CallerWithSkipFrameCount adds the file:line of the caller with the zerolog.CallerFieldName key. The specified skipFrameCount int will override the global CallerSkipFrameCount for this context's respective logger. If set to -1 the global CallerSkipFrameCount will be used.

## CreateArray

```go
func (c Context) CreateArray() *Array
```

CreateArray creates an Array to be used with the Context.Array method. It preserves the stack, hooks, and context from the logger. Call usual field methods like Str, Int etc to add elements to this array and give it as argument the Context.Array method.

## CreateDict

```go
func (c Context) CreateDict() *Event
```

CreateDict creates an Event to be used with the Context.Dict method. It preserves the stack, hooks, and context from the logger. Call usual field methods like Str, Int etc to add fields to this event and give it as argument the Context.Dict method.

## Ctx

```go
func (c Context) Ctx(ctx context.Context) Context
```

Ctx adds the context.Context to the logger context. The context.Context is not rendered in the error message, but is made available for hooks to use. A typical use case is to extract tracing information from the context.Context.

## Dict

```go
func (c Context) Dict(key string, dict *Event) Context
```

Dict adds the field key with the dict to the logger context.

### Example: Context_Dict

```go
{
	ctx := zerolog.New(os.Stdout).With().
		Str("foo", "bar")

	logger := ctx.Dict("dict", ctx.CreateDict().
		Str("bar", "baz").
		Int("n", 1),
	).Logger()

	logger.Log().Msg("hello world")

}
```

Output:

```text
{"foo":"bar","dict":{"bar":"baz","n":1},"message":"hello world"}
```

## Dur

```go
func (c Context) Dur(key string, d time.Duration) Context
```

Dur adds the field key with d divided by unit and stored as a float.

### Example: Context_Dur

```go
{
	d := 10 * time.Second

	log := zerolog.New(os.Stdout).With().
		Str("foo", "bar").
		Dur("dur", d).
		Logger()

	log.Log().Msg("hello world")

}
```

Output:

```text
{"foo":"bar","dur":10000,"message":"hello world"}
```

## Durs

```go
func (c Context) Durs(key string, d []time.Duration) Context
```

Durs adds the field key with d divided by unit and stored as a float.

### Example: Context_Durs

```go
{
	d := []time.Duration{
		10 * time.Second,
		20 * time.Second,
	}

	log := zerolog.New(os.Stdout).With().
		Str("foo", "bar").
		Durs("durs", d).
		Logger()

	log.Log().Msg("hello world")

}
```

Output:

```text
{"foo":"bar","durs":[10000,20000],"message":"hello world"}
```

## EmbedObject

```go
func (c Context) EmbedObject(obj LogObjectMarshaler) Context
```

EmbedObject marshals and Embeds an object that implement the LogObjectMarshaler interface.

### Example: Context_EmbedObject

```go
{
	price := Price{val: 6449, prec: 2, unit: "$"}

	log := zerolog.New(os.Stdout).With().
		Str("foo", "bar").
		EmbedObject(price).
		Logger()

	log.Log().Msg("hello world")

}
```

Output:

```text
{"foo":"bar","price":"$64.49","message":"hello world"}
```

## Err

```go
func (c Context) Err(err error) Context
```

Err adds the field "error" with serialized err to the logger context.

## Errs

```go
func (c Context) Errs(key string, errs []error) Context
```

Errs adds the field key with errs as an array of serialized errors to the logger context.

## Fields

```go
func (c Context) Fields(fields interface{}) Context
```

Fields is a helper function to use a map or slice to set fields using type assertion. Only map\[string]interface{} and \[]interface{} are accepted. \[]interface{} must alternate string keys and arbitrary values, and extraneous ones are ignored.

## Float32

```go
func (c Context) Float32(key string, f float32) Context
```

Float32 adds the field key with f as a float32 to the logger context.

## Float64

```go
func (c Context) Float64(key string, f float64) Context
```

Float64 adds the field key with f as a float64 to the logger context.

## Floats32

```go
func (c Context) Floats32(key string, f []float32) Context
```

Floats32 adds the field key with f as a \[]float32 to the logger context.

## Floats64

```go
func (c Context) Floats64(key string, f []float64) Context
```

Floats64 adds the field key with f as a \[]float64 to the logger context.

## Hex

```go
func (c Context) Hex(key string, val []byte) Context
```

Hex adds the field key with val as a hex string to the logger context.

## IPAddr

```go
func (c Context) IPAddr(key string, ip net.IP) Context
```

IPAddr adds the field key with ip as a net.IP IPv4 or IPv6 Address to the context

### Example: Context_IPAddr

```go
{
	hostIP := net.IP{192, 168, 0, 100}
	log := zerolog.New(os.Stdout).With().
		IPAddr("HostIP", hostIP).
		Logger()

	log.Log().Msg("hello world")

}
```

Output:

```text
{"HostIP":"192.168.0.100","message":"hello world"}
```

## IPAddrs

```go
func (c Context) IPAddrs(key string, ip []net.IP) Context
```

IPAddrs adds the field key with ip as a \[]net.IP array of IPv4 or IPv6 Address to the context

### Example: Context_IPAddrs

```go
{
	hostIP := net.IP{192, 168, 0, 100}
	log := zerolog.New(os.Stdout).With().
		IPAddrs("HostIP", []net.IP{hostIP}).
		Logger()

	log.Log().Msg("hello world")

}
```

Output:

```text
{"HostIP":["192.168.0.100"],"message":"hello world"}
```

## IPPrefix

```go
func (c Context) IPPrefix(key string, pfx net.IPNet) Context
```

IPPrefix adds the field key with pfx as a \[]net.IPNet IPv4 or IPv6 Prefix (address and mask) to the context

### Example: Context_IPPrefix

```go
{
	route := net.IPNet{IP: net.IP{192, 168, 0, 0}, Mask: net.CIDRMask(24, 32)}
	log := zerolog.New(os.Stdout).With().
		IPPrefix("Route", route).
		Logger()

	log.Log().Msg("hello world")

}
```

Output:

```text
{"Route":"192.168.0.0/24","message":"hello world"}
```

## IPPrefixes

```go
func (c Context) IPPrefixes(key string, pfx []net.IPNet) Context
```

IPPrefix adds the field key with pfx as a \[]net.IPNet array of IPv4 or IPv6 Prefix (address and mask) to the context

### Example: Context_IPPrefixes

```go
{
	route := net.IPNet{IP: net.IP{192, 168, 0, 0}, Mask: net.CIDRMask(24, 32)}
	log := zerolog.New(os.Stdout).With().
		IPPrefixes("Route", []net.IPNet{route}).
		Logger()

	log.Log().Msg("hello world")

}
```

Output:

```text
{"Route":["192.168.0.0/24"],"message":"hello world"}
```

## Int

```go
func (c Context) Int(key string, i int) Context
```

Int adds the field key with i as a int to the logger context.

## Int16

```go
func (c Context) Int16(key string, i int16) Context
```

Int16 adds the field key with i as a int16 to the logger context.

## Int32

```go
func (c Context) Int32(key string, i int32) Context
```

Int32 adds the field key with i as a int32 to the logger context.

## Int64

```go
func (c Context) Int64(key string, i int64) Context
```

Int64 adds the field key with i as a int64 to the logger context.

## Int8

```go
func (c Context) Int8(key string, i int8) Context
```

Int8 adds the field key with i as a int8 to the logger context.

## Interface

```go
func (c Context) Interface(key string, i interface{}) Context
```

Interface adds the field key with obj marshaled using reflection.

### Example: Context_Interface

```go
{
	obj := struct {
		Name string `json:"name"`
	}{
		Name: "john",
	}

	log := zerolog.New(os.Stdout).With().
		Str("foo", "bar").
		Interface("obj", obj).
		Logger()

	log.Log().Msg("hello world")

}
```

Output:

```text
{"foo":"bar","obj":{"name":"john"},"message":"hello world"}
```

## Ints

```go
func (c Context) Ints(key string, i []int) Context
```

Ints adds the field key with i as a \[]int to the logger context.

## Ints16

```go
func (c Context) Ints16(key string, i []int16) Context
```

Ints16 adds the field key with i as a \[]int16 to the logger context.

## Ints32

```go
func (c Context) Ints32(key string, i []int32) Context
```

Ints32 adds the field key with i as a \[]int32 to the logger context.

## Ints64

```go
func (c Context) Ints64(key string, i []int64) Context
```

Ints64 adds the field key with i as a \[]int64 to the logger context.

## Ints8

```go
func (c Context) Ints8(key string, i []int8) Context
```

Ints8 adds the field key with i as a \[]int8 to the logger context.

## Logger

```go
func (c Context) Logger() Logger
```

Logger returns the logger with the context previously set.

## MACAddr

```go
func (c Context) MACAddr(key string, ha net.HardwareAddr) Context
```

MACAddr adds the field key with ha as a net.HardwareAddr MAC address to the context

### Example: Context_MACAddr

```go
{
	mac := net.HardwareAddr{0x00, 0x14, 0x22, 0x01, 0x23, 0x45}
	log := zerolog.New(os.Stdout).With().
		MACAddr("hostMAC", mac).
		Logger()

	log.Log().Msg("hello world")

}
```

Output:

```text
{"hostMAC":"00:14:22:01:23:45","message":"hello world"}
```

## Object

```go
func (c Context) Object(key string, obj LogObjectMarshaler) Context
```

Object marshals an object that implement the LogObjectMarshaler interface.

### Example: Context_Object

```go
{

	u := User{"John", 35, time.Time{}}

	log := zerolog.New(os.Stdout).With().
		Str("foo", "bar").
		Object("user", u).
		Logger()

	log.Log().Msg("hello world")

}
```

Output:

```text
{"foo":"bar","user":{"name":"John","age":35,"created":"0001-01-01T00:00:00Z"},"message":"hello world"}
```

## Objects

```go
func (c Context) Objects(key string, objs []LogObjectMarshaler) Context
```

Objects adds the field key with objs to the logger context as an array of objects that implement the LogObjectMarshaler interface.

This is the array version that accepts a slice of LogObjectMarshaler objects.

### Example: Context_Objects

```go
{

	u := User{"John", 35, time.Time{}}
	u2 := User{"Bono", 54, time.Time{}}

	log := zerolog.New(os.Stdout).With().
		Str("foo", "bar").
		Objects("users", []zerolog.LogObjectMarshaler{u, u2}).
		Logger()

	log.Log().Msg("hello world")

}
```

Output:

```text
{"foo":"bar","users":[{"name":"John","age":35,"created":"0001-01-01T00:00:00Z"},{"name":"Bono","age":54,"created":"0001-01-01T00:00:00Z"}],"message":"hello world"}
```

## ObjectsV

```go
func (c Context) ObjectsV(key string, objs ...LogObjectMarshaler) Context
```

ObjectsV adds the field key with objs to the logger context as an array of objects that implement the LogObjectMarshaler interface.

This is a variadic version that accepts a list of individual LogObjectMarshaler objects.

### Example: Context_ObjectsV

```go
{

	u := User{"John", 35, time.Time{}}
	u2 := User{"Bono", 54, time.Time{}}

	log := zerolog.New(os.Stdout).With().
		Str("foo", "bar").
		ObjectsV("users", u, u2).
		Logger()

	log.Log().Msg("hello world")

}
```

Output:

```text
{"foo":"bar","users":[{"name":"John","age":35,"created":"0001-01-01T00:00:00Z"},{"name":"Bono","age":54,"created":"0001-01-01T00:00:00Z"}],"message":"hello world"}
```

## RawJSON

```go
func (c Context) RawJSON(key string, b []byte) Context
```

RawJSON adds already encoded JSON to context.

No sanity check is performed on b; it must not contain carriage returns and be valid JSON.

## Reset

```go
func (c Context) Reset() Context
```

Reset removes all the context fields.

## Stack

```go
func (c Context) Stack() Context
```

Stack enables stack trace printing for the error passed to Err().

## Str

```go
func (c Context) Str(key, val string) Context
```

Str adds the field key with val as a string to the logger context.

## Stringer

```go
func (c Context) Stringer(key string, val fmt.Stringer) Context
```

Stringer adds the field key with val.String() (or null if val is nil) to the logger context.

## Stringers

```go
func (c Context) Stringers(key string, vals []fmt.Stringer) Context
```

Stringers adds the field key with vals to the logger context where each individual val is added by calling val.String().

This is the array version that accepts a slice of fmt.Stringer values.

## StringersV

```go
func (c Context) StringersV(key string, vals ...fmt.Stringer) Context
```

StringersV adds the field key with vals  to the logger context where each individual val is added by calling val.String().

This is a variadic version that accepts a list of individual fmt.Stringer values.

### Example: Context_StringersV

```go
{

	a := net.IPv4bcast
	b := net.IPv4allrouter

	log := zerolog.New(os.Stdout).With().
		StringersV("ips", a, b).
		Logger()

	log.Log().Msg("hello world")

}
```

Output:

```text
{"ips":["255.255.255.255","224.0.0.2"],"message":"hello world"}
```

## Strs

```go
func (c Context) Strs(key string, vals []string) Context
```

Strs adds the field key with val as a string to the logger context.

This is the array version that accepts a slice of string values.

## StrsV

```go
func (c Context) StrsV(key string, vals ...string) Context
```

StrsV adds the field key with vals as a \[]string to the logger context.

This is a variadic version that accepts a list of individual strings.

## Time

```go
func (c Context) Time(key string, t time.Time) Context
```

Time adds the field key with t formatted as string using zerolog.TimeFieldFormat.

## Times

```go
func (c Context) Times(key string, t []time.Time) Context
```

Times adds the field key with t formatted as string using zerolog.TimeFieldFormat.

### Example: Context_Times

```go
{
	t1 := time.Time{}
	t2 := t1.Add(time.Second * 10)
	t := []time.Time{t1, t2}

	log := zerolog.New(os.Stdout).With().
		Str("foo", "bar").
		Times("times", t).
		Logger()

	log.Log().Msg("hello world")

}
```

Output:

```text
{"foo":"bar","times":["0001-01-01T00:00:00Z","0001-01-01T00:00:10Z"],"message":"hello world"}
```

## Timestamp

```go
func (c Context) Timestamp() Context
```

Timestamp adds the current local time to the logger context with the "time" key, formatted using zerolog.TimeFieldFormat. To customize the key name, change zerolog.TimestampFieldName. To customize the time format, change zerolog.TimeFieldFormat.

NOTE: It won't dedupe the "time" key if the \*Context has one already.

## Type

```go
func (c Context) Type(key string, val interface{}) Context
```

Type adds the field key with val's type using reflection.

## Uint

```go
func (c Context) Uint(key string, i uint) Context
```

Uint adds the field key with i as a uint to the logger context.

## Uint16

```go
func (c Context) Uint16(key string, i uint16) Context
```

Uint16 adds the field key with i as a uint16 to the logger context.

## Uint32

```go
func (c Context) Uint32(key string, i uint32) Context
```

Uint32 adds the field key with i as a uint32 to the logger context.

## Uint64

```go
func (c Context) Uint64(key string, i uint64) Context
```

Uint64 adds the field key with i as a uint64 to the logger context.

## Uint8

```go
func (c Context) Uint8(key string, i uint8) Context
```

Uint8 adds the field key with i as a uint8 to the logger context.

## Uints

```go
func (c Context) Uints(key string, i []uint) Context
```

Uints adds the field key with i as a \[]uint to the logger context.

## Uints16

```go
func (c Context) Uints16(key string, i []uint16) Context
```

Uints16 adds the field key with i as a \[]uint16 to the logger context.

## Uints32

```go
func (c Context) Uints32(key string, i []uint32) Context
```

Uints32 adds the field key with i as a \[]uint32 to the logger context.

## Uints64

```go
func (c Context) Uints64(key string, i []uint64) Context
```

Uints64 adds the field key with i as a \[]uint64 to the logger context.

## Uints8

```go
func (c Context) Uints8(key string, i []uint8) Context
```

Uints8 adds the field key with i as a \[]uint8 to the logger context.

## Event

```go
type Event struct {
	// contains filtered or unexported fields
}
```

Event represents a log event. It is instanced by one of the level method of Logger and finalized by the Msg or Msgf method.

## AnErr

```go
func (e *Event) AnErr(key string, err error) *Event
```

AnErr adds the field key with serialized err to the \*Event context. If err is nil, no field is added.

## Any

```go
func (e *Event) Any(key string, i interface{}) *Event
```

Any is a wrapper around Event.Interface.

## Array

```go
func (e *Event) Array(key string, arr LogArrayMarshaler) *Event
```

Array adds the field key with an array to the event context. Use e.CreateArray() to create the array or pass a type that implement the LogArrayMarshaler interface.

### Example: Event_Array

```go
{
	log := zerolog.New(os.Stdout)

	e := log.Log().
		Str("foo", "bar")

	e.Array("array", e.CreateArray().
		Str("baz").
		Int(1).
		Dict(e.CreateDict().
			Str("bar", "baz").
			Int("n", 1),
		),
	).
		Msg("hello world")

}
```

Output:

```text
{"foo":"bar","array":["baz",1,{"bar":"baz","n":1}],"message":"hello world"}
```

## Bool

```go
func (e *Event) Bool(key string, b bool) *Event
```

Bool adds the field key with val as a bool to the \*Event context.

## Bools

```go
func (e *Event) Bools(key string, b []bool) *Event
```

Bools adds the field key with val as a \[]bool to the \*Event context.

## Bytes

```go
func (e *Event) Bytes(key string, val []byte) *Event
```

Bytes adds the field key with val as a string to the \*Event context.

Runes outside of normal ASCII ranges will be hex-encoded in the resulting JSON.

## Caller

```go
func (e *Event) Caller(skip ...int) *Event
```

Caller adds the file:line of the caller with the zerolog.CallerFieldName key. The argument skip is the number of stack frames to ascend Skip If not passed, use the global variable CallerSkipFrameCount

## CallerSkipFrame

```go
func (e *Event) CallerSkipFrame(skip int) *Event
```

CallerSkipFrame instructs any future Caller calls to skip the specified number of frames. This includes those added via hooks from the context.

## CreateArray

```go
func (e *Event) CreateArray() *Array
```

CreateArray creates an Array to be used with the \*Event.Array method. It preserves the stack, hooks, and context from the parent event. Call usual field methods like Str, Int etc to add elements to this array and give it as argument the \*Event.Array method.

## CreateDict

```go
func (e *Event) CreateDict() *Event
```

CreateDict creates an Event to be used with the \*Event.Dict method. It preserves the stack, hooks, and context from the parent event. Call usual field methods like Str, Int etc to add fields to this event and give it as argument the \*Event.Dict method.

## Ctx

```go
func (e *Event) Ctx(ctx context.Context) *Event
```

Ctx adds the Go Context to the \*Event context.  The context is not rendered in the output message, but is available to hooks and to Func() calls via the GetCtx() accessor. A typical use case is to extract tracing information from the Go Ctx.

## Dict

```go
func (e *Event) Dict(key string, dict *Event) *Event
```

Dict adds the field key with a dict to the event context. Use e.CreateDict() to create the dictionary.

### Example: Event_Dict

```go
{
	log := zerolog.New(os.Stdout)

	e := log.Log().
		Str("foo", "bar")

	e.Dict("dict", e.CreateDict().
		Str("bar", "baz").
		Int("n", 1),
	).
		Msg("hello world")

}
```

Output:

```text
{"foo":"bar","dict":{"bar":"baz","n":1},"message":"hello world"}
```

## Discard

```go
func (e *Event) Discard() *Event
```

Discard disables the event so Msg(f) won't print it.

## Dur

```go
func (e *Event) Dur(key string, d time.Duration) *Event
```

Dur adds the field key with duration d stored as zerolog.DurationFieldUnit. If zerolog.DurationFieldInteger is true, durations are rendered as integer instead of float.

### Example: Event_Dur

```go
{
	d := 10 * time.Second

	log := zerolog.New(os.Stdout)

	log.Log().
		Str("foo", "bar").
		Dur("dur", d).
		Msg("hello world")

}
```

Output:

```text
{"foo":"bar","dur":10000,"message":"hello world"}
```

## Durs

```go
func (e *Event) Durs(key string, d []time.Duration) *Event
```

Durs adds the field key with duration d stored as zerolog.DurationFieldUnit. If zerolog.DurationFieldInteger is true, durations are rendered as integer instead of float.

### Example: Event_Durs

```go
{
	d := []time.Duration{
		10 * time.Second,
		20 * time.Second,
	}

	log := zerolog.New(os.Stdout)

	log.Log().
		Str("foo", "bar").
		Durs("durs", d).
		Msg("hello world")

}
```

Output:

```text
{"foo":"bar","durs":[10000,20000],"message":"hello world"}
```

## EmbedObject

```go
func (e *Event) EmbedObject(obj LogObjectMarshaler) *Event
```

EmbedObject marshals an object that implement the LogObjectMarshaler interface.

### Example: Event_EmbedObject

```go
{
	log := zerolog.New(os.Stdout)

	price := Price{val: 6449, prec: 2, unit: "$"}

	log.Log().
		Str("foo", "bar").
		EmbedObject(price).
		Msg("hello world")

}
```

Output:

```text
{"foo":"bar","price":"$64.49","message":"hello world"}
```

## Enabled

```go
func (e *Event) Enabled() bool
```

Enabled return false if the \*Event is going to be filtered out by log level or sampling.

## Err

```go
func (e *Event) Err(err error) *Event
```

Err adds the field "error" with serialized err to the \*Event context. If err is nil, no field is added.

To customize the key name, change zerolog.ErrorFieldName.

If Stack() has been called before and zerolog.ErrorStackMarshaler is defined, the err is passed to ErrorStackMarshaler and the result is appended to the zerolog.ErrorStackFieldName.

## Errs

```go
func (e *Event) Errs(key string, errs []error) *Event
```

Errs adds the field key with errs as an array of serialized errors to the \*Event context.

## Fields

```go
func (e *Event) Fields(fields interface{}) *Event
```

Fields is a helper function to use a map or slice to set fields using type assertion. Only map\[string]interface{} and \[]interface{} are accepted. \[]interface{} must alternate string keys and arbitrary values, and extraneous ones are ignored.

## Float32

```go
func (e *Event) Float32(key string, f float32) *Event
```

Float32 adds the field key with f as a float32 to the \*Event context.

## Float64

```go
func (e *Event) Float64(key string, f float64) *Event
```

Float64 adds the field key with f as a float64 to the \*Event context.

## Floats32

```go
func (e *Event) Floats32(key string, f []float32) *Event
```

Floats32 adds the field key with f as a \[]float32 to the \*Event context.

## Floats64

```go
func (e *Event) Floats64(key string, f []float64) *Event
```

Floats64 adds the field key with f as a \[]float64 to the \*Event context.

## Func

```go
func (e *Event) Func(f func(e *Event)) *Event
```

Func allows an anonymous func to run only if the event is enabled.

## GetCtx

```go
func (e *Event) GetCtx() context.Context
```

GetCtx retrieves the Go context.Context which is optionally stored in the Event. This allows Hooks and functions passed to Func() to retrieve values which are stored in the context.Context. This can be useful in tracing, where span information is commonly propagated in the context.Context.

## Hex

```go
func (e *Event) Hex(key string, val []byte) *Event
```

Hex adds the field key with val as a hex string to the \*Event context.

## IPAddr

```go
func (e *Event) IPAddr(key string, ip net.IP) *Event
```

IPAddr adds the field key with ip as a net.IP IPv4 or IPv6 Address to the event

## IPAddrs

```go
func (e *Event) IPAddrs(key string, ip []net.IP) *Event
```

IPAddrs adds the field key with ip as a net.IP array of IPv4 or IPv6 Address to the event

## IPPrefix

```go
func (e *Event) IPPrefix(key string, pfx net.IPNet) *Event
```

IPPrefix adds the field key with pfx as a net.IPNet IPv4 or IPv6 Prefix (address and mask) to the event

## IPPrefixes

```go
func (e *Event) IPPrefixes(key string, pfx []net.IPNet) *Event
```

IPPrefixes the field key with pfx as a net.IPNet array of IPv4 or IPv6 Prefixes (address and mask) to the event

## Int

```go
func (e *Event) Int(key string, i int) *Event
```

Int adds the field key with i as a int to the \*Event context.

## Int16

```go
func (e *Event) Int16(key string, i int16) *Event
```

Int16 adds the field key with i as a int16 to the \*Event context.

## Int32

```go
func (e *Event) Int32(key string, i int32) *Event
```

Int32 adds the field key with i as a int32 to the \*Event context.

## Int64

```go
func (e *Event) Int64(key string, i int64) *Event
```

Int64 adds the field key with i as a int64 to the \*Event context.

## Int8

```go
func (e *Event) Int8(key string, i int8) *Event
```

Int8 adds the field key with i as a int8 to the \*Event context.

## Interface

```go
func (e *Event) Interface(key string, i interface{}) *Event
```

Interface adds the field key with i marshaled using reflection.

### Example: Event_Interface

```go
{
	log := zerolog.New(os.Stdout)

	obj := struct {
		Name string `json:"name"`
	}{
		Name: "john",
	}

	log.Log().
		Str("foo", "bar").
		Interface("obj", obj).
		Msg("hello world")

}
```

Output:

```text
{"foo":"bar","obj":{"name":"john"},"message":"hello world"}
```

## Ints

```go
func (e *Event) Ints(key string, i []int) *Event
```

Ints adds the field key with i as a \[]int to the \*Event context.

## Ints16

```go
func (e *Event) Ints16(key string, i []int16) *Event
```

Ints16 adds the field key with i as a \[]int16 to the \*Event context.

## Ints32

```go
func (e *Event) Ints32(key string, i []int32) *Event
```

Ints32 adds the field key with i as a \[]int32 to the \*Event context.

## Ints64

```go
func (e *Event) Ints64(key string, i []int64) *Event
```

Ints64 adds the field key with i as a \[]int64 to the \*Event context.

## Ints8

```go
func (e *Event) Ints8(key string, i []int8) *Event
```

Ints8 adds the field key with i as a \[]int8 to the \*Event context.

## MACAddr

```go
func (e *Event) MACAddr(key string, ha net.HardwareAddr) *Event
```

MACAddr the field key with ha as a net.HardwareAddr MAC address to the event

## Msg

```go
func (e *Event) Msg(msg string)
```

Msg sends the \*Event with msg added as the message field if not empty.

NOTICE: once this method is called, the \*Event should be disposed. Calling Msg twice can have unexpected result.

## MsgFunc

```go
func (e *Event) MsgFunc(createMsg func() string)
```

## Msgf

```go
func (e *Event) Msgf(format string, v ...interface{})
```

Msgf sends the event with formatted msg added as the message field if not empty.

NOTICE: once this method is called, the \*Event should be disposed. Calling Msgf twice can have unexpected result.

## Object

```go
func (e *Event) Object(key string, obj LogObjectMarshaler) *Event
```

Object marshals an object that implement the LogObjectMarshaler interface.

### Example: Event_Object

```go
{
	log := zerolog.New(os.Stdout)

	u := User{"John", 35, time.Time{}}

	log.Log().
		Str("foo", "bar").
		Object("user", u).
		Msg("hello world")

}
```

Output:

```text
{"foo":"bar","user":{"name":"John","age":35,"created":"0001-01-01T00:00:00Z"},"message":"hello world"}
```

## Objects

```go
func (e *Event) Objects(key string, objs []LogObjectMarshaler) *Event
```

Objects adds the field key with objs as an array of objects that implement the LogObjectMarshaler interface to the event.

This is the array version that accepts a slice of LogObjectMarshaler objects.

### Example: Event_Objects

```go
{
	log := zerolog.New(os.Stdout)

	u := User{"John", 35, time.Time{}}
	u2 := User{"Bono", 54, time.Time{}}
	users := []User{u, u2}

	log.Log().
		Objects("users", zerolog.AsLogObjectMarshalers(users)).
		Msg("hello world")

}
```

Output:

```text
{"users":[{"name":"John","age":35,"created":"0001-01-01T00:00:00Z"},{"name":"Bono","age":54,"created":"0001-01-01T00:00:00Z"}],"message":"hello world"}
```

## ObjectsV

```go
func (e *Event) ObjectsV(key string, objs ...LogObjectMarshaler) *Event
```

ObjectsV adds the field key with objs as an array of objects that implement the LogObjectMarshaler interface to the event.

This is a variadic version that accepts a list of individual LogObjectMarshaler objects.

### Example: Event_ObjectsV

```go
{
	log := zerolog.New(os.Stdout)

	u := User{"John", 35, time.Time{}}
	u2 := User{"Bono", 54, time.Time{}}

	log.Log().
		ObjectsV("users", u, u2).
		Msg("hello world")

}
```

Output:

```text
{"users":[{"name":"John","age":35,"created":"0001-01-01T00:00:00Z"},{"name":"Bono","age":54,"created":"0001-01-01T00:00:00Z"}],"message":"hello world"}
```

## RawCBOR

```go
func (e *Event) RawCBOR(key string, b []byte) *Event
```

RawCBOR adds already encoded CBOR to the log line under key.

No sanity check is performed on b Note: The full featureset of CBOR is supported as data will not be mapped to json but stored as data-url

## RawJSON

```go
func (e *Event) RawJSON(key string, b []byte) *Event
```

RawJSON adds already encoded JSON to the log line under key.

No sanity check is performed on b; it must not contain carriage returns and be valid JSON.

## Send

```go
func (e *Event) Send()
```

Send is equivalent to calling Msg("").

NOTICE: once this method is called, the \*Event should be disposed.

## Stack

```go
func (e *Event) Stack() *Event
```

Stack enables stack trace printing for the error passed to Err().

ErrorStackMarshaler must be set for this method to do something.

## Str

```go
func (e *Event) Str(key, val string) *Event
```

Str adds the field key with val as a string to the \*Event context.

## Stringer

```go
func (e *Event) Stringer(key string, val fmt.Stringer) *Event
```

Stringer adds the field key and a val to the \*Event context. If val is not nil, it is added by calling val.String(). If val is nil, it is encoded as null without calling String().

## Stringers

```go
func (e *Event) Stringers(key string, vals []fmt.Stringer) *Event
```

Stringers adds the field key with vals to the \*Event context. If a val is not nil, it is added by calling val.String(). If a val is nil, it is encoded as null without calling String().

This is the array version that accepts a slice of fmt.Stringer values.

### Example: Event_Stringers

```go
{
	log := zerolog.New(os.Stdout)

	a := net.IP{127, 0, 0, 1}
	b := net.IP{127, 0, 0, 2}
	ips := []net.IP{a, b}

	log.Log().
		Stringers("ips", zerolog.AsStringers(ips)).
		Msg("hello world")

}
```

Output:

```text
{"ips":["127.0.0.1","127.0.0.2"],"message":"hello world"}
```

## StringersV

```go
func (e *Event) StringersV(key string, vals ...fmt.Stringer) *Event
```

StringersV adds the field key with vals to the \*Event context. If a val is not nil, it is added by calling val.String(). If a val is nil, it is encoded as null without calling String().

This is a variadic version that accepts a list of individual fmt.Stringer values.

## Strs

```go
func (e *Event) Strs(key string, vals []string) *Event
```

Strs adds the field key with vals as a \[]string to the \*Event context.

This is the array version that accepts a slice of string values.

## StrsV

```go
func (e *Event) StrsV(key string, vals ...string) *Event
```

StrsV adds the field key with vals as a \[]string to the \*Event context.

This is a variadic version that accepts a list of individual strings.

## Time

```go
func (e *Event) Time(key string, t time.Time) *Event
```

Time adds the field key with t formatted as string using zerolog.TimeFieldFormat.

## TimeDiff

```go
func (e *Event) TimeDiff(key string, t time.Time, start time.Time) *Event
```

TimeDiff adds the field key with positive duration between time t and start. If time t is not greater than start, duration will be 0. Duration format follows the same principle as Dur().

## Times

```go
func (e *Event) Times(key string, t []time.Time) *Event
```

Times adds the field key with t formatted as string using zerolog.TimeFieldFormat.

## Timestamp

```go
func (e *Event) Timestamp() *Event
```

Timestamp adds the current local time as UNIX timestamp to the \*Event context with the "time" key. To customize the key name, change zerolog.TimestampFieldName.

NOTE: It won't dedupe the "time" key if the \*Event (or \*Context) has one already.

## Type

```go
func (e *Event) Type(key string, val interface{}) *Event
```

Type adds the field key with val's type using reflection.

## Uint

```go
func (e *Event) Uint(key string, i uint) *Event
```

Uint adds the field key with i as a uint to the \*Event context.

## Uint16

```go
func (e *Event) Uint16(key string, i uint16) *Event
```

Uint16 adds the field key with i as a uint16 to the \*Event context.

## Uint32

```go
func (e *Event) Uint32(key string, i uint32) *Event
```

Uint32 adds the field key with i as a uint32 to the \*Event context.

## Uint64

```go
func (e *Event) Uint64(key string, i uint64) *Event
```

Uint64 adds the field key with i as a uint64 to the \*Event context.

## Uint8

```go
func (e *Event) Uint8(key string, i uint8) *Event
```

Uint8 adds the field key with i as a uint8 to the \*Event context.

## Uints

```go
func (e *Event) Uints(key string, i []uint) *Event
```

Uints adds the field key with i as a \[]int to the \*Event context.

## Uints16

```go
func (e *Event) Uints16(key string, i []uint16) *Event
```

Uints16 adds the field key with i as a \[]int16 to the \*Event context.

## Uints32

```go
func (e *Event) Uints32(key string, i []uint32) *Event
```

Uints32 adds the field key with i as a \[]int32 to the \*Event context.

## Uints64

```go
func (e *Event) Uints64(key string, i []uint64) *Event
```

Uints64 adds the field key with i as a \[]int64 to the \*Event context.

## Uints8

```go
func (e *Event) Uints8(key string, i []uint8) *Event
```

Uints8 adds the field key with i as a \[]int8 to the \*Event context.

## FilteredLevelWriter

```go
type FilteredLevelWriter struct {
	Writer LevelWriter
	Level  Level
}
```

FilteredLevelWriter writes only logs at Level or above to Writer.

It should be used only in combination with MultiLevelWriter when you want to write to multiple destinations at different levels. Otherwise you should just set the level on the logger and filter events early. When using MultiLevelWriter then you set the level on the logger to the lowest of the levels you use for writers.

- `Writer: LevelWriter`: 
- `Level: Level`: 

## Close

```go
func (w *FilteredLevelWriter) Close() error
```

Call the underlying writer's Close method if it is an io.Closer. Otherwise does nothing.

## Write

```go
func (w *FilteredLevelWriter) Write(p []byte) (int, error)
```

Write writes to the underlying Writer.

## WriteLevel

```go
func (w *FilteredLevelWriter) WriteLevel(level Level, p []byte) (int, error)
```

WriteLevel calls WriteLevel of the underlying Writer only if the level is equal or above the Level.

## Formatter

```go
type Formatter func(interface{}) string
```

Formatter transforms the input into a formatted string.

## FormatterByFieldName

```go
type FormatterByFieldName func(interface{}, string) string
```

FormatterByFieldName transforms the input into a formatted string, being able to differentiate formatting based on field name.

## Hook

```go
type Hook interface {
	// Run runs the hook with the event.
	Run(e *Event, level Level, message string)
}
```

Hook defines an interface to a log hook.

- `Run: func(e *Event, level Level, message string)`: Run runs the hook with the event.

## HookFunc

```go
type HookFunc func(e *Event, level Level, message string)
```

HookFunc is an adaptor to allow the use of an ordinary function as a Hook.

## Run

```go
func (h HookFunc) Run(e *Event, level Level, message string)
```

Run implements the Hook interface.

## Level

```go
type Level int8
```

Level defines log levels.

## MarshalText

```go
func (l Level) MarshalText() ([]byte, error)
```

MarshalText implements encoding.TextMarshaler to allow for easy writing into toml/yaml/json formats

## String

```go
func (l Level) String() string
```

## UnmarshalText

```go
func (l *Level) UnmarshalText(text []byte) error
```

UnmarshalText implements encoding.TextUnmarshaler to allow for easy reading from toml/yaml/json formats

## LevelHook

```go
type LevelHook struct {
	NoLevelHook, TraceHook, DebugHook, InfoHook, WarnHook, ErrorHook, FatalHook, PanicHook Hook
}
```

LevelHook applies a different hook for each level.

- `NoLevelHook: Hook`: 
- `TraceHook: Hook`: 
- `DebugHook: Hook`: 
- `InfoHook: Hook`: 
- `WarnHook: Hook`: 
- `ErrorHook: Hook`: 
- `FatalHook: Hook`: 
- `PanicHook: Hook`: 

## Run

```go
func (h LevelHook) Run(e *Event, level Level, message string)
```

Run implements the Hook interface.

## LevelSampler

```go
type LevelSampler struct {
	TraceSampler, DebugSampler, InfoSampler, WarnSampler, ErrorSampler Sampler
}
```

LevelSampler applies a different sampler for each level.

- `TraceSampler: Sampler`: 
- `DebugSampler: Sampler`: 
- `InfoSampler: Sampler`: 
- `WarnSampler: Sampler`: 
- `ErrorSampler: Sampler`: 

## Sample

```go
func (s LevelSampler) Sample(lvl Level) bool
```

## LevelWriter

```go
type LevelWriter interface {
	io.Writer
	WriteLevel(level Level, p []byte) (n int, err error)
}
```

LevelWriter defines as interface a writer may implement in order to receive level information with payload.

- `io.Writer: io.Writer`: 
- `WriteLevel: func(level Level, p []byte) (n int, err error)`: 

## LevelWriterAdapter

```go
type LevelWriterAdapter struct {
	io.Writer
}
```

LevelWriterAdapter adapts an io.Writer to support the LevelWriter interface.

- `io.Writer: io.Writer`: 

## Close

```go
func (lw LevelWriterAdapter) Close() error
```

Call the underlying writer's Close method if it is an io.Closer. Otherwise does nothing.

## WriteLevel

```go
func (lw LevelWriterAdapter) WriteLevel(l Level, p []byte) (n int, err error)
```

WriteLevel simply writes everything to the adapted writer, ignoring the level.

## LogArrayMarshaler

```go
type LogArrayMarshaler interface {
	MarshalZerologArray(a *Array)
}
```

LogArrayMarshaler provides a strongly-typed and encoding-agnostic interface to be implemented by types used with Event/Context's Array methods.

- `MarshalZerologArray: func(a *Array)`: 

## LogObjectMarshaler

```go
type LogObjectMarshaler interface {
	MarshalZerologObject(e *Event)
}
```

LogObjectMarshaler provides a strongly-typed and encoding-agnostic interface to be implemented by types used with Event/Context's Object methods.

- `MarshalZerologObject: func(e *Event)`: 

## Logger

```go
type Logger struct {
	// contains filtered or unexported fields
}
```

A Logger represents an active logging object that generates lines of JSON output to an io.Writer. Each logging operation makes a single call to the Writer's Write method. There is no guarantee on access serialization to the Writer. If your Writer is not thread safe, you may consider a sync wrapper.

## Debug

```go
func (l *Logger) Debug() *Event
```

Debug starts a new message with debug level.

You must call Msg on the returned event in order to send the event.

### Example: Logger_Debug

```go
{
	log := zerolog.New(os.Stdout)

	log.Debug().
		Str("foo", "bar").
		Int("n", 123).
		Msg("hello world")

}
```

Output:

```text
{"level":"debug","foo":"bar","n":123,"message":"hello world"}
```

## Err

```go
func (l *Logger) Err(err error) *Event
```

Err starts a new message with error level with err as a field if not nil or with info level if err is nil.

You must call Msg on the returned event in order to send the event.

## Error

```go
func (l *Logger) Error() *Event
```

Error starts a new message with error level.

You must call Msg on the returned event in order to send the event.

### Example: Logger_Error

```go
{
	log := zerolog.New(os.Stdout)

	log.Error().
		Err(errors.New("some error")).
		Msg("error doing something")

}
```

Output:

```text
{"level":"error","error":"some error","message":"error doing something"}
```

## Fatal

```go
func (l *Logger) Fatal() *Event
```

Fatal starts a new message with fatal level. The FatalExitFunc interceptor function is called by the Msg method, which by default terminates the program immediately using os.Exit(1), any desired behavior can be implemented by setting FatalExitFunc.

You must call Msg on the returned event in order to send the event.

## GetLevel

```go
func (l Logger) GetLevel() Level
```

GetLevel returns the current Level of l.

## Hook

```go
func (l Logger) Hook(hooks ...Hook) Logger
```

Hook returns a logger with the h Hook.

### Example: Logger_Hook

```go
{
	var levelNameHook LevelNameHook
	var messageHook MessageHook = "The message"

	log := zerolog.New(os.Stdout).Hook(levelNameHook, messageHook)

	log.Info().Msg("hello world")

}
```

Output:

```text
{"level":"info","level_name":"info","the_message":"hello world","message":"hello world"}
```

## Info

```go
func (l *Logger) Info() *Event
```

Info starts a new message with info level.

You must call Msg on the returned event in order to send the event.

### Example: Logger_Info

```go
{
	log := zerolog.New(os.Stdout)

	log.Info().
		Str("foo", "bar").
		Int("n", 123).
		Msg("hello world")

}
```

Output:

```text
{"level":"info","foo":"bar","n":123,"message":"hello world"}
```

## Level

```go
func (l Logger) Level(lvl Level) Logger
```

Level creates a child logger with the minimum accepted level set to level.

### Example: Logger_Level

```go
{
	log := zerolog.New(os.Stdout).Level(zerolog.WarnLevel)

	log.Info().Msg("filtered out message")
	log.Error().Msg("kept message")

}
```

Output:

```text
{"level":"error","message":"kept message"}
```

## Log

```go
func (l *Logger) Log() *Event
```

Log starts a new message with no level. Setting GlobalLevel to Disabled will still disable events produced by this method.

You must call Msg on the returned event in order to send the event.

### Example: Logger_Log

```go
{
	log := zerolog.New(os.Stdout)

	log.Log().
		Str("foo", "bar").
		Str("bar", "baz").
		Msg("")

}
```

Output:

```text
{"foo":"bar","bar":"baz"}
```

## Output

```go
func (l Logger) Output(w io.Writer) Logger
```

Output duplicates the current logger and sets w as its output.

## Panic

```go
func (l *Logger) Panic() *Event
```

Panic starts a new message with panic level. The panic() function is called by the Msg method, which stops the ordinary flow of a goroutine.

You must call Msg on the returned event in order to send the event.

## Print

```go
func (l *Logger) Print(v ...interface{})
```

Print sends a log event using debug level and no extra field. Arguments are handled in the manner of fmt.Print.

### Example: Logger_Print

```go
{
	log := zerolog.New(os.Stdout)

	log.Print("hello world")

}
```

Output:

```text
{"level":"debug","message":"hello world"}
```

## Printf

```go
func (l *Logger) Printf(format string, v ...interface{})
```

Printf sends a log event using debug level and no extra field. Arguments are handled in the manner of fmt.Printf.

### Example: Logger_Printf

```go
{
	log := zerolog.New(os.Stdout)

	log.Printf("hello %s", "world")

}
```

Output:

```text
{"level":"debug","message":"hello world"}
```

## Println

```go
func (l *Logger) Println(v ...interface{})
```

Println sends a log event using debug level and no extra field. Arguments are handled in the manner of fmt.Println.

### Example: Logger_Println

```go
{
	log := zerolog.New(os.Stdout)

	log.Println("hello world")

}
```

Output:

```text
{"level":"debug","message":"hello world\n"}
```

## Sample

```go
func (l Logger) Sample(s Sampler) Logger
```

Sample returns a logger with the s sampler.

### Example: Logger_Sample

```go
{
	log := zerolog.New(os.Stdout).Sample(&zerolog.BasicSampler{N: 2})

	log.Info().Msg("message 1")
	log.Info().Msg("message 2")
	log.Info().Msg("message 3")
	log.Info().Msg("message 4")

}
```

Output:

```text
{"level":"info","message":"message 1"}
{"level":"info","message":"message 3"}
```

## Trace

```go
func (l *Logger) Trace() *Event
```

Trace starts a new message with trace level.

You must call Msg on the returned event in order to send the event.

### Example: Logger_Trace

```go
{
	log := zerolog.New(os.Stdout)

	log.Trace().
		Str("foo", "bar").
		Int("n", 123).
		Msg("hello world")

}
```

Output:

```text
{"level":"trace","foo":"bar","n":123,"message":"hello world"}
```

## UpdateContext

```go
func (l *Logger) UpdateContext(update func(c Context) Context)
```

UpdateContext updates the internal logger's context.

Caution: This method is not concurrency safe. Use the With method to create a child logger before modifying the context from concurrent goroutines.

## Warn

```go
func (l *Logger) Warn() *Event
```

Warn starts a new message with warn level.

You must call Msg on the returned event in order to send the event.

### Example: Logger_Warn

```go
{
	log := zerolog.New(os.Stdout)

	log.Warn().
		Str("foo", "bar").
		Msg("a warning message")

}
```

Output:

```text
{"level":"warn","foo":"bar","message":"a warning message"}
```

## With

```go
func (l Logger) With() Context
```

With creates a child logger with the field added to its context.

### Example: Logger_With

```go
{
	log := zerolog.New(os.Stdout).
		With().
		Str("foo", "bar").
		Logger()

	log.Info().Msg("hello world")

}
```

Output:

```text
{"level":"info","foo":"bar","message":"hello world"}
```

## WithContext

```go
func (l Logger) WithContext(ctx context.Context) context.Context
```

WithContext returns a copy of ctx with the receiver attached. The Logger attached to the provided Context (if any) will not be affected. If the receiver's log level is Disabled it will only be attached to the returned Context if the provided Context has a previously attached Logger. If the provided Context has no attached Logger, a Disabled Logger will not be attached.

Note: to modify the existing Logger attached to a Context (instead of replacing it in a new Context), use UpdateContext with the following notation:

	ctx := r.Context()
	l := zerolog.Ctx(ctx)
	l.UpdateContext(func(c Context) Context {
	    return c.Str("bar", "baz")
	})

## WithLevel

```go
func (l *Logger) WithLevel(level Level) *Event
```

WithLevel starts a new message with level. Unlike Fatal and Panic methods, WithLevel does not terminate the program or stop the ordinary flow of a goroutine when used with their respective levels.

You must call Msg on the returned event in order to send the event.

### Example: Logger_WithLevel

```go
{
	log := zerolog.New(os.Stdout)

	log.WithLevel(zerolog.InfoLevel).
		Msg("hello world")

}
```

Output:

```text
{"level":"info","message":"hello world"}
```

## Write

```go
func (l Logger) Write(p []byte) (n int, err error)
```

Write implements the io.Writer interface. This is useful to set as a writer for the standard library log.

### Example: Logger_Write

```go
{
	log := zerolog.New(os.Stdout).With().
		Str("foo", "bar").
		Logger()

	stdlog.SetFlags(0)
	stdlog.SetOutput(log)

	stdlog.Print("hello world")

}
```

Output:

```text
{"foo":"bar","message":"hello world"}
```

## RandomSampler

```go
type RandomSampler uint32
```

RandomSampler use a PRNG to randomly sample an event out of N events, regardless of their level.

## Sample

```go
func (s RandomSampler) Sample(lvl Level) bool
```

Sample implements the Sampler interface.

## Sampler

```go
type Sampler interface {
	// Sample returns true if the event should be part of the sample, false if
	// the event should be dropped.
	Sample(lvl Level) bool
}
```

Sampler defines an interface to a log sampler.

- `Sample: func(lvl Level) bool`: Sample returns true if the event should be part of the sample, false if the event should be dropped.

## SlogHandler

```go
type SlogHandler struct {
	// contains filtered or unexported fields
}
```

SlogHandler implements the slog.Handler interface using a zerolog.Logger as the underlying log backend. This allows code that uses the standard library's slog package to route log output through zerolog.

## Enabled

```go
func (h *SlogHandler) Enabled(_ context.Context, level slog.Level) bool
```

Enabled reports whether the handler handles records at the given level. It mirrors Logger.should's level and writer checks (without sampling).

## Handle

```go
func (h *SlogHandler) Handle(ctx context.Context, record slog.Record) error
```

Handle handles the Record. It converts the slog.Record into a zerolog event and writes it using the underlying zerolog.Logger.

## WithAttrs

```go
func (h *SlogHandler) WithAttrs(attrs []slog.Attr) slog.Handler
```

WithAttrs returns a new Handler with the given attributes pre-attached. These attributes will be included in every subsequent log record.

## WithGroup

```go
func (h *SlogHandler) WithGroup(name string) slog.Handler
```

WithGroup returns a new Handler with the given group name. All subsequent attributes will be nested under this group name in the output.

## SyslogWriter

```go
type SyslogWriter interface {
	io.Writer
	Debug(m string) error
	Info(m string) error
	Warning(m string) error
	Err(m string) error
	Emerg(m string) error
	Crit(m string) error
}
```

SyslogWriter is an interface matching a syslog.Writer struct.

- `io.Writer: io.Writer`: 
- `Debug: func(m string) error`: 
- `Info: func(m string) error`: 
- `Warning: func(m string) error`: 
- `Err: func(m string) error`: 
- `Emerg: func(m string) error`: 
- `Crit: func(m string) error`: 

## TestWriter

```go
type TestWriter struct {
	T TestingLog

	// Frame skips caller frames to capture the original file and line numbers.
	Frame int
}
```

TestWriter is a writer that writes to testing.TB.

- `T: TestingLog`: 
- `Frame: int`: Frame skips caller frames to capture the original file and line numbers.

## Write

```go
func (t TestWriter) Write(p []byte) (n int, err error)
```

Write to testing.TB.

## TestingLog

```go
type TestingLog interface {
	Log(args ...interface{})
	Logf(format string, args ...interface{})
	Helper()
}
```

TestingLog is the logging interface of testing.TB.

- `Log: func(args ...interface{})`: 
- `Logf: func(format string, args ...interface{})`: 
- `Helper: func()`: 

## TriggerLevelWriter

```go
type TriggerLevelWriter struct {
	// Destination writer. If LevelWriter is provided (usually), its WriteLevel is used
	// instead of Write.
	io.Writer

	// ConditionalLevel is the level (and below) at which lines are buffered until
	// a trigger level (or higher) line is emitted. Usually this is set to DebugLevel.
	ConditionalLevel Level

	// TriggerLevel is the lowest level that triggers the sending of the conditional
	// level lines. Usually this is set to ErrorLevel.
	TriggerLevel Level
	// contains filtered or unexported fields
}
```

TriggerLevelWriter buffers log lines at the ConditionalLevel or below until a trigger level (or higher) line is emitted. Log lines with level higher than ConditionalLevel are always written out to the destination writer. If trigger never happens, buffered log lines are never written out.

It can be used to configure "log level per request".

- `io.Writer: io.Writer`: Destination writer. If LevelWriter is provided (usually), its WriteLevel is used instead of Write.
- `ConditionalLevel: Level`: ConditionalLevel is the level (and below) at which lines are buffered until a trigger level (or higher) line is emitted. Usually this is set to DebugLevel.
- `TriggerLevel: Level`: TriggerLevel is the lowest level that triggers the sending of the conditional level lines. Usually this is set to ErrorLevel.

## Close

```go
func (w *TriggerLevelWriter) Close() error
```

Close closes the writer and returns the buffer to the pool.

## Trigger

```go
func (w *TriggerLevelWriter) Trigger() error
```

Trigger forces flushing the buffer and change the trigger state to triggered, if the writer has not already been triggered before.

## WriteLevel

```go
func (w *TriggerLevelWriter) WriteLevel(l Level, p []byte) (n int, err error)
```
