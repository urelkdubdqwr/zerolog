# internal/json

## JSONMarshalFunc

```go
var JSONMarshalFunc func(v interface{}) ([]byte, error)
```

JSONMarshalFunc is used to marshal interface to JSON encoded byte slice. Making it package level instead of embedded in Encoder brings some extra efforts at importing, but avoids value copy when the functions of Encoder being invoked. DO REMEMBER to set this variable at importing, or you might get a nil pointer dereference panic at runtime.

## Encoder

```go
type Encoder struct{}
```

## AppendArrayDelim

```go
func (Encoder) AppendArrayDelim(dst []byte) []byte
```

AppendArrayDelim adds markers to indicate end of a particular array element.

## AppendArrayEnd

```go
func (Encoder) AppendArrayEnd(dst []byte) []byte
```

AppendArrayEnd adds markers to indicate the end of an array.

## AppendArrayStart

```go
func (Encoder) AppendArrayStart(dst []byte) []byte
```

AppendArrayStart adds markers to indicate the start of an array.

## AppendBeginMarker

```go
func (Encoder) AppendBeginMarker(dst []byte) []byte
```

AppendBeginMarker inserts a map start into the dst byte array.

## AppendBool

```go
func (Encoder) AppendBool(dst []byte, val bool) []byte
```

AppendBool converts the input bool to a string and appends the encoded string to the input byte slice.

## AppendBools

```go
func (Encoder) AppendBools(dst []byte, vals []bool) []byte
```

AppendBools encodes the input bools to json and appends the encoded string list to the input byte slice.

## AppendBytes

```go
func (Encoder) AppendBytes(dst, s []byte) []byte
```

AppendBytes is a mirror of appendString with \[]byte arg

## AppendDuration

```go
func (e Encoder) AppendDuration(dst []byte, d time.Duration, unit time.Duration, format string, useInt bool, precision int) []byte
```

AppendDuration formats the input duration with the given unit & format and appends the encoded string to the input byte slice.

## AppendDurations

```go
func (e Encoder) AppendDurations(dst []byte, vals []time.Duration, unit time.Duration, format string, useInt bool, precision int) []byte
```

AppendDurations formats the input durations with the given unit & format and appends the encoded string list to the input byte slice.

## AppendEndMarker

```go
func (Encoder) AppendEndMarker(dst []byte) []byte
```

AppendEndMarker inserts a map end into the dst byte array.

## AppendFloat32

```go
func (Encoder) AppendFloat32(dst []byte, val float32, precision int) []byte
```

AppendFloat32 converts the input float32 to a string and appends the encoded string to the input byte slice.

## AppendFloat64

```go
func (Encoder) AppendFloat64(dst []byte, val float64, precision int) []byte
```

AppendFloat64 converts the input float64 to a string and appends the encoded string to the input byte slice.

## AppendFloats32

```go
func (Encoder) AppendFloats32(dst []byte, vals []float32, precision int) []byte
```

AppendFloats32 encodes the input float32s to json and appends the encoded string list to the input byte slice.

## AppendFloats64

```go
func (Encoder) AppendFloats64(dst []byte, vals []float64, precision int) []byte
```

AppendFloats64 encodes the input float64s to json and appends the encoded string list to the input byte slice.

## AppendHex

```go
func (Encoder) AppendHex(dst, s []byte) []byte
```

AppendHex encodes the input bytes to a hex string and appends the encoded string to the input byte slice.

The operation loops though each byte and encodes it as hex using the hex lookup table.

## AppendIPAddr

```go
func (e Encoder) AppendIPAddr(dst []byte, ip net.IP) []byte
```

AppendIPAddr adds a net.IP IPv4 or IPv6 address to dst.

## AppendIPAddrs

```go
func (e Encoder) AppendIPAddrs(dst []byte, ips []net.IP) []byte
```

AppendIPAddrs adds a \[]net.IP array of IPv4 or IPv6 address to dst.

## AppendIPPrefix

```go
func (e Encoder) AppendIPPrefix(dst []byte, pfx net.IPNet) []byte
```

AppendIPPrefix adds a net.IPNet IPv4 or IPv6 Prefix (address & mask) to dst.

## AppendIPPrefixes

```go
func (e Encoder) AppendIPPrefixes(dst []byte, pfxs []net.IPNet) []byte
```

AppendIPPrefixes adds a \[]net.IPNet array of IPv4 or IPv6 Prefix (address & mask) to dst.

## AppendInt

```go
func (Encoder) AppendInt(dst []byte, val int) []byte
```

AppendInt converts the input int to a string and appends the encoded string to the input byte slice.

## AppendInt16

```go
func (Encoder) AppendInt16(dst []byte, val int16) []byte
```

AppendInt16 converts the input int16 to a string and appends the encoded string to the input byte slice.

## AppendInt32

```go
func (Encoder) AppendInt32(dst []byte, val int32) []byte
```

AppendInt32 converts the input int32 to a string and appends the encoded string to the input byte slice.

## AppendInt64

```go
func (Encoder) AppendInt64(dst []byte, val int64) []byte
```

AppendInt64 converts the input int64 to a string and appends the encoded string to the input byte slice.

## AppendInt8

```go
func (Encoder) AppendInt8(dst []byte, val int8) []byte
```

AppendInt8 converts the input \[]int8 to a string and appends the encoded string to the input byte slice.

## AppendInterface

```go
func (e Encoder) AppendInterface(dst []byte, i interface{}) []byte
```

