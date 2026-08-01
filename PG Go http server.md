---
tags:
- Go
- Programming-Language
MOC: Programming
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Go index|Back to index]]

# Go for backend development
- Go is very good at creating http servers, due to it being very portable (compiling into a binary), and thanks to its goroutines allowing for handling multiple requests simultaneously using concurrency, which utilizes a machine's various threads, but are lighter weight than traditional multi-threading
## Goroutines in servers
- In Go, _goroutines_ are used to serve _many_ requests at the same time, but not all servers are quite so performant.
- Go was built by Google, and one of the purposes of its creation was to power Google's massive web infrastructure. Go's goroutines are a great fit for web servers because they're lighter weight than operating system threads, but still take advantage of multiple cores. Let's compare a Go web server's concurrency model to other popular languages and frameworks.
### Node.js / Express.js
- In JavaScript land, servers are typically single-threaded. A [Node.js](https://nodejs.org/en/) server (often using the [Express](https://expressjs.com/) framework) only uses one CPU core at a time. It can still handle many requests at once by using an [async event loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Event_loop). That just means whenever a request has to wait on I/O (like to a database), the server puts it on pause and does something else for a bit.
![[Pasted image 20260727213710.png]]
- This might sound _horribly_ inefficient, but it's not _too_ bad. Node servers do just fine with the I/O workloads associated with most CRUD apps (Where processing is offloaded to the Database). You only start to run into trouble with this model when you need your server to do CPU-intensive work
### Takeaways
- Go servers are great for performance whether the workload is I/O _or_ CPU-bound
- Node.js and Express work well for I/O-bound tasks, but struggle with CPU-bound tasks
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
## Architecture
### Monolith
- A monolithic application is a single large program that contains all of the functionality of the front and back ends
- Sometimes monoliths host a REST API for raw data (like JSON data) within a subpath, like `/api` as shown in the image. 
- That said, there are even more tightly coupled kinds of monoliths that inject the dynamic data directly into the HTML as well. 
- The nice thing about separate data endpoints is that they can be consumed by any client, (like a mobile app) and not just the website. 
- That said, injection is typically more performant, so it's a trade-off. WordPress and other web_site_ builders typically work this way.
![[Pasted image 20260726222800.png]]
#### Pros for Monoliths
- Simpler to get started with
- Easier to deploy new versions because everything is always in sync
- In the case of the data being embedded in the HTML, the performance can result in better UX and SEO
#### Monolithic Deployment
- Deploying a monolith is straightforward. 
- Because your server is just one program, you just need to get it running on a server that's exposed to the internet and point your DNS records to it.
- You could upload and run it on classic server, something like:
	- AWS EC2
	- GCP Compute Engine (GCE)
	- Digital Ocean Droplets
	- Azure Virtual Machines
- Alternatively, you could use a platform that's specifically designed to run web applications, like:
	- Heroku
	- Google App Engine
	- Fly.io
	- AWS Elastic Beanstalk
### Decoupled
- A "decoupled" architecture is one where the front-end and back-end are separated into different codebases. 
- For example, the front-end might be hosted by a static file server on one domain, and the back-end might be hosted on a subdomain by a different server.
- Depending on whether or not a load balancer is sitting in front of a decoupled architecture, the API server might be hosted on a separate domain (as shown in the image) _or_ on a subpath, as shown in the monolithic architecture. 
- A decoupled architecture allows for either approach.
![[Pasted image 20260726222913.png]]
#### Pros for Decoupled Architectures
- Easier, and probably cheaper, to scale as traffic grows
- Easier to practice good separation of concerns as the codebase grows
- Can be hosted on separate servers and using separate technologies
- Embedding data in the HTML is still possible with pre-rendering (similar to how Next.js works), it's just more complicated
### Best of both worlds
- It's normally a good idea when building a new app from scratch to start with a monolith while keeping the frontend and backend logically decoupled
- This makes it easy to get started, and decouple later as needed
#### Decoupled Deployment
- With a decoupled architecture, you have _two_ different programs that need to be deployed. 
- You would typically deploy your _back-end_ to the same kinds of places you would deploy a monolith.
- For your front-end server, you can do the same, _or_ you can use a platform that's specifically designed to host static files and server-side rendered front-end apps, something like:
	- Vercel
	- Netlify
	- GitHub Pages
- Because the front-end bundle is likely just static files, you can host it easily on a [CDN (Content Delivery Network)](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/) inexpensively.
## JSON
- Nothing exactly new will be mentioned here as this part is also covered in [[PG Go http client#Parsing JSON into a byte slice]], however, to reconfirm the process
### Decode JSON Request Body
- It's _very_ common for `POST` requests to send JSON data in the request body. Here's how you can handle that incoming data:
```json
{
  "name": "John",
  "age": 30
}
```
```go
func handler(w http.ResponseWriter, r *http.Request){
    type parameters struct {
        Name string `json:"name"`
        Age int `json:"age"`
    }

    decoder := json.NewDecoder(r.Body)
    params := parameters{}
    err := decoder.Decode(&params)
    if err != nil {
		log.Printf("Error decoding parameters: %s", err)
		w.WriteHeader(500)
		return
    }
    // params is a struct with data populated successfully
    // ...
}
```

- The struct tags (e.g., `` `json:"name"` ``) indicate how the keys in the JSON should be mapped to the struct fields. The struct fields themselves must be exported (start with a capital letter) if you want them parsed.
- `decoder.Decode()` will return an error if the JSON is invalid or has the wrong types, and any missing fields will simply have their values in the struct set to their zero value.
### Encode JSON Response Body
```go
func handler(w http.ResponseWriter, r *http.Request){
    // ...

    type returnVals struct {
        CreatedAt time.Time `json:"created_at"`
        ID int `json:"id"`
    }
    respBody := returnVals{
        CreatedAt: time.Now(),
        ID: 123,
    }
    dat, err := json.Marshal(respBody)
	if err != nil {
			log.Printf("Error marshalling JSON: %s", err)
			w.WriteHeader(500)
			return
	}
    w.Header().Set("Content-Type", "application/json")
    w.WriteHeader(200)
    w.Write(dat)
}
```
- Again, we use struct tags to specify how the field names will be encoded in the JSON data. If you omit the tags, the keys will be encoded as the same names of struct fields (e.g., `CreatedAt`, `ID`).
## Storage
### Memory vs. Disk
- When you run a program on your computer (like our HTTP server), the program is loaded into _memory_
- Memory is a lot like a scratch pad. It's fast, but it's not permanent. If the program terminates or restarts, the data in memory is _lost_
- When you're building a web server, any data you store in memory (in your program's variables) is lost when the server is restarted. 
- Any important data needs to be saved to disk via the file system
#### Option 1: Raw Files
- We _could_ take our user's data, serialize it to JSON, and save it to disk in `.json` files (or any other format for that matter)
- It's simple, and will even work for small applications. 
- Trouble is, it will run into problems _fast_:
	- **Concurrency:** If two requests try to write to the same file at the same time, you'll get overwritten data.
	- **Scalability:** It's not efficient to read and write large files to disk for every request.
	- **Complexity:** You'll have to write a lot of code to manage the files, and the chances of bugs are high.
#### Option 2: Database
- At the end of the day, a database technology like MySQL, PostgreSQL, or MongoDB "just" writes files to disk
- The difference is that they _also_ come with all the fancy code and algorithms that make managing those files efficient and safe. In the case of a SQL database, the files are abstracted away from us entirely. 
- You just write SQL queries and let the DB handle the rest
##### Setting up postgres in Go
###### The DB driver
- First, you need to make sure the actual DB driver is on your system (or in docker)
- Postgres uses `psql` as its CLI tool, which we can use to interact with it easily without needing a GUI
- We will also need to update postgres's password when installed directly on our machine (Linux only requirement though)
	- `sudo passwd postgres`
- Start the service `sudo service postgresql start`
- After connecting with `sudo -u postgres psql` the next prompt we will see is `postgres=#`
- Now we create a new DB `CREATE DATABASE chirpy;`
- And connect to it `\c chirpy`
- The prompt becomes `chirpy=#`
- Again, a Linux only requirement is to set the user password for the DB, so `ALTER USER postgres WITH PASSWORD 'postgres';`
- Now we can query the DB `SELECT version();`
- Postgres also has an annoying quirk in Go, where we have to install the postgres driver in Go, which is expected `go get github.com/lib/pq`, but then, we also have to import it in `main.go` even though we wont use it directly `import _ "github.com/lib/pq"` where the underscore in the import tells Go that you want to import this lib for its side effects, not to actually use it
- It's then a good idea to have the connection string in your `.env` file `DB_URL="YOUR_CONNECTION_STRING_HERE"`
- You can then import it like so `dbURL := os.Getenv("DB_URL")`
- Finally, you have to open a connection to the DB from Go `db, err := sql.Open("postgres", dbURL)`
###### Migrations with Goose
- [Goose](https://github.com/pressly/goose) is a database migration tool written in Go. 
- It runs migrations from a set of SQL files, making it a perfect fit for this project (we wanna stay close to the raw SQL).
- `go install github.com/pressly/goose/v3/cmd/goose@latest`
- A migration file in goose is a normal SQL file with a couple of special comments
- The conventional naming scheme is `<number>_<name>.sql`
```sql
-- +goose Up
CREATE TABLE users (
	id UUID PRIMARY KEY, 
	created_at TIMESTAMP NOT NULL, 
	updated_at TIMESTAMP NOT NULL, 
	email TEXT UNIQUE NOT NULL
);


-- +goose Down
DROP TABLE users;
```
- Now, to use goose, we have two ways
- With the connection string
	- `goose postgres "postgres://chirpy:chirpy@localhost:5432/chirpy" up`
- With CLI variables 
	- `GOOSE_DRIVER=postgres GOOSE_DBSTRING="postgres://chirpy:chirpy@localhost:5432/chirpy" GOOSE_MIGRATION_DIR="./sql/schema" goose up`
###### Generating Go code from SQL with SQLC
- [SQLC](https://sqlc.dev/) is an _amazing_ Go program that generates Go code from SQL queries. 
- It's not exactly an [ORM](https://www.freecodecamp.org/news/what-is-an-orm-the-meaning-of-object-relational-mapping-database-tools/), but rather a tool that makes working with raw SQL easy and type-safe.
- `go install github.com/sqlc-dev/sqlc/cmd/sqlc@latest`
- SQLC is always run from the root of the project, and requires a `sqlc.yaml` file to configure it
```yaml
version: "2"
sql:
  - schema: "sql/schema"
    queries: "sql/queries"
    engine: "postgresql"
    gen:
      go:
        out: "internal/database"
```
- We're telling SQLC to look in the `sql/schema` directory for our schema structure (which is the same set of files that Goose uses, but `sqlc` automatically ignores "down" migrations), and in the `sql/queries` directory for queries. 
- We're also telling it to generate Go code in the `internal/database` directory.
- SQLC also needs some special comments to tell it how to generate the Go code for the relevant SQL command
```sql
-- name: CreateUser :one
INSERT INTO users (id, created_at, updated_at, email, hashed_password)
VALUES (
    gen_random_uuid(),
    NOW(),
    NOW(),
    $1,
    $2
)
RETURNING *;
```
- Where `:one` means that we're returning one value
- Other options are `:many` for queries returning many values, and `:exec` for queries with no return data
- Also make sure to use `RETURNING *` for queries that don't return data by default, such as insertions and updates. Select returns by default though
- Keep the [SQLC postgres docs](https://docs.sqlc.dev/en/latest/tutorials/getting-started-postgresql.html) handy, you'll probably need to refer to them again later.
- Running SQLC is then as easy as typing `sqlc generate`
- After running `sqlc generate` the first time, a `*database.Queries` pointer will be created, and it's a good idea to store it in an `apiConfig` struct to have access to it anywhere in the codebase `dbQueries := database.New(db)`
- You'll need to maintain access to the config then either by creating all your functions as methods to the config struct, or by passing a pointer to that struct as an argument to your functions
## Context
- The [`context` package](https://pkg.go.dev/context) in Go's standard library is used to pass request-scoped information through your program.
- In HTTP servers, the most important parts are **cancellation** and **timeouts**
- When a client disconnects, a request times out, or the server shuts down, the request's context can tell the rest of your code to stop working on that request
### Request context
- Every `http.Request` has a context 
```go
ctx := r.Context()
```
- That context _belongs to the current HTTP request_. If the request is canceled, `ctx` is canceled too
### Database Calls
- Many database APIs accept a [`context.Context`](https://pkg.go.dev/context#Context) as their first argument. 
- SQLC-generated methods are no exception:
```go
user, err := cfg.db.CreateUser(ctx, params.Email)
```
- By passing the request context to the database call, the database work is tied to the lifetime of the HTTP request. 
- If the client gives up before the query finishes, the query can be canceled instead of wasting server resources
- In a handler, this usually means passing `r.Context()` directly:
```go
user, err := cfg.db.CreateUser(r.Context(), params.Email)
```
### Background Context
- You'll also see [`context.Background()`](https://pkg.go.dev/context#Background) in Go code. 
- It's useful when a `Context` is expected but there's no incoming request or parent operation to start from – like in startup code or a background job.
- For web handlers, prefer `r.Context()`. 
- It carries the cancellation signal for the specific request you're handling.
## Collections and Singletons
- The Go http servers course, Chirp, by boot.dev is a _fairly_ [RESTful API](https://restfulapi.net/).
- REST is a set of guidelines for how to build APIs. 
- It's not a standard, but it's a set of conventions that many people follow. 
- Not all back-end APIs are RESTful, but many are. 
- As a back-end developer, you'll need to know how to build RESTful APIs.
### Resource Naming Conventions
- In REST, it's conventional to name all of your endpoints after the resource that they represent and for the name to be plural. 
- That's why we use `POST /api/chirps` to create a new chirp instead of `POST /api/chirp`.
- To get a collection of resources it's conventional to use a `GET` request to the plural name of the resource. 
- So we are going to use `GET /api/chirps` to get all of the chirps.
- To get a _singleton_, or a _single instance_ of a resource, it's conventional to use a `GET` request to the plural name of the resource, followed by the `ID` of the resource. 
- So we are going to use `GET /api/chirps/94b7e44c-3604-42e3-bef7-ebfcc3efff8f` to get the chirp with ID `94b7e44c-3604-42e3-bef7-ebfcc3efff8f`.
## Authentication With Passwords
- Authentication is the process of verifying _who_ a user is. 
- If you don't have a secure authentication system, your back-end systems will be open to attack!
- Imagine if I could make an HTTP request to the YouTube API and upload a video to _your_ channel. 
- YouTube's authentication system prevents this from happening by verifying that I am who I say I am.
### Passwords
- Passwords are a common way to authenticate users. You know how they work: When a user signs up for a new account, they choose a password. 
- When they log in, they enter their password again. 
- The server will then compare the password they entered with the password that was stored in the database.
- There are 2 _really important_ things to consider when storing passwords:
	1. **Storing passwords in plain text is awful.** If someone gets access to your database, they will be able to see all of your users' passwords. If you store passwords in plain text, you are giving away your users' passwords to anyone who gets access to your database.
	2. **Password strength matters.** If you allow users to choose weak passwords, they will be more likely to reuse the same password on other websites. If someone gets access to your database, they will be able to log in to your users' other accounts.
### Hashing
- On the other hand, we _will_ be writing code to store passwords in a way that prevents them from being read by anyone who gets access to your database. 
- This is called _hashing_. Hashing is a one-way function. 
- It takes a string as input and produces a string as output. 
- The output string is called a _hash_.
## Types of Authentication
- Here are a few of the most common authentication methods you'll see in the wild:
	1. Password + ID (username, email, etc.)
	2. 3rd Party Authentication ("Sign in with Google", "Sign in with GitHub", etc)
	3. Magic Links
	4. API Keys
### 1. Password + ID
- This is the most common type of authentication that requires a manual login from a user. 
- When users use password managers, it's one of the more secure ways to authenticate users. 
- Unfortunately, many users don't, so it's not as secure as it could be.
- That said, it's a valid choice.
### 2. 3rd Party Authentication
- 3rd party authentication is a way to authenticate users using a service like Google or GitHub. 
- 3rd party auth is great for user experience because it allows users to use their existing accounts to log in to your app, lowering friction.
- It's also nice because you don't need to worry about storing passwords yourself, meaning you can outsource the security of your users' passwords to a company that, _hopefully_, does a good job.
- The only real drawbacks to 3rd party auth is that you're trusting a 3rd party and if your users don't have an account with that 3rd party, they won't be able to log in.
### 3. Magic Links
- Magic links are a way to authenticate users without a password. 
- It relies on the assumption that the user's email is something that they have unique access to.
- The webserver sends a link to the user's email and encodes a unique token in that link. When the user clicks the link, the webserver can decode the token and use it to authenticate the user. Eg:
	- `https://example.com/login?token=...`
### 4. API Keys
- API keys are a fantastic way to authenticate users and systems programmatically. 
- An API Key is just a long, secure string that uniquely identifies a user or system, and that can't be guessed. 
- Because they're intended to be used in code, they don't need to be memorized and, as such, can be much longer and double as an identifier. 
- An API key might look something like this:
	- `bd_JDS543J3n5NMKspDXNRlowiqw523lKHK32K43kl`
## JWTs
- There are several different ways to handle authentication. 
- We'll use [JWTs](https://www.boot.dev/blog/backend/hmac-and-macs-in-jwts/) in this course. 
- They're a popular choice for APIs that are consumed by web applications and mobile apps.
### What Is a JWT?
- A JWT is a JSON Web Token. 
- It's a cryptographically signed JSON object that contains information about the user.
- You'll learn about how the cryptography of JWTs work in our [Learn Cryptography](https://boot.dev/courses/learn-cryptography) course, for now, it's just important to know that once the token is created by the server, the data in the token can't be changed without the server knowing.
- _When your server issues a JWT to Bob, Bob can use that token to make requests as Bob to your API. 
- Bob won't be able to change the token to make requests as  _Alice._
- Remember that JWTs:
	- Can't be changed due to the digital signature
	- Are not encrypted, and are viewable by anyone, so never store sensitive data in them
![[Pasted image 20260726233843.png]]
### Revoking JWTs
- One of the main benefits of JWTs is that they're _stateless_. The server doesn't need to keep track of which users are logged in via JWT. The server just needs to issue a JWT to a user and the user can use that JWT to authenticate themselves. Statelessness is _fast and scalable_ because your server doesn't need to consult a database to see if a user is currently logged in.
- However, that same benefit poses a potential problem. JWTs can't be revoked. If a user's JWT is stolen, there's no easy way to stop the JWT from being used. JWTs are just a signed string of text.
- The JWTs we've been using so far are more specifically _access tokens_. Access tokens are used to authenticate a user to a server, and they provide _access_ to protected resources. Access tokens are:
	- Stateless
	- Short-lived (15m-24h)
	- Irrevocable
- They _must_ be short-lived because they can't be revoked. The shorter the lifespan, the more secure they are. Trouble is, this can create a poor user experience. We don't want users to have to log in every 15 minutes.
#### A Solution: Refresh Tokens
- Refresh tokens don't provide access to resources directly, but they can be used to get new access tokens. Refresh tokens are much longer lived, and importantly, they _can_ be revoked. They are:
	- Statefull
	- Long-lived (24h-60d)
	- Revocable
- Now we get the best of both worlds! Our endpoints and servers that provide access to protected resources can use access tokens, which are fast, stateless, simple, and scalable. On the other hand, refresh tokens are used to keep users logged in for longer periods of time, and they can be revoked if a user's access token is compromised.
## Cookies
### What Is an HTTP Cookie?
- A cookie is a small piece of data that a server sends to a client. The client then dutifully stores the cookie and sends it back to the server on subsequent requests.
- Cookies can store any arbitrary data:
	- A user's name or other tracking information
	- A JWT (refresh and access tokens)
	- Items in a shopping cart
	- etc.
- The server decides _what_ to put in a cookie, and the client's job is simply to store it and send it back.
### How Do Cookies Work?
- Simply put, cookies work through HTTP headers.
- Cookies are sent from the server to the client in the `Set-Cookie` header. Cookies are most popular for web (browser-based) applications because browsers _automatically_ send any cookies they have back to the server in the `Cookie` header.
### Why Aren't We Using Cookies?
- Simply put, Chirpy's API is designed to be consumed by mobile apps and other servers. Cookies are primarily for browsers.
- A good use-case for cookies is to serve as a more strict and secure transport layer for JWTs within the context of a browser-based application.
- For example, when using [httpOnly cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies#block_access_to_your_cookies), you can ensure that 3rd party JavaScript that's being executed on your website can't access any cookies. That's a lot better than storing JWTs in the browser's local storage, where it's easily accessible to any JavaScript running on the page.
## Webhooks
- A webhook is just an event that's sent to your server by an external service. There are just a couple of things to keep in mind when building a webhook handler:
	- The third-party system will probably retry requests multiple times, so your handler should be [idempotent](https://en.wikipedia.org/wiki/Idempotence).
	- Be extra careful to never "acknowledge" a webhook request unless you processed it successfully. By sending a `2XX` code, you're telling the third-party system that you processed the request successfully, and they'll stop retrying it.
	- When you're writing a server, you typically get to define the API. However, when you're integrating a webhook from a service like Stripe, you'll probably need to adhere to their API: they'll tell you what shape the events will be sent in.
## API Keys
- A key that an API can provide with its requests to prove its identity