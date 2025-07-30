
<https://youtu.be/3bxAm3XIFmk>

The Asynchronous Request-Reply pattern is used in distributed systems to handle responses in an asynchronous environment, such as one using message queues. It allows a client to send a request and continue its own processing without blocking, while still being able to receive a response when it's ready.

The document describes three common ways to implement this pattern:

### 1. Using a Correlation ID
This is the most common approach.
*   **How it works:** The system uses two message queues: a `request queue` and a `reply queue`.
    1.  The client (requester) sends a message to the `request queue`. This message includes a unique `message ID` (e.g., `124`).
    2.  The client then waits for a message on the `reply queue` that has a specific `correlation ID` matching the original `message ID`.
    3.  The server (receiver) processes the request and sends the response back to the `reply queue`, setting the `correlation ID` of the response message to the ID of the request message it's replying to.
    4.  The client receives the message with the matching `correlation ID` and knows it's the response to its specific request.

### 2. Using a Temporary Queue
This method simplifies the process by creating a dedicated queue for each reply.
*   **How it works:**
    1.  Instead of a permanent `reply queue`, the client creates a temporary, private queue for a specific request.
    2.  It sends the request message with a `reply-to` header that contains the address of this temporary queue.
    3.  The server sends the response directly to the queue specified in the `reply-to` header.
    4.  Since this queue is exclusive to that one request, the client can simply retrieve the message without needing to filter by a correlation ID.
    5.  Once the response is received, the temporary queue is deleted.

### 3. Using HTTP Polling
This is an alternative approach often used in web APIs for long-running tasks.
*   **How it works:**
    1.  A client sends a request to an API endpoint to start a long-running process.
    2.  The API immediately responds with an `HTTP 202 (Accepted)` status, which includes a `Location` header pointing to a status-check URL.
    3.  The client periodically sends `GET` requests to this status URL to check if the process is complete. The server will keep responding with `202` as long as the task is running.
    4.  Once the task is finished, the status endpoint responds with a redirect (e.g., `HTTP 302 Found`) that points to the URL of the final resource.
    5.  The client then follows the redirect to fetch the result of the operation.

