# diode/internal/diodes

## NewManyToOne

```go
func NewManyToOne(size int, alerter Alerter) *ManyToOne
```

NewManyToOne creates a new diode (ring buffer). The ManyToOne diode is optimized for many writers (on go-routines B-n) and a single reader (on go-routine A). The alerter is invoked on the read's go-routine. It is called when it notices that the writer go-routine has passed it and wrote over data. A nil can be used to ignore alerts.

## NewOneToOne

```go
func NewOneToOne(size int, alerter Alerter) *OneToOne
```

NewOneToOne creates a new diode is meant to be used by a single reader and a single writer. The alerter is invoked on the read's go-routine. It is called when it notices that the writer go-routine has passed it and wrote over data. A nil can be used to ignore alerts.

## NewPoller

```go
func NewPoller(d Diode, opts ...PollerConfigOption) *Poller
```

NewPoller returns a new Poller that wraps the given diode.

## NewWaiter

```go
func NewWaiter(d Diode, opts ...WaiterConfigOption) *Waiter
```

NewWaiter returns a new Waiter that wraps the given diode.

## WithPollingContext

```go
func WithPollingContext(ctx context.Context) PollerConfigOption
```

WithPollingContext sets the context to cancel any retrieval (Next()). It will not change any results for adding data (Set()). Default is context.Background().

## WithPollingInterval

```go
func WithPollingInterval(interval time.Duration) PollerConfigOption
```

WithPollingInterval sets the interval at which the diode is queried for new data. The default is 10ms.

## WithWaiterContext

```go
func WithWaiterContext(ctx context.Context) WaiterConfigOption
```

WithWaiterContext sets the context to cancel any retrieval (Next()). It will not change any results for adding data (Set()). Default is context.Background().

## AlertFunc

```go
type AlertFunc func(missed int)
```

AlertFunc type is an adapter to allow the use of ordinary functions as Alert handlers.

## Alert

```go
func (f AlertFunc) Alert(missed int)
```

Alert calls f(missed)

## Alerter

```go
type Alerter interface {
	Alert(missed int)
}
```

Alerter is used to report how many values were overwritten since the last write.

- `Alert: func(missed int)`: 

## Diode

```go
type Diode interface {
	Set(GenericDataType)
	TryNext() (GenericDataType, bool)
}
```

Diode is any implementation of a diode.

- `Set: func(GenericDataType)`: 
- `TryNext: func() (GenericDataType, bool)`: 

## GenericDataType

```go
type GenericDataType unsafe.Pointer
```

GenericDataType is the data type the diodes operate on.

## ManyToOne

```go
type ManyToOne struct {
	// contains filtered or unexported fields
}
```

ManyToOne diode is optimal for many writers (go-routines B-n) and a single reader (go-routine A). It is not thread safe for multiple readers.

## Set

```go
func (d *ManyToOne) Set(data GenericDataType)
```

Set sets the data in the next slot of the ring buffer.

## TryNext

```go
func (d *ManyToOne) TryNext() (data GenericDataType, ok bool)
```

TryNext will attempt to read from the next slot of the ring buffer. If there is no data available, it will return (nil, false).

## OneToOne

```go
type OneToOne struct {
	// contains filtered or unexported fields
}
```

OneToOne diode is meant to be used by a single reader and a single writer. It is not thread safe if used otherwise.

## Set

```go
func (d *OneToOne) Set(data GenericDataType)
```

Set sets the data in the next slot of the ring buffer.

## TryNext

```go
func (d *OneToOne) TryNext() (data GenericDataType, ok bool)
```

TryNext will attempt to read from the next slot of the ring buffer. If there is no data available, it will return (nil, false).

## Poller

```go
type Poller struct {
	Diode
	// contains filtered or unexported fields
}
```

Poller will poll a diode until a value is available.

- `Diode: Diode`: 

## Next

```go
func (p *Poller) Next() GenericDataType
```

Next polls the diode until data is available or until the context is done. If the context is done, then nil will be returned.

## PollerConfigOption

```go
type PollerConfigOption func(*Poller)
```

PollerConfigOption can be used to setup the poller.

## Waiter

```go
type Waiter struct {
	Diode
	// contains filtered or unexported fields
}
```

Waiter will use a conditional mutex to alert the reader to when data is available.

- `Diode: Diode`: 

## Next

```go
func (w *Waiter) Next() GenericDataType
```

Next returns the next data point on the wrapped diode. If there is not any new data, it will Wait for set to be called or the context to be done. If the context is done, then nil will be returned.

## Set

```go
func (w *Waiter) Set(data GenericDataType)
```

Set invokes the wrapped diode's Set with the given data and uses Broadcast to wake up any readers.

## WaiterConfigOption

```go
type WaiterConfigOption func(*Waiter)
```

WaiterConfigOption can be used to setup the waiter.
