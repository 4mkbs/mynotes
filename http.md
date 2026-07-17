# HTTP

HTTP stands for Hypertext Transfer Protocol. It is the foundation of the World Wide Web (WWW).

- **Layer**: Application layer (Layer 7 of OSI model)
- **Purpose**: HTTP is the protocol used for client-server communication
- **Data Format**: HTTP requests carry encoded information in the form of headers and body

## HTTP Messages

Both HTTP requests and responses are structured as **HTTP messages**, which consist of three main parts:

1. **Start Line**: Indicates the message type (request or response) and provides essential information (such as the HTTP method/verb, target URI, and protocol version for requests, or the protocol version and status code/text for responses).
2. **Headers**: Case-insensitive key-value pairs separated by a colon (`:`), providing metadata about the message. Some header lines can be very long (e.g., the `Cookie` header).
3. **Empty Line**: Indicates the end of the headers.
4. **Body (Optional)**: Contains the actual data or payload of the message (e.g., JSON payload, HTML, image file).

## HTTP Methods

**Definition**: HTTP methods are a set of request methods to indicate the desired action to be performed for a given resource.

| Method  | Purpose                                                             | Safe? | Idempotent?     |
| ------- | ------------------------------------------------------------------- | ----- | --------------- |
| GET     | Retrieve a representation of a resource                             | Yes   | Yes             |
| POST    | Create a new resource or submit data to be processed by the server  | No    | No              |
| PUT     | Replace a resource or create it at the target URI                   | No    | Yes             |
| DELETE  | Remove a resource                                                   | No    | Yes (usually)   |
| PATCH   | Apply partial modifications to a resource                           | No    | Not necessarily |
| HEAD    | Same as GET but returns only headers (no body)                      | Yes   | Yes             |
| OPTIONS | Describe communication options for the target resource or server    | Yes   | Yes             |
| TRACE   | Perform a message loop-back test along the request path             | No    | Yes             |
| CONNECT | Establish a tunnel to the server (commonly for TLS through a proxy) | No    | No              |

## HTTP Status Codes

**Definition**: HTTP status codes are three-digit numbers returned by the server to indicate the result of the client's request.

There are five classes of status codes:

- **1xx (Informational)**: The request was received and the process is continuing.
  - `100 Continue`
  - `101 Switching Protocols`
  - `102 Processing`
  - `103 Early Hints`

- **2xx (Success)**: The request was received, understood, and accepted.
  - `200 OK`
  - `201 Created`
  - `202 Accepted`
  - `204 No Content`

- **3xx (Redirection)**: Further action needs to be taken by the client to fulfill the request.
  - `301 Moved Permanently`
  - `302 Found`
  - `304 Not Modified`
  - `307 Temporary Redirect`
  - `308 Permanent Redirect`

- **4xx (Client Error)**: The request contains incorrect syntax or cannot be fulfilled.
  - `400 Bad Request`
  - `401 Unauthorized`
  - `402 Payment Required`
  - `403 Forbidden`
  - `404 Not Found`
  - `405 Method Not Allowed`
  - `409 Conflict`
  - `422 Unprocessable Entity`
  - `429 Too Many Requests`

- **5xx (Server Error)**: The server failed to fulfill an apparently valid request.
  - `500 Internal Server Error`
  - `501 Not Implemented`
  - `502 Bad Gateway`
  - `503 Service Unavailable`
  - `504 Gateway Timeout`

## HTTP Headers

**Definition**: HTTP headers are key-value pairs sent between the client and server to provide additional information about the request or response.

### Common Headers

- `Content-Type` - Specifies the media type of the resource
- `Content-Length` - Specifies the size of the response body
- `User-Agent` - Identifies the client application
- `Accept` - Specifies the media types accepted by the client
- `Authorization` - Provides credentials for authentication
- `Connection` - Controls whether the network connection stays open after the current transaction finishes
- `Date` - Contains the date and time at which the message was originated
- `Cache-Control` - Controls caching behavior for requests and responses

### Request Headers

- `Host` - Specifies the domain name of the server
- `Cookie` - Sends stored client-side data to the server
- `Accept-Encoding` - Specifies supported compression formats
- `Accept-Language` - Specifies the natural languages and locales that the client prefers
- `Origin` - Specifies the origin of a cross-origin request
- `Referer` - Specifies the address of the previous web page from which a link was followed
- `If-None-Match` - Makes the request conditional, checking if the resource ETag has changed
- `If-Modified-Since` - Makes the request conditional, checking if the resource has changed since a timestamp

### Response Headers

- `Set-Cookie` - Sends cookies from the server to the client
- `Server` - Identifies the server software
- `Location` - Provides a redirect target URI
- `WWW-Authenticate` - Indicates how to authenticate to access a resource
- `Access-Control-Allow-Origin` - Specifies which origins are allowed to access the resource (CORS)
- `Content-Encoding` - Specifies the compression algorithm used on the response body (e.g., `gzip`, `br`)
- `ETag` - A unique version identifier for the returned resource representation
- `Last-Modified` - The date and time at which the resource was last modified
- `Strict-Transport-Security` - Forces the browser to connect to the server only via secure HTTPS
- `Content-Security-Policy` - Restricts resources (scripts, images, etc.) the browser is allowed to load

