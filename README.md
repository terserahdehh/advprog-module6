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
