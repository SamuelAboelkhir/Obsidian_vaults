---
tags: 
- Other
MOC: Programming
---

[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Other index|Back to index]]
# SOLID Principles - summarized from an AI discussion

SOLID originated in **object-oriented programming**, especially class/inheritance-based design, but the underlying ideas apply well to Go. Go expresses them primarily through **interfaces, composition, and dependency injection** rather than inheritance.

The principles are less about following rigid rules and more about **managing coupling, change, and responsibilities**.

---

## S — Single Responsibility Principle

> **A type/function/package should have one responsibility — more precisely, one reason to change.**

It's not necessarily "one function = one line of work." A type can have multiple methods as long as they collectively serve one coherent responsibility.

For example, a `PubChemQueryBuilder` can reasonably be responsible for the **PubChem query use case**, even if that involves:

1. collecting input
2. constructing a query
3. calling PubChem
4. processing the result
5. presenting the result

However, extracting those individual steps into functions can improve readability, testing, and separation of concerns.

**Key idea:** Think in terms of **cohesion**, not arbitrary function/type size.

---

## O — Open/Closed Principle

> **Software should be open for extension but closed for modification.**

The idea is that adding new behavior shouldn't require continually modifying existing, stable code.

Inheritance is one way to achieve this in OOP, but Go has other mechanisms.

Your command registry is a good example:

```go
type Command interface {
    Execute(...) error
    Name() string
    Help() string
}
```

You can add:

```go
type SearchCommand struct{}
```

and register it:

```go
commands.Register("search", &SearchCommand{})
```

without modifying the registry itself.

Go commonly achieves OCP through:

* interfaces
* composition
* registration
* dependency injection
* function parameters

---

## L — Liskov Substitution Principle

The textbook definition:

> "Derived classes must be able to replace their base classes."

is **misleading when taken literally**, especially in Go.

A better definition:

> **An implementation must honor the behavioral contract of the abstraction it claims to implement.**

Or, in Go terms:

> **Don't lie with your interfaces.**

Suppose:

```go
type Sender interface {
    Send(data []byte) error
}
```

If consumers understand `Send` to mean:

```text
connect → send → disconnect
```

then a WebSocket implementation that:

```text
connect → send → keep connection open
```

may technically satisfy the interface but violate its behavioral contract.

The solution may be to distinguish the abstractions:

```go
type OneShotSender interface {
    Send(data []byte) error
}

type PersistentConnection interface {
    Connect() error
    Send(data []byte) error
    Disconnect() error
}
```

LSP is therefore primarily about **behavioral substitutability**, not whether two types have identical capabilities.

### Important distinction

If:

```go
type Payment interface {
    Pay(float64) error
}
```

then both `CreditCard` and `PayPal` can substitute for `Payment` **for code that only relies on `Pay`**.

This does **not** mean PayPal must support every operation that CreditCard supports.

The consumer defines what abstraction it requires.

In Go, LSP is therefore better understood as:

```text
Interface = behavioral contract
Implementation = must honor that contract
```

---

## I — Interface Segregation Principle

> **Don't force consumers to depend on functionality they don't need.**

Instead of creating one enormous interface:

```go
type Client interface {
    Connect()
    Disconnect()
    GetCompounds()
    GetGenes()
    Subscribe()
    SendWebhook()
    ...
}
```

prefer small, capability-specific interfaces:

```go
type Connectable interface {
    Connect() error
    Disconnect() error
}

type CompoundProvider interface {
    GetCompounds(...) (...)
}

type StreamProvider interface {
    Subscribe(...) (...)
}
```

This is particularly important in Go because **interfaces are often defined around what the consumer needs**, rather than around everything an implementation can do.

Your client architecture is a good example: different external clients may share some capabilities without actually being the same kind of thing.

### Mental model

Don't give every client a giant "everything a client might ever do" interface.

Give consumers the **smallest contract they actually require**.

---

## D — Dependency Inversion Principle

> **High-level code should not depend directly on low-level implementation details. Both should depend on abstractions.**

Your command registry already demonstrates this:

```go
type Commands struct {
    RegisteredCommands map[string]Command
}
```

The registry depends on:

```go
Command
```

rather than:

```go
PubChemQueryBuilder
FetchJSONFromURL
ShowTable
```

The concrete commands satisfy the abstraction.

Similarly, ideally your PubChem command would depend on something like:

```go
type CompoundProvider interface {
    GetCompounds(...) (Compounds, error)
}
```

rather than:

```go
*clients.HTTPClient
```

Then:

```text
PubChemQueryCommand
        │
        ▼
 CompoundProvider
        ▲
        │
   HTTPClient
```

The high-level application logic knows **what capability it needs**, not **how that capability is implemented**.

---

# SOLID as a whole

The principles aren't five isolated rules. They reinforce each other.

For your project, they might interact like this:

```text
                    Command
                       │
                  needs a capability
                       │
                       ▼
              CompoundProvider
                       ▲
                       │
                  HTTPClient
```

### SRP

The command has a coherent responsibility: execute the PubChem query use case.

### OCP

You can add another command or another implementation without modifying the registry/use case unnecessarily.

### LSP

Anything claiming to be a `CompoundProvider` must actually behave like one.

### ISP

`CompoundProvider` should contain only compound-related capabilities rather than every capability a client might have.

### DIP

The command depends on `CompoundProvider`, not directly on `HTTPClient`.

---

# Most important takeaway for Go

**SOLID is not a checklist.**

Don't create:

```text
Controller
Service
Repository
Factory
Manager
Provider
Handler
Interface
AbstractFactory
```

just because a textbook says separation is good.

Instead, look for actual problems:

* **SRP:** Is this thing changing for unrelated reasons?
* **OCP:** Do I have to modify stable code every time I add a new behavior?
* **LSP:** Does this implementation actually honor the interface's behavioral contract?
* **ISP:** Is this interface forcing consumers/implementations to depend on irrelevant capabilities?
* **DIP:** Does high-level logic know unnecessarily much about low-level implementation details?

And especially in Go:

> **Prefer small, meaningful interfaces and composition over elaborate abstractions.**

Your `Client`/`Command` architecture is already exercising several of these ideas. The next useful step isn't to redesign everything around SOLID; it's to let the project grow and use these principles to recognize where an existing boundary is becoming problematic.
