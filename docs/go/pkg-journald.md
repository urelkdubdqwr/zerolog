# journald

## SendFunc

```go
var SendFunc func(string, journal.Priority, map[string]string) error
```

SendFunc is the function used to send logs to journald. It can be replaced in tests for mocking. If nil, journal.Send is used directly. This variable should only be modified in tests and must not be changed while the writer is in use. Tests that modify this variable should not use t.Parallel().

## NewJournalDWriter

```go
func NewJournalDWriter() io.Writer
```

NewJournalDWriter returns a zerolog log destination to be used as parameter to New() calls. Writing logs to this writer will send the log messages to journalD running in this system.

### Example: NewJournalDWriter

```go
{
	log := zerolog.New(NewJournalDWriter())
	log.Info().Str("foo", "bar").Uint64("small", 123).Float64("float", 3.14).Uint64("big", 1152921504606846976).Msg("Journal Test")

}
```
