---
tags:
  - CYBER
MOC: IT
---
[[_0000 Home|Home]] | [[_0005 IT MOC|Back to IT MOC]] | [[CYB Cybersecurity index|Back to index]]
# OWASP Top 10
- [OWASP](https://owasp.org) (the Open Worldwide Application Security Project) publishes a new list of critical web application security risks every few years. The [OWASP Top 10:2025](https://owasp.org/Top10/2025/0x00_2025-Introduction/) combines contributed application-testing data with a survey of security practitioners. **It's based on real-world data!**

> [!WARNING] The Top 10 is a great starting point, but it's not a complete security checklist.

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

> [!QUOTE] You do _not_ need to memorize this list. You _do_ need to know where to find it and how to use it as a **guide** for identifying potential issues in your application.
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
> [!WARNING] Lowercasing an email address is a deliberate product rule for _this_ app, not a universal rule regarding email. SMTP technically [preserves case in the local part](https://datatracker.ietf.org/doc/html/rfc5321#section-2.4), while domains are case-insensitive. The important security rule is _consistency_.

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
> [!INFO] To be fair, a JWT doesn't _automatically_ make a system stateless, the server _can_ still check it against stored state, but that sometimes defeats the purpose of it being a JWT in the first place.
### Stateful Authentication
- **The server remembers you.** Typically:
	1. After successful login, the server creates a session record and sends a secret session identifier to the client
	2. The client sends the secret session identifier with future requests.
	3. The server accepts or rejects the request after checking if the session identifier maps to a valid session record (usually in a database or in-memory store).
> [!INFO] Web apps usually carry the session identifier in a [cookie](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Cookies).
	
Also check out [[PG Go http server#Cookies|Cookies]] from these notes
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
> [!DANGER] A session ID is a [bearer credential](https://www.rfc-editor.org/info/rfc6750/), so it **needs to stay secret**. Don't put raw session IDs in URLs or logs.

## Cookies
[Cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Cookies) are small name-value pairs that a browser stores and sends with matching HTTP requests. Many web applications happen to store session IDs in cookies, but they're _totally different concepts_. A cookie contains:

- A **name**, like: `bootdev_session_id`
- A **value**, like: `7f3c9a21d84e4b6fa2c18e57b9034d6a`
- _Optional_ attributes like:
    - `Secure` (sent only over HTTPS, with a localhost exception)
    - `HttpOnly` (unavailable to `document.cookie`)
    - `SameSite` (restricts cross-site sending)
> [!TIP] Aside from session IDs, cookies are used for stuff like preferences, shopping carts, and yes, _tracking_. For example, the Google Analytics cookie's name is `_ga` and has a unique value like `GA1.1.1734567890.1723927600`.

When a server sends a valid cookie, the browser automatically stores it and attaches it to future requests that match the cookie's scope and security attributes. Your client-side code doesn't need to muck with the cookie at all! The server sets it in a response header, and the browser echoes it back in future request headers.

Servers create cookies with the [`Set-Cookie`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Set-Cookie) response header:
```
HTTP/1.1 200 OK
Content-Type: text/html
Set-Cookie: bootdev_session_id=7f3c9a21d84e4b6fa2c18e57b9034d6a; Secure; HttpOnly; SameSite=Lax
```
Browsers then attach the cookie to matching requests with the [`Cookie`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Cookie) request header:

```text
GET /account HTTP/1.1
Host: localhost:3030
Cookie: bootdev_session_id=7f3c9a21d84e4b6fa2c18e57b9034d6a
```

## Full Cookie Auth Flow

1. The user logs in with their username and password.
2. The server verifies the credentials and creates a session for that user.
3. The server sends a response with a `Set-Cookie` header with the session ID.
4. The browser stores the cookie.
5. The user makes a request to post a comment, and the browser automatically sends the session cookie with the request.
6. The server reads the cookie, loads the session, and checks that it's still valid. If it is, the hot-take is posted.
# Cookie Security

From the server's point of view, cookies are convenient. From an attacker's point of view, session cookies are **exploitable**.

If an attacker steals a bearer session cookie and reuses it, the server (usually) can't tell those requests apart from the user's. Some common mistakes are:

- If a session cookie is readable through `document.cookie`, injected JavaScript can steal it.
- If a session cookie is sent over plain HTTP, an attacker who can observe the connection can capture it.
- If a cookie has an unnecessarily broad domain or long expiration, an attacker has more opportunities to steal and reuse it.
- If the browser attaches a cookie to a malicious cross-site request, [cross-site request forgery (CSRF)](https://owasp.org/www-community/attacks/csrf) is possible... which we'll talk about later.
## Cookie Security Flags

Modern browsers give us [tools to limit how cookies behave](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Cookies#security). The most important ones are:

- [`HttpOnly` (boolean)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Set-Cookie#httponly): Prevents JavaScript from reading the cookie through `document.cookie`.
- [`Secure` (boolean)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Set-Cookie#secure): Sends the cookie only over HTTPS (and localhost, for development).
- [`SameSite` (Strict, Lax, None)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Set-Cookie#samesitesamesite-value): Restricts when the browser sends the cookie with cross-site requests. `Strict` is the most restrictive. `Lax` also permits top-level cross-site navigations using safe methods, which many login flows need.

> [!TIP] Set cookie attributes deliberately instead of relying on framework or browser defaults!

# Session Lifetime

Once a user is authenticated, the next question isn't "Who are you?" It's:

> _How long should I keep trusting you?_

...which is answered by **session timeouts**, **expiration**, and **revocation**.

## Session Expiration

Every session should have a server-enforced _expiration_, commonly stored in an `expires_at` column. Without a well-defined lifetime:

- A stolen session may remain useful for far too long.
- Old, still-valid sessions accumulate across devices.
- Clearing one browser's cookie can leave copied session IDs valid on the server.

The _server_ must enforce the expiration. A client-side expiration timestamp helps the browser know when to discard its copy, but an attacker can _always bypass browser behavior_.

## So... Short Sessions?

As you can expect, there's a tradeoff here. Short-lived sessions reduce the time an attacker can reuse a stolen session, but they also force users to reauthenticate more often, which is annoying.

Long-lived sessions are more convenient, but they increase the damage window when they're stolen.

There's no single "correct" duration. The right choice depends on **what the user is doing**, the device, and the damage a stolen session could cause. Applications often combine two limits:

- An **inactivity timeout** ends a session after no activity, for example, 24 hours.
- An **overall timeout** caps the session's total lifetime, for example, 30 days.

## Not All Actions Are Equal

Different actions carry different levels of risk. For example:

| Action                          | Risk Level     |
| ------------------------------- | -------------- |
| Submitting a lesson on Boot.dev | Low            |
| Viewing account settings        | Medium         |
| Making a payment                | High           |
| Launching a nuclear missile     | Super Critical |

**Higher-risk actions should require fresher trust.**

## Revoking Sessions

Expiration isn't enough. You and your users need to be able to _revoke access_ when you suspect a session may be hijacked. Revoking a session means **invalidating it immediately**. There are also times when a server should automatically revoke sessions, like when a user logs out. It's not enough to _just_ clear the browser cookie.

This is where stateful sessions shine. A server can revoke a session by invalidating its record. A self-contained stateless token can't be individually revoked. That problem can be mitigated (but not eliminated) by short token lifetimes and refresh tokens that _can_ be revoked.
# Password Resets

We all forget passwords. So, as developers, we have _no choice_ but to offer password recovery. But with that come some pretty big risks.

Instead of authenticating with their password (because they presumably forgot it), the user proves control of something else: in most cases, **their email inbox**. So, now their email address is just as viable an attack vector as the password itself!

## How Password Resets Work

1. The user requests a [password reset](https://cheatsheetseries.owasp.org/cheatsheets/Forgot_Password_Cheat_Sheet.html).
2. The server **generates a random token**, puts it in a link and (usually) emails it to the user.
3. The user opens the link, and the server validates the token.
4. The user sets new password for the account.

## Where's the Danger?

Most reset vulnerabilities come from several predictable mistakes:

- Tokens that are guessable
- Tokens that never expire
- Tokens that can be reused
- Tokens not tied to a specific user
- Tokens leaked through logs, analytics, browser history, or referrer headers

A password reset token should be treated like a **temporary password**, so it should be:

- **Unpredictable** (long, random, and not guessable)
- **Short-lived** (expires after a short window, usually minutes)
- **Single-use** (invalid after use)
- **Bound to one account** (tied to the specific user who requested it)
- **Stored as a hash** (the database never stores the raw token)

> [!WARNING] The underlying assumption here is that the user's email is both **secure and private**... which is _not_ always true for every user. So, password resets should also _always_ be accompanied by a notification to the user via any other communications channels (sms, push, email) and should invalidate existing sessions.
# Broken Password Reset Flow

A password reset flow can do everything right – use strong, short-lived, single-use tokens – and _still_ leak account information. If the reset request tells a user:

> No account found for that email address.

... then attackers can use it as an account lookup tool. They can submit a list of email addresses and learn [which ones actually have accounts](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html#authentication-and-error-messages). It's especially risky in authentication flows because it gives attackers a better target list for [phishing](https://en.wikipedia.org/wiki/Phishing), [credential stuffing](https://owasp.org/www-community/attacks/Credential_stuffing), and [social engineering](https://www.ibm.com/think/topics/social-engineering).

## Neutral Responses

Password reset request pages should return the same public status and message _whether or not_ the email exists:

> If an account exists for that email, we'll send a reset link shortly.

Frustratingly, showing a neutral response message still isn't enough in some cases; _response timing_ can also be a problem. If the password reset endpoint returns immediately when no account is found, and takes a bit longer to return when a reset token is actually generated, then attackers may notice that.

The strongest production systems defend against timing attacks by [processing password reset requests asynchronously](https://cheatsheetseries.owasp.org/cheatsheets/Forgot_Password_Cheat_Sheet.html#forgot-password-request). An attacker should receive **no signal** of whether an account was found.
# OAuth 2.0

[OAuth 2.0](https://datatracker.ietf.org/doc/html/rfc6749) lets users grant your app **limited access** to another service without sharing their password with your app.

It's **delegated authorization**: a user allows your app to act on their behalf.

Click to hide video

Your browser does not support playing HTML5 video. You can instead. Here is a description of the content: OAuth Explainer

## OAuth Is Not Authentication

An OAuth access token tells a resource server:

> "This app is allowed to access these resources on behalf of this user."

An access token isn't a standardized identity assertion for logging the user into your app. It can be used as part of a larger authentication system, but it requires additional work. For standardized identity verification, you can use [OpenID Connect](https://openid.net/specs/openid-connect-core-1_0.html) (OIDC), an identity layer built on OAuth 2.0.

## OAuth Roles

- **Resource owner:** The user granting access
- **Client:** Your application
- **Authorization server:** Authenticates the user, obtains authorization, and issues tokens
- **Resource server:** The API that accepts access tokens and protects the user's data

## The Authorization Code Flow

1. Your app creates a one-time `state` value bound to the browser transaction and a [PKCE](https://www.rfc-editor.org/info/rfc7636/) verifier and challenge.
2. Your app redirects the browser to the authorization server with the requested scopes, `state`, and PKCE challenge.
3. The user authenticates with the authorization server and approves the requested access.
4. The authorization server redirects the browser back to your registered callback URI with a short-lived authorization code and the `state` value.
5. Your app verifies that `state` matches the browser transaction, then exchanges the code and PKCE verifier for an access token from the authorization server.
6. Your app uses the access token to call the resource server.

## Scopes

Scopes define what access you're requesting, for example:

- `read:user`
- `email`
- `repo`

**Request the minimum scopes required.** Overbroad scopes increase the blast radius if a token is stolen.

## Token Types

There are two main types of tokens in OAuth 2.0: _access tokens_ and _refresh tokens_:

- **Access tokens:** Are typically short-lived and authorize direct calls to a resource server.
- **Refresh tokens:** Are typically longer-lived and are used to obtain new access tokens without requiring the user to re-authenticate.
## An example to clarify the difference between oauth2 and OIDC
### 1. Identity Brokering (Federation)

In your example:

- **Microsoft** is the upstream **Identity Provider (IdP)**.
- **Authentik** (or Okta, Keycloak, Auth0) acts as an **Identity Broker** and **Authorization Server**.
- **Your App** is the **Client / Relying Party**.

When you configure Authentik to use Microsoft, you are setting up an **OIDC federation chain**:

1. You go to your app. Your app sends you to Authentik via OIDC.
2. Authentik says: _"You can log in with Microsoft."_
3. Authentik redirects you to Microsoft via OIDC.
4. Microsoft authenticates you and returns an **ID token** to Authentik saying: _"This user is `alice@company.com`."_
5. Authentik consumes that ID token, matches `alice@company.com` to its internal directory (or provisions an account automatically on the fly), and establishes who you are.

---

### 2. OIDC (Authentication) vs. OAuth (Authorization) at the App Level

Once Authentik knows you are Alice, it still has to decide what happens next:

1. **Authentication (OIDC):**
    
    - Authentik creates its own ID token signed for **your app**.
    - Your app reads this and says: _"Great, Authentik verified that Alice is logged in."_
2. **Authorization (OAuth 2.0 / Policy):**
    
    - Just because Alice proved her identity doesn't mean she can access the app!
    - In Authentik, you might have an access rule that says: _"Only users in the `Engineering` group can access this app."_ If Alice is in Marketing, Authentik denies access right at the prompt.
    - If she _is_ allowed, Authentik issues an **access token** with specific scopes (e.g., `scope: read-only` or roles in the token claims).
    - Your app's API consumes that access token to restrict what Alice is permitted to do inside the app.

---

### The Big Picture

- **OIDC** handles the **"Who are you?"** handoff from Microsoft → Authentik → Your App.
- **OAuth 2.0** handles the **"What are you allowed to touch?"** permissions that Authentik grants your app.

Tools like Authentik exist precisely to bridge this gap: they handle user directories, federate identity through OIDC, and issue OAuth tokens to your internal applications.
# SAML and OIDC

Enterprise systems often need **single sign-on (SSO)**: a way for users to authenticate with one identity provider, and then access multiple applications.

[SAML 2.0](https://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf) and [OpenID Connect](https://openid.net/specs/openid-connect-core-1_0.html) (OIDC) support SSO through [federated identity](https://pages.nist.gov/800-63-4/sp800-63c.html). Your app relies on an identity provider (IdP) to authenticate the user, then validates a protocol message from that IdP.

## SAML vs. OIDC

**SAML** is widely used for ~~legacy~~ enterprise SSO. It uses XML documents called **assertions** to tell your app, the service provider, what the IdP verified about the user.

**OIDC** is an identity layer built on OAuth 2.0. Its ID tokens are [JWTs](https://www.rfc-editor.org/info/rfc7519/), and the protocol fits naturally into web, mobile, and API architectures that already use OAuth.

Both can solve the same SSO problem, but their message formats, flows, and validation rules are different.

## How They Work

**SAML flow:**

1. The user visits your app.
2. Your app redirects the user to the IdP.
3. The user logs in with the IdP.
4. The IdP returns a SAML response through the browser to your app's assertion consumer service.
5. Your app validates the response and assertion before creating a local session.

**OIDC flow:**

1. The user visits your app.
2. Your app creates request-binding values like `state`, `nonce`, and a PKCE challenge, then redirects the user to the IdP.
3. The user logs in with the IdP.
4. The IdP redirects back with an authorization code.
5. Your app verifies the callback and exchanges the code for tokens, including an **ID token**.
6. Your app validates the ID token, including its `nonce`, before creating a local session.

The important detail is that **your app should not ask for the user's password**. The IdP handles authentication; your app validates the result. For an OIDC ID token, that includes its signature, issuer, audience, expiration, and `nonce` when one was sent. Don't trust identity claims until the protocol response passes every required check!

> [!WARNING] **Don't implement SAML or OIDC from scratch**! Use a mature protocol library or a trusted identity product, then configure and validate it according to the provider and protocol documentation.
# API Keys

[API keys](https://www.ibm.com/think/topics/api-key) are often used to authenticate programmatic requests. They're _not_ user sessions or OAuth tokens: they're often **long-lived bearer credentials**, which makes them very dangerous if leaked.

> [!QUOTE] Just imagine how much you could run up my Claude bill if you had my API key...

## Common Ways API Keys Leak

- Hard-coded in source repositories
- Logged during debugging
- Stored in frontend JavaScript
- Pasted into Slack messages
- Committed in `.env` files

## Generating Secure Keys

Keys must be **cryptographically random** and **long enough to resist guessing**. Thirty-two random bytes (256 bits) is a strong default.

Predictable values like this are unsafe:

```go
apiKey := fmt.Sprintf("%d-%d", userID, time.Now().UnixNano())
```

Use [`crypto/rand`](https://pkg.go.dev/crypto/rand) and [`encoding/hex`](https://pkg.go.dev/encoding/hex) instead:

```go
keyBytes := make([]byte, 32)
if _, err := rand.Read(keyBytes); err != nil {
  return "", err
}
apiKey := hex.EncodeToString(keyBytes)
```

> [!DANGER] Never generate keys from user data, timestamps, or anything predictable, just use a secure random generator.

## Secure Storage

If your service only needs to verify incoming keys, store a **hash** of each high-entropy key rather than the plaintext. Go's [`crypto/sha256`](https://pkg.go.dev/crypto/sha256) package can produce that fixed-length digest:

```go
func hashAPIKey(apiKey string) string {
  digest := sha256.Sum256([]byte(apiKey))
  return hex.EncodeToString(digest[:])
}
```

Generate the raw key, display it once, and then _store_ its hash. That way if an attacker secretly gains read-access to your database, they won't have the plaintext keys to use.

## Key Rotation

Rotate keys:

- Immediately after suspected compromise
- When the client or owner no longer needs access
- Before expiration or on the schedule required by your risk policy

If a key is compromised, revoke it immediately, notify the owner, and investigate the incident.

> [!WARNING] Treat API keys like passwords: transmit them only over HTTPS, keep them out of URLs and logs, and make revocation easy.
# Reauthentication

Not all actions deserve the same level of trust. Viewing your profile? _Low risk_. Changing your password? _High risk_.

[**Reauthentication**](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html#require-re-authentication-for-sensitive-features) asks a logged-in user for fresh proof of identity _again_ before they perform a particularly sensitive action. **Step-up authentication** goes further by raising the session's assurance level, like requiring an additional or stronger factor.

Someone with a stolen session cookie may be able to access the account, but they should _not_ be able to change its email address without also knowing the current password.

## When to Reauthenticate

Good candidates for reauth/step-up auth include:

- Changing passwords, email, or MFA settings
- Transferring money or updating payment methods
- Resetting MFA or generating recovery codes
- Responding to unusual device or location signals

> [!TIP] Every reauthentication prompt _adds user friction_. Ask too often, and users will hate your app. Ask too little, and attackers with stolen sessions can cause damage.
# Authentication Misconceptions

Many password rules you've encountered in the wild are, unfortunately, just **security theater**. They add friction without stopping how attackers guess and reuse passwords. For example, "change your password every 90 days" may _sound_ secure, but users usually just make predictable changes:

- `OtterRiver1!`
- `OtterRiver2!`
- `OtterRiver3!`

Current [NIST password guidance](https://pages.nist.gov/800-63-4/sp800-63b.html#passwordver) says _not_ to require periodic password changes. Only force a change when there's evidence the password has been compromised!

## Complexity Requirements Are Harmful

The silly game of "Your password must include uppercase and lowercase letters, numbers, and symbols" leads to passwords like `Password1!`.

Humans are predictable, and given simple rules, we follow patterns:

- Capital letter at the start
- Number and symbol at the end
- Common substitutions (`@` for `a`, `3` for `e`)

Attackers know this! Password crackers are built around these patterns. It's _way_ better to prioritize **length**, block known-compromised passwords, and support password managers. A randomly generated **passphrase** can be easier to remember while still being hard to guess.

![[Pasted image 20261027232137.png]]

-- [The incredible XKCD](https://xkcd.com/936/)

## Weak Security Questions

"What was your first pet's name?" is _not_ effective authentication. These kinds of questions are:

- Easy to guess
- Discoverable on social media
- Answers rarely change

They're just **recovery vulnerabilities** disguised as security features.

# Multi-Factor Authentication

A stolen password shouldn't be enough to take over an account. If a password is your _only_ barrier, an attacker who nabs it is _in_.

[Multi-factor authentication](https://pages.nist.gov/800-63-4/sp800-63/model/) (MFA) requires proof from two or more **distinct** factor types:

- **Something you know:** A password or PIN
- **Something you have:** A device containing an authenticator secret, like a phone or security key
- **Something you are:** A biometric characteristic, like a fingerprint or face

A password plus a PIN... _is still weak_. Both are "something you know." We need to **mix factor types**. A physical security key is a _good_ second factor for a password because it's "something you have." A fingerprint or face can activate an authenticator, but a biometric characteristic isn't an authenticator by itself.

## Why MFA

Obviously, we don't want passwords to get stolen... _but it happens_. MFA makes stolen passwords **less useful**, because an attacker still needs control of a separate authenticator. Common second factors include:

- **[TOTP](https://www.rfc-editor.org/info/rfc6238/) codes:** Short-lived codes generated from a secret held by an authenticator app
- **Out-of-band authentication:** A separate device exchanges a short-lived secret with the login flow
- **Biometric-activated authenticators:** A device checks a fingerprint or face locally before using its stored authenticator secret
- **SMS codes:** Short-lived codes sent via text message (should be [restricted](https://pages.nist.gov/800-63-4/sp800-63b/authenticators/#pstnOOB) because phone numbers can be hijacked)
# TOTP

[TOTP](https://www.rfc-editor.org/info/rfc6238/) (Time-Based One-Time Password) generates short-lived codes from a shared secret and the current time. The codes are usually six digits and rotate on a 30-second time step.

Its appeal is **low friction**: it works offline in an authenticator app and doesn't require a separate hardware security key.

## How TOTP Works

TOTP is built on a unique **shared secret** known to both the server and the user's authenticator. Both sides _independently_ compute the expected code for the current time step. Here's some _pseudocode_. Assume that `sharedSecret` is stored on the server and in the user's authenticator app, and `userSuppliedCode` is the code the user entered during login:

```go
timeStep := (unixTime - t0) / 30
serverComputedCode := hotp(sharedSecret, timeStep)

if serverComputedCode == userSuppliedCode {
  // TOTP code accepted
} else {
  // TOTP code rejected
}
```

The shared secret is _not_ sent during login, but the **generated code** is. If both clocks and algorithm settings agree, the server computes the same value and checks whether they match.

> [!WARNING] Because codes are so short, servers _need_ to be careful to rate-limit guesses so an attacker can't brute-force them.

## Enrollment

You're probably familiar with this process, but the way users get the shared secret on their device during setup is typically:

1. The server generates a secure random secret and stores it for the user.
2. The server encodes the secret in a [QR code](https://en.wikipedia.org/wiki/QR_code) and displays it on the enrollment page.
3. The user scans the QR code with an authenticator app (or enters the secret manually) to save the secret on their device.

## Backup Codes

Devices get lost, so [recovery codes](https://pages.nist.gov/800-63-4/sp800-63b/events/#savedrecovery) are often provided as a fallback. They're one-time secrets that can be used _in place of_ a TOTP code. The user is expected to download and store them in a safe place, like a password manager.

> [!DANGER] TOTP codes _can_ be phished. A six-digit code can be typed into a fake login page and relayed to the real site. TOTP is stronger than a password alone, but it isn't phishing-resistant like a [FIDO2/WebAuthn authenticator](https://www.w3.org/TR/webauthn-3/).
# Hardware Tokens

A **hardware token** is a physical authenticator that proves possession of a secret stored on the device. Some display one-time codes. FIDO security keys instead connect to a computer or phone and use public-key cryptography to sign a challenge.

A [FIDO (Fast IDentity Online)](https://fidoalliance.org/specifications/) security key is a specific type of hardware token that works with the [WebAuthn](https://www.w3.org/TR/webauthn/) standard.

## Asymmetric Cryptography

Unlike TOTP, where the authenticator and server share a secret, a security key uses [asymmetric cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography), meaning the server _never sees the private key_. With FIDO/WebAuthn, the flow looks like this:

![[Pasted image 20261030093132.png]]


The credential's private key stays on the security key. The key receives the challenge, signs it, and returns the _signature_ to the server. The server can verify the signature with the public key it has on file, but it can't derive the private key from that signature.

## Phishing Resistance

FIDO is [phishing-resistant](https://pages.nist.gov/800-63-4/sp800-63b/authenticators/#verifimpers), which means a malicious site called `faceb0ok.com` might trick your grandma into typing in her `facebook.com` _password_, but it _can't_ trick her hardware token into signing a challenge for the wrong domain.
# Passkeys

A [passkey](https://fidoalliance.org/passkeys-2/) is a FIDO credential (similar to a good hardware key) designed to _outright replace_ a password. Instead of being on a physically separate device, it typically lives directly on the user's phone or computer.

Click to hide video

Your browser does not support playing HTML5 video. You can instead. Here is a description of the content: Passkeys Explainer

Passkeys use the [WebAuthn standard](https://www.w3.org/TR/webauthn-3/) on the web. The application receives an _[asymmetric](https://www.ibm.com/think/topics/asymmetric-encryption) cryptographic proof_ and a user-verification result, not the user's PIN or biometric data.

## Device-Bound and Synced Passkeys

A **device-bound passkey** is _not_ synced to other devices. That doesn't necessarily mean you can only sign in _on_ that device though. For example, you can use a phone to [approve a sign-in](https://fidoalliance.org/passkeys/) on your computer.

A **synced passkey** can be copied through an end-to-end encrypted credential provider so it's available on the user's other devices.

In both cases, the private key is _never_ sent to the application server, but "never leaves the device" is accurate only for device-bound credentials.

## Can Passkeys Provide MFA?

A passkey always proves control of a cryptographic key: **something you have**. Whether it provides just that one factor or multiple factors depends on _how the authenticator protects that key_.

If the authenticator requires a PIN or biometric before using the key, it can act as a [multi-factor cryptographic authenticator](https://pages.nist.gov/800-63-4/sp800-63b/authenticators/#multi-factor-cryptographic-verifiers). The device provides **something you have**, while the PIN or biometric provides **something you know** or **something you are**.
# Choosing an MFA Method

The strongest multi-factor authentication method _on paper_ isn't necessarily the right choice for every situation. You have to balance phishing resistance against device support, cost, user friction, and recovery. Some of the trade-offs are:

- **TOTP** is inexpensive, works offline, and runs in widely available authenticator apps. However, its codes can be phished and relayed, and the server has to protect a shared secret.
- **Dedicated security keys** use [WebAuthn](https://www.w3.org/TR/webauthn-3/) to provide phishing-resistant authentication. They're good for privileged staff and high-risk actions, but the organization has to purchase, provision, replace, and support the keys.
- **Passkeys** provide similar origin-bound protection to dedicated WebAuthn keys without always requiring a separate device. They _are awesome_, but recovery and moving between credential-provider ecosystems can still be challenging.

## Weakest Path Wins

If multiple auth methods are enrolled for an account, its protection is ultimately determined by **the easiest sign-in or recovery path that an attacker can exploit**. If an account accepts TOTP as a fallback, an attacker can target that method, even if the user normally signs in with a passkey.
# Account Recovery

Strong MFA doesn't do much good if an attacker can bypass it through an easier recovery path. [Account recovery](https://pages.nist.gov/800-63-4/sp800-63b/events/#recovery) restores control after a user loses the authenticators needed to sign in, and it has to answer a tough question:

> How do we restore access without making it easy for attackers to exploit the recovery process?

Email recovery is not _automatically_ insecure. A strong, short-lived recovery code sent to a previously verified address can be appropriate in some cases. But an email-only bypass can be too weak for a high-risk account.

Attackers know this, so they target recovery flows directly. Recovery has to balance:

1. **Safety:** Don't let attackers gain access
2. **Usability:** Don't permanently lock out legitimate users

Alternatives to email recovery include:

- **Saved recovery codes:** Random secrets issued during enrollment and stored offline by the user
- **Recovery contacts:** Prearranged contacts (like an organization admin) that participate in recovery
- **Repeated identity proofing:** Repeating enough of the original identity checks to establish that the claimant matches the identity tied to the account

> [!TIP] Recovery should be **rare**, **rate-limited**, and **auditable** – and no easier for an attacker than the account's risk level permits. Notify the user through an independent channel after every recovery!

# Principle of Least Privilege

In web security, you're gonna hear about the [principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege) _all the time_. In basic terms:

> Give users and systems _only_ the access they need _and nothing more_.

Every permission is a potential liability. Over-privileged accounts can turn small mistakes into major breaches.

## Sources of Over-Privilege

Most systems don't start over-privileged; they become that way over time, often due to laziness or convenience:

- Roles growing without review
- Copying permissions from "admin" to save time
- Combining unrelated responsibilities into a single role
- Giving new users bigger roles than they need to avoid friction

## Least Privilege in Practice

Permissions should be:

- **Explicit:** every permission is intentionally granted, not assumed
- **Minimal:** only what the role actually needs to function
- **Reviewed regularly:** permissions that aren't audited will drift over time

Don't grant a permission just because it might be useful later. Ask one question for every permission:

> What breaks if this permission is removed?

"Nothing"? **Remove it**.
# Preventing Broken Access Control

_Authentication_ answers the question, "Who are you?" [Authorization](https://csrc.nist.gov/glossary/term/authorization) answers "What are you allowed to do?"

[Broken access control](https://owasp.org/Top10/2025/A01_2025-Broken_Access_Control/) happens when the server forgets to ask that second question. It's usually a careless mistake, not some clever Linux-kernel exploit:

- A route checks that you're logged in, but not what you can access.
- A route trusts a role specified by the _client_.
- A route uses an ID provided in the URL, but never verifies the user owns that resource.

## The Rule of Thumb

Every protected action needs a server-side permission check. Not in the UI. Not in the mobile app. Not in a hidden form field. **On the server. On every request.**

A secure endpoint does two things, in order:

![[Pasted image 20261104101723.png]]

This endpoint has a problem:

```go
document, found, err := store.FindByID(request.Context(), documentID)
if err != nil {
  return err
}
if !found {
  respondNotFound(responseWriter)
  return nil
}
respondWithJSON(responseWriter, document)
```

It checks that the user is logged in, but it lets any logged-in user see any document just by changing the ID in the URL!

To fix it, compare the document's owner with the authenticated user. If the document belongs to another user, return a [`404 Not Found`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status/404) response:

```go
document, found, err := store.FindByID(request.Context(), documentID)
if err != nil {
  return err
}
if !found || document.UserID != current.User.ID {
  respondNotFound(responseWriter)
  return nil
}
respondWithJSON(responseWriter, document)
```

Returning the same `404 Not Found` response for missing and unauthorized documents also avoids confirming that another user's document exists. A [`403 Forbidden`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status/403) response is fine when it's not sensitive that a resource exists, but a `404` reveals even less here.

## How Attackers Find These Bugs

Attackers don't need clever exploits. They try the obvious things and see what the server lets through:

- Calling endpoints directly
- Changing IDs in the URL
- Repeating requests with small variations

If the server doesn't enforce permissions, an attacker will evenually find something that works.
# Access Control Models

Once an application has more than a few protected routes, repeated and inconsistent authorization checks become hard to keep track of. An access control _model_ gives those checks a consistent structure.

## Role-Based Access Control

Role-Based Access Control ([RBAC](https://en.wikipedia.org/wiki/Role-based_access_control)) assigns permissions to _roles_, then _roles to users_. For example, you may have roles like:

- `developer` for API access but no financial data
- `accountant` for financial-only access
- `owner` for complete ownership of all resources

Instead of hard-coding 14 different `if/else` permission checks into every route, you can write a shared helper that accepts the current user and the allowed roles, then rejects the request if the user's role isn't allowed.

## Keep Policies Centralized

Centralization doesn't change who is allowed to do what. It makes authentication and role evaluation easier to find, review, and apply consistently. Poorly organized code isn't inherently insecure, but it's _more likely_ to become insecure as it changes.

Scattered copies of a policy drift. One copy gets a bug fix, another doesn't. One is written as an allowlist that denies by default, another only checks the roles its author was thinking about that day and quietly lets everyone else through. A single shared helper means there's one place to get it right.

RBAC works well when permissions follow stable job functions. Its tradeoff is rigidity. As systems grow, teams sometimes invent increasingly specific roles like `admin-read-only`, `editor-plus`, or `support-admin`. That can lead to [role explosion](https://docs.evolveum.com/iam/iga/rbac/role-explosion/).
# Attribute-Based Access Control

[Attribute-Based Access Control](https://csrc.nist.gov/pubs/sp/800/162/upd2/final) (ABAC) makes authorization decisions by evaluating _facts_ about a request:

- **Subject attributes**, like the authenticated user's ID or department
- **Resource attributes**, like a review's owner or an order's status
- **Action attributes**, like whether the request reads, updates, or deletes data
- **Environment attributes**, like the time, network, or device

Two customers may have the same `customer` role, but that role shouldn't let Alice edit or delete Bob's review! Furthermore, _hiding_ those controls from Alice may streamline the user experience, but it doesn't _enforce_ authorization – she could just call the handlers directly with Bob's review ID.

The server needs to validate the relationship between the user and the specific review:

> A customer can edit or delete a review only when the review's `user_id` matches the authenticated user's `id`.

ABAC offers nice flexibility, but its policies can become scattered and hard to audit. Use reusable policy helpers where possible to keep ownership rules consistent across related actions.
# RBAC vs. ABAC

Choosing an access control model isn't about one being better than the other. It's about which trusted facts a policy needs.

**Role-Based Access Control (RBAC)** asks:

> What role does this user have?

**Attribute-Based Access Control (ABAC)** asks:

> What is true about this user, this resource, this action, and this request?

![[Pasted image 20261107160406.png]]

## RBAC for Stable Responsibilities

RBAC is a natural fit when permissions follow a user's **job or responsibility**. It's popular in [business-to-business (B2B) SaaS](https://docs.aws.amazon.com/prescriptive-guidance/latest/saas-multitenant-api-access-authorization/access-control-types.html) applications where users have stable roles like `admin`, `support`, or `accountant`.

## ABAC for Resource-Specific Rules

ABAC is a better fit when the decision depends on a more complex relationship, resource state, or request context. For example, an ecommerce shop might allow a promo code to be applied to a cart only if:

- The user is in a specific geographic region, _and_
- The cart contains at least one item from a specific category, _and_
- The request is made before the promo code's expiration date.
## Mixing Models

Real applications often use both RBAC and ABAC. That's okay!

RBAC and ABAC are _not_ mutually exclusive models, and a role can itself be treated as a user's attribute. Use the simplest model your application needs so its authorization policies stay easy to understand and maintain.

## Choose the Best-Fit Model

Use RBAC when a stable role captures the whole rule. Use ABAC when the decision depends on ownership, state, or context. Combine them when both kinds of facts matter.
# Insecure Direct Object References

Putting database identifiers in URLs is normal. It becomes unsafe when the server uses a client-controlled identifier to load an object **without checking whether the requester is allowed to access it**.

When broken access control involves an object identifier, it's called an [Insecure Direct Object Reference](https://cheatsheetseries.owasp.org/cheatsheets/Insecure_Direct_Object_Reference_Prevention_Cheat_Sheet.html) (IDOR).

The fix is the same: **check permissions on the server** before returning any object data. Database IDs are _not_ secrets, so just because an incoming request knows a user's order ID does _not_ mean that user is allowed to see it.

```go
order, found, err := store.FindByID(request.Context(), orderID)
if err != nil {
  return err
}
if !found || order.UserID != current.User.ID {
  respondNotFound(responseWriter)
  return nil
}
respondWithJSON(responseWriter, order)
```

Another strong pattern is to include ownership directly in the database lookup. You still need to handle a missing result, but an unauthorized order never leaves the database:

```sql
SELECT *
FROM orders
WHERE id = ? AND user_id = ?
```
# Securing File Downloads

A protected file presents an _easy-to-overlook_ authorization problem. Many file servers map URL paths to files, and if the server doesn't check that the requester is _allowed_ to access a file, any authenticated user can download another user's file by changing the URL.

The simplest solution is often to avoid exposing the file directly. Instead, the server checks permission and _then_ streams the file to the authorized user.

```go
file, found, err := store.FindByID(request.Context(), fileID)
if err != nil {
  return err
}
if !found || (file.UserID != current.User.ID && current.User.Role != "support" && current.User.Role != "admin") {
  respondNotFound(responseWriter)
  return nil
}
serveDocument(responseWriter, request, file.OriginalName, file.ContentType, file.StoragePath)
```

Sending a file with Go's [`net/http`](https://pkg.go.dev/net/http) package does not decide _who_ should receive it. The authorization check needs to happen before the handler streams the file.

The storage path should also come from trusted server-side metadata, not directly from a URL parameter. Treat the URL's file ID as an untrusted reference, then resolve it through a database or other server-side mapping.

A secure direct-download flow follows one guarded path:

1. Authenticate the requester.
2. Load the file metadata.
3. Authorize access to that file.
4. Send the file to the authorized requester.
# Signed URLs

A signed URL contains a temporary bearer credential. Anyone who has the complete URL can use it to access a resource until it expires, even without a session cookie.

That makes signed URLs convenient for handing an authorized download to a file server or storage service... but it also means the URL _must_ be short-lived and tightly scoped.

## Creating a Signed URL

Signed URLs typically follow this flow:

1. An authenticated user asks for a download URL
2. The route verifies that the requester owns the file or has a support/admin role.
3. The route returns or redirects to a signed URL
4. The signed endpoint validates the URL
5. If valid, the signed endpoint streams the file to the requester
## Signed URL Structure

A signed URL contains the resource path, an expiration timestamp, and a signature. With dummy values, the complete URL looks like this:

```text
http://localhost:3030/files/1/signed-download?expires=1300&signature=15a4b83da8db92f7d83ac2684ebe7d1a98108b165d4d18fbae94098ae291e5f2
```
- `/files/1/signed-download` identifies the exact resource and action.
- `expires=1300` is an illustrative [Unix timestamp](https://en.wikipedia.org/wiki/Unix_time).
- `signature=...` is a 64-character hexadecimal HMAC over the method, path, and timestamp.

The signature is a [Hash-based Message Authentication Code (HMAC)](https://www.rfc-editor.org/info/rfc2104/), created with Go's [`crypto/hmac`](https://pkg.go.dev/crypto/hmac) package:

```go
mac := hmac.New(sha256.New, signingKey)
fmt.Fprintf(mac, "GET\n/files/%d/signed-download\n%d", fileID, expires)
signature := hex.EncodeToString(mac.Sum(nil))
```

The `\n` characters separate fields in the payload being signed. They do **not** appear in the URL. The secret key turns that payload into a signature, so changing the file ID, path, or expiration invalidates it.

## Validating a Signed URL

The signed endpoint, `/files/1/signed-download` in this case, should reject malformed or expired timestamps, recompute the expected HMAC, and compare signatures in constant time with [`crypto/subtle`](https://pkg.go.dev/crypto/subtle):

```go
if expires <= now.Unix() || subtle.ConstantTimeCompare(expectedSignature, providedSignature) != 1 {
  respondUnavailable(responseWriter)
  return
}
```

This verification step is what prevents an attacker from tampering with the URL. If an attacker manually changes the expiration in the URL, the signature will no longer match and the request will be rejected.

> [!WARNING] Keep expiration short! Signed URLs are bearer credentials, so if an attacker gets hold of a signed URL, they can use it until it expires. Five minutes is a reasonable limit for many use cases.

# Injection

[Injection](https://owasp.org/Top10/2025/A05_2025-Injection/) refers to a category of vulnerabilities where **untrusted input is treated as instructions**.

Instead of being handled as ordinary _data_, user input gets _executed_ by a database, shell, or template engine. Injection is a _major_ application-security risk and is A05 in the [OWASP Top 10:2025](https://owasp.org/Top10/2025/).

The consequences can be _ugly_. An attacker might read protected data, `DROP` database tables, or execute arbitrary commands on the server. A single injection bug can compromise an entire application and its data.

## SQL Injection

One of my [favorite XKCD comics](https://xkcd.com/327/) of all time demonstrates the problem of SQL injection specifically:

![[Pasted image 20261108081125.png]]
Injection bugs often start with [string concatenation](https://en.wikipedia.org/wiki/Concatenation). The server has an _outline_ of some code to run, like a `SELECT` query, and fills in the missing pieces with user-provided data.

Imagine an internal customer-directory endpoint that takes an `email` from the URL and stitches it directly into a SQL query:

```go
email := request.URL.Query().Get("email")
query := `
  SELECT id, email, plan
  FROM customers
  WHERE email = '` + email + `'`
rows, err := database.QueryContext(request.Context(), query)
```

An attacker can supply an `email` like this:

```sql
' OR 1=1 --
```

The resulting query becomes:

```sql
SELECT id, email, plan FROM customers WHERE email = '' OR 1=1 --'
```

The `--` comments out the rest of the line, and `OR 1=1` is always true, so, _every_ row in the `customers` table is now returned.
# Fixing SQL Injection

The best defense against SQL injection is to **parameterize every untrusted value**. A parameterized query sends SQL text and values to the database separately:

```go
rows, err := database.QueryContext(
    ctx,
    "SELECT id, name FROM products WHERE name LIKE ? LIMIT ?",
    "%"+query+"%",
    maxResults,
)
```

The database parses the statement's structure before binding `query` and `maxResults` as data. Quotes and comment markers inside those values cannot become SQL syntax.

Query generators such as [sqlc](https://sqlc.dev/) preserve that boundary while adding compile-time types. Bearly Secure already has a generated `SearchActiveProducts` query, so application code can call it without constructing SQL at runtime.
![[Pasted image 20261108214532.png]]
# Injection Beyond SQL

While SQL injection is the most _famous_ kind of injection attack, it's far from the _only_ kind. Anywhere that code is built dynamically from user input is a potential injection target:

- **Shell commands:** building a shell command from untrusted input with `exec.Command("sh", "-c", ...)`
- **Template engines:** evaluating user input as template source or expressions
- **Search filters and expressions:** query languages that accept user-defined operators

Any input that should be _data_, but can be interpreted as _instructions_ is an injection risk.

> [!DANGER] 99% of the time, the fix is to use robust, context-aware APIs that separate data from instructions, and not try to hand-roll your own interpreters and parsing logic. If you _absolutely must_ build your own interpreter, use its context-specific escaping rules instead of simply concatenating strings.

## Shell Injection

Say you want to offer an endpoint that allows a user to specify a domain name and get back its DNS information via [`nslookup`](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/nslookup). A naïve implementation might look like this:

```go
domain := request.FormValue("domain")
command := exec.CommandContext(request.Context(), "sh", "-c", "nslookup "+domain)
output, err := command.Output()
```

The endpoint concatenates whatever `domain` the user provides _directly_ into a shell command! What happens if the user submits **this**?

```sh
example.com; rm -rf ./uploads
```

Our server runs _two_ shell commands, and the second one deletes all files in the `uploads` directory!

## Avoid the Shell

One fix in this scenario would be to run `nslookup` directly with Go's [`exec.CommandContext`](https://pkg.go.dev/os/exec#CommandContext) instead of passing the command through a shell:

```go
domain := request.FormValue("domain")
command := exec.CommandContext(request.Context(), "nslookup", domain)
output, err := command.Output()
```

With `exec.CommandContext`, we specify the binary we want to run (`nslookup`), then pass its arguments separately. The `domain` string is one argument, so shell metacharacters can't launch another command.

...but that still doesn't make every argument safe. An attacker might still supply an option or another value that changes how `nslookup` behaves. The endpoint also needs to validate `domain` against the hostname syntax it intends to accept.

Once you've done everything you can to use safe APIs, it can also be a good idea to explicitly whitelist the allowed inputs and reject anything that doesn't exactly match the expected pattern.
# Safe Validation and Sanitization

User input is untrusted. I know we've been over this... sorry to beat a dead horse... but it's worth repeating.

## Validation vs. Sanitization

Two key practices here are _validation_ and _sanitization_, and they solve different problems.

- **Validation** asks: is this input allowed? It usually happens at the application boundary, when input first arrives. Invalid requests get rejected immediately.
    
- **Sanitization** deliberately transforms input according to a defined content policy. For example, a rich-text sanitizer might remove tags and attributes the application doesn't support.
    

Sanitization is different from context-specific **escaping or encoding**. HTML escaping, shell argument handling, and similar defenses belong deeper in the logic where the application knows how the value is being used. Validation usually happens earlier, and complements those defenses; it does _not_ replace them.

## Validate, Then Reject

For most user-provided data, the safest pattern is explicit validation followed by a hard reject on failure.

Imagine a user registration endpoint that accepts a `username`, limited to 20 alphanumeric characters. The endpoint could "sanitize" the input by dropping any invalid characters and applying a length cap:

```go
var invalidUsernameCharacters = regexp.MustCompile(`[^a-zA-Z0-9]`)

cleanedName := invalidUsernameCharacters.ReplaceAllString(username, "")
if len(cleanedName) > 20 {
  cleanedName = cleanedName[:20]
}
```

But that doesn't make much sense... the user's input has been silently changed! You're going to create them a username they didn't type in?!? Bad. In this case, it would be simpler _and_ safer to simply return an error on misshapen input:

```go
var usernamePattern = regexp.MustCompile(`^[a-zA-Z0-9]{3,20}$`)

func isValidUsername(input string) bool {
  return usernamePattern.MatchString(input)
}

username := request.FormValue("username")
if !isValidUsername(username) {
  http.Error(responseWriter, "Invalid username", http.StatusBadRequest)
  return
}
```
# When to Sanitize

Of course, there _are_ times when sanitization makes sense. For example, you might want to remove trailing and leading whitespace from the username input silently, as most users expect that behavior. But you _still_ should have a validation step that rejects invalid ones.

For example, you might use it when:

- Sanitizing rich text fields (comments, posts) that allow a limited set of formatting options like bold and italics
- Removing unsupported control characters from text that will be stored or processed
- Escaping characters that won't render correctly in your chosen output format (e.g., replacing `\n` with `<br>` in HTML)

## Sanitize to a Policy

Here's a "create comment" handler that validates input, sanitizes it to a small allowlist of HTML tags, and saves it:

```go
content := request.FormValue("content")
if utf8.RuneCountInString(content) > 280 {
  http.Error(responseWriter, "Invalid content", http.StatusBadRequest)
  return
}

sanitizedContent := richTextPolicy.Sanitize(content)
if strings.TrimSpace(sanitizedContent) == "" {
  http.Error(responseWriter, "Empty content", http.StatusBadRequest)
  return
}

saveComment(sanitizedContent) // Error handling omitted for brevity.
responseWriter.WriteHeader(http.StatusCreated)
```

Notice that _validation happens before and after_ the tightly scoped sanitization.
# Unsafe Archive Extraction

A [ZIP archive](https://en.wikipedia.org/wiki/ZIP_\(file_format\)) is not a single document. It's a _container_ whose entries each have a name like `july/invoice.pdf`. Extracting the archive means trusting those names enough to turn them into new file paths on your server.

... and that's dangerous when the archive came from a user.

## Path Traversal

Imagine an invoice-processing service that extracts `.zip` archives of monthly invoices into a `data/monthly-invoices` directory on the server. A naïve extractor might join that directory with every entry name:

```go
for _, entry := range archive.File {
    destination := filepath.Join(extractionDirectory, entry.Name)
    // Write the entry to the destination dir
}
```

This _looks_ well-scoped to the `extractionDirectory`, but [`filepath.Join`](https://pkg.go.dev/path/filepath#Join) only builds and normalizes a path. An attacker can provide an entry like `../../app.log`, and after normalization, the destination points to an `app.log` _outside_ of `data/monthly-invoices`!

This vulnerability is commonly called [Zip Slip](https://security.snyk.io/research/zip-slip-vulnerability). ZIP is just the delivery mechanism; the same path-traversal problem can occur whenever application code turns untrusted names into filesystem paths.

> [!DANGER] Checking the uploaded archive's filename or [MIME type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/MIME_types) does not help. The dangerous paths are _inside_ the archive, and every entry has its own attacker-controlled name.
# Safe Archive Extraction

The fix for our file-path problems is to validate each entry name _after_ resolving its destination path. We can use the _relative_ path of the destination, starting from a trusted extraction root, to make sure it doesn't escape:

```go
relativePath, err := filepath.Rel(root, destination)
if err != nil ||
    relativePath == "." ||
    relativePath == ".." ||
    strings.HasPrefix(relativePath, ".."+string(filepath.Separator)) {
    return errors.New("archive entry escapes the extraction directory")
}
```

In Go, [`filepath.Rel`](https://pkg.go.dev/path/filepath#Rel) describes how to reach a destination from a given root. Since we want to accept only destinations that fall under the extraction directory, we reject paths that meet any of these conditions:

- The path is `.`, so the destination is the root itself (bad).
- The path is `..`, so the destination is the parent directory of the root (bad).
- The path [starts with](https://pkg.go.dev/strings#HasPrefix) `..` plus the platform's [`filepath.Separator`](https://pkg.go.dev/path/filepath#pkg-constants), so the destination is outside the root (bad).

Other platform-aware checks are also worth running against archive entries before extraction. A Windows-style path with backslashes cannot safely be treated as an ordinary document path. Nor can a symbolic link.

> [!INFO] A [symbolic link](https://en.wikipedia.org/wiki/Symbolic_link), or **symlink**, is a file that _points to another_ filesystem path. These demand careful handling (or rejection). In Go, you can tell if a file is a symlink by checking whether its `Mode()` has the [`ModeSymlink` bit](https://pkg.go.dev/os#ModeSymlink) set.

For a primer on symlinks, see [our Linux course](https://www.boot.dev/lessons/7d7efcc2-abfb-4cc1-864a-be51dae65401).

## Validate Before Writing

When it comes to archives, always validate _all_ entry destinations before extracting _any_ of them. If the fifth entry is malicious, but you only discover that _after_ writing the first four, you'll get a partial extraction and leave the system in an inconsistent state.

## Zip Bombs

Archive extraction also needs **resource limits**. Compressed input can expand dramatically, so production systems should cap the number of entries and total uncompressed size. If you don't, you're open to [zip bomb](https://en.wikipedia.org/wiki/Zip_bomb) attacks. They don't leak anything, but they can bring down your servers' ability to respond by exhausting their disk space or memory as a type of [denial-of-service attack](https://owasp.org/www-community/attacks/Denial_of_Service).
