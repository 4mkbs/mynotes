# 🌐 How a Web Browser Works — The Complete Picture

> A comprehensive guide explaining everything that happens from typing a URL to displaying a webpage.

---

# Table of Contents

1. Introduction
2. Browser Architecture
3. Navigation Process
4. URL Parsing
5. DNS Resolution
6. TCP Connection
7. TLS Handshake (HTTPS)
8. HTTP Request
9. Server Processing
10. HTTP Response
11. Browser Networking Stack
12. HTML Parsing
13. DOM Construction
14. CSS Parsing
15. CSSOM Construction
16. JavaScript Engine
17. Render Tree
18. Layout (Reflow)
19. Paint
20. Compositing
21. GPU Rendering
22. Event Loop
23. Browser Storage
24. Caching
25. Cookies
26. Service Workers
27. Security Model
28. Same-Origin Policy
29. CORS
30. Browser Processes
31. Modern Browser Architecture
32. Complete Timeline
33. Performance Optimizations
34. Summary

---

# 1. Introduction

When you type:

```
https://example.com
```

and press **Enter**, thousands of operations happen within milliseconds.

The browser must:

- Understand the URL
- Find the server
- Establish a secure connection
- Download resources
- Parse HTML
- Execute JavaScript
- Build the page
- Paint pixels
- Handle user interaction

Everything happens in a carefully optimized pipeline.

---

# 2. Browser Architecture

Modern browsers use multiple processes.

```
                Browser Process
               ┌────────────────┐
               │ UI             │
               │ Address Bar    │
               │ History        │
               │ Downloads      │
               └────────────────┘
                      │
      ┌───────────────┼───────────────┐
      │               │               │
      ▼               ▼               ▼
 Renderer        Renderer       Renderer
 Process         Process        Process

      │
      ▼
 JavaScript Engine
 HTML Parser
 CSS Engine
 Layout
 Paint

      │
      ▼
 GPU Process
```

Popular browsers:

- Chrome → Blink + V8
- Firefox → Gecko + SpiderMonkey
- Safari → WebKit + JavaScriptCore

---

# 3. Navigation Process

User types:

```
https://example.com
```

Browser workflow:

```
User
   │
   ▼
Parse URL
   │
DNS Lookup
   │
TCP Connect
   │
TLS Handshake
   │
HTTP Request
   │
Receive HTML
   │
Parse HTML
   │
Download CSS
   │
Download JS
   │
Build DOM
   │
Build CSSOM
   │
Render Tree
   │
Layout
   │
Paint
   │
Composite
   │
Display Page
```

---

# 4. URL Parsing

Input:

```
https://example.com:443/products?id=5#reviews
```

Browser splits:

```
Protocol:
https

Host:
example.com

Port:
443

Path:
/products

Query:
id=5

Fragment:
reviews
```

---

# 5. DNS Resolution

The browser needs an IP address.

```
example.com
↓

93.184.216.34
```

Lookup order:

```
Browser Cache
↓

Operating System Cache
↓

Router Cache
↓

ISP DNS

↓

Root DNS

↓

TLD DNS

↓

Authoritative DNS
```

Finally:

```
example.com

↓

93.184.216.34
```

---

# 6. TCP Connection

Browser establishes TCP.

Three-way handshake:

```
Browser               Server

SYN
---------------------->

        SYN + ACK
<----------------------

ACK
---------------------->

Connection Established
```

---

# 7. TLS Handshake (HTTPS)

For HTTPS:

```
Client Hello

↓

Server Hello

↓

Certificate

↓

Key Exchange

↓

Encrypted Connection
```

After this:

```
All communication is encrypted.
```

---

# 8. HTTP Request

Example:

```
GET / HTTP/1.1

Host: example.com

User-Agent: Chrome

Accept: text/html

Cookie: session=abc123
```

---

# 9. Server Processing

Server:

```
Receive Request

↓

Routing

↓

Authentication

↓

Database Query

↓

Business Logic

↓

Generate HTML

↓

Send Response
```

---

# 10. HTTP Response

Example:

```
HTTP/1.1 200 OK

Content-Type: text/html

Cache-Control: max-age=3600
```

Body:

```html
<html>
<body>
<h1>Hello</h1>
</body>
</html>
```

---

# 11. Browser Networking Stack

```
Application Layer

↓

HTTP

↓

TLS

↓

TCP

↓

IP

↓

Ethernet

↓

Physical Network
```

---

# 12. HTML Parsing

Input:

```html
<html>

<body>

<h1>Hello</h1>

</body>

</html>
```

Tokenizer creates tokens:

```
<html>

<body>

<h1>

Text

</h1>

</body>

</html>
```

---

# 13. DOM Construction

DOM Tree:

```
Document

│

└── html

    ├── head

    └── body

         └── h1

             └── "Hello"
```

DOM is an object representation.

---

# 14. CSS Parsing

Example:

```css
h1{
color:red;
font-size:32px;
}
```

Tokenizer →

Parser →

CSS Rules

---

# 15. CSSOM Construction

```
CSS

↓

Parser

↓

CSSOM
```

Example:

```
Stylesheet

│

└── h1

     ├── color:red

     └── font-size:32px
```

---

# 16. JavaScript Engine

Pipeline:

```
Source Code

↓

Lexer

↓

Parser

↓

AST

↓

Interpreter

↓

JIT Compiler

↓

Machine Code
```

Example:

```javascript
console.log("Hello");
```

becomes executable machine instructions.

---

# 17. Render Tree

Browser combines:

```
DOM

+

CSSOM

↓

Render Tree
```

Render Tree includes only visible elements.

Example:

```
html

↓

body

↓

div

↓

button
```

---

# 18. Layout (Reflow)

Browser calculates:

```
Width

Height

Position

Margins

Padding
```

