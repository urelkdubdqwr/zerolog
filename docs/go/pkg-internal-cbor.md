# internal/cbor

Package cbor provides primitives for storing different data in the CBOR (binary) format. CBOR is defined in RFC7049.

## IntegerTimeFieldFormat

```go
var IntegerTimeFieldFormat = time.RFC3339
```

IntegerTimeFieldFormat indicates the format of timestamp decoded from an integer (time in seconds).

## JSONMarshalFunc

```go
var JSONMarshalFunc func(v interface{}) ([]byte, error)
```

JSONMarshalFunc is used to marshal interface to JSON encoded byte slice. Making it package level instead of embedded in Encoder brings some extra efforts at importing, but avoids value copy when the functions of Encoder being invoked. DO REMEMBER to set this variable at importing, or you might get a nil pointer dereference panic at runtime.

## NanoTimeFieldFormat

```go
var NanoTimeFieldFormat = time.RFC3339Nano
```

NanoTimeFieldFormat indicates the format of timestamp decoded from a float value (time in seconds and nanoseconds).

## AppendEmbeddedCBOR

```go
func AppendEmbeddedCBOR(dst, s []byte) []byte
```

AppendEmbeddedCBOR adds a tag and embeds input CBOR as such.

## AppendEmbeddedJSON

```go
func AppendEmbeddedJSON(dst, s []byte) []byte
```

AppendEmbeddedJSON adds a tag and embeds input JSON as such.

## Cbor2JsonManyObjects

```go
func Cbor2JsonManyObjects(src io.Reader, dst io.Writer) (err error)
```

Cbor2JsonManyObjects decodes all the CBOR Objects read from src reader. It keeps on decoding until reader returns EOF (error when reading). Decoded string is written to the dst. At the end of every CBOR Object newline is written to the output stream.

Returns error (if any) that was encountered during decode. The child functions will generate a panic when error is encountered and this function will recover non-runtime Errors and return the reason as error.

## DecodeIfBinaryToBytes

```go
func DecodeIfBinaryToBytes(in []byte) []byte
```

DecodeIfBinaryToBytes checks if the input is a binary format, if so, it will decode all Objects and return the decoded string as byte array.

## DecodeIfBinaryToString

```go
func DecodeIfBinaryToString(in []byte) string
```

DecodeIfBinaryToString converts a binary formatted log msg to a JSON formatted String Log message - suitable for printing to Console/Syslog.

## DecodeObjectToStr

```go
func DecodeObjectToStr(in []byte) string
```

DecodeObjectToStr checks if the input is a binary format, if so, it will decode a single Object and return the decoded string.

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

AppendBool encodes and inserts a boolean value into the dst byte array.

## AppendBools

```go
func (e Encoder) AppendBools(dst []byte, vals []bool) []byte
```

AppendBools encodes and inserts an array of boolean values into the dst byte array.

## AppendBytes

```go
func (Encoder) AppendBytes(dst, s []byte) []byte
```

AppendBytes encodes and adds an array of bytes to the dst byte array.

## AppendDuration

```go
func (e Encoder) AppendDuration(dst []byte, d time.Duration, unit time.Duration, format string, useInt bool, unused int) []byte
```

AppendDuration encodes and adds a duration to the dst byte array. useInt field indicates whether to store the duration as seconds (integer) or as seconds+nanoseconds (float).

## AppendDurations

```go
func (e Encoder) AppendDurations(dst []byte, vals []time.Duration, unit time.Duration, format string, useInt bool, unused int) []byte
```

AppendDurations encodes and adds an array of durations to the dst byte array. useInt field indicates whether to store the duration as seconds (integer) or as seconds+nanoseconds (float).

## AppendEndMarker

```go
func (Encoder) AppendEndMarker(dst []byte) []byte
```

AppendEndMarker inserts a map end into the dst byte array.

## AppendFloat32

```go
func (Encoder) AppendFloat32(dst []byte, val float32, unused int) []byte
```

