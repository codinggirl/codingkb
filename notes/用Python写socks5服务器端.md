---
title: 用Python写socks5服务器端
created: '2019-09-12T08:24:35.868Z'
modified: '2019-09-12T08:24:40.598Z'
---

## [用Python写socks5服务器端](https://web.archive.org/web/20120419032623/http://xiaoxia.org/2011/03/29/written-by-python-socks5-server/ "用Python写socks5服务器端")

参考自RFC1928: [http://xiaoxia.org/?p=2672](https://web.archive.org/web/20120419032623/http://xiaoxia.org/?p=2672)

直接运行这个程序就给本机建立了一个socks5的代理服务器。

代码如下：

[view plain](#)[copy to clipboard](#)[print](#) [?](#)

1.  import socket, sys, select, SocketServer, struct, time

3.  class ThreadingTCPServer(SocketServer.ThreadingMixIn, SocketServer.TCPServer): pass
4.  class Socks5Server(SocketServer.StreamRequestHandler):
5.      def handle\_tcp(self, sock, remote):
6.          fdset = \[sock, remote\]
7.          whileTrue:
8.              r, w, e = select.select(fdset, \[\], \[\])
9.              if sock in r:
10.                  if remote.send(sock.recv(4096)) <=  0: break
11.              if remote in r:
12.                  if sock.send(remote.recv(4096)) <=  0: break
13.      def handle(self):
14.          try:
15.              print'socks connection from ', self.client\_address
16.              sock = self.connection
17.              # 1. Version
18.              sock.recv(262)
19.              sock.send(b"\\x05\\x00");
20.              # 2. Request
21.              data = self.rfile.read(4)
22.              mode = ord(data\[1\])
23.              addrtype = ord(data\[3\])
24.              if addrtype == 1:       # IPv4
25.                  addr = socket.inet\_ntoa(self.rfile.read(4))
26.              elif addrtype == 3:     # Domain name
27.                  addr = self.rfile.read(ord(sock.recv(1)\[0\]))
28.              port = struct.unpack('>H', self.rfile.read(2))
29.              reply = b"\\x05\\x00\\x00\\x01"
30.              try:
31.                  if mode == 1:  # 1. Tcp connect
32.                      remote = socket.socket(socket.AF\_INET, socket.SOCK\_STREAM)
33.                      remote.connect((addr, port\[0\]))
34.                      print'Tcp connect to', addr, port\[ 0\]
35.                  else:
36.                      reply = b"\\x05\\x07\\x00\\x01"# Command not supported
37.                  local = remote.getsockname()
38.                  reply += socket.inet\_aton(local\[0\]) + struct.pack(">H", local\[ 1\])
39.              except socket.error:
40.                  # Connection refused
41.                  reply = '\\x05\\x05\\x00\\x01\\x00\\x00\\x00\\x00\\x00\\x00'
42.              sock.send(reply)
43.              # 3. Transfering
44.              if reply\[1\] == '\\x00':   # Success
45.                  if mode == 1:     # 1. Tcp connect
46.                      self.handle\_tcp(sock, remote)
47.          except socket.error:
48.              print'socket error'
49.  def main():
50.      server = ThreadingTCPServer(('', 1080), Socks5Server)
51.      server.serve\_forever()
52.  if \_\_name\_\_ == '\_\_main\_\_':
53.      main()

import socket, sys, select, SocketServer, struct, time

class ThreadingTCPServer(SocketServer.ThreadingMixIn, SocketServer.TCPServer): pass
class Socks5Server(SocketServer.StreamRequestHandler):
    def handle\_tcp(self, sock, remote):
        fdset = \[sock, remote\]
        while True:
            r, w, e = select.select(fdset, \[\], \[\])
            if sock in r:
                if remote.send(sock.recv(4096)) <= 0: break
            if remote in r:
                if sock.send(remote.recv(4096)) <= 0: break
    def handle(self):
        try:
            print 'socks connection from ', self.client\_address
            sock = self.connection
            # 1. Version
            sock.recv(262)
            sock.send(b"\\x05\\x00");
            # 2. Request
            data = self.rfile.read(4)
            mode = ord(data\[1\])
            addrtype = ord(data\[3\])
            if addrtype == 1:       # IPv4
                addr = socket.inet\_ntoa(self.rfile.read(4))
            elif addrtype == 3:     # Domain name
                addr = self.rfile.read(ord(sock.recv(1)\[0\]))
            port = struct.unpack('>H', self.rfile.read(2))
            reply = b"\\x05\\x00\\x00\\x01"
            try:
                if mode == 1:  # 1. Tcp connect
                    remote = socket.socket(socket.AF\_INET, socket.SOCK\_STREAM)
                    remote.connect((addr, port\[0\]))
                    print 'Tcp connect to', addr, port\[0\]
                else:
                    reply = b"\\x05\\x07\\x00\\x01" # Command not supported
                local = remote.getsockname()
                reply += socket.inet\_aton(local\[0\]) + struct.pack(">H", local\[1\])
            except socket.error:
                # Connection refused
                reply = '\\x05\\x05\\x00\\x01\\x00\\x00\\x00\\x00\\x00\\x00'
            sock.send(reply)
            # 3. Transfering
            if reply\[1\] == '\\x00':  # Success
                if mode == 1:    # 1. Tcp connect
                    self.handle\_tcp(sock, remote)
        except socket.error:
            print 'socket error'
def main():
    server = ThreadingTCPServer(('', 1080), Socks5Server)
    server.serve\_forever()
if \_\_name\_\_ == '\_\_main\_\_':
    main()

已经修正Google SyntaxHighlighter无法正确显示Python代码的问题。问题出自shBrushPython.js中定义regexList数组的时候末尾多了一个","。删去就行了。神奇的是，只有IEcore才报错……