### Notes

- Header names are case-insensitive
- Each header uses a colon to separate the name and value
- Long header values can be split across lines in some formats, but are usually written on one line

## Evolution of HTTP

HTTP has evolved through several versions since its inception:

- **HTTP/0.9**: The first version, very simple with only a GET method and no headers.
- **HTTP/1.0**: Added headers, status codes, and support for methods like POST and PUT.
- **HTTP/1.1**: Added persistent connections, pipelining, chunked transfer encoding, and caching improvements.
- **HTTP/2**: Added multiplexing, header compression, binary framing, and server push.
- **HTTP/3**: Uses the QUIC protocol over UDP for lower latency and better connection handling.

### Key Idea

Each new version focused on making HTTP faster, more efficient, and more reliable.

## HTTP Caching

HTTP caching allows clients (browsers), proxies, and CDNs to store responses and reuse them for later requests. This reduces latency, bandwidth usage, and server load.

### Cache Locations

- **Browser cache (private cache)**: Stored per user/device.
- **Shared cache (public cache)**: Stored by proxies or CDNs and shared across users.

### Core Concepts

- **Freshness**: A response is fresh if it can be used without contacting the origin server.
- **Stale**: A response is stale when its freshness lifetime has expired.
- **Revalidation**: A stale response can be validated with the origin using conditional requests.

### Important Headers

#### Response Headers

- `Cache-Control`: Main caching directives.
  - `max-age=<seconds>`: Fresh for this duration.
  - `s-maxage=<seconds>`: Freshness for shared caches (e.g. CDNs).
  - `public`: May be cached by public/shared caches.
  - `private`: Only browser/private cache may store it.
  - `no-cache`: May be stored, but must revalidate before reuse.
  - `no-store`: Do not store at all.
  - `must-revalidate`: Once stale, must revalidate.
  - `proxy-revalidate`: Similar to `must-revalidate` but applies only to shared caches.
  - `immutable`: Content will not change during freshness lifetime.
  - `stale-while-revalidate=<seconds>`: Allow serving stale content while fetching a fresh copy in background.
  - `stale-if-error=<seconds>`: Allow serving stale content if server returns an error.
  - `no-transform`: Prevent intermediaries (CDNs/proxies) from transforming content (like compressing images).
- `Expires`: Absolute expiration date/time (older mechanism).
- `ETag`: Entity tag (version identifier).
- `Last-Modified`: Timestamp of last change.
- `Vary`: Defines which request headers affect cache key (for example `Vary: Accept-Encoding`).

#### Request Headers

- `Cache-Control`: Client-side cache behavior:
  - `no-cache`: Must revalidate cached response with server before serving.
  - `no-store`: Client requests that no cache store request/response.
  - `max-age=<seconds>`: Client only accepts response younger than specified age.
  - `max-stale[=<seconds>]`: Client accepts a stale response, optionally within limit.
  - `min-fresh=<seconds>`: Client accepts response only if it remains fresh for at least specified seconds.
  - `only-if-cached`: Return only a cached response without making network request.
- `If-None-Match`: Validator using `ETag`.
- `If-Modified-Since`: Validator using `Last-Modified`.

### Validation Flow

1. Client requests a resource.
2. Server returns `200 OK` with cache headers (`Cache-Control`, `ETag`, etc.).
3. Cache stores response and serves it while fresh.
4. After stale, client/cache sends conditional request:
   - `If-None-Match: <etag>` and/or `If-Modified-Since: <date>`.
5. Server responds:
   - `304 Not Modified` (reuse cached body), or
   - `200 OK` with updated body and new validators.

### Common Caching Strategies

- **Static assets (CSS/JS/images)**:
  - `Cache-Control: public, max-age=31536000, immutable`
  - Use file hashing/versioning in filenames.
- **HTML/documents**:
  - Usually shorter TTL, often with revalidation.
  - Example: `Cache-Control: no-cache` (store + revalidate).
- **Sensitive data**:
  - `Cache-Control: no-store`.

### Practical Notes

- `no-cache` does **not** mean “do not cache”; it means “revalidate before reuse.”
- `no-store` is the directive that prevents storage.
- Use `Vary` carefully; too many variations reduce cache hit rate.
- Correct caching policy greatly improves performance and scalability.

## HTTP Cookies

HTTP cookies are small key-value data stored by the browser and sent with matching requests to help maintain state.

### What cookies are used for

- **Session management**: login sessions, shopping carts
- **Personalization**: language/theme preferences
- **Analytics and tracking**: usage measurement (first-party or third-party)

### How cookies work