AppendFloat32 encodes and inserts a single precision float value into the dst byte array.

## AppendFloat64

```go
func (Encoder) AppendFloat64(dst []byte, val float64, unused int) []byte
```

AppendFloat64 encodes and inserts a double precision float value into the dst byte array.

## AppendFloats32

```go
func (e Encoder) AppendFloats32(dst []byte, vals []float32, unused int) []byte
```

AppendFloats32 encodes and inserts an array of single precision float value into the dst byte array.

## AppendFloats64

```go
func (e Encoder) AppendFloats64(dst []byte, vals []float64, unused int) []byte
```

AppendFloats64 encodes and inserts an array of double precision float values into the dst byte array.

## AppendHex

```go
func (e Encoder) AppendHex(dst []byte, val []byte) []byte
```

AppendHex adds a TAG and inserts a hex bytes as a string.

## AppendIPAddr

```go
func (e Encoder) AppendIPAddr(dst []byte, ip net.IP) []byte
```

AppendIPAddr adds a net.IP IPv4 or IPv6 address into the dst byte array.

## AppendIPAddrs

```go
func (e Encoder) AppendIPAddrs(dst []byte, ips []net.IP) []byte
```

AppendIPAddrs adds a \[]net.IP array of IPv4 or IPv6 address into the dst byte array.

## AppendIPPrefix

```go
func (e Encoder) AppendIPPrefix(dst []byte, pfx net.IPNet) []byte
```

AppendIPPrefix adds a net.IPNet IPv4 or IPv6 Prefix (address & mask) into the dst byte array.

## AppendIPPrefixes

```go
func (e Encoder) AppendIPPrefixes(dst []byte, pfxs []net.IPNet) []byte
```

AppendIPPrefixes adds a \[]net.IPNet array of IPv4 or IPv6 Prefix (address & mask) into the dst byte array.

## AppendInt

```go
func (Encoder) AppendInt(dst []byte, val int) []byte
```

AppendInt encodes and inserts an integer value into the dst byte array.

## AppendInt16

```go
func (e Encoder) AppendInt16(dst []byte, val int16) []byte
```

AppendInt16 encodes and inserts a int16 value into the dst byte array.

## AppendInt32

```go
func (e Encoder) AppendInt32(dst []byte, val int32) []byte
```

AppendInt32 encodes and inserts a int32 value into the dst byte array.

## AppendInt64

```go
func (Encoder) AppendInt64(dst []byte, val int64) []byte
```

AppendInt64 encodes and inserts a int64 value into the dst byte array.

## AppendInt8

```go
func (e Encoder) AppendInt8(dst []byte, val int8) []byte
```

AppendInt8 encodes and inserts an int8 value into the dst byte array.

## AppendInterface

```go
func (e Encoder) AppendInterface(dst []byte, i interface{}) []byte
```

AppendInterface takes an arbitrary object and converts it to JSON and embeds it dst.

## AppendInts

```go
func (e Encoder) AppendInts(dst []byte, vals []int) []byte
```

AppendInts encodes and inserts an array of integer values into the dst byte array.

## AppendInts16

```go
func (e Encoder) AppendInts16(dst []byte, vals []int16) []byte
```

AppendInts16 encodes and inserts an array of int16 values into the dst byte array.

## AppendInts32

```go
func (e Encoder) AppendInts32(dst []byte, vals []int32) []byte
```

AppendInts32 encodes and inserts an array of int32 values into the dst byte array.

## AppendInts64

```go
func (e Encoder) AppendInts64(dst []byte, vals []int64) []byte
```

AppendInts64 encodes and inserts an array of int64 values into the dst byte array.

## AppendInts8

```go
func (e Encoder) AppendInts8(dst []byte, vals []int8) []byte
```

AppendInts8 encodes and inserts an array of integer values into the dst byte array.

## AppendKey

