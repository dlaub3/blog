---
draft: false
date: 2018-08-28
author:
title: "Understanding HTTP"
description: A quick introduction to the HTTP specification
tags: ['http', 'http/2']
categories: ['web']
toc: true
meta:
  description: A quick introduction to the HTTP specification
  keywords: "http, http/2"

---

The web is built on the [HTTP](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol) protocol, but what is it and how does it work?

## OSI Model

HTTP is implemented on layer 7 of the [OSI Model](https://en.wikipedia.org/wiki/OSI_model). 

1. Physical
2. Data link
3. Network
4. Transport
5. Session
6. Presentation
7. **Application**

This means that HTTP is an application layer protocol. It is a protocol that the web browser and web sever agree to use to communicate with one another. It is built on top of the [Transmission Control Protocal](https://en.wikipedia.org/wiki/Transmission_Control_Protocol) which is most commonly referred to as TCP. TCP is implemented on the transport layer of the [Internet Protocal Suite](https://en.wikipedia.org/wiki/Internet_protocol_suite) (TCP/IP), which is another model for understanding networking. The OSI model and TCP/IP are different ways of categorizing networking layers. 

## HTTP/1.1

The HTTP protocol specifies how messages should be formatted for communication between the client (browser), and the server.  It's possible to see some of this information in the request and response headers in a browsers networking tools. But an even easier way is to use a tool called [curl](https://curl.haxx.se/). 

This command will display the HTTP headers and the HTML body of the response. 

```bash
curl -D - https://www.google.com
```

The text for a simple HTTP GET request might look like this:

```bash
GET / HTTP/1.0
Host: google.com
Accept-Language: en
Connection: close
\r\n
\r\n
```

The `\r\n\r\n` part is the formatting for [new lines](https://en.wikipedia.org/wiki/Newline).

And an example response. 

```bash
HTTP/1.0 200 OK
Date: Tue, 28 Aug 2018 23:45:13 GMT
Content-Type: text/html; charset=ISO-8859-1
Vary: Accept-Encoding

<!doctype html><html><body>Hello World<h1></h1></body></html>
```

This formatting of messages is all it takes to transform TCP into HTTP. 

## HTTP/2

[HTTP/2](https://http2.github.io/faq/) is an improvement of the HTTP protocol. It focuses on improving HTTP performance by transmitting binary instead of text, [multiplexing](https://en.wikipedia.org/wiki/Multiplexing) requests and responses over a single TCP connection (multiple simultaneous requests and responses), and allowing the server to push resources to the client before the client requests them. There's more to it than that. But those are my favorites. Browsers require HTTP/2 transmissions to be encrypted, so `https://` not `http://` . However, the standard doesn't actually require it.  If you have curl installed with nghttp you can use it to view HTTP/2 requests. Otherwise you can use a tool called [nghttp](https://nghttp2.org/). 

```bash
curl --version
# curl 7.61.0 (x86_64-pc-linux-gnu) libcurl/7.61.0 OpenSSL/1.1.0h zlib/1.2.11 libidn2/2.0.5 libpsl/0.20.2 (+libidn2/2.0.4) nghttp2/1.32.0
curl -vso - --http2 https://google.com 

nghttp -nv https://google.com
```

## References 

- https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol
- https://httpwg.org/specs/rfc7230.html
- https://http2.github.io/faq/
- https://http2-explained.haxx.se/content/en/
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Resources_and_specifications
- https://en.wikipedia.org/wiki/Transmission_Control_Protocol
- https://blog.cloudflare.com/tools-for-debugging-testing-and-using-http-2/
- https://en.wikipedia.org/wiki/OSI_model#Comparison_with_TCP/IP_model
- https://en.wikipedia.org/wiki/Internet_protocol_suite#Comparison_of_TCP/IP_and_OSI_layering
