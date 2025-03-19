# Commit 1 Reflection Notes

The handle_connection method starts by creating a buffered reader to read data from the TCP stream efficiently. It then reads the HTTP request one line at a time until it finds an empty line, which indicates the end of the header section. Each line is unwrapped to convert the result into a string, and all the lines are collected into a vector. The collected header lines are then printed in a formatted way. This method is designed to capture only the header part of an HTTP request. While it works well for basic request handling and debugging, it does not handle errors or process the request body, which are needed for a complete server implementation.

# Commit 2 Reflection Notes

![Commit 2 screen capture](/assets/images/commit2.png)

The updated handle_connection function creates a buffered reader for the TCP stream to read the incoming request. It collects all the HTTP request headers by reading each line until it encounters an empty line. The code then sets a status line to "HTTP/1.1 200 OK" to signal a successful response. It reads the "hello.html" file and calculates the length of its contents. The HTTP response is built by combining the status line, a header for content length, and the file contents. Finally, the response is written back to the TCP stream and sent to the client.