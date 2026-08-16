---
tags:
- Projects
MOC: Personal
---
[[_0000 Home|Home]] | [[_0003 Personal MOC]] | [[PER Project ideas index|Back to index]]

# CompGoR
## The dream
- This project is going to be a reflection of my education, of everything I know
- It will combine
	- Biology
	- Chemistry
	- Data sciences
	- Bioinformatics
	- Medical sciences
	- Full stack development
	- Low level concepts
	- High level concepts
	- Polyglot architecture
- And yet, I have no idea what I'm even building, I just know that I want to build it
- I expect this project to live with me for a quite a while
## The stack
- As of 03-05-2026
	- Go 1.26.2
	- R 4.5.3
	- RStudio
```R
> version
               _                           
platform       x86_64-pc-linux-gnu         
arch           x86_64                      
os             linux-gnu                   
system         x86_64, linux-gnu           
status                                     
major          4                           
minor          5.3                         
year           2026                        
month          03                          
day            11                          
svn rev        89597                       
language       R                           
version.string R version 4.5.3 (2026-03-11)
nickname       Reassured Reassurer
```
## Current execution
- The project currently is a repl CLI tool that utilizes Go as an HTTP client querying PubChem's REST API to fetch chemical data
- The core function so far is a query builder that takes user input and assembles it into a valid PubChem URL where the data is fetched from
- The app also features 
	- A caching system that make use of Go's concurrency
	- A generic API client (I call it the postman) that can fetch any generic data
	- Wrapper functions around the generic API client that properly type the data
	- A command interface that any new command must satisfy
	- A commands struct that stores a map of all the available commands
## Future ideas
- Add a postgres database that optionally stores data based on user input
- A dockerized R server in a small private network with the main Go system for that handles data analytics
- Various scientific tools
- A clients interface that allows for extending the app with clients for connecting with different data sources
- A generic query builder, and dedicated query builders for very specific data sources (like PubChem)
- Proper database connection handling (retry logic, disconnection logic)
- Request rate limiting, as to not accidentally crash any API


# Lessons learned - (summarized by boots)
# CompGoR Project: Lessons Learned & Architectural Journey
## 1. Architecture & Design Patterns
- **Modular Internal Structure:** Organized the application into distinct internal packages (`commands`, `clients`, `cache`, `config`) to maintain a clean separation of concerns and avoid circular dependencies.
- **The Command Pattern:** Implemented a unified `Command` interface (`Execute`, `Name`, `Help`) managed by a central `Commands` registry. This allows the REPL to dispatch commands dynamically and makes adding new features frictionless.
- **Client Registry & Polymorphism:** Developed a `Client` interface and a `Clients` registry, allowing the app to manage multiple API clients (e.g., a specialized `PubChem` client vs. a generic `default` client) while abstracting away their underlying protocols.
## 2. Concurrency, OS Pipes, and `os/exec`
- **Process Piping (`os/exec`):** Mastered wiring external OS utilities together by piping `StdoutPipe()` into `Stdin`. 
- **Async Execution Hygiene:** Learned that `Start()` launches background OS processes asynchronously (similar to `async/await`), and calling `Wait()` is mandatory for two critical reasons:
  1. **Synchronization:** Prevents reading from streams before data is fully written.
  2. **OS Hygiene:** Reaps the process exit status and releases file descriptors, preventing zombie processes.
## 3. Terminal REPLs & Third-Party Dependencies
- **The "Build vs. Buy" Balance:** Wrestled with the philosophy of external dependencies. While writing everything from scratch maximizes learning, complex infrastructural domains (like raw-mode terminal handling and ANSI escape sequences) are best solved with robust community tools.
- **Integration of `github.com/chzyer/readline`:** Replaced manual `bufio.Scanner` loops with `readline` to natively gain:
  - Arrow-key command history persistence (tied directly to `.history`).
  - Tab-completion via `readline.NewPrefixCompleter`.
  - Clean raw-mode input handling without managing low-level terminal flags.
## 4. Go Idioms & Gotchas
- **File Pointer Management:** Discovered the trap of `os.O_APPEND`, which automatically places the file pointer at the end of the file. To read existing history at startup, explicit re-positioning via `file.Seek(0, 0)` is required before scanning.
- **Map Type Assertions:** Remembered that pulling an interface out of a `map[string]Client` yields the interface type. To access concrete methods (like `GetCompounds` on `*HTTPClient`), a two-step lookup and type assertion (`clientGeneric.(*clients.HTTPClient)`) is required.
- **Struct-Based Configuration vs. Global Constants:** Transitioned away from package-level configuration constants in favor of instance fields on structs (like `baseURL` and `timeout`), ensuring true encapsulation and reusability for client factories.
## 5. Statistics & Systems Integration (The Polyglot Mindset)
- **Probability Distributions (PDF vs. CDF vs. Quantile):** 
  - **PDF (`dbeta`):** The "height" or density of a curve at a single point (rate of change).
  - **CDF (`pbeta`):** The "running total" or accumulated area from the left up to $x$ (Probability lookup: Value $\rightarrow$ Area).
  - **Quantile (`qbeta`):** The inverse lookup (Percentile/Quota $\rightarrow$ Value).
- **Bayes' Rule & Base Rates:** Derived Positive Predictive Value (PPV) from first principles, proving why rare-event detection (like medical testing or security intrusion alerts) is easily swamped by false positives if the base rate is exceptionally low.
- **Tail Latency (p99):** Connected statistical percentiles to API performance testing, realizing that the "long tail" of latency matters far more than the average (p50) when scaling to millions of requests.
## 6. HTTP Networking & API Integration

- **RESTful Client Design:** Engineered custom HTTP clients with explicit timeouts, custom user-agent headers, and robust error handling rather than relying on default global clients.
- **JSON Marshaling and Unmarshaling:** Mastered converting complex JSON payloads into strongly-typed Go structs using `encoding/json`, handling nested structures, and dealing with dynamic or optional fields.
- **Query Parameter Construction:** Built dynamic URL query builders (like the PubChem query assembler) using `net/url` to cleanly encode parameters and avoid malformed request strings.
## 7. Caching & State Management

- **In-Memory Caching:** Implemented thread-safe caching layers (leveraging Go's `sync.RWMutex`) to store API responses, minimizing redundant network calls and respecting rate limits.
- **TTL (Time-To-Live) Invalidation:** Designed expiration strategies for cached items to ensure data freshness while retaining performance benefits.
- When passing fields to structs, the choice of passing a value of pointer depends on whether or not the value being passed can be copied, or if it has to persist
	- For example, a mutex must absolutely not be copied, it should be always the same value
- When passing a pointer to a struct, the struct can be passed as either a value or pointer, that's fine because the underlying pointers are just memory addresses, and even if copied, they will be pointing to the same thing
- If the value in the st
## 8. Error Handling & Robustness

- **Custom Error Types:** Defined domain-specific error types and sentinel errors to allow the REPL and command handlers to make intelligent decisions based on _why_ an operation failed (e.g., distinguishing a network timeout from a 404 Not Found or a bad user input).
- **Graceful Degradation:** Ensured that non-critical failures (like failing to write a history file or missing a cache hit) log warnings rather than crashing the interactive shell.