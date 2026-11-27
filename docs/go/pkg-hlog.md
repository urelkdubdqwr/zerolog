# hlog

Package hlog provides a set of http.Handler helpers for zerolog.

## AccessHandler

```go
func AccessHandler(f func(r *http.Request, status, size int, duration time.Duration)) func(next http.Handler) http.Handler
```

AccessHandler returns a handler that call f after each request.

## CtxWithID

```go
func CtxWithID(ctx context.Context, id xid.ID) context.Context
```

CtxWithID adds the given xid.ID to the context

## CustomHeaderHandler

```go
func CustomHeaderHandler(fieldKey, header string) func(next http.Handler) http.Handler
```

CustomHeaderHandler adds given header from request's header as a field to the context's logger using fieldKey as field key.

## EtagHandler

```go
func EtagHandler(fieldKey string) func(next http.Handler) http.Handler
```

EtagHandler adds Etag header from response's header as a field to the context's logger using fieldKey as field key.

## FromRequest

```go
func FromRequest(r *http.Request) *zerolog.Logger
```

FromRequest gets the logger in the request's context. This is a shortcut for log.Ctx(r.Context())

## HTTPVersionHandler

```go
func HTTPVersionHandler(fieldKey string) func(next http.Handler) http.Handler
```

HTTPVersionHandler is similar to ProtoHandler, but it does not store the "HTTP/" prefix in the protocol name.

## HostHandler

```go
func HostHandler(fieldKey string, trimPort ...bool) func(next http.Handler) http.Handler
```

HostHandler adds the request's host as a field to the context's logger using fieldKey as field key. If trimPort is set to true, then port is removed from the host.

## IDFromCtx

```go
func IDFromCtx(ctx context.Context) (id xid.ID, ok bool)
```

IDFromCtx returns the unique id associated to the context if any.

## IDFromRequest

```go
func IDFromRequest(r *http.Request) (id xid.ID, ok bool)
```

IDFromRequest returns the unique id associated to the request if any.

## MethodHandler

```go
func MethodHandler(fieldKey string) func(next http.Handler) http.Handler
```

MethodHandler adds the request method as a field to the context's logger using fieldKey as field key.

## NewHandler

```go
func NewHandler(log zerolog.Logger) func(http.Handler) http.Handler
```

NewHandler injects log into requests context.

## ProtoHandler

```go
func ProtoHandler(fieldKey string) func(next http.Handler) http.Handler
```

ProtoHandler adds the requests protocol version as a field to the context logger using fieldKey as field Key.

## RefererHandler

```go
func RefererHandler(fieldKey string) func(next http.Handler) http.Handler
```

RefererHandler adds the request's referer as a field to the context's logger using fieldKey as field key.

## RemoteAddrHandler

```go
func RemoteAddrHandler(fieldKey string) func(next http.Handler) http.Handler
```

RemoteAddrHandler adds the request's remote address as a field to the context's logger using fieldKey as field key.

## RemoteIPHandler

```go
func RemoteIPHandler(fieldKey string) func(next http.Handler) http.Handler
```

RemoteIPHandler is similar to RemoteAddrHandler, but logs only an IP, not a port.

## RequestHandler

```go
func RequestHandler(fieldKey string) func(next http.Handler) http.Handler
```

RequestHandler adds the request method and URL as a field to the context's logger using fieldKey as field key.

## RequestIDHandler

```go
func RequestIDHandler(fieldKey, headerName string) func(next http.Handler) http.Handler
```

RequestIDHandler returns a handler setting a unique id to the request which can be gathered using IDFromRequest(req). This generated id is added as a field to the logger using the passed fieldKey as field name. The id is also added as a response header if the headerName is not empty.

The generated id is a URL safe base64 encoded mongo object-id-like unique id. Mongo unique id generation algorithm has been selected as a trade-off between size and ease of use: UUID is less space efficient and snowflake requires machine configuration.

## ResponseHeaderHandler

```go
func ResponseHeaderHandler(fieldKey, headerName string) func(next http.Handler) http.Handler
```

## URLHandler

```go
func URLHandler(fieldKey string) func(next http.Handler) http.Handler
```

URLHandler adds the requested URL as a field to the context's logger using fieldKey as field key.

## UserAgentHandler

```go
func UserAgentHandler(fieldKey string) func(next http.Handler) http.Handler
```

UserAgentHandler adds the request's user-agent as a field to the context's logger using fieldKey as field key.
