---
tags: 
- type/reference/linux
- topic/scripting
---

### Python

#### Socket illustration script
	
```
#!/bin/python3

import socket

HOST = '127.0.0.1'
PORT = 7777

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

s.connect((HOST, PORT))

```

This example shows the use of socket to
1. Create a local socket
2. Connect to the local socket at the given coordinates

- When you run this script the OS will create a client socket at a random port to connect to the chosen remove/server socket
- You can listen on this port to see the connection being established with netcat `nc -nvlp 7777`
- When creating a port scanner using the socket module, when we use `socket.socket(socket.AF_INET, socket.SOCK_STREAM)`. AF_INET is IPV4 and SOCK_STREAM is a port
- If you assigned the previous code to variable s, then now you do s.connect(HOST, PORT) where HOST is a chosen IPV4 address and PORT is a chosen port

### Challenge
-  Write a python script that utilizes web scrappers to gather first and last names of employees from an org on linkdin