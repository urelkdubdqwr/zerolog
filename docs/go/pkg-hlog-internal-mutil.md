# hlog/internal/mutil

Package mutil contains various functions that are helpful when writing http middleware.

It has been vendored from Goji v1.0, with the exception of the code for Go 1.8: [https://github.com/zenazn/goji/](https://github.com/zenazn/goji/)

## WrapWriter

```go
func WrapWriter(w http.ResponseWriter) WriterProxy
```

WrapWriter wraps an http.ResponseWriter, returning a proxy that allows you to hook into various parts of the response process.

## WriterProxy

```go
type WriterProxy interface {
	http.ResponseWriter
	// Status returns the HTTP status of the request, or 0 if one has not
	// yet been sent.
	Status() int
	// BytesWritten returns the total number of bytes sent to the client.
	BytesWritten() int
	// Tee causes the response body to be written to the given io.Writer in
	// addition to proxying the writes through. Only one io.Writer can be
	// tee'd to at once: setting a second one will overwrite the first.
	// Writes will be sent to the proxy before being written to this
	// io.Writer. It is illegal for the tee'd writer to be modified
	// concurrently with writes.
	Tee(io.Writer)
	// Unwrap returns the original proxied target.
	Unwrap() http.ResponseWriter
}
```

WriterProxy is a proxy around an http.ResponseWriter that allows you to hook into various parts of the response process.

- `http.ResponseWriter: http.ResponseWriter`: 
- `Status: func() int`: Status returns the HTTP status of the request, or 0 if one has not yet been sent.
- `BytesWritten: func() int`: BytesWritten returns the total number of bytes sent to the client.
- `Tee: func(io.Writer)`: Tee causes the response body to be written to the given io.Writer in addition to proxying the writes through. Only one io.Writer can be tee'd to at once: setting a second one will overwrite the first. Writes will be sent to the proxy before being written to this io.Writer. It is illegal for the tee'd writer to be modified concurrently with writes.
- `Unwrap: func() http.ResponseWriter`: Unwrap returns the original proxied target.
