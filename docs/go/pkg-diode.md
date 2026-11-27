# diode

Package diode provides a thread-safe, lock-free, non-blocking io.Writer wrapper.

## NewWriter

```go
func NewWriter(w io.Writer, size int, pollInterval time.Duration, f Alerter) Writer
```

NewWriter creates a writer wrapping w with a many-to-one diode in order to never block log producers and drop events if the writer can't keep up with the flow of data.

Use a diode.Writer when

	wr := diode.NewWriter(w, 1000, 0, func(missed int) {
	    log.Printf("Dropped %d messages", missed)
	})
	log := zerolog.New(wr)

If pollInterval is greater than 0, a poller is used otherwise a waiter is used.

See code.cloudfoundry.org/go-diodes for more info on diode.

### Example: NewWriter

```go
{
	w := diode.NewWriter(os.Stdout, 1000, 0, func(missed int) {
		fmt.Printf("Dropped %d messages\n", missed)
	})
	log := zerolog.New(w)
	log.Print("test")

	w.Close()

}
```

Output:

```text
{"level":"debug","message":"test"}
```

## Alerter

```go
type Alerter func(missed int)
```

## Writer

```go
type Writer struct {
	// contains filtered or unexported fields
}
```

Writer is a io.Writer wrapper that uses a diode to make Write lock-free, non-blocking and thread safe.

## Close

```go
func (dw Writer) Close() error
```

Close releases the diode poller and call Close on the wrapped writer if io.Closer is implemented.

## Write

```go
func (dw Writer) Write(p []byte) (n int, err error)
```
