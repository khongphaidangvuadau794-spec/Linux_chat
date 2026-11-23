# LinuxChat

A multi-client TCP chat application written in C using POSIX sockets and
POSIX threads (`pthread`) on Linux.

## Overview

LinuxChat is a learning project focused on Linux system programming and
TCP/IP socket programming. It demonstrates how a server can accept
multiple TCP connections and handle connected clients concurrently.

## Features

-   TCP client-server communication using IPv4.
-   Socket setup with `socket()`, `bind()`, `listen()`, `accept()`, and
    `connect()`.
-   Data exchange using `send()` and `recv()`.
-   Concurrent client handling with POSIX threads.
-   Support for up to 10 connected clients.
-   Message broadcasting between connected clients.
-   Shared client-state protection using a mutex.
-   Basic connection and disconnection handling.

> Note: Update this list if your final implementation differs from any
> item above.

## Project structure

``` text
LinuxChat/
├── src/
│   ├── server.c
│   └── client.c
├── include/
│   └── chat.h
└── Makefile
```

The exact files may vary depending on your final implementation.

## Requirements

-   Linux or WSL 2 with a Linux distribution such as Ubuntu.
-   GCC.
-   GNU Make.
-   POSIX threads (`pthread`), normally provided by the system's C
    library/toolchain.

On Ubuntu/WSL, install the build tools with:

``` bash
sudo apt update
sudo apt install build-essential
```

## Build

From the project root directory:

``` bash
make
```

If you are compiling manually, link with pthread support. For example:

``` bash
gcc -Wall -Wextra -g -pthread src/server.c -o server
gcc -Wall -Wextra -g -pthread src/client.c -o client
```

If your source files are split across additional `.c` files, include
those files in the build command or configure them in the `Makefile`.

## Run

Open separate terminal windows or tabs.

### 1. Start the server

From the project root:

``` bash
./server
```

### 2. Start clients

In another terminal:

``` bash
./client
```

Run the client command in additional terminals to connect more users, up
to the server's configured client limit.

If the client accepts a server address or port as command-line
arguments, use the syntax implemented by your program.

## How it works

1.  The server creates an IPv4 TCP socket.
2.  It binds the socket to its configured IP address and port.
3.  It starts listening for incoming connections.
4.  The server accepts a client connection.
5.  A worker thread handles communication with that client.
6.  Connected clients can exchange messages through the server's
    broadcast logic.
7.  Shared client information is protected with a mutex to prevent
    concurrent threads from modifying it unsafely.
8.  When a client disconnects, the server handles the disconnect and
    releases the associated resources.

## Key concepts demonstrated

  -----------------------------------------------------------------------
  Concept                             Purpose
  ----------------------------------- -----------------------------------
  TCP                                 Reliable, connection-oriented
                                      byte-stream communication

  `socket()`                          Creates a communication endpoint

  `bind()`                            Assigns a local address and port to
                                      the server socket

  `listen()`                          Marks the socket as passive and
                                      ready to accept connections

  `accept()`                          Accepts an incoming client
                                      connection

  `connect()`                         Connects a client socket to the
                                      server

  `send()` / `recv()`                 Transmit and receive bytes over a
                                      socket

  `pthread_create()`                  Starts a worker thread

  `pthread_detach()` /                Manages thread resource lifetime
  `pthread_join()`                    

  `pthread_mutex_t`                   Protects shared data accessed by
                                      multiple threads
  -----------------------------------------------------------------------

## Configuration

Check the source code or `include/chat.h` for the configured server
port, maximum number of clients, buffer sizes, and other constants.

## Troubleshooting

### `Address already in use`

A previous server process may still be running, or another process may
be using the configured port.

Check the port (replace `8080` if your project uses another port):

``` bash
ss -lntp | grep 8080
```

Stop the old server process before starting another instance.

### Client cannot connect

-   Make sure the server is running.
-   Check that the client uses the correct server IP address and port.
-   If the client and server run on different machines, check firewall
    and network settings.

### Build errors related to pthread

Make sure the compile and link command includes:

``` bash
-pthread
```

## Learning objectives

This project provides practice with:

-   C programming on Linux.
-   TCP/IP networking and socket APIs.
-   Blocking I/O and client-server communication.
-   Multithreaded programming with pthreads.
-   Synchronization and race-condition prevention.
-   Resource management and error handling.

## License

No license has been specified. Add a `LICENSE` file if you intend to
distribute this project publicly.
