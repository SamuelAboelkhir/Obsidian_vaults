---
tags:
- Go
- General
- Programming-Language
MOC: Programming
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Go index|Back to index]]

# Go for backend development
- Go is very good at creating http servers, due to it being very portable (compiling into a binary), and thanks to its goroutines allowing for handling multiple requests simultaneously using concurrency, which utilizes a machine's various threads, but are lighter weight than traditional multi-threading
## Creating a server
### Server multiplexer
- To create a server, you need a server multiplexer (basically the same idea as a controller, where you can define different routes). You can make a mux struct with `http.NewServeMux`
- A mux can then be assigned handlers for different routes, which are basically just structs that satisfy the `handler` interface, which run functions that return the desired http response to whoever hits the route
- In the case of a file server, Go provides a default `http.FilerServer` handler, which can be passed a local pass to the static asset to be served, and assigned to a certain http route, where the route is an actual path on the host's filesystem, that you can specifiy with `http.Dir`
- You can name the path anything you want, even if that path doesn't exist on the host machine, as along as your use `http.StripPrefix` to remove the fake path (that's meant to be shown in the URL only) and pass the true path to the handler
- You can serve your mux with all of its paths and handlers by passing it to an `http.Server` struct
### Server
- In that struct you can also specify the port to use for exposing the server to the internet
- An instantiated server can start listening for incoming requests via its `server.ListenAndServe` method, which will cause the main function's goroutine to block until the server is shut down
	- Servers are like `repl` in that way, where no matter what, the program is not allowed to exit on its own
```Go
package main

import (
	"net/http"
	"sync/atomic"
)

func main() {
	mux := http.NewServeMux()
	dir := http.Dir(".")
	cfg := apiConfig{
		fileserverHits: atomic.Int32{},
	}
	mux.Handle("/app", cfg.middlewareMetricsInc(http.StripPrefix("/app", http.FileServer(dir))))

	mux.HandleFunc("GET /healthz", customHandler)
	mux.HandleFunc("GET /metrics", cfg.middlewareMetricsGet())
	mux.HandleFunc("POST /reset", cfg.middlewareMetricsReset(resetHandler))

	httpServer := &http.Server{
		Addr:    ":8080",
		Handler: mux,
	}

	startServer(httpServer)
}

func startServer(server *http.Server) {
	server.ListenAndServe()
}
```
## Fileservers
- Those are simple web servers that serve static files from the host machine
- Static assets are things like
	- HTML
	- CSS
	- JavaScript
	- Images
- By default, the http convention is that any route is implicitly terminated with an `index.html`, unless otherwise specified
- Go's standard library's `FileServer` can find and load `index.html` files automatically
### Handlers
- The file server method in Go, returns a full on handler type
- A struct is considered a handler, if it has a `ServeHTTP` method
- When creating a custom handler, we can either satisfy the handler constraint by creating a struct with a `ServeHTTP` or, we can use `http.HandlerFunc` which only focuses on the `ServeHTTP` function itself
	- This means that instead of a full handler, `http.HandlerFunc` only expects a function, unnamed or otherwise, matching the function signature of `ServeHTTP` `func(w http.ResponseWriter, r *http.Request)`
```Go
type Handler interface {
	ServeHTTP(ResponseWriter, *Request)
}
```
#### When to use either?
- As per bootdev
	- You will typically use a `Handler` for more complex use cases, such as when you want to implement a custom router, middleware, or other custom logic.
	- You'll typically use a `HandlerFunc` when you want to implement a simple handler. The `HandlerFunc` type is just a function that matches the `ServeHTTP` signature above.
### Middleware
- Since Go support higher order functions, that gives us a cool way of implementing middleware
- A middleware is basically a wrapper function for a handler
- It receives the request, and does some actions on it, before passing it to the handler itself
- A middleware is good for writing DRY code, if we wanted for example to do a check that a user is logged in and authenticated before every request, or if we wanted to adjust the http request or response in some way, or even if we wanted to gather telemetry about the server by keeping track of the number of times a certain endpoint was visited
```Go
func middlewareLog(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		log.Printf("%s %s", r.Method, r.URL.Path)
		next.ServeHTTP(w, r)
	})
}

mux.Handle("/app/", middlewareLog(handler))
```
## Routing
- Not only can we specify a route on the mux with a path and a handler, but we can also limit the HTTP methods that are usable there
```Go
mux.HandleFunc("POST /articles", handlerArticlesCreate)
mux.HandleFunc("DELETE /articles", handlerArticlesDelete)
```
### Patterns
- It's important to note that `ServeMux` follows some set patterns when it comes to dispatching requests to the appropriate route paths
- A pattern looks like this `[METHOD ][HOST]/[PATH]` where all three parts are optional
#### Rules
- The rules of the patterns are as follows
##### Fixed URL Paths
- A pattern that exactly matches the URL path. 
- For example, if you have a pattern `/about`, it will match the URL path `/about` and no other paths.
##### Subtree Paths
- If a pattern ends with a slash `/`, it matches all URL paths that have the same prefix. 
- For example, a pattern `/images/` matches `/images/`, `/images/logo.png`, and `/images/css/style.css`. As we saw with our `/app/` path, this is useful for serving a directory of static files or for structuring your application into sub-sections.
##### Longest Match Wins
- If more than one pattern matches a request path, the longest match is chosen. 
	- This allows more specific handlers to override more general ones. 
- For example, if you have patterns `/` (root) and `/images/`, and the request path is `/images/logo.png`, the `/images/` handler will be used because it's the longest match.
##### Host-Specific Patterns
- Patterns can also start with a hostname (e.g., `www.example.com/`). 
- This allows you to serve different content based on the Host header of the request. 
- If both host-specific and non-host-specific patterns match, the host-specific pattern takes precedence.