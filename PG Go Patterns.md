---
tags:
- Go
- Programming-Language
MOC: Programming
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Go index|Back to index]]
# Functional Options Pattern
## Package initialization and configuration
- When building a package, there are a few different ways to configure and initialize it
- Below, will be 3 different methods being compared using as an example, building a server package
```Go
package server

type server struct {
  host string
  port int
}

func New(host string, port int) *Server {
  return &Server{host, port}
}

func (s *Server) Start() error {
  // todo
}
```
-  Then to use the package
```Go
package main

import (
  "log"
  
  "github.com/acme/pkg/server"
)

func main() {
  svr := server.New("localhost", 1234)
  if err := svr.Start(); err != nil {
    log.Fatal(err)
  }
}
```
- Here we simply start the server with a host and port without any configuration, so lets look at a few different ways of configuring it
### Constructors
- First off, when we have a fixed set of options that are not likely to change, we can use different constructor functions for different scenarios
```Go
package server

type server struct {
  host string
  port int
  timeout time.Duration
  maxConn int
}

func New(host string, port int) *Server {
  return &Server{host, port, time.Minute, 100}
}

func NewWithTimeout(host string, port int, timeout time.Duration) *Server {
  return &Server{host, port, timeout}
}

func NewWithTimeoutAndMaxConn(host string, port int, timeout time.Duration, maxConn int) *Server {
  return &Server{host, port, timeout, maxConn}
}

func (s *Server) Start() error {
  // todo
}
```
- Then we start the server as follows
```Go
package main

import (
  "log"
  
  "github.com/acme/pkg/server"
)

func main() {
  svr := server.NewWithTimeoutAndMaxConn("localhost", 1234, 30*time.Second, 10)
  if err := svr.Start(); err != nil {
    log.Fatal(err)
  }
}
```
- The problem with this approach is that its rigid and doesn't offer much in terms of flexibility in case the configuration options ever increase
### Custom config struct
- Next up, a more flexible option is to use a dedicated configuration struct
- This is the most common approach used today, and allows for configuration options to be easily added or removed
```Go
package server

type server struct {
  cfg Config
}

type Config struct {
  host string
  port int
  timeout time.Duration
  maxConn int
}

func New(cfg Config) *Server {
  return &Server{cfg}
}

func (s *Server) Start() error {
  // todo
}
```
- And the usage
```Go
package main

import (
  "log"
  
  "github.com/acme/pkg/server"
)

func main() {
  svr := server.New(server.Config{"localhost", 1234, 30*time.Second, 10})
  if err := svr.Start(); err != nil {
    log.Fatal(err)
  }
}
```
- Despite this approach being more flexible, we will still have to introduce breaking changes to the structure of the config struct when we want to make changes
### Functional Options Pattern
- This is the best option when it comes to flexibility
- Here, we use a set of functions, somewhat similarly to a middleware chain to extend and configure the server type
```Go
package server

type server struct {
  host string
  port int
  timeout time.Duration
  maxConn int
}

func New(options ...func(*Server)) *Server {
  svr := &Server{}
  for _, o := range options {
    o(svr)
  }
  return svr
}

func (s *Server) Start() error {
  // todo
}

func WithHost(host string) func(*Server) {
  return func(s *Server) {
    s.host = host
  }
}

func WithPort(port int) func(*Server) {
  return func(s *Server) {
    s.port = port
  }
}

func WithTimeout(timeout time.Duration) func(*Server) {
  return func(s *Server) {
    s.timeout = timeout
  }
}

func WithMaxConn(maxConn int) func(*Server) {
  return func(s *Server) {
    s.maxConn = maxConn
  }
}
```
- Usage
```Go
package main

import (
  "log"
  
  "github.com/acme/pkg/server"
)

func main() {
  svr := server.New(
    server.WithHost("localhost"),
    server.WithPort(8080),
    server.WithTimeout(time.Minute),
    server.WithMaxConn(120),
  )
  if err := svr.Start(); err != nil {
    log.Fatal(err)
  }
}
```
# Adapter Pattern
- This pattern is used to adapt a type to another type through coercion
- One of the most popular examples is `HandlerFunc` from the `net/http` standard package
- The `HandlerFunc` type is a function type with a `ServeHTTP` method that allows it to implement the `Handler` interface
```Go
type HandlerFunc func(ResponseWrite, *Request)

func (f *HandlerFunc) ServeHTTP(ResponseWrite, *Request) {
	f(w, r)
}
```
- This allows us to wrap other functions with `HandlerFunc` converting their type into one that implements `Handler`, which is quite handy in handlers and middlewares in web dev
```Go
func (cfg *apiConfig) middlewareMetricsInc(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		cfg.fileserverHits.Add(1)
		next.ServeHTTP(w, r)
	})
}
```