```go
func (e Encoder) AppendKey(dst []byte, key string) []byte
```

AppendKey adds a key (string) to the binary encoded log message

## AppendLineBreak

```go
func (Encoder) AppendLineBreak(dst []byte) []byte
```

AppendLineBreak is a noop that keep API compat with json encoder.

## AppendMACAddr

```go
func (e Encoder) AppendMACAddr(dst []byte, ha net.HardwareAddr) []byte
```

AppendMACAddr encodes and inserts a Hardware (MAC) address.

## AppendNil

```go
func (Encoder) AppendNil(dst []byte) []byte
```

AppendNil inserts a 'Nil' object into the dst byte array.

## AppendObjectData

```go
func (Encoder) AppendObjectData(dst []byte, o []byte) []byte
```

AppendObjectData takes an object in form of a byte array and appends to dst.

## AppendString

```go
func (Encoder) AppendString(dst []byte, s string) []byte
```

AppendString encodes and adds a string to the dst byte array.

## AppendStringer

```go
func (e Encoder) AppendStringer(dst []byte, val fmt.Stringer) []byte
```

AppendStringer encodes and adds the Stringer value to the dst byte array.

## AppendStringers

```go
func (e Encoder) AppendStringers(dst []byte, vals []fmt.Stringer) []byte
```

AppendStringers encodes and adds an array of Stringer values to the dst byte array.

## AppendStrings

```go
func (e Encoder) AppendStrings(dst []byte, vals []string) []byte
```

AppendStrings encodes and adds an array of strings to the dst byte array.

## AppendTime

```go
func (e Encoder) AppendTime(dst []byte, t time.Time, unused string) []byte
```

AppendTime encodes and adds a timestamp to the dst byte array.

## AppendTimes

```go
func (e Encoder) AppendTimes(dst []byte, vals []time.Time, unused string) []byte
```

AppendTimes encodes and adds an array of timestamps to the dst byte array.

## AppendType

```go
func (e Encoder) AppendType(dst []byte, i interface{}) []byte
```

AppendType appends the parameter type (as a string) to the input byte slice.

## AppendUint

```go
func (e Encoder) AppendUint(dst []byte, val uint) []byte
```

AppendUint encodes and inserts an unsigned integer value into the dst byte array.

## AppendUint16

```go
func (e Encoder) AppendUint16(dst []byte, val uint16) []byte
```

AppendUint16 encodes and inserts a uint16 value into the dst byte array.

## AppendUint32

```go
func (e Encoder) AppendUint32(dst []byte, val uint32) []byte
```

AppendUint32 encodes and inserts a uint32 value into the dst byte array.

## AppendUint64

```go
func (Encoder) AppendUint64(dst []byte, val uint64) []byte
```

AppendUint64 encodes and inserts a uint64 value into the dst byte array.

## AppendUint8

```go
func (e Encoder) AppendUint8(dst []byte, val uint8) []byte
```

AppendUint8 encodes and inserts a unsigned int8 value into the dst byte array.

## AppendUints

```go
func (e Encoder) AppendUints(dst []byte, vals []uint) []byte
```

AppendUints encodes and inserts an array of unsigned integer values into the dst byte array.

## AppendUints16

```go
func (e Encoder) AppendUints16(dst []byte, vals []uint16) []byte
```

AppendUints16 encodes and inserts an array of uint16 values into the dst byte array.

## AppendUints32

```go
func (e Encoder) AppendUints32(dst []byte, vals []uint32) []byte
```

AppendUints32 encodes and inserts an array of uint32 values into the dst byte array.

## AppendUints64

```go
func (e Encoder) AppendUints64(dst []byte, vals []uint64) []byte
```

AppendUints64 encodes and inserts an array of uint64 values into the dst byte array.

## AppendUints8

```go
func (e Encoder) AppendUints8(dst []byte, vals []uint8) []byte
```

AppendUints8 encodes and inserts an array of uint8 values into the dst byte array.