Example:

```
Button

x = 100

y = 200

width = 250

height = 50
```

---

# 19. Paint

Browser paints:

```
Text

↓

Borders

↓

Background

↓

Images

↓

Shadows

↓

Gradients
```

Each becomes drawing commands.

---

# 20. Compositing

Page divided into layers.

```
Background

──────────────

Text

──────────────

Buttons

──────────────

Video

──────────────

Popup
```

Layers improve performance.

---

# 21. GPU Rendering

GPU receives paint commands.

```
CPU

↓

Paint Commands

↓

GPU

↓

Rasterization

↓

Framebuffer

↓

Display
```

GPU accelerates:

- Animations
- Transforms
- Opacity
- Video
- Canvas

---

# 22. Event Loop

JavaScript is single-threaded.

```
Call Stack

↓

Event Loop

↓

Task Queue

↓

Microtask Queue
```

Order:

```
Call Stack

↓

Microtasks

↓

Rendering

↓

Macrotasks
```

---

# 23. Browser Storage

Different storage options:

```
Cookies

Session Storage

Local Storage

IndexedDB

Cache Storage
```

---

# 24. Caching

Multiple cache levels:

```
Memory Cache

↓

Disk Cache

↓

Service Worker Cache

↓

CDN Cache

↓

Server
```

Caching avoids unnecessary downloads.

---

# 25. Cookies

Example:

```
session=abc123

theme=dark
```

Sent with every matching request.

Used for:

- Login sessions
- Preferences
- Analytics

---

# 26. Service Workers

Acts like a programmable proxy.

```
Browser

↓

Service Worker

↓

Cache

↓

Network
```

Enables:

- Offline support
- Push notifications
- Background sync

---

# 27. Security Model

Browser security includes:

- HTTPS
- Sandbox
- Permissions
- Site Isolation
- CSP
- Mixed Content Blocking

---

# 28. Same-Origin Policy

Allowed:

```
example.com

↓

example.com
```

Blocked:

```
example.com

↓

evil.com
```

Unless explicitly allowed.

---

# 29. CORS

Server response:

```
Access-Control-Allow-Origin:

https://myapp.com
```

Browser checks permission before exposing the response.

---

# 30. Browser Processes

Chrome architecture:

```
Browser Process

↓

Renderer Process

↓

GPU Process

↓

Network Process

↓

Utility Process

↓

Extension Process
```

Each tab usually has its own renderer process.

---

# 31. Modern Browser Architecture

```
User

↓

Browser UI

↓

Browser Process

↓

Network Service

↓

Renderer

↓

HTML Parser

↓

DOM

↓

CSS Parser

↓

CSSOM

↓

JavaScript Engine

↓

Render Tree

↓

Layout

↓

Paint

↓

GPU

↓

Monitor
```

---

# 32. Complete Timeline

```
User enters URL
        │
        ▼
Parse URL
        │
        ▼
DNS Lookup
        │
        ▼
TCP Handshake
        │
        ▼
TLS Handshake
        │
        ▼
HTTP Request
        │
        ▼
Server Processing
        │
        ▼
HTTP Response
        │
        ▼
HTML Parser
        │
        ▼
DOM
        │
        ▼
CSS Download
        │
        ▼
CSSOM
        │
        ▼
JavaScript Execution
        │
        ▼
Render Tree
        │
        ▼
Layout
        │
        ▼
Paint
        │
        ▼
Compositing
        │
        ▼
GPU Rendering
        │
        ▼
Page Visible
```

---

# 33. Performance Optimizations

Browsers optimize rendering by using techniques such as:

- DNS prefetching
- TCP connection reuse (Keep-Alive)
- HTTP/2 multiplexing
- HTTP/3 (QUIC)
- Resource preloading
- Lazy loading images
- Script deferring (`defer`)
- Async script loading (`async`)
- Code splitting
- Compression (Gzip/Brotli)
- Browser caching
- Service workers
- GPU acceleration
- Incremental rendering
- Speculative parsing
- Request prioritization

---

# 34. Summary

The browser performs a sophisticated sequence of networking, parsing, execution, and rendering tasks:

```
Type URL
     │
     ▼
Parse URL
     │
     ▼
DNS Resolution
     │
     ▼
TCP/TLS Connection
     │
     ▼
HTTP Request
     │
     ▼
Receive HTML
     │
     ▼
Parse HTML → DOM
     │
     ▼
Parse CSS → CSSOM
     │
     ▼
Execute JavaScript
     │
     ▼
Create Render Tree
     │
     ▼
Layout
     │
     ▼
Paint
     │
     ▼
Composite Layers
     │
     ▼
GPU Rasterization
     │
     ▼
Display Page
     │
     ▼
Handle User Events
     │
     ▼
Update DOM/CSSOM
     │
     ▼
Repeat Layout → Paint → Composite as Needed
```

---

# Key Concepts at a Glance

| Component | Purpose |
|-----------|---------|
| URL Parser | Splits the URL into protocol, host, path, query, and fragment |
| DNS Resolver | Converts domain names to IP addresses |
| TCP | Establishes a reliable connection |
| TLS | Encrypts communication for HTTPS |
| HTTP | Transfers requests and responses |
| HTML Parser | Builds the DOM tree |
| CSS Parser | Builds the CSSOM tree |
| JavaScript Engine | Executes scripts and manipulates the DOM |
| Render Tree | Combines DOM and CSSOM for rendering |
| Layout | Calculates element sizes and positions |
| Paint | Draws pixels for each visual element |
| Compositor | Combines layers for efficient rendering |
| GPU | Accelerates rasterization and display |
| Event Loop | Coordinates JavaScript execution and asynchronous events |
| Cache | Speeds up future page loads |
| Service Worker | Enables offline support and request interception |