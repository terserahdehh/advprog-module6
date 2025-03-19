# Commit 1 Reflection Notes

The handle_connection method starts by creating a buffered reader to read data from the TCP stream efficiently. It then reads the HTTP request one line at a time until it finds an empty line, which indicates the end of the header section. Each line is unwrapped to convert the result into a string, and all the lines are collected into a vector. The collected header lines are then printed in a formatted way. This method is designed to capture only the header part of an HTTP request. While it works well for basic request handling and debugging, it does not handle errors or process the request body, which are needed for a complete server implementation.

# Commit 2 Reflection Notes

![Commit 2 screen capture](/assets/images/commit2.png)

The updated handle_connection function creates a buffered reader for the TCP stream to read the incoming request. It collects all the HTTP request headers by reading each line until it encounters an empty line. The code then sets a status line to "HTTP/1.1 200 OK" to signal a successful response. It reads the "hello.html" file and calculates the length of its contents. The HTTP response is built by combining the status line, a header for content length, and the file contents. Finally, the response is written back to the TCP stream and sent to the client.

# Commit 3 Reflection Notes

![Commit 3 screen capture](/assets/images/commit3.png)

The code sets up a TCP listener on the local address 127.0.0.1:7878 and waits for incoming connections. When a connection is received, the main function calls the handle_connection function to process the request. Inside handle_connection, a buffered reader is created from the TCP stream to efficiently read the request, and the first line of the HTTP request is extracted. The code then checks if the request line is exactly "GET / HTTP/1.1"; if it is, the server sets the response status to "HTTP/1.1 200 OK" and chooses "hello.html" as the file to serve, otherwise it sets the status to "HTTP/1.1 404 NOT FOUND" and selects "404.html". The chosen HTML file is read into a string, and its length is calculated to include in the response header. Finally, an HTTP response is constructed with the status line, a Content-Length header, and the file contents, and this response is sent back to the client via the TCP stream.

### Before Refactoring

![Before Refactoring screen capture](/assets/images/before_refactoring.png)

### After Refactoring 

![After Refactoring screen capture](/assets/images/after_refactoring.png)

Before refactoring, the code built the response in two separate parts, causing duplicate code. This duplication made the code longer and harder to manage. The refactoring separates the decision of which status and file to use from the response-building process. The code now selects the status line and filename first and stores them in a tuple. Then, the same block of code is used to build and send the final response. This makes the function shorter, clearer, and easier to update in the future.

# Commit 4 Reflection Notes 

The updated code starts by accepting connections from a TCP listener. It reads the first line of the request and uses a match expression to choose the right response. If the request is "GET /sleep HTTP/1.1", the code makes the thread sleep for 10 seconds before choosing a response. This sleep delays the response and shows how a slow request can block the server from handling new requests quickly. The response is built once by reading a file and then sent back to the client. This design highlights that if many users send requests that cause delays, the overall performance of the server can suffer.

# Commit 5 Reflection Notes 

The ThreadPool is used to manage a fixed number of threads that can run tasks concurrently. The ThreadPool struct holds a list of worker threads and a sender channel to pass jobs to these workers. Each worker is created with its own thread that continuously waits for a job from a shared receiver. When a new connection is received, the main function creates a job (a closure that calls handle_connection) and sends it to the thread pool. A worker thread locks the receiver, gets the job, and then executes it, printing a message that it has started the job. This design makes the server more efficient and easier to manage when handling many tasks at the same time.

# Commit Bonus Reflection Notes

The updated code replaces the old "new" function with a new function named "build" in the ThreadPool implementation. The new function name makes it clear that this function sets up and returns a new thread pool. It first checks that the size is greater than zero, ensuring that a valid number of threads is requested. Then, it creates a channel and wraps the receiver in an Arc and Mutex so it can be safely shared among workers. Next, it creates a vector of workers, each with its own thread that waits for and executes incoming jobs. Finally, the ThreadPool is returned with its list of workers and the sender, ready to process tasks.