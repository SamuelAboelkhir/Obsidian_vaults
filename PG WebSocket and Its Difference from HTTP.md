---
tags: 
- Other
MOC: Programming
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Other index|Back to index]]

Last Updated : 5 May, 2026

Web applications handle data in two main ways: non-real-time, where updates occur only when the client requests them, and real-time, where information is delivered instantly as it changes. Choosing the right communication protocol is important for performance and user experience.

- HTTP is used for non-real-time communication where the client requests data when needed.
- WebSocket is used for real-time communication where data is delivered instantly between client and server.

## HTTP

HTTP follows a request-response model where the connection closes after each request. It’s simple and reliable but not ideal for real-time updates.

- Each request carries full HTTP headers, which increases overhead especially for frequent updates.
- Suitable for traditional web applications where instant updates are not critical, as it is stateless and transactional.

> ****Example:**** A news website fetching the latest headlines every 10 seconds using standard HTTP requests.

Normal HTTP Flow (When we visit a webpage)

![http_connection](https://media.geeksforgeeks.org/wp-content/uploads/20260331105423566956/http_connection.webp "Click to enlarge")

HTTP

## WebSocket

WebSocket enables persistent, full-duplex communication between client and server for real-time data exchange. Unlike polling methods, it maintains a continuous connection, making it more efficient for frequent and instant updates.

- Keeps the connection open to enable continuous two-way communication between client and server.
- Reduces overhead by exchanging only message data, making it ideal for live chat, online games, and stock tickers.

> ****Example:**** A chat application where messages appear instantly on both client and server without new HTTP requests.

WebSocket Flow (When we use a web chat based application)

![websocket_connection](https://media.geeksforgeeks.org/wp-content/uploads/20260331105423643965/websocket_connection.webp "Click to enlarge")

Web Socket

### Woking

WebSocket enables real-time, full-duplex communication by upgrading a standard HTTP connection. Here’s the flow:

- ****Connection Initiation:**** The client sends an HTTP request with the header `Upgrade: websocket` to request a protocol switch.
- ****Handshake:**** The server responds with `101 Switching Protocols` to confirm the upgrade to WebSocket.
- ****Persistent Connection:**** The connection switches from HTTP to WebSocket, remaining open for continuous communication.
- ****Full-Duplex Communication:**** Both client and server can send messages independently at any time without initiating new requests.
- ****Data Frames:**** Messages are transmitted as frames (text or binary), allowing efficient and structured data exchange.
- ****Low Overhead:**** Only message data is exchanged; HTTP headers are not repeated, reducing bandwidth usage.
- ****Keep-Alive:**** The connection stays active until either the client or server explicitly closes it.
- ****Close Handshake:**** One side sends a close frame, the other acknowledges, and then the TCP connection is terminated gracefully.

### Applications

WebSocket is widely used in applications requiring real-time, low-latency, bidirectional communication. Common use cases include:

- ****Instant Messaging & Chat Apps:**** WhatsApp Web, Slack, Facebook Messenger – keep messages in sync between devices instantly.
- ****Collaborative Tools:**** Google Docs – real-time collaborative editing where changes appear immediately for all users.
- ****Video Conferencing & Live Collaboration:**** Zoom, Microsoft Teams – live presence indicators, chat messages, and interactive meeting controls.
- ****Financial & Trading Platforms:**** TradingView – real-time updates of stock, crypto, and forex prices.
- ****Live Location & Ride-Tracking Services:**** Uber – updates driver location and ride status in real time.
- ****Online Multiplayer Games:**** Many browser-based games use WebSockets to provide low-latency interactions between players.

### Limitations

WebSocket is ideal for real-time, continuous data streams, but it is not always the best choice.

- Use WebSocket for live updates, continuous feeds, or interactive applications that require near real-time communication.
- Use HTTP for one-time requests or infrequently accessed data, such as fetching historical records or static content.
- WebSocket keeps connections open, which can increase server resource usage.
- Choosing the right protocol ensures better performance and avoids unnecessary overhead.

## HTTP Vs WebSocket Connection

HTTP is a stateless protocol over TCP (connection-oriented, reliable via retransmission), while WebSocket is a stateful, full-duplex, bidirectional protocol.

|HTTP|WebSocket|
|---|---|
|Stateless; connection closes after each request|Persistent; connection stays open until closed|
|Request-response only|Full-duplex; client and server can send anytime|
|Higher overhead due to repeated headers|Low overhead; only message data is exchanged|
|Higher latency for frequent updates|Low latency; near real-time updates|
|Used for standard web pages, APIs, forms|Used for live chat, online games, stock tickers, dashboards|

> ****Note:**** Depending on your project you have to choose where it will be WebSocket or HTTP Connection.