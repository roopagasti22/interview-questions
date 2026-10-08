# Technical Interview Questions and Answers

This guide covers five common questions about JavaScript and Node.js, React, Python, REST APIs, and cloud computing.

## 1. What is asynchronous programming in JavaScript and Node.js?

### Answer

Asynchronous programming lets a program start an operation that may take time—such as reading a file, making a network request, or querying a database—without blocking the rest of the program while it waits. The program can continue doing other work and handle the result when it becomes available.

JavaScript commonly represents asynchronous work with **callbacks**, **Promises**, and the `async`/`await` syntax:

- A **callback** is a function passed to another function to be called later.
- A **Promise** represents a value that may be available now, later, or never. It can be fulfilled or rejected.
- An `async` function returns a Promise. `await` pauses that function until a Promise settles, without blocking the JavaScript thread.

```js
async function loadUser(userId) {
  try {
    const response = await fetch(`/api/users/${userId}`);
    if (!response.ok) {
      throw new Error(`Request failed: ${response.status}`);
    }
    return await response.json();
  } catch (error) {
    console.error("Could not load user:", error);
    throw error;
  }
}
```

JavaScript runs synchronous code on a call stack. In Node.js, many I/O operations are handled asynchronously by the runtime and operating system; when they complete, their callbacks or Promise continuations are scheduled through the event loop. This allows a Node.js process to serve other work while waiting for I/O.

Asynchronous does **not** necessarily mean that JavaScript is executing CPU-heavy code in parallel. Long-running synchronous computation can still block the event loop. CPU-intensive work may need worker threads, child processes, or a separate service.

**In short:** asynchronous programming keeps a program responsive while it waits for operations to complete.

## 2. What is the Virtual DOM in React?

### Answer

The **Virtual DOM** is a commonly used name for React's in-memory representation of the UI. React elements describe what the interface should look like for the current application state. When state or props change, React renders an updated description and reconciles it with the previous one.

React then applies the necessary updates to the actual browser DOM. This process helps developers describe the desired UI declaratively instead of manually finding and changing DOM nodes after every state change.

```jsx
function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}
```

If `name` changes, React renders the component again, compares the resulting UI description as part of reconciliation, and updates the browser DOM as needed. React's reconciliation is not simply a guarantee that every update is faster than manually writing DOM operations; performance depends on the application and how it is implemented.

**In short:** the Virtual DOM is an in-memory UI representation that React uses during rendering and reconciliation to keep the browser DOM in sync with application state.

## 3. What is exception handling in Python?

### Answer

**Exception handling** is the process of responding to errors that occur while a program runs. Python uses `try`, `except`, `else`, and `finally` to control what happens when an exception is raised.

- Put code that may fail in a `try` block.
- Use one or more `except` blocks to handle specific exception types.
- Use `else` for code that should run only if the `try` block succeeds.
- Use `finally` for cleanup code that should run whether an exception occurred or not.
- Use `raise` to signal an error or re-raise an exception.

```python
def read_score(value: str) -> int:
    try:
        score = int(value)
    except ValueError as error:
        raise ValueError("Score must be a whole number") from error
    else:
        if score < 0:
            raise ValueError("Score cannot be negative")
        return score
```

Handle the most specific exceptions that make sense for the operation. Avoid catching every exception indiscriminately, since that can hide programming errors. For resources such as files, prefer a context manager (`with open(...)`) so cleanup happens reliably.

**In short:** exception handling lets a program handle expected runtime errors gracefully while keeping error reporting and cleanup explicit.

## 4. What is a REST API, and how does it work?

### Answer

A **REST API** is an API designed around the principles of **Representational State Transfer (REST)**. It exposes resources—such as users, products, or orders—through URLs and commonly uses HTTP methods to describe the operation to perform.

For example, an API might expose a collection of books at `/api/books` and an individual book at `/api/books/42`:

| HTTP method | Typical purpose | Example |
| --- | --- | --- |
| `GET` | Retrieve a resource | `GET /api/books/42` |
| `POST` | Create a resource | `POST /api/books` |
| `PUT` | Replace a resource | `PUT /api/books/42` |
| `PATCH` | Partially update a resource | `PATCH /api/books/42` |
| `DELETE` | Delete a resource | `DELETE /api/books/42` |

A client sends an HTTP request containing a method, URL, headers, and sometimes a body. The server processes the request, may read or change data, and returns an HTTP response with a status code, headers, and often a representation such as JSON.

```http
GET /api/books/42 HTTP/1.1
Accept: application/json
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

{"id": 42, "title": "Example Book"}
```

Common response codes include `200 OK`, `201 Created`, `204 No Content`, `400 Bad Request`, `401 Unauthorized`, `404 Not Found`, and `500 Internal Server Error`.

REST emphasizes constraints such as a uniform interface and stateless requests. **Stateless** means each request contains the information needed to process it; the server does not rely on hidden conversational state from a previous request. Authentication can still be used—the client sends appropriate credentials or a token with each request.

**In short:** a REST API lets clients work with server resources over HTTP using resource URLs, HTTP methods, and structured responses.

## 5. What is cloud computing? Explain IaaS, PaaS, and SaaS.

### Answer

**Cloud computing** is the delivery of computing resources—such as servers, storage, databases, networking, and software—over a network, typically the internet. Customers can provision resources when needed and scale their use without owning and operating all the underlying physical infrastructure.

The service models differ mainly in how much of the technology stack the provider manages:

| Model | Name | What the provider offers | What the customer typically manages |
| --- | --- | --- | --- |
| **IaaS** | Infrastructure as a Service | Virtual machines, storage, and networking | Operating system, applications, and data |
| **PaaS** | Platform as a Service | A managed platform and runtime for building and deploying applications | Application code and data |
| **SaaS** | Software as a Service | A complete application accessed over a network | How the application is used and its configured data |

- **IaaS:** The customer rents fundamental infrastructure and has substantial control over configuration. Example: provisioning a virtual machine and installing an operating system and application on it.
- **PaaS:** The provider manages more of the platform, so developers can focus on application code and deployment rather than managing the operating system and runtime.
- **SaaS:** The provider operates a complete application. Users access it through a browser or app and usually do not manage its servers or runtime.

These models form a general spectrum, not a promise that every provider product fits perfectly into one category. The exact division of responsibilities varies by service, so customers should check which security, maintenance, and configuration tasks remain theirs.

**In short:** IaaS provides infrastructure, PaaS provides a managed application platform, and SaaS provides a finished application.
