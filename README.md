# lib_netsockets
C++ light wrapper for POSIX and Winsock sockets using TCP. Examples include:
<br /> 
* TCP client/server using custom JSON messages
* TCP client/server sending SQL as JSON to SQLite database in TCP server
* HTTP client/server
* FTP client
* SSL TCP client

Dependencies 
------------

[gason](https://github.com/vivkin/gason) (included)
<br /> 

[SQLite](https://www.sqlite.org/) (included)
<br />

[OpenSSL](https://www.openssl.org/) ( included)
<br />


Building
------------

Get source:
<pre>
git clone https://github.com/pedro-vicente/lib_netsockets.git
cd build
cmake ..
make
</pre>

Building in Windows 
------------
<pre>
bld.bat
</pre>

# Usage
lib_netsockets is C++ light wrapper for POSIX and Winsock sockets with implementation of TCP client/server using JSON messages,and HTTP, FTP clients.

# TCP server example
```c++
tcp_server_t server(2000);
while (true)
{
 socket_t socket = server.accept_client();
 handle_client(socket);
 socket.close();
}
server.close();
```

# TCP client example
```c++
tcp_client_t client("127.0.0.1", 2000);
client.open();
client.write(buf, strlen(buf));
client.read_some(buf, sizeof(buf));
client.close();
```

# HTTP client example
```c++
http_t client("www.mysite.com", 80);
client.get("/my/path/to/file", true);
```

# FTP client example
Get file list from FTP server and first file in list
```c++
ftp_t ftp("my.ftp.site", 21);
ftp.login("my user", "anonymous");
ftp.get_file_list();
ftp.get_file(ftp.m_file_nslt.at(0).c_str());
ftp.logout();
```

# SQLite server/client example
Send SQL commands as JSON array from TCP client to TCP server

```
CREATE TABLE IF NOT EXISTS table_places(place_id TEXT PRIMARY KEY NOT NULL,address CHAR(50) NOT NULL,rank INTEGER NOT NULL);
INSERT INTO table_places VALUES('home', '102 E. Green St. Urbana IL 61801', 1);
SELECT * FROM table_places WHERE place_id = 'home';
```

# TCP JSON server/client output

```
server listening on port 2001
Wed May 16 13:53:21 2018 server accepted: 127.0.0.1 <260>
Wed May 16 13:53:21 2018 server received: {"start_year":2017}
Wed May 16 13:53:21 2018 server sent: {"next_year":2018}
```

# HTTP REST example request

Example REST API

https://jsonplaceholder.typicode.com/

```
./http_client -s jsonplaceholder.typicode.com -t "GET  /posts/1"
```

request

```
client connected to: jsonplaceholder.typicode.com:80
GET  /posts/1 HTTP/1.1
Host: jsonplaceholder.typicode.com
Accept: application/json
Connection: close
```


response

```
HTTP/1.1 200 OK
Date: Wed, 16 May 2018 22:34:45 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 292
Connection: close
Set-Cookie: __cfduid=d1f77010e32b4333f04dff766134fd4eb1526510085; expires=Thu, 16-May-19 22:34:45 GMT; path=/; domain=.typicode.com; HttpOnly
X-Powered-By: Express
Vary: Origin, Accept-Encoding
Access-Control-Allow-Credentials: true
Cache-Control: public, max-age=14400
Pragma: no-cache
Expires: Thu, 17 May 2018 02:34:45 GMT
X-Content-Type-Options: nosniff
Etag: W/"124-yiKdLzqO5gfBrJFrcdJ8Yq0LGnU"
Via: 1.1 vegur
CF-Cache-Status: HIT
Server: cloudflare
CF-RAY: 41c1503f5416241a-IAD

{
  "userId": 1,
  "id": 1,
  "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
  "body": "quia et suscipit\nsuscipit recusandae consequuntur expedita et cum\nreprehenderit molestiae ut ut quas totam\nnostrum rerum est autem sunt rem eveniet architecto"
}
```