1. Server sends a `Set-Cookie` response header.
2. Browser stores the cookie based on its attributes.
3. Browser sends it back in the `Cookie` request header for matching domain/path.

Example:

```http
Set-Cookie: sessionId=abc123; Path=/; HttpOnly; Secure; SameSite=Lax
```

```http
Cookie: sessionId=abc123
```

### Common cookie attributes

- `Name=Value`: Cookie key and value.
- `Domain`: Which host(s) can receive the cookie.
- `Path`: URL path prefix that must match.
- `Expires`: Absolute expiration date/time.
- `Max-Age`: Lifetime in seconds (preferred over `Expires` when both exist).
- `Secure`: Sent only over HTTPS.
- `HttpOnly`: Not accessible via JavaScript (`document.cookie`).
- `SameSite`: Controls cross-site sending behavior:
  - `Strict`: Only same-site requests.
  - `Lax`: Same-site + top-level navigation GETs.
  - `None`: Cross-site allowed, must also include `Secure`.

### Cookie types

- **Session cookie**: No `Expires`/`Max-Age`; removed when browser session ends.
- **Persistent cookie**: Has `Expires` or `Max-Age`; stored until expiry.
- **First-party cookie**: Set by the site the user is visiting.
- **Third-party cookie**: Set by another domain (often restricted by browsers).

### Security and privacy notes

- Use `Secure` + `HttpOnly` for authentication/session cookies.
- Use `SameSite` to reduce CSRF risk.
- Avoid storing sensitive data directly in cookies.
- Keep cookie values small and minimal.
- Clear cookies by setting `Max-Age=0` (or past `Expires`).

### Limits (practical)

- Around 4 KB per cookie (varies by browser).
- Browsers also limit total cookies per domain.


## HTTP CORS

Cross-Origin Resource Sharing (CORS) is a browser mechanism that allows a web page from one origin to request resources from another origin.

### Key idea

- Browsers enforce the same-origin policy by default.
- CORS relaxes that restriction using HTTP response headers.
- The server decides which cross-origin requests are allowed.

### Origin

An origin is made of:

- Scheme: `http` or `https`
- Host: domain name or IP address
- Port: optional, but part of the origin

Examples:

- `https://example.com`
- `https://example.com:443`
- `http://localhost:3000`

### CORS headers

#### Response headers

- `Access-Control-Allow-Origin` - Allowed origin
- `Access-Control-Allow-Methods` - Allowed HTTP methods
- `Access-Control-Allow-Headers` - Allowed request headers
- `Access-Control-Allow-Credentials` - Allows credentials in cross-origin requests
- `Access-Control-Expose-Headers` - Exposes extra response headers to JavaScript
- `Access-Control-Max-Age` - Caches preflight results

#### Request headers

- `Origin` - Origin of the requesting page
- `Access-Control-Request-Method` - Requested method for preflight
- `Access-Control-Request-Headers` - Requested headers for preflight

### Simple requests

Simple requests can be sent directly without a preflight when they use allowed methods and headers.

Common simple methods:

- `GET`
- `HEAD`
- `POST`

### Preflight requests

For non-simple requests, the browser sends an `OPTIONS` preflight request first.

Flow:

1. Browser sends `OPTIONS` with `Origin` and request details.
2. Server responds with `Access-Control-Allow-*` headers.
3. Browser sends the actual request only if it is allowed.

### Credentials

If cookies, authorization headers, or client certificates are included:

- The server must send `Access-Control-Allow-Credentials: true`.
- `Access-Control-Allow-Origin` cannot be `*`; it must be a specific origin.
- The client must opt in to credentials.

### Examples

Allow one origin:

```http
Access-Control-Allow-Origin: https://example.com
```

Allow methods:

```http
Access-Control-Allow-Methods: GET, POST, PUT, DELETE, OPTIONS
```

Allow headers:

```http
Access-Control-Allow-Headers: Content-Type, Authorization
```

Allow credentials:

```http
Access-Control-Allow-Credentials: true
```

### Notes

- CORS is enforced by browsers, not servers.
- CORS does not stop requests from being sent; it limits JavaScript access to the response.
- Misconfigured CORS can expose sensitive data.

# How HTTP Works

HTTP (Hypertext Transfer Protocol) is a request-response protocol used by clients and servers to exchange data.

## Basic flow

1. A client sends an HTTP request to a server.
2. The request includes a method, path, headers, and sometimes a body.
3. The server processes the request and returns an HTTP response.
4. The response includes a status code, headers, and sometimes a body.

## Common methods

- `GET` — retrieve data
- `POST` — create data
- `PUT` — replace data
- `PATCH` — partially update data
- `DELETE` — remove data

## Example

```http
GET /users HTTP/1.1
Host: example.com
Accept: application/json
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

[{"id":1,"name":"Alice"}]
```

## Key ideas

- HTTP is stateless: each request is independent.
- Headers add metadata such as content type, caching, and authentication.
- HTTPS is HTTP over TLS and encrypts traffic.
