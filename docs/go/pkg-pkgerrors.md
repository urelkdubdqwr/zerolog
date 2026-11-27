# pkgerrors

## StackSourceFileName

```go
var (
	StackSourceFileName     = "source"
	StackSourceLineName     = "line"
	StackSourceFunctionName = "func"
)
```

## StackSourceLineName

```go
var (
	StackSourceFileName     = "source"
	StackSourceLineName     = "line"
	StackSourceFunctionName = "func"
)
```

## StackSourceFunctionName

```go
var (
	StackSourceFileName     = "source"
	StackSourceLineName     = "line"
	StackSourceFunctionName = "func"
)
```

## MarshalStack

```go
func MarshalStack(err error) interface{}
```

MarshalStack implements pkg/errors stack trace marshaling.

zerolog.ErrorStackMarshaler = MarshalStack
