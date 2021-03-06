# Logger
HTTP request/response logger for Fiber

### Example
The middleware packages comes with the official Fiber framework.
```go
import (
  "github.com/gofiber/fiber"
  "github.com/gofiber/fiber/middleware"
)

func main() {
  // ...

  // Default Logger
  app.Use(middleware.Logger())

  // Pass a custom output
  app.Use(middleware.Logger(os.Stdout))

  // Pass a custom timeformat
  app.Use(middleware.Logger("15:04:05"))

  // Pass a custom format
  app.Use(middleware.Logger("${time} ${method} ${path}"))

  // Pass a custom  output + timeformat + format
  app.Use(middleware.Logger(os.Stdout, "15:04:05", "${time} ${method} ${path}"))

  // Order does not matter
  app.Use(middleware.Logger("${time} ${method} ${path}", os.Stdout, "15:04:05"))

  // Pass a custom config
  app.Use(middleware.Logger(middleware.LoggerConfig{
      Format:     "${time} ${method} ${path}",
      TimeFormat: "15:04:05",
      Output:     os.Stdout,
  }))

  // ...
}
```

### Signature
```go
func Logger(options ...interface{}) fiber.Handler {}
```

### Config
```go
type LoggerConfig struct {
  // Next defines a function to skip this middleware.
  Next func(ctx *fiber.Ctx) bool

  // Format defines the logging tags
  //
  // - time
  // - ip
  // - ips
  // - url
  // - host
  // - method
  // - path
  // - protocol
  // - route
  // - referer
  // - ua
  // - latency
  // - status
  // - body
  // - error
  // - bytesSent
  // - bytesReceived
  // - header:<key>
  // - query:<key>
  // - form:<key>
  // - cookie:<key>
  //
  // Optional. Default: ${time} ${method} ${path} - ${ip} - ${status} - ${latency}\n
  Format string

  // TimeFormat https://programming.guide/go/format-parse-string-time-date-example.html
  //
  // Optional. Default: 15:04:05
  TimeFormat string

  // Output is a writter where logs are written
  //
  // Default: os.Stderr
  Output io.Writer
}
```