# TCP Client Script

## Introduction

The TCP Client Script establishes a connection to a TCP server, sends a series of messages, and receives their MD5 hash responses from the server. This script is useful for testing TCP server functionality, learning about network communication, and understanding message hashing.

## Features

- **Connect to TCP Server**: Establishes a connection to a specified server and port.
- **Send Messages**: Sends a predefined set of messages to the server.
- **Receive MD5 Hashes**: Receives and displays the MD5 hash of each message from the server.
- **Error Handling**: Gracefully handles connection errors and other exceptions.

## Prerequisites

- Python 3.6 or higher.

## Modules

- **socket**: Provides low-level networking interface.
- **sys**: Allows for system-specific parameters and functions, including error handling.

```python
import socket
import sys
```

## Functions

### `connect_to_server(hostname, port)`

Creates a TCP socket and connects it to the specified server and port.

- **Parameters:**
  - `hostname` (str): The server's hostname or IP address.
  - `port` (int): The port number on which the server is listening.
- **Returns:**
  - `socket`: The connected client socket.

#### Example

```python
def connect_to_server(hostname, port):
    client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    client_socket.connect((hostname, port))
    return client_socket
```

### `send_messages(client_socket)`

Sends a series of messages to the connected server and prints the received MD5 hash responses.

- **Parameters:**
  - `client_socket` (socket): The connected client socket.
- **Returns:**
  - `None`

#### Example

```python
def send_messages(client_socket):
    messages = [
        "Hello, server!",
        "How are you today?",
        "It's a nice day for coding.",
        "Python is fun.",
        "I'm learning about networks.",
        "What's your favorite protocol?",
        "TCP is reliable.",
        "UDP is faster.",
        "Have a great day!",
        "Goodbye, server!"
    ]
    
    for message in messages:
        print(f"Sending: {message}")
        client_socket.sendall(message.encode('utf-8'))
        response = client_socket.recv(2048)
        md5_hash = response.decode('utf-8')
        print(f"Original message: {message}")
        print(f"MD5 Hash received: {md5_hash}\n")
```

## Usage

1. **Set the Server Hostname and Port**: The script is set to connect to a server running on the same host (`localhost`) on port `5555`. Ensure the server is running and listening on the specified port.

    ```python
    PORT = 5555
    localHost = socket.gethostname()
    ```

    **Note**: Make sure to replace `'localhost'` and `5555` with the actual hostname and port number of your server if they are different.

2. **Run the Script**: Execute the script in your Python environment.

    ```bash
    python script_name.py
    ```

3. **Monitor the Output**: The script will display messages indicating the connection status, the messages sent, and the MD5 hashes received from the server.

## Detailed Instructions

1. **Ensure Server Availability**: Before running the client script, make sure the server is running and listening on the specified hostname and port. You can use a simple TCP server script for testing purposes.

    Example server script:
    ```python
    import socket
    import threading
    import hashlib

    def handle_client(client_socket):
        while True:
            message = client_socket.recv(1024)
            if not message:
                break
            md5_hash = hashlib.md5(message).hexdigest()
            client_socket.send(md5_hash.encode('utf-8'))
        client_socket.close()

    def start_server():
        server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        server.bind(('0.0.0.0', 5555))
        server.listen(5)
        print("Server listening on port 5555")
        
        while True:
            client_socket, addr = server.accept()
            print(f"Accepted connection from {addr}")
            client_handler = threading.Thread(target=handle_client, args=(client_socket,))
            client_handler.start()

    if __name__ == "__main__":
        start_server()
    ```

2. **Understand the Client Script Flow**:
    - **Connect to Server**: The `connect_to_server` function creates a socket and connects to the specified server.
    - **Send Messages**: The `send_messages` function sends a series of predefined messages to the server, receives the MD5 hash responses, and prints them.
    - **Handle Errors**: If any error occurs during connection or message sending, the script will print an error message and exit.

3. **Modify Messages**: You can customize the list of messages in the `send_messages` function to send different messages to the server.

4. **Close the Socket**: The client socket is closed after all messages are sent and responses are received to ensure proper resource cleanup.

## Example Output

When the client successfully connects and communicates with the server, the output will be similar to:

```plaintext
Client Application
Establishing a connection to the server
Available on the same host using PORT 5555

Successfully connected to: localhost on PORT 5555
Sending: Hello, server!
Original message: Hello, server!
MD5 Hash received: <md5_hash_of_message>

...
```

## Error Handling

- **Connection Errors**: If the client fails to connect to the server, an error message will be displayed, and the program will exit.

    ```plaintext
    An error occurred: [Errno 111] Connection refused
    ```

- **Other Exceptions**: Any unexpected errors will be caught and displayed, and the program will exit.

## Security Considerations

- **Secure Communication**: Consider using secure communication protocols like TLS/SSL for sensitive data.
- **Input Validation**: Ensure proper validation of input data to prevent injection attacks.

## FAQs

**Q: What happens if the server is not running?**
A: The client will fail to connect and display an error message indicating the connection issue.

**Q: Can the script handle different message formats?**
A: The script currently sends and receives string messages. It can be modified to handle different formats if needed.

**Q: How can I modify the server hostname and port?**
A: Update the `localHost` and `PORT` variables in the `main()` function to match your server's hostname and port.

## Troubleshooting

- **Connection Issues**: Ensure the server is running and listening on the specified hostname and port.
- **Module Not Found Errors**: Ensure Python is installed correctly and all necessary modules are available. Install any missing modules using `pip`.

For further assistance or to report bugs, please reach out to the repository maintainers or open an issue on the project's issue tracker.