AppendInterface marshals the input interface to a string and appends the encoded string to the input byte slice.

## AppendInts

```go
func (Encoder) AppendInts(dst []byte, vals []int) []byte
```

AppendInts encodes the input ints to json and appends the encoded string list to the input byte slice.

## AppendInts16

```go
func (Encoder) AppendInts16(dst []byte, vals []int16) []byte
```

AppendInts16 encodes the input int16s to json and appends the encoded string list to the input byte slice.

## AppendInts32

```go
func (Encoder) AppendInts32(dst []byte, vals []int32) []byte
```

AppendInts32 encodes the input int32s to json and appends the encoded string list to the input byte slice.

## AppendInts64

```go
func (Encoder) AppendInts64(dst []byte, vals []int64) []byte
```

AppendInts64 encodes the input int64s to json and appends the encoded string list to the input byte slice.

## AppendInts8

```go
func (Encoder) AppendInts8(dst []byte, vals []int8) []byte
```

AppendInts8 encodes the input int8s to json and appends the encoded string list to the input byte slice.

## AppendKey

```go
func (e Encoder) AppendKey(dst []byte, key string) []byte
```

AppendKey appends a new key to the output JSON.

## AppendLineBreak

```go
func (Encoder) AppendLineBreak(dst []byte) []byte
```

AppendLineBreak appends a line break.

## AppendMACAddr

```go
func (e Encoder) AppendMACAddr(dst []byte, ha net.HardwareAddr) []byte
```

AppendMACAddr adds a net.HardwareAddr MAC address to dst.

## AppendNil

```go
func (Encoder) AppendNil(dst []byte) []byte
```

AppendNil inserts a 'Nil' object into the dst byte array.

## AppendObjectData

```go
func (Encoder) AppendObjectData(dst []byte, o []byte) []byte
```

AppendObjectData takes in an object that is already in a byte array and adds it to the dst.

## AppendString

```go
func (Encoder) AppendString(dst []byte, s string) []byte
```

AppendString encodes the input string to json and appends the encoded string to the input byte slice.

The operation loops though each byte in the string looking for characters that need json or utf8 encoding. If the string does not need encoding, then the string is appended in its entirety to the byte slice. If we encounter a byte that does need encoding, switch up the operation and perform a byte-by-byte read-encode-append.

## AppendStringer

```go
func (e Encoder) AppendStringer(dst []byte, val fmt.Stringer) []byte
```

AppendStringer encodes the input Stringer to json and appends the encoded Stringer value to the input byte slice.

## AppendStringers

```go
func (e Encoder) AppendStringers(dst []byte, vals []fmt.Stringer) []byte
```

AppendStringers encodes the provided Stringer list to json and appends the encoded Stringer list to the input byte slice.

## AppendStrings

```go
func (e Encoder) AppendStrings(dst []byte, vals []string) []byte
```

AppendStrings encodes the input strings to json and appends the encoded string list to the input byte slice.

## AppendTime

```go
func (e Encoder) AppendTime(dst []byte, t time.Time, format string) []byte
```

AppendTime formats the input time with the given format and appends the encoded string to the input byte slice.

## AppendTimes

```go
func (Encoder) AppendTimes(dst []byte, vals []time.Time, format string) []byte
```

AppendTimes converts the input times with the given format and appends the encoded string list to the input byte slice.

## AppendType

```go
func (e Encoder) AppendType(dst []byte, i interface{}) []byte
```

AppendType appends the parameter type (as a string) to the input byte slice.

## AppendUint

```go
func (Encoder) AppendUint(dst []byte, val uint) []byte
```

AppendUint converts the input uint to a string and appends the encoded string to the input byte slice.

## AppendUint16

```go
func (Encoder) AppendUint16(dst []byte, val uint16) []byte
```

AppendUint16 converts the input uint16 to a string and appends the encoded string to the input byte slice.

## AppendUint32

```go
func (Encoder) AppendUint32(dst []byte, val uint32) []byte
```

AppendUint32 converts the input uint32 to a string and appends the encoded string to the input byte slice.

## AppendUint64

```go
func (Encoder) AppendUint64(dst []byte, val uint64) []byte
```

AppendUint64 converts the input uint64 to a string and appends the encoded string to the input byte slice.

## AppendUint8

```go
func (Encoder) AppendUint8(dst []byte, val uint8) []byte
```

AppendUint8 converts the input uint8 to a string and appends the encoded string to the input byte slice.

## AppendUints

```go
func (Encoder) AppendUints(dst []byte, vals []uint) []byte
```

AppendUints encodes the input uints to json and appends the encoded string list to the input byte slice.

## AppendUints16

```go
func (Encoder) AppendUints16(dst []byte, vals []uint16) []byte
```

AppendUints16 encodes the input uint16s to json and appends the encoded string list to the input byte slice.

## AppendUints32

```go
func (Encoder) AppendUints32(dst []byte, vals []uint32) []byte
```

AppendUints32 encodes the input uint32s to json and appends the encoded string list to the input byte slice.

## AppendUints64

```go
func (Encoder) AppendUints64(dst []byte, vals []uint64) []byte
```

AppendUints64 encodes the input uint64s to json and appends the encoded string list to the input byte slice.

## AppendUints8

```go
func (Encoder) AppendUints8(dst []byte, vals []uint8) []byte
```

AppendUints8 encodes the input uint8s to json and appends the encoded string list to the input byte slice.
