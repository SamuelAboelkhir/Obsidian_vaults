---
tags:
- CYBER
MOC: IT
---
[[_0000 Home|Home]] | [[_0005 IT MOC|Back to IT MOC]] | [[CYB Cybersecurity index|Back to index]]
# OWASP Top 10
- [OWASP](https://owasp.org) (the Open Worldwide Application Security Project) publishes a new list of critical web application security risks every few years. The [OWASP Top 10:2025](https://owasp.org/Top10/2025/0x00_2025-Introduction/) combines contributed application-testing data with a survey of security practitioners. **It's based on real-world data!**

>The Top 10 is a great starting point, but it's not a complete security checklist.


- Here are the current categories for **2026**:
	- _A01: Broken Access Control_ – Users can do things they shouldn't be allowed to do.
	- _A02: Security Misconfiguration_ – The system is deployed or configured in an unsafe way.
	- _A03: Software Supply Chain Failures_ – Vulnerabilities enter through outdated or compromised third-party libraries or build tools.
	- _A04: Cryptographic Failures_ – Sensitive data is exposed because cryptographic protections are missing or misused.
	- _A05: Injection_ – Untrusted input is treated as executable instructions.
	- _A06: Insecure Design_ – The system's design lacks the controls needed to resist attacks.
	- _A07: Authentication Failures_ – The system doesn't reliably verify who a user is.
	- _A08: Software or Data Integrity Failures_ – The application trusts software or data without verifying its integrity.
	- _A09: Security Logging and Alerting Failures_ – Attacks happen without triggering the records or alerts needed for a response.
	- _A10: Mishandling of Exceptional Conditions_ – Unexpected conditions expose details, bypass controls, or leave the system in a bad state.

>You do _not_ need to memorize this list. You _do_ need to know where to find it and how to use it as a **guide** for identifying potential issues in your application.

## NoSniff
- Missing browser security headers are a concrete example of **A02: Security Misconfiguration**. One of the simplest headers to add is [`X-Content-Type-Options: nosniff`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/X-Content-Type-Options).
- This header tells browsers to respect a response's declared [`Content-Type`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Content-Type) instead of guessing from its contents.
- This helps prevent [security vulnerabilities related to MIME sniffing](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/MIME_types#mime_sniffing). For example, without `nosniff`, a browser might treat attacker-controlled content as _executable code_ despite its declared type!
- Bearly Secure composes global middleware around its main [`http.Handler`](https://pkg.go.dev/net/http#Handler). Middleware can set the header before passing the request to the next handler, which applies the policy to both application routes and static files.
## How Attackers Think
- When attackers target a web application, they're _usually_ after:
	- **Money:** Theft through payments, refunds, or extortion
	- **Data:** Email addresses, names, and credentials to abuse or sell
	- **Access:** Control of powerful accounts or admin panels
	- **Disruption:** Making a service unreliable for notoriety or competitive advantage
- Attackers don't _have_ to use your front-end interface or obey its client-side restrictions. They can interact with your back end directly. Just because your website's UI hides the "Delete All Users" button for non-admins doesn't mean an attacker can't send a direct `DELETE` request to your `/users` endpoint using [`curl`](https://github.com/curl/curl) or [Postman](https://www.postman.com/).
### Trust Boundaries
- A [trust boundary](https://owasp.org/www-community/Threat_Modeling) separates parts of a system with different levels of trust. For example, you control your server code, but you don't control the code or data in a user's browser.
![[Pasted image 20260917120600.png|762]]
- Treat data crossing from the browser to your server as attacker-controlled. The server needs to validate that data and [authorize](https://owasp.org/www-community/Access_Control) the requested operation instead of blindly trusting client-side checks.
- [Threat modeling](https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html) means examining a system from an attacker's perspective. We can't predict _every_ possible attack, but we can use [STRIDE](https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html#threat-identification) to prompt us to look for six common classes of threats:
	- **S**poofing: Pretending to be another user
	- **T**ampering: Changing data in transit or at rest without authorization
	- **R**epudiation: Denying an action because the system lacks reliable records
	- **I**nformation disclosure: Accessing data that shouldn't be visible
	- **D**enial of service: Making the system slow or unavailable
	- **E**levation of privilege: Gaining permissions you shouldn't have
- So, at each trust boundary, ask:

> If someone were trying to break this, which failures could occur, and what evidence would reveal them?

# Authentication
- Many attacks on web applications start with a simple question:
> Who does the system _think_ I am?

- [Authentication](https://pages.nist.gov/800-63-4/sp800-63b/introduction/) is the process of verifying _who_ a user is. If your application gets that answer wrong, an attacker can take over another user's account.
- Imagine if I could make an HTTP request to the YouTube API and upload a video to _your_ channel... not so good.
## Authentication and Authorization
- Successful authentication changes a request's security context, but it doesn't make the input trustworthy. Just because you're authenticated on Boot.dev doesn't mean we're going to let you change someone else's username!
- The server still needs [authorization](https://owasp.org/www-community/Access_Control) checks before returning private data or allowing protected actions. That said, an attacker who steals an active session credential can use the victim's permissions. **Authentication is a high-value target!**
## Canonical Identities
- A common authentication mistake is letting one account identifier appear in multiple formats without a consistent rule. For example, an app might treat these email addresses as different:
	- `ligma@example.com`
	- `LIGMA@example.com`
	- `Ligma@example.com`
- But they all _almost certainly_ point to the same mailbox, because email systems tend to treat addresses as case-_insensitive_. Apps should choose one **canonical** representation for any type of ID before storing it or looking it up... and for our purposes, we'll just lowercase the email address and strip whitespace.
- Bearly Secure currently allows for different formats of email addresses! Our architect has decided that we **should trim surrounding whitespace** and **force lowercase** for all account emails.
>Lowercasing an email address is a deliberate product rule for _this_ app, not a universal rule regarding email. SMTP technically [preserves case in the local part](https://datatracker.ietf.org/doc/html/rfc5321#section-2.4), while domains are case-insensitive. The important security rule is _consistency_.

## Stateless vs. Stateful Authentication
- Whenever a web server receives a request, it first needs to answer:
> _Is this request coming from an authenticated user?_

- Requests typically carry proof-of-authentication through **stateless tokens** or **stateful sessions**.
### Stateless Authentication
- Every request carries a credential that the server can validate **without looking up a server-side session record.** Typically:
	1. After successful login, the server issues a signed token to the client.
	2. The client sends the signed token with each request.
	3. The server accepts or rejects the request based on the token without loading a session record.
- Sure, the server may still load user data, but "stateless" means the actual token validation doesn't depend on any stored session state. Many stateless systems use [**bearer tokens**](https://www.rfc-editor.org/info/rfc6750/) encoded as [JWTs](https://www.rfc-editor.org/info/rfc7519/), and anyone who gets ahold of one can use it until it expires.
	- Also check out [[PG Go http server#JWTs|JWTs]] 
- To be fair, a JWT doesn't _automatically_ make a system stateless, the server _can_ still check it against stored state, but that sometimes defeats the purpose of it being a JWT in the first place.
### Stateful Authentication
- **The server remembers you.** Typically:
	1. After successful login, the server creates a session record and sends a secret session identifier to the client
	2. The client sends the secret session identifier with future requests.
	3. The server accepts or rejects the request after checking if the session identifier maps to a valid session record (usually in a database or in-memory store).
- Web apps usually carry the session identifier in a [cookie](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Cookies).
	- Also check out [[PG Go http server#Cookies|Cookies]]
### Pros and Cons
- **Stateless token validation**:
	- Avoids a server-side session lookup during token validation (simplifies scaling and reduces latency)
	- Makes immediate per-token revocation harder (unless it _also_ checks shared state)
- **Stateful sessions**:
	- Require a session store that all application instances can consult (can be slower and more complex to scale)
	- Allow the server to easily revoke a session immediately by invalidating its record
## What Are Sessions?
- An authenticated session is created when a user successfully logs in.
- It's a record stored on the server that says "When a user presents this session ID, let them access this account while the session is valid."
- In a typical browser flow, the client stores and sends that ID in a [cookie](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Cookies), and the server re-checks it against the session in the database on every request.
### Session IDs
- A session ID is a reference to server-side data (often a row in a `sessions` table) that stores:
	- A unique ID for the session (`id`, which is secret and given to the client)
	- Which user authenticated (`user_id`)
	- When the session expires (`expires_at`)
	- Whether the session has been revoked (`revoked_at`, can be `NULL` if the session is still valid)
	![[Pasted image 20260920214802.png|602]]
- For each request, the server reads the session ID from the [`Cookie` header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Cookie), looks up the record, and rejects sessions that are missing, expired, or revoked.
>A session ID is a [bearer credential](https://www.rfc-editor.org/info/rfc6750/), so it **needs to stay secret**. Don't put raw session IDs in URLs or logs.

