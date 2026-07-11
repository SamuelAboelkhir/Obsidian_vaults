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
