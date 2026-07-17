
# RESTful Architecture: The Core Principles & Design Guide

## 1. What is REST?
**REST (Representational State Transfer)** is not a protocol, tool, or library. It is an **architectural style** introduced by Roy Fielding in his 2000 PhD dissertation. It defines a set of constraints for creating web services. 

An API that adheres to these constraints is called a **RESTful API**. In REST, data is treated as **Resources**, and the client modifies the state of these resources through **Representations** (usually JSON or XML).

---

## 2. The 6 Architectural Constraints (Roy Fielding's Rules)
To be classified as truly RESTful, an architecture must comply with the following constraints (the 6th is optional):

### I. Client-Server Separation
The user interface concerns (Client) are strictly separated from data storage and business logic concerns (Server). This improves the portability of the UI across multiple platforms and allows the backend to scale independently.

### II. Statelessness
The server **must not** store any client context (session state) between requests. Every request from the client must contain all the information necessary to understand and process it. 
*   **Advantage:** High scalability and reliability, as any server in a cluster can handle any request independently.

### III. Cacheability
Responses must implicitly or explicitly define themselves as cacheable or non-cacheable. If cacheable, the client has the right to reuse that response data for later, equivalent requests.

### IV. Layered System
A client cannot ordinarily tell whether it is connected directly to the end server or to an intermediary along the way (e.g., Load Balancers, Proxies, CDNs). This enables load balancing and shared caches to improve scalability.

### V. Uniform Interface (The Heart of REST)
This is the central feature that distinguishes REST from other network styles. It decouples the architecture, allowing it to evolve independently. It has 4 sub-principles:
1.  **Identification of Resources:** Resources are identified in requests (e.g., using URIs in web-based REST systems).
2.  **Manipulation through Representations:** The client holds a representation of a resource (like a JSON object) and uses it to modify or delete the resource on the server.
3.  **Self-descriptive Messages:** Each message includes enough information to describe how to process it (e.g., specifying `Content-Type: application/json`).
4.  **HATEOAS (Hypermedia as the Engine of Application State):** The server provides links dynamically within the response, guiding the client on what actions are available next.

### VI. Code on Demand (Optional)
Servers can temporarily extend or customize the functionality of a client by transferring executable code (e.g., JavaScript).

---

## 3. Resource-Oriented URI Design (Best Practices)
In REST, the URI is solely used to identify the **Resource**. It should never indicate the action being performed.

### 🎯 The Golden Rules of Naming:
*   **Use Nouns, Not Verbs:** Action words belong in the HTTP Method, not the URI.
    *   ❌ `/get-books`
    *   ✅ `/books`
*   **Use Plural Nouns:** Represents a collection of resources.
    *   ❌ `/user/10`
    *   ✅ `/users/10`
*   **Keep Hierarchy Logical (Max 2-3 levels):** Express relationships cleanly.
    *   ❌ `/articles-of-author-5`
    *   ✅ `/authors/5/articles`
*   **Use Query Parameters for Actions/Modifiers:** Use `?` for filtering, sorting, or pagination rather than creating new URIs.
    *   ❌ `/active-users`
    *   ✅ `/users?status=active`

---

## 4. Mapping REST to HTTP
While REST can work on other protocols, it is almost exclusively implemented over HTTP. A RESTful API maps CRUD (Create, Read, Update, Delete) operations to HTTP methods cleanly:

| HTTP Method | REST Action on a Collection (`/users`) | REST Action on a Specific Resource (`/users/1`) | Idempotent? |
| :--- | :--- | :--- | :--- |
| **GET** | Retrieve a list of all users | Retrieve details of user 1 | Yes |
| **POST** | Create a new user in the collection | *Generally not used on specific resources* | No |
| **PUT** | Replace the entire collection *(Rare)* | Replace/Update the entire user 1 data | Yes |
| **PATCH** | *Not applicable* | Partially update user 1 (e.g., only email) | No (Technically) |
| **DELETE** | Delete the entire collection *(Rare)* | Delete user 1 | Yes |

---

## 5. The Richardson Maturity Model (RMM)
A model to grade how "RESTful" an API actually is, ranging from Level 0 to Level 3.

*   **Level 0 (The Swamp of POX):** Uses HTTP strictly as a transport system. (e.g., sending XML/JSON over a single `POST /api` endpoint for all actions).
*   **Level 1 (Resources):** Introduces URIs to identify individual resources (e.g., `/users/1`), but still uses only one HTTP method (usually POST) for everything.
*   **Level 2 (HTTP Verbs):** Correctly uses standard HTTP methods (GET, POST, PUT, DELETE) and HTTP status codes to perform and report actions on resources. *(Most modern APIs stop here).*
*   **Level 3 (Hypermedia Controls / HATEOAS):** The API includes hypermedia links in responses, making it fully self-discoverable. *(The true definition of REST).*