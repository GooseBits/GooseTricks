# Contents
[[_TOC_]]

# Servers
## Python TCP Server
```python
import sys
import socket
import ssl

if (sys.version_info > (3, 0)):
    import socketserver as SocketServer
else:
    import SocketServer


class TCPHandler(SocketServer.StreamRequestHandler):

    def handle(self):
        self.request.settimeout(60)
        data = self.rfile.readline()
        print(data)
        self.wfile.write(b"hello\n")
        # Loop to maintain connection, otherwise connection ends here


server = SocketServer.ThreadingTCPServer(('', 8081), TCPHandler, bind_and_activate=False)
server.allow_reuse_address = True
server.server_bind()
server.server_activate()
# ~ server.socket = ssl.wrap_socket(
    # ~ server.socket, 
    # ~ keyfile="<CERT_KEY>", 
    # ~ certfile="<CERT_FILE>"
    # ~ cert_reqs=ssl.CERT_NONE
# ~ )
server.daemon_threads = True

server.serve_forever()
```

## Python UDP Server
```python
import sys
import socket

if (sys.version_info > (3, 0)):
    import socketserver as SocketServer
else:
    import SocketServer


class UDPHandler(SocketServer.BaseRequestHandler):

    def handle(self):
        
        data = self.request[0].strip()
        port = self.server.server_address[1]
        print(data)
        
        socket = self.request[1]
        socket.sendto(b"hello\n", self.client_address)


server = SocketServer.ThreadingUDPServer(('', 8081), UDPHandler, bind_and_activate=False)
server.allow_reuse_address = True
server.server_bind()
server.server_activate()
server.daemon_threads = True

server.serve_forever()
```

# Clients
## Python TCP Client
```python
import socket

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM, socket.IPPROTO_IP)

sock.connect(("127.0.0.1", 8080))

# Set timeout in seconds
sock.settimeout(5)

# Write/read with file object
sfile = sock.makefile("rw") # Could add 'b' for bytes mode
sfile.write("hi\n")
sfile.flush() # Flush buffer and actually send
resp = sfile.read(4)
print(resp)

# Write/read raw
sock.send(b"raw send\n") # Sending raw, no file object
resp = sock.recv(4)
print(resp) # These are bytes

sock.close()
```

## Python UDP Client
```python
import socket

sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM, socket.IPPROTO_IP)

sock.sendto(b"raw send\n", ("127.0.0.1", 8080)) # Sending raw, no file object
resp = sock.recvfrom(4)
print(resp)

sock.close()
```
