---
tags:
- EH
MOC: IT
---
[[_0000 Home|Home]] | [[_0005 IT MOC|Back to IT MOC]] | [[EH Ethical Hacking index|Back to index]]
# Server-side request forgery
## What it is
- This is a vulnerability were the attacker causes a server application to send a request to an unintended location
- An example would be to cause the server to connect to an internal service within an organization's infrastructure
- We can also cause the server to connect to arbitrary external systems, which can lead to the leakage of sensitive data
## Impact
- A successful SSRF attack can often result in unauthorized actions or access to data within the organization. This can be in the vulnerable application, or on other back-end systems that the application can communicate with. In some situations, the SSRF vulnerability might allow an attacker to perform arbitrary command execution.
- An SSRF exploit that causes connections to external third-party systems might result in malicious onward attacks. These can appear to originate from the organization hosting the vulnerable application.
## How it's done
- An SSRF attack normally plays around the trusts relationships of the vulnerable application and other services it can communicate with
- A common example would be causing the application to make an HTTP request back to the server that is hosting it through the loopback network interface `127.0.0.1`
### Example
#### SSRF attack against the server
- Imagine we have a shopping app, that lets users view if items are in stock for a particular store
- The stock information comes by having the app query various backend REST APIs, which is done by passing the URL to the relevant endpoint via a frontend HTTP request
- So for the user to view the stock status for an item, their browser would make the following request
```
POST /product/stock HTTP/1.0 Content-Type: application/x-www-form-urlencoded Content-Length: 118 stockApi=http://stock.weliketoshop.net:8080/product/stock/check%3FproductId%3D6%26storeId%3D1
```
- In the request, the server will use the following API URL to retrieve the stock data, and return it to the user
- So the server trusts requests coming from the frontend, and will query any URL specified in the request, so an attacker can modify the request as follows
```
POST /product/stock HTTP/1.0 Content-Type: application/x-www-form-urlencoded Content-Length: 118 stockApi=http://localhost/admin
```
- And now the server will instead fetch the contents of the `/admin` URL and return it to the user
- The attacker could have queried the URL himself, but as he's not trusted, and has no access because he's not an authenticated user, he wouldn't get any meaningful information
- So by using the server's trust relationship to his advantage, he was able to bypass this restriction
- So basically
![[Pasted image 20260711231421.png]]
#### SSRF attack against other backend systems
- Sometimes, the app server can interact with other systems that are not directly reachable by users, and have non-routable private IPs
- These systems are normally protected by the network topology itself, meaning they have weaker security
- They usually contain sensitive functionalities that can be accessed without authentication by anyone who can interact with them
- One such system, is still an admin interface, but this time, it's on a different IP in the network rather than the same server as the backend
```
POST /product/stock HTTP/1.0 Content-Type: application/x-www-form-urlencoded Content-Length: 118 stockApi=http://192.168.0.68/admin
```
- Burp intruder is a good tool for iterating over all the networks in the range of `http://192.168.0.X/admin` to find the right one
## Circumventing common SSRF defenses
### SSRF with blacklist-based input filters
- Some applications block input containing hostnames like `127.0.0.1` and `localhost`, or sensitive URLs like `/admin`. In this situation, you can often circumvent the filter using the following techniques:
	- Use an alternative IP representation of `127.0.0.1`, such as `2130706433`, `017700000001`, or `127.1`.
	- Register your own domain name that resolves to `127.0.0.1`. You can use `spoofed.burpcollaborator.net` for this purpose.
	- Obfuscate blocked strings using URL encoding or case variation.
	- Provide a URL that you control, which redirects to the target URL. Try using different redirect codes, as well as different protocols for the target URL. For example, switching from an `http:` to `https:` URL during the redirect has been shown to bypass some anti-SSRF filters.
### SSRF with whitelist-based input filters
- Some applications only allow inputs that match, a whitelist of permitted values. The filter may look for a match at the beginning of the input, or contained within in it. You may be able to bypass this filter by exploiting inconsistencies in URL parsing.
- The URL specification contains a number of features that are likely to be overlooked when URLs implement ad-hoc parsing and validation using this method:
	- You can embed credentials in a URL before the hostname, using the `@` character. For example:
	    `https://expected-host:fakepassword@evil-host`
	- You can use the `#` character to indicate a URL fragment. For example:
	    `https://evil-host#expected-host`
	- You can leverage the DNS naming hierarchy to place required input into a fully-qualified DNS name that you control. For example:
	    `https://expected-host.evil-host`
	- You can URL-encode characters to confuse the URL-parsing code. This is particularly useful if the code that implements the filter handles URL-encoded characters differently than the code that performs the back-end HTTP request. You can also try [double-encoding](https://portswigger.net/web-security/essential-skills/obfuscating-attacks-using-encodings#obfuscation-via-double-url-encoding) characters; some servers recursively URL-decode the input they receive, which can lead to further discrepancies.
	- You can use combinations of these techniques